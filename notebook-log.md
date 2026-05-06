Reproducible Script for Phylogenetics Analysis 
===
## Getting Data 
### Data was taken from the NCBI
Link to all data can be found here: ...
On the NCBI website, I looked for specific species of penguins like *Aptenodytes forsteri* (my main species). 

Selected the "Protein sequences (FASTA)" as my download option. Given a zip file containing all the information, I went to the file named "protein.faa" an FAA file. I read the contents in Notepad, a text reader.
Contents looked like:
```Notes
>YP_009166094.1 COX1 [organism=Aptenodytes forsteri] [GeneID=26045193]
MTFINRWLFSTNHKDIGTLYLIFGAWAGMAGTALSLLIRAELGQPGTLLGDDQIYNVIVTAHAFVMIFFM
VMPIMIGGFGNWLVPLMIGAPDMAFPRMNNMSFWLLPPSFLLLLASSTVEAGAGTGWTVYPPLAGNLAHA
GPSVDLAIFSLHLAGVSSILGAINFITTAINMKPPALSQYQTPLFVWSVLITAVLLLLSLPVLAAGITML
LTDRNLNTTFFDPAGGGDPVLYQHLFWFFGHPEVYILILPGFGIISHVVTYYAGKKEPFGYMGMVWAMLS
IGFLGFIVWAHHMFTVGMDVDTRAYFTSATMIIAIPTGIKVFSWLATLHGGTIKWDPPMLWALGFIFLFT
IGGLTGIVLANSSLDIALHDTYYVVAHFHYVLSMGAVFAILAGFTHWFPLFTGYTLHTTWAKAHFGVMFT
GVNLTFFPQHFLGLAGMPRRYSDYPDAYTVWNTMSSIGSLISMTAVIMLMFIIWEAFTSKRKVLQPELTA
TNVEWIHGCPPPYHTFEEPAFVQVQE
```
Combined all species into a readable file for MAFFT to run. End product looked like:
```
>Aptenodytes_forsteri_COX1
MTFINRWLFSTNHKDIGTLYLIFGAWAGMAGTALSLLIRAELGQPGTLLGDDQIYNVIVTAHAFVMIFFM
VMPIMIGGFGNWLVPLMIGAPDMAFPRMNNMSFWLLPPSFLLLLASSTVEAGAGTGWTVYPPLAGNLAHA
GPSVDLAIFSLHLAGVSSILGAINFITTAINMKPPALSQYQTPLFVWSVLITAVLLLLSLPVLAAGITML
>Aptenodytes_patagonicus_COX1
MTFINRWLFSTNHKDIGTLYLIFGAWAGMAGTALSLLIRAELGQPGTLLGDDQIYNVIVTAHAFVMIFFM
VMPIMIGGFGNWLVPLMIGAPDMAFPRMNNMSFWLLPPSFLLLLASSTVEAGAGTGWTVYPPLAGNLAHA
GPSVDLAIFSLHLAGVTSILGAINFITTAINMKPPALSQYQTPLFVWSVLITAVLLLLSLPVLAAGITML
```
*This is a small snippet from the entire file*
## MAFFT - Aligning the Data 
### Requirements

- Windows with PowerShell
- [Python 3](https://www.python.org/downloads/)
- [MAFFT for Windows](https://mafft.cbrc.jp/alignment/software/)
- A GitHub account with a repository set up
---
### Step 1: File Preparation
Prior to alignment, the input FASTA file was renamed to ensure compatibility with downstream software:
```powershell
# Rename the input file to proper FASTA format
Rename-Item p.fasta.txt p.fasta
```

### Step 2: Sequence Alignment
Multiple sequence alignment was performed using MAFFT (v7.526) with the automatic parameter selection option:
```powershell
.\mafft.bat --auto p.fasta > aligned_penguins.fasta
```
The `--auto` flag allows MAFFT to determine the most appropriate alignment strategy based on the size and similarity of the input sequences.

---
### Alignment Details

MAFFT selected the **L-INS-i algorithm**, an iterative refinement method that incorporates local pairwise alignment information. This approach is generally considered one of the most accurate alignment strategies, particularly for datasets with conserved domains and moderate sequence divergence.
Key parameters and steps included:
- **Substitution matrix**: BLOSUM62
- **Alignment type**: Local pairwise alignment with iterative refinement
- **Guide tree construction**: UPGMA (Unweighted Pair Group Method with Arithmetic Mean)
- **Iterations**: Up to 16 refinement cycles
- **Gap penalty**: Adjusted dynamically (default MAFFT settings)
The alignment process involved:
1) Initial pairwise comparisons
2) Guide tree construction
3) Progressive alignment
4) Iterative refinement until convergence

--- 

### Assumptions and Limitations

**Assumptions**
MAFFT assumes that the input sequences (in this case, COX1 protein sequences) are homologous, meaning they share a common evolutionary origin. It also assumes that sequence similarity reflects evolutionary relationships.
**Limitations**
While MAFFT is highly efficient and accurate, certain limitations should be considered:
- Performance may decrease with **highly divergent sequences**, where alignment becomes ambiguous
- **Gap penalty** schemes may not fully capture biological insertion/deletion events
- Default parameters may introduce additional gaps in **gap-rich regions** (a known change since version 7.110)

---
To mitigate excessive gap insertion, the `--leavegappyregion` option can be used if necessary.
*Notes:*
 -  *Software: MAFFT v7.526*
 -  *Platform: Windows (command-line interface)*
 -  *Input: __p.fasta__*
 -  *Output: __aligned_penguins.fasta__*

## Viewing Alignment

## Parsimony and Distance Trees

## IQ-TREE

## MrBayes
This pipeline takes an aligned protein FASTA file and runs a Bayesian phylogenetic analysis using MrBayes. Follow each step in order in PowerShell.

---

### Requirements

- Windows with PowerShell
- [Python 3](https://www.python.org/downloads/)
- [MrBayes 3.2.7 for Windows](https://github.com/NBISweden/MrBayes/releases)
- A GitHub account with a repository set up
- Your sequences aligned in FASTA format before starting (e.g. using MAFFT or MUSCLE)

---

### Step 1: Install Biopython

Biopython is a Python library used to convert your FASTA file to NEXUS format.

```powershell
pip install biopython
```

You should see `Successfully installed biopython` at the end.

---

### Step 2: Convert FASTA to NEXUS

MrBayes requires NEXUS format. This script also appends the MrBayes settings block to the bottom of the file automatically.

> **Note:** This file is UTF-16 encoded (common when saved from Windows tools), so we specify `encoding='utf-16'`. If your file was saved differently you may need to remove that argument.

> **Note:** This is protein (amino acid) data, so `molecule_type` is set to `protein`. Change to `DNA` if your sequences are nucleotides.

```powershell
python -c "
from Bio import AlignIO, SeqIO
from Bio.Align import MultipleSeqAlignment
import io

# Read the UTF-16 encoded FASTA file
with open(r'C:\Users\name\software\data_test\penguin_data\aligned_penguins.fasta', encoding='utf-16') as f:
    content = f.read()

# Parse sequences and set molecule type to protein
records = list(SeqIO.parse(io.StringIO(content), 'fasta'))
for record in records:
    record.annotations['molecule_type'] = 'protein'

# Write as NEXUS
aln = MultipleSeqAlignment(records)
AlignIO.write(aln, r'C:\Users\name\software\data_test\penguin_data\aligned_penguins.nex', 'nexus')

# Append MrBayes settings block
mrbayes_block = '''
BEGIN MRBAYES;
    set autoclose=yes;
    prset aamodelpr=mixed;
    mcmc ngen=1000000 samplefreq=500 printfreq=1000 nchains=4;
    sump burnin=250;
    sumt burnin=250;
END;
'''

with open(r'C:\Users\name\software\data_test\penguin_data\aligned_penguins.nex', 'a') as f:
    f.write(mrbayes_block)

print('Conversion done!')
"
```

You should see `Conversion done!`

**MrBayes block settings explained**

| Setting | Value | Meaning |
|---|---|---|
| `autoclose` | yes | Closes MrBayes automatically when done |
| `aamodelpr` | mixed | Tries multiple amino acid substitution models and picks the best |
| `ngen` | 1000000 | Number of MCMC generations to run |
| `samplefreq` | 500 | Sample the chain every 500 generations |
| `nchains` | 4 | Run 4 chains in parallel |
| `burnin` | 250 | Discard the first 250 samples before convergence |

---

### Step 3: Verify the NEXUS file

Check that the file contains both the data matrix and the MrBayes block:

```powershell
Get-Content ".\software\data_test\penguin_data\aligned_penguins.nex" | Select-Object -Last 10
```

You should see the `BEGIN MRBAYES;` block at the bottom.

---

### Step 4: Run MrBayes

```powershell
& ".\software\MrBayes\MrBayes-3.2.7-WIN\bin\mb.3.2.7-win64.exe" ".\software\data_test\penguin_data\aligned_penguins.nex"
```

MrBayes will print generation numbers as it runs. When finished you should see:

```
Tasks completed, exiting program because mode is noninteractive
```

**Output files produced**

| File | Description |
|---|---|
| `aligned_penguins.nex.con.tre` | Consensus tree — **main result** |
| `aligned_penguins.nex.lstat` | Log likelihood statistics |
| `aligned_penguins.nex.pstat` | Parameter statistics |
| `aligned_penguins.nex.tstat` | Tree statistics |
| `aligned_penguins.nex.vstat` | Variance statistics |
| `aligned_penguins.nex.run1.t` | Raw sampled trees run 1 (large file) |
| `aligned_penguins.nex.run2.t` | Raw sampled trees run 2 (large file) |

---

### Step 5: Copy results to your project and push to GitHub

Copy the important output files to your git repository folder:

```powershell
Copy-Item ".\software\data_test\penguin_data\aligned_penguins.nex" ".\project\"
Copy-Item ".\software\data_test\penguin_data\aligned_penguins.nex.con.tre" ".\project\"
Copy-Item ".\software\data_test\penguin_data\aligned_penguins.nex.lstat" ".\project\"
Copy-Item ".\software\data_test\penguin_data\aligned_penguins.nex.pstat" ".\project\"
Copy-Item ".\software\data_test\penguin_data\aligned_penguins.nex.tstat" ".\project\"
Copy-Item ".\software\data_test\penguin_data\aligned_penguins.nex.vstat" ".\project\"
```

Then commit and push to GitHub:

```powershell
cd ".\project"
git add .
git commit -m "Add penguin MrBayes phylogenetic analysis results"
git push
```

---

### Notes

- The `.run1.t`, `.run2.t`, `.run1.p`, `.run2.p` files are very large and not essential to commit. You can add them to `.gitignore` if needed.
- Good convergence is indicated by an **average standard deviation of split frequencies below 0.01** — check this in the MrBayes output before trusting your tree.
- The consensus tree file (`.con.tre`) can be visualised in [FigTree](http://tree.bio.ed.ac.uk/software/figtree/) or [iTOL](https://itol.embl.de/).
  
### MrBayes Assumptions and Limitations 

## ASTRAL



