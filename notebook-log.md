Data was taken from this article. 
===

Fasta file was found via [ncbi]([url](https://trace.ncbi.nlm.nih.gov/Traces/?view=run_browser&acc=ERR11777491&display=download)) 

These are the lines of code will be edited tmmr cause me tired



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

The default gap scoring scheme has been changed in version 7.110 (2013 Oct).
It tends to insert more gaps into gap-rich regions than previous versions.
To disable this change, add the --leavegappyregion option.

PS C:\Users\sabah\mafft-7.526-win64-signed\mafft-win>
