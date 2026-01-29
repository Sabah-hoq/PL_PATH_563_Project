# PL_PATH_563_Project
--- 
#### Quiz one ~ shell Basics -> Notes 
**Video notes** 
Types of commands in the terminal
* PS C:\Users\sabah\PL_PATH_563_Project> PWD 
  * Means "print working directory"
    ```shell
    PS C:\Users\sabah\PL_PATH_563_Project> pwd
    Path
    ----
    C:\Users\sabah\PL_PATH_563_Project
    ```
* PS C:\Users\sabah\PL_PATH_563_Project> ls 
  - Means "list" to list content in wd 
  - ```shell
    PS C:\Users\sabah\PL_PATH_563_Project> ls

    Directory: C:\Users\sabah\PL_PATH_563_Project
  	Mode                 LastWriteTime         Length Name
    ----                 -------------         ------ ----
    -a----         1/20/2026   5:31 PM             21 README.md
    ```
* PS C:\Users\sabah\PL_PATH_563_Project> clear
  * Cleans up the terminal and makes it look fresh while keeping your wd
* PS C:\Users\sabah\PL_PATH_563_Project> mkdir name
  * makes a directory with the given name after
* PS C:\Users\sabah\PL_PATH_563_Project> rmdir name
  * Removes the directory with the given name
* New-Item: allows you to make a new file 
* rm file_name: Removes file
* **nano: not working atm will fix**
--- 
## In Class Notes 1/29/2026
**Alignment 1**
ACAT -> AGAT
These allignments can reflect the evolutionary process 
AC - AT
A - GAT
A - CAT
AG-AT

Q) 9.1 
So we have ACATTA -> TACA (which evolves) 
With the deletion of AC -> ATTA
Deletion of 2nd T -> ATA
Substitution of T -> C -> ACA
Insertion of T in the front -> TACA
In the end, we have **'TACA'** as the final form of these nucleotides 

**Answer in class**: '*_ACATTA
                 T_ _AC_A*'
Q) 9.2 
This would follow the TACA from above 

### MSA Algorithm
Input: Unaligned sequences (diff length) 
Output: Aligned sequences (same length) where each site is an assertion of homology 
**Steps in MSA**
 1) Define the cost of each event: deletion, insertion, and substitution 
 2) Learn and obtain the optimal pairwise alignment with the minimum cost ("edit distance")
 3) Learn to obtain the optimal multiple sequence alignment: we need to be able to align alignments 
**Cost of evolutionary events?**
- When you add or delete a nucleotide, you want to add a gap of 1 "_" <- detonated as this
   - You want to minimize the cost of changes that occur
In class actvity
Q) 9.3 -> How would you change the alignment between sequences S = AACT and S'= CTGG if the costs were:
1) Cost of gap: 4
2) Cost of substitution 1
   AACT
   CTGG
   this would become w/ a cost of 4: _ _ AACT
                                           CT _ _ 
**Pairwise sequence Alignment**
Needleman-Wunsch Algorithm
* Ingredients
  *  Two sequences: $$A = a_1 a_2 ... a_m$$ and $$B = b_1 b_2 ... b_n$$
  *  1) cost of gap and 2) cost of substitution
We will define a matrix _**F**_ whose entries are **F(i,j)** will represent the minimum cost to align sub-sequences $$A_i$$ and $$B_j$$ based on the costs
Matrix Ex:

$$
\begin{bmatrix}
     b_1 & b_2 & b_n \\
a_1 & - & - & - \\
a_2 & - & - & - \\
a_m & - & - & -
\end{bmatrix}
$$
