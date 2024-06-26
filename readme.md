# Frequency Chaos Game Representation

Frequency Chaos Game Representation (FCGR), an extension of Chaos Game Representation (CGR), is a reliable method for encoding DNA sequences. The CGR for DNA utilizes a square with vertices that represent the four nucleotides: guanine (G), thymine (T), adenine (A), and cytosine (C). These vertices are positioned at coordinates (-1, -1), (-1, 1), (1, -1), and (1, 1), respectively, as shown in Fig. (a). In this figure (a), the walker's path for the DNA sequence GTCA is depicted. Starting at the origin, the walker moves halfway towards G, then continues to move halfway towards each subsequent nucleotide, using the new halfway point as the starting point each time, until the entire sequence is processed.

![image](https://github.com/abhi1238/FCGR/assets/115684930/34d03daf-2d25-4436-849d-80a0b6cdd9d1)


Subsequently, depending on the user-selected $k$-mer length ($k$), the ranges of $x = (-1, 1)$ and $y = (-1, 1)$ are divided into a $2^k \times 2^k$ grid. Each cell in this grid corresponds to one $k$-mer of length $k$, as illustrated in Fig. (b). This grid, which labels each cell with a specific $k$-mer, is referred to as the $k$-mer key matrix in this paper. The Frequency Chaos Game Representation (FCGR) matrix is then generated by counting the occurrences of each $k$-mer in the DNA sequence, determined by the frequency with which the walker visits each cell during its traversal.

## Usage

The fcgr Python package offers a wide array of functionalities for generating FCGR matrix and more:

* Read a FASTA file to generate a concatenated DNA sequence.
* Generate the FCGR *k*-mer matrix key based on the specified *k*-mer length.
* Retrieve the position of a *k*-mer within the FCGR key matrix as a tuple.
* Return the *k*-mer of the specified length located at the given index.
* Compute the FCGR matrix for a provided DNA sequence and kmer length using the FCGR *k*-mer key matrix.
* Calculate the frequency of *k*-mers with a specified length in the FASTA file and provide them in dictionary format.
* Obtain the count of the specified *k*-mer within the provided DNA sequence.

## Installation
To install fcgr you must make sure that your python version is 3.10.13+.

### Prerequisites
* pandas = 2.1.4
* numpy = 1.22.4
* biopython = 1.81
* collections = 0.1.6

## Setup
fcgr can be installed with pip as shown below,

```bash
!pip install fcgr-0.1-py3-none-any.whl

```

## fcgr functionality

```bash
fcgr.read_fasta(file_path: str)
```

Reads a FASTA file at the given `file_path` and concatenates the sequences into a single string. For accurate FCGR matrix creation, it is recommended to use the entire genome sequence from the FASTA file to maintain the DNA sequence's integrity.

#### Parameters

- `file_path` (str): The path to the FASTA file.

#### Return

- `str`: Concatenated DNA sequence from the FASTA file.

```bash
fcgr.chaos_game_representation_key(kmer_length: int)
```
Generates the FCGR *k*-mer key matrix for FCGR for the given *k*-mer length.

#### Parameters

- `kmer_length` (int): The length of the *k*-mer.

#### Returns

- `np.ndarray`: A 2D numpy array representing the *k*-mer key matrix for FCGR.

```bash
fcgr.return_kmer_index(kmer: str)
```
Returns the index of a specific *k*-mer in the FCGR *k*-mer key matrix.

#### Parameters

- `kmer` (str): The *k*-mer for which the index is to be found.

#### Returns

- `tuple`: The row and column indices of the *k*-mer in the FCGR key matrix.

```bash
fcgr.return_kmer_at_index(kmer_length: int, tuple_index: tuple)
```
Returns the *k*-mer at the specified index in the FCGR key matrix.

#### Parameters
* kmer_length (int): The length of the *k*-mer used to generate the FCGR matrix.
* tuple_index (tuple): The index (row, column) of the *k*-mer in the matrix.

#### Return
* str: The *k*-mer at the specified index.
  
```bash
fcgr.chaos_frequency_matrix(fasta_string: str, kmer_length: int, chaos_game_kmer_array: np.array = None, pseudo_count: bool = True)
```

This function generates the FCGR matrix for a given DNA sequence and *k*-mer length, using the FCGR key matrix.

#### Parameters
* fasta_string (str): The DNA sequence in FASTA format.
* kmer_length (int): The length of the *k*-mers to consider.
* chaos_game_kmer_array (np.array, optional):  FCGR *k*-mer key matrix if precomputed, otherwise it will be calculated internally. Defaults to None, which calculates the FCGR *k*-mer key matrix internally.
* pseudo_count (bool, optional): Whether to add 1 to each *k*-mer of the matrix. Defaults to True.

#### Return
* tuple: A tuple containing:
  - np.array: The FCGR matrix representing *k*-mer frequencies.
  - np.array: The FCGR *k*-mer key matrix used.
 
```bash    
fcgr.chaos_frequency_dictionary(fasta_string: str, kmer_length: int, chaos_game_kmer_array: np.array = None, pseudo_count: bool = True)
```
Calculate the frequency dictionary of *k*-mers.

#### Parameters
* fasta_string (str): The input DNA sequence in FASTA format.
* kmer_length (int): The length of *k*-mers.
* chaos_game_kmer_array (np.array): FCGR *k*-mer key matrix if precomputed, otherwise it will be calculated internally. Defaults to None, which calculates the FCGR *k*-mer key matrix internally.
* pseudo_count (bool): Whether to add 1 to each *k*-mer of the FCGR matrix. Defaults to True.
#### Returns
* frequency_dictionary (dict): A dictionary containing *k*-mer as keys and their frequencies as values.

```bash   
fcgr.return_kmer_count_individual(key_name: str, fasta_content: str)
```

Calculate the count of a specific *k*-mer in a given DNA sequence.

#### Parameters
* key_name (str): The *k*-mer sequence for which the count is to be calculated.
* fasta_content (str): The input DNA sequence in which the *k*-mer count is to be calculated.

#### Returns
* count (int): The count of the specified *k*-mer in the DNA sequence.

## fcgr example use

The [tutorial section](./tutorial) provides detailed explanations for each functionality, including their docstrings.
Here is an example of how to use fcgr in Python:

### Reads a FASTA file at the given `file_path` and concatenates the sequences into a single string.
```bash 
file_path = "GCF_000005845.2_ASM584v2_genomic.fna"
fasta_seq = fcgr.read_fasta(file_path)

```

### Frequency chaos game kmer matrix key

```bash 
chaos_game_kmer_array = fcgr.chaos_game_representation_key(kmer_length=2)
chaos_game_kmer_array
```

```bash 
Output : [['TT', 'CT', 'TC', 'CC'],
          ['GT', 'AT', 'GC', 'AC'],
          ['TG', 'CG', 'TA', 'CA'],
          ['GG', 'AG', 'GA', 'AA']]

```
### Returns the index of a specific $k$-mer in the FCGR key matrix.
```bash 
fcgr.return_kmer_index(kmer = "AAA")

```

```bash 
Output : (7, 7)
```
### Returns the $k$-mer at the specified index in the FCGR *k*-mer key matrix.
```bash 
fcgr.return_kmer_at_index(kmer_length=3, tuple_index=(7, 0))

```
```bash 
Output: 'GGG'
```

### Generates the FCGR matrix for the given DNA sequence.
```bash 
fasta_seq_dummy  = "ATTGCNATRATTT" 
fcgr_freq_matrix, fcgr_key_array = fcgr.chaos_frequency_matrix(fasta_string= fasta_seq_dummy, kmer_length=3, chaos_game_kmer_array=None,  pseudo_count = False)
fcgr_freq_matrix
```
```bash 
Output:(array([[1., 0., 0., 0., 0., 0., 0., 0.],
            [0., 2., 0., 0., 0., 0., 0., 0.],
            [0., 0., 0., 0., 1., 0., 0., 0.],
            [0., 0., 0., 0., 0., 0., 0., 0.],
            [1., 0., 0., 0., 0., 0., 0., 0.],
            [0., 0., 0., 0., 0., 0., 0., 0.],
            [0., 0., 0., 0., 0., 0., 0., 0.],
            [0., 0., 0., 0., 0., 0., 0., 0.]]),
     array([['TTT', 'CTT', 'TCT', 'CCT', 'TTC', 'CTC', 'TCC', 'CCC'],
          ['GTT', 'ATT', 'GCT', 'ACT', 'GTC', 'ATC', 'GCC', 'ACC'],
          ['TGT', 'CGT', 'TAT', 'CAT', 'TGC', 'CGC', 'TAC', 'CAC'],
          ['GGT', 'AGT', 'GAT', 'AAT', 'GGC', 'AGC', 'GAC', 'AAC'],
          ['TTG', 'CTG', 'TCG', 'CCG', 'TTA', 'CTA', 'TCA', 'CCA'],
          ['GTG', 'ATG', 'GCG', 'ACG', 'GTA', 'ATA', 'GCA', 'ACA'],
          ['TGG', 'CGG', 'TAG', 'CAG', 'TGA', 'CGA', 'TAA', 'CAA'],
          ['GGG', 'AGG', 'GAG', 'AAG', 'GGA', 'AGA', 'GAA', 'AAA']],
         dtype='<U3'))
```



### Calculate the frequency dictionary of $k$-mers

```bash
fasta_seq_dummy  = "ATTGCNATRATTT" 
fcgr.chaos_frequency_dictionary(fasta_string= fasta_seq_dummy, kmer_length=3, chaos_game_kmer_array=chaos_game_kmer_array, pseudo_count = False)
```

{'TTT': 1.0,
 'CTT': 0.0,
 'TCT': 0.0,
 'CCT': 0.0,
 'TTC': 0.0,
 'CTC': 0.0,
 'TCC': 0.0,
 'CCC': 0.0,
 'GTT': 0.0,
 'ATT': 2.0,
 'GCT': 0.0,
 'ACT': 0.0,
 'GTC': 0.0,
 'ATC': 0.0,
 'GCC': 0.0,
 'ACC': 0.0,
 'TGT': 0.0,
 'CGT': 0.0,
 'TAT': 0.0,
 'CAT': 0.0,
 'TGC': 1.0,
 'CGC': 0.0,
 'TAC': 0.0,
 'CAC': 0.0,
 'GGT': 0.0,
 'AGT': 0.0,
 'GAT': 0.0,
 'AAT': 0.0,
 'GGC': 0.0,
 'AGC': 0.0,
 'GAC': 0.0,
 'AAC': 0.0,
 'TTG': 1.0,
 'CTG': 0.0,
 'TCG': 0.0,
 'CCG': 0.0,
 'TTA': 0.0,
 'CTA': 0.0,
 'TCA': 0.0,
 'CCA': 0.0,
 'GTG': 0.0,
 'ATG': 0.0,
 'GCG': 0.0,
 'ACG': 0.0,
 'GTA': 0.0,
 'ATA': 0.0,
 'GCA': 0.0,
 'ACA': 0.0,
 'TGG': 0.0,
 'CGG': 0.0,
 'TAG': 0.0,
 'CAG': 0.0,
 'TGA': 0.0,
 'CGA': 0.0,
 'TAA': 0.0,
 'CAA': 0.0,
 'GGG': 0.0,
 'AGG': 0.0,
 'GAG': 0.0,
 'AAG': 0.0,
 'GGA': 0.0,
 'AGA': 0.0,
 'GAA': 0.0,
 'AAA': 0.0}


### Calculate the count of a specific $k$-mer in a given DNA sequence.

```bash
fasta_seq_dummy  = "ATTGCNATRATTT" 
fcgr.return_kmer_count_individual(key_name = "ATT", fasta_content =fasta_seq_dummy)
```
```bash 
output: 2
```



