Data was taken from the NCBI
===
Link to all data can be found here: ...

## Getting Data 
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
## MAFFT

```shell
# This renames the file correctly
Rename-Item p.fasta.txt p.fasta
```

```shell
.\mafft.bat --auto p.fasta > aligned_penguins.fasta
``` 

Active code page: 65001♪◙ ♪◙Preparing environment to run MAFFT on Windows. ♪◙This may take a while, if real-time scanni◙
It may take a while before the calculation starts
if being scanned by anti-virus software.
Also consider using a faster version for Windows 10:
https://mafft.cbrc.jp/alignment/software/wsl.html
outputhat23=16
treein = 0
compacttree = 0
rescale = 1
All-to-all alignment.
tbfast-pair (aa) Version 7.526
alg=L, model=BLOSUM62, 2.00, -0.10, +0.10, noshift, amax=0.0
0 thread(s)

outputhat23=16
Loading 'hat3.seed' ...
done.
Writing hat3 for iterative refinement
rescale = 1
Gap Penalty = -1.53, +0.00, +0.00
tbutree = 1, compacttree = 0
Constructing a UPGMA tree ...
    0 / 5
done.

Progressive alignment ...
STEP     4 /4
done.
C:\Users\sabah\mafft-7.526-win64-signed\mafft-win\usr\lib\mafft\tbfast.exe (aa) Version 7.526
alg=A, model=BLOSUM62, 1.53, -0.00, -0.00, noshift, amax=0.0
1 thread(s)

minimumweight = 0.000010
autosubalignment = 0.000000
nthread = 0
randomseed = 0
blosum 62 / kimura 200
poffset = 0
niter = 16
sueff_global = 0.100000
nadd = 16
Loading 'hat3' ... done.
rescale = 1

    0 / 5
Segment   1/  1    1- 517
STEP 002-003-1  identical.
Converged.

done
C:\Users\sabah\mafft-7.526-win64-signed\mafft-win\usr\lib\mafft\dvtditr.exe (aa) Version 7.526
alg=A, model=BLOSUM62, 1.53, -0.00, -0.00, noshift, amax=0.0
0 thread(s)


Strategy:
 L-INS-i (Probably most accurate, very slow)
 Iterative refinement method (<16) with LOCAL pairwise alignment information

If unsure which option to use, try 'mafft --auto input > output'.
For more information, see 'mafft --help', 'mafft --man' and the mafft page.


### MAFFT Assumptions and Limitations yuh
Description: MAFFT uses Fast Fourier Transform (FFT) to rapidly find homologous segments, making it significantly faster than traditional methods while maintaining high accuracy for protein sequences.

Assumptions: The method assumes that the input sequences (COX1 proteins) are orthologous (descended from a common ancestor) and share global homology.

Limitations: It may struggle with highly divergent sequences where gap penalties (scoring how much a "dash" costs) don't perfectly match biological reality.

The default gap scoring scheme has been changed in version 7.110 (2013 Oct).
It tends to insert more gaps into gap-rich regions than previous versions.
To disable this change, add the --leavegappyregion option.


PS C:\Users\sabah\mafft-7.526-win64-signed\mafft-win>

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

## ASTRAL



