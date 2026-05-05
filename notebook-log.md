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

## ASTRAL



