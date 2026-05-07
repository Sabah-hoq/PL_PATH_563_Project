Reproducible Script for Phylogenetics Analysis 
===
## Getting Data 
### Data was taken from the NCBI
Link to all data can be found here: NCBI 2024a-e
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

## Parsimony and Distance Trees
### Install & load packages

```r
packages <- c("ape", "phangorn")

for (p in packages) {
  if (!require(p, character.only = TRUE)) {
    install.packages(p, dependencies = TRUE)
    library(p, character.only = TRUE)
  }
}
```

### Load alignment (FASTA protein)
```r
alignment_file <- "aligned_penguins_fixed.fasta"

aa <- read.phyDat(alignment_file, format = "fasta", type = "AA")
```
---
### Distance tree (Neighbor-Joining)
```r
dist_matrix <- dist.ml(aa)
tree_nj <- nj(dist_matrix)

```

### Parsimony tree
```r
tree_pars <- optim.parsimony(tree_nj, aa)
```
### Plot trees

```r
# Plot Distance tree with fixed margins
par(mar = c(2, 2, 2, 8))  
nj <- plot(tree_nj, main = "Distance-Based (NJ) Tree", cex = 0.8)
# Plot Parsimony tree with fixed margins
par(mar = c(2, 2, 2, 8)) 
pars <- plot(tree_pars, main = "Maximum Parsimony Tree", cex = 0.8)
```
### Rerooting the Trees
```r
par(mar = c(2, 2, 2, 8))
plot(tree_pars_rooted, main = "Maximum Parsimony Tree", cex = 0.8)

par(mar = c(2, 2, 2, 8))
plot(tree_nj_rooted, type = "phylogram", main = "NJ Distance Tree", cex = 0.8)
add.scale.bar()
```
--- 
### Viewing Alignment
Trees were visualized using SeaView
[SeaView](https://doua.prabi.fr/software/seaview)

- Fasta file uploaded
- Used the Distance and Parsimony method to view trees
- Rerooted at *Eudyptula minor* as it is outgroup

--- 
## IQ-TREE
**Software:** IQ-TREE v3.0.1  
**Reference:** Nguyen et al. (2015); Minh et al. (2020)  
**Input:** Aligned FASTA file from Step 1  
**Output:** Maximum likelihood tree with bootstrap support values  
[IQ-TREE Windows Download](https://iqtree.github.io/)

IQ-TREE uses ModelFinder Plus (`-m MFP`) to automatically select the best-fit substitution model, then infers the maximum likelihood tree with 1000 ultrafast bootstrap replicates.
 
```powershell
# Set base directory variable
$base = "C:\Users\sabah\software"
 
# Create output directory for IQ-TREE results
mkdir "$base\data_test\iqtree_results" -Force
 
# Run IQ-TREE with ModelFinder and ultrafast bootstrap
# -s : input alignment file
# -m MFP : ModelFinder Plus - automatically selects best substitution model
# -bb 1000 : 1000 ultrafast bootstrap replicates for branch support
# -pre : prefix for all output files
& "$base\iqtree\iqtree-3.0.1-Windows\bin\iqtree3" `
    -s "$base\data_test\penguin_data\aligned_penguins.fasta" `
    -m MFP `
    -bb 1000 `
    -pre "$base\data_test\iqtree_results\penguins"
```
 
**Output files:**
| File | Description |
|------|-------------|
| `penguins.treefile` | Best maximum likelihood tree (use this in ASTRAL) |
| `penguins.log` | Full log of the run including selected model |
| `penguins.iqtree` | Full report including model parameters and tree |
| `penguins.contree` | Consensus tree with bootstrap values |
 
---
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
**Software:** ASTRAL 5.7.8  
**Reference:** Mirarab (2019)  
**Input:** Gene tree(s) in Newick format — use the IQ-TREE `.treefile` from Step 2  
**Output:** Species tree with local posterior probability (LPP) support values  
[ASTRAL for Windows](https://github.com/smirarab/ASTRAL)
 
ASTRAL estimates a species tree under the multispecies coalescent model by finding the tree that maximizes the number of shared quartet topologies with the input gene trees.
 
> **Note:** ASTRAL requires Java to be installed. Verify with `java -version` before running.
 
```powershell
# Set base directory variable
$base = "C:\Users\sabah\software"
 
# Verify Java is installed
java -version
 
# Run ASTRAL
# -i: input gene tree file (IQ-TREE .treefile from Step 2)
# -o: output species tree file
# 2: redirect log output to a text file (ASTRAL logs to stderr)
java -jar "$base\ASTRAL\Astral\astral.5.7.8.jar" `
    -i "$base\data_test\iqtree_results\penguins.treefile" `
    -o "$base\ASTRAL\penguins_species_tree.tre" `
    2> "$base\ASTRAL\astral_log.txt"
```
 
**Output files:**
| File | Description |
|------|-------------|
| `penguins_species_tree.tre` | Species tree with LPP support values |
| `astral_log.txt` | Full log including quartet scores |
 
**Check ASTRAL log for:**
- Normalized quartet score (higher = more gene tree concordance)
- Local posterior probability values at each node
---
## Step 5: Tree Visualization with iTOL
 
All trees were visualized using the Interactive Tree of Life (iTOL) web tool (https://itol.embl.de/).
 
**To upload a tree:**
1. Go to https://itol.embl.de/
2. Click **Upload** in the top menu
3. Upload your tree file:
   - IQ-TREE: `penguins.treefile`
   - MrBayes: `aligned_penguins.nex.con.tre`
   - ASTRAL: `penguins_species_tree.tre`
   - Parsimony: `aligned_penguins_clean-Protpars.`
   - Distance: `aligned_penguins_clean-BioNJ_tree.`
5. Customize visualization as needed
6. Export as PDF with **white background** for publication quality
   - Use 600 dpi for clear images
**iTOL settings used:**
- Display mode: Normal (rectangular)
- Bootstrap/support values: displayed at nodes
- Branch lengths: displayed
- Background: white
---
 
## Summary of Output Files
 
| Analysis | Key Output File | Support Measure |
|----------|----------------|-----------------|
| MAFFT | `aligned_penguins.fasta` | N/A |
| IQ-TREE | `penguins.treefile` | Bootstrap (68-96) |
| MrBayes | `aligned_penguins.nex.con.tre` | Posterior probability (0.98-1.00) |
| ASTRAL | `penguins_species_tree.tre` | Local posterior probability (0.67) |
 
---

## References
 
Huelsenbeck, J. P., & Ronquist, F. (2001). MRBAYES: Bayesian inference of phylogenetic trees. *Bioinformatics*, 17(8), 754–755.
 
Katoh, K., Misawa, K., Kuma, K., & Miyata, T. (2002). MAFFT: a novel method for rapid multiple sequence alignment based on fast Fourier transform. *Nucleic Acids Research*, 30(14), 3059–3066.
 
Minh, B. Q., Schmidt, H. A., Chernomor, O., Schrempf, D., Woodhams, M. D., von Haeseler, A., & Lanfear, R. (2020). IQ-TREE 2: New models and efficient methods for phylogenetic inference in the genomic era. *Molecular Biology and Evolution*, 37(5), 1530–1534.
 
Mirarab, S. (2019). Species tree estimation using ASTRAL: Practical considerations. *arXiv:1904.03826*.
 
National Center for Biotechnology Information (NCBI). (2024a). *Cytochrome c oxidase subunit I (COX1), Aptenodytes forsteri* [Gene ID: 26045193]. NCBI Gene. https://www.ncbi.nlm.nih.gov/datasets/gene/26045193
 
National Center for Biotechnology Information (NCBI). (2024b). *Cytochrome c oxidase subunit I (COX1), Aptenodytes patagonicus* [Gene ID: 26737703]. NCBI Gene. https://www.ncbi.nlm.nih.gov/datasets/gene/26737703
 
National Center for Biotechnology Information (NCBI). (2024c). *Cytochrome c oxidase subunit I (COX1), Eudyptula minor* [Gene ID: 806254]. NCBI Gene. https://www.ncbi.nlm.nih.gov/datasets/gene/806254
 
National Center for Biotechnology Information (NCBI). (2024d). *Cytochrome c oxidase subunit I (COX1), Pygoscelis adeliae* [Gene ID: 36935527]. NCBI Gene. https://www.ncbi.nlm.nih.gov/datasets/gene/36935527
 
National Center for Biotechnology Information (NCBI). (2024e). *Cytochrome c oxidase subunit I (COX1), Pygoscelis papua* [Gene ID: 42904018]. NCBI Gene. https://www.ncbi.nlm.nih.gov/datasets/gene/42904018
 
Nguyen, L. T., Schmidt, H. A., von Haeseler, A., & Minh, B. Q. (2015). IQ-TREE: A fast and effective stochastic algorithm for estimating maximum-likelihood phylogenies. *Molecular Biology and Evolution*, 32(1), 268–274.
 
Ronquist, F., & Huelsenbeck, J. P. (2003). MrBayes 3: Bayesian phylogenetic inference under mixed models. *Bioinformatics*, 19(12), 1572–1574.
 
