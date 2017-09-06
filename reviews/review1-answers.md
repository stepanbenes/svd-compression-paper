# Reviewer 1

* Language should be thoroughly revised! There are lots of mistakes (missing definite/indefinite articles, etc, sometimes the subject is missing in the sentences).

The language was completely revised. Missing articles, commas, wrong word order, and misspellings were corrected.

* The authors should explain more clearly in the introduction what is the main novelty of the paper. It would be nice to include more detailed introduction (state of the art) explaining the different topics included (I see that only 5 citations out of 19 are made in the introduction, which is kind of strange).

Section Introduction was extended with summarization of included topics. Several citations were added. Also, we tried to better explain the main novelty of the paper. Following paragraphs were added to the paper:

```... SVD method [4, 5, 6] was chosen as the foundation of the compression algorithm. Considering that the results from the FEM analyses can be viewed as a series of arbitrary rectangular matrices, the implementation of compression algorithm based on SVD is straightforward as it can be applied to any rectangular matrix.```

```Compression methods usually yield approximated data. In the following text, the term approximation error denotes an error resulted from compression, i.e. difference between original results of FEM analysis and their compressed form. It should not be confused with the error of the Finite Element Method itself that yields approximate solution to mathematical problems used to model physical reality. To quantify the approximation error, several error metrics were investigated. In [7], there are some of them used in similar area of research. The ability to control the quality of compression was a key requirement for the implementation of the compression algorithm.```

```The quality of any compression method depends on the nature of input data. Therefore, several different non-trivial finite element analyses should be used as benchmarks for an implementation of the compression algorithm. Also, the size of input data (the output from the FEM analysis) should be large enough to test the algorithmic complexity under heavy load. Details about the analyses that were used as benchmarks can be found in [8, 9, 10, 11].```

```Performance is very important aspect for development of the compression algorithm. SVD being very computational intensive, randomized algorithms for SVD decomposition were investigated [12, 13, 14, 15]. Especially, the implementation described in [16] was used in the compression algorithm for the optimization. Memory consumption and the suitable format of the output data from the compression algorithm also in uence overall usability of the resulting work. Ecient data storage is connected with data structure [17, 18]. This area will be subject of further research.```

```Although the idea to use SVD for data compression is not new, it is applied in an entirely new area of post-processing of results from the FEM analysis. In contrast with image compression, where the SVD was used in a lossy compression algorithms, the area of post-processing puts great emphasis on the mathematical accuracy of the aproximation instead of the human perceptions of visualizations. Also, the storage and dimensions of the input data, and the variance of the values in data, are very different. The main contribution of this paper is therefore to explore this area and to offer a description of an implementation of the algorithm that is successfully used in the post-processing of large data.```

```The rest of the paper is organized as follows. Section 2 contains mathematical background of the SVD compression (i.e. SVD method itself, its use in low-rank matrix approximation, and description of the randomized SVD method). Implementation of the compression algorithm is presented in Section 3. Section 4 summarizes the results of the benchmarks that were designed to measure quality of the output from the compression algorithm, and also the performance of its the implementation. The paper is concluded in Section 5.```

* There is confusing mistake on line 138, the singular values of S are not eigenvalues of U and V!!!

The mistake was corrected. The line was replaced with the following text:

```...which are at the same time the nonzero square roots of the eigenvalues of AA^T and A^TA.```

* Seems that for equation (2) it is assumed that n < m(!), but it is never mentioned before. (?)

The variable n was misused here to mean something different than in the rest of the paper. In equation (2), n is meant to be the value equal to the lesser of two dimensions of A. n was replaced with k, that is used in the rest of the paper and defined as k = min(m, n).

* line 379 " .... results will nearly span the range of A with extremely high probability" is confusing in my opinion. Seems like it is not dependent on the number r.

Approximation of the matrix A depends definitely on the number r which is the rank of the approximation of A. The higher r, the better approximation. All approximations of A with the rank r are not identical. We are looking for the approximation which is the best one. The quality of approximation can be measured by the probability of incidence of a random vector \tilde{U}\tilde{S}\tilde{V}^Tv in the range of the matrix A, where v denotes a random vector.

* First paragraph of section 3 it is completely unclear and should be reformulated.

The first paragraph of section 3 talks about the structure of the results from FEM, and about the way it is transformed to the input to the SVD compression algorithm. We have checked the paragraph but unfortunately we do not understand the source of unclarity. We will be grateful to the reviewer for more detailed comment to this paragraph.


# Reviewer 2

* Standard SVD is well described but I miss better explanation of randomized SVD

The following paragraphs were added to section about randomized SVD to better introduce the randomized SVD algorithm:

```There are many algorithms with different approaches to compute singular value decompositions. One approach is based on diagonalization of the matrix which essentially yields the whole decomposition at the same time. The other approach is the use of an iterative algorithm that yields one or several singular values at a time and can be stopped after desired number of singular values and vectors has been computed. Although these algorithms have proven to work very well for relatively small matrices, they are not well suited for using with large data sets. The exact SVD of a m x n matrix has computational complexity O(min(mn^2,m^2n)) using the \big-O" notation. When applied on large data sets it tends to be very time-consuming. Also, the implementation on modern hardware environment. As these algorithms often need random access to the memory where the input matrix is stored, it can increase communication between different levels in memory hierarchy, which causes higher latency when accessing data. From a numerical linear algebra perspective, an additional problem resulting from increasing matrix sizes is that noise in the data, and propagation of rounding errors, become increasingly problematic.```

```In [12, 13, 14, 15], there are described randomized methods for constructing approximate matrix factorizations which offer signicant speedups over classical methods. The particular implementation of the randomized decomposition is based on the algorithm described in [16]. The authors proposed an algorithm for effcient computation of low-rank approximation to a given matrix. The method uses random sampling to identify a subspace that captures most of the action of a matrix. The input matrix is compressed to this subspace, and deterministic manipulations are then used to obtain the desired low-rank factorization. For a matrix that is too large to t in fast memory, the randomized techniques require only a constant number of passes over the data, as opposed to O(k) passes for classical algorithms.```

Also, we would like to emphasize the fact, that the main novelty of the paper is not the implementation of the randomized SVD algorithm. We rather use the existing implementations as an optimization to our compression algorithm. For better explanation of this topic we reffer to the literature, especially to [16] that contains very thorough explanation of the randomization approach.

* Figures 5, 7, 8: The error estimation decreasing very fast and therefore the graph is not clear. It would be good to use the logarithmic merit (as in Figures 11 and 12)

The graphs in figures 5, 7, 8, and 10 were redrawn in logarithmic scale. Also, the secondary x-axis was added to inform about r-values.


# Reviewer 3

* Page 8: Line 414: what does it mean "a nonnegative, diagonal matrix"?

The comma was removed. The term denotes a diagonal matrix with positive elements or zeroes on the diagonal.

* Line 589: while NRMSD < e -> while NRMSD > e

This line in algorithm description is correct. Each iteration the value of the variable NRMSD is getting bigger and the loop continues until the threshold e is reached or exceeded. It should be emphasized that Algorithm 1 summarizes a pseudo-code, where the desired approximation is obtained from the full SVD, i.e. the particular singular values with appropriate singular vectors are removed from the factorization and therefore the error NRMSD of the approximation growths.

* "the" is often missing

The language was completely revised. Missing articles, commas, wrong word order, and misspellings were corrected.

* Line 656: These results confirm - where have been the results computed?

Description of the benchmark was added:

```The rst benchmark was designed to measure computational complexity of the SVD algorithm. Series of 100 random matrices with standard distribution were generated and execution times were recorded and averaged. The execution time with respect to the number of the stored incremental or time steps (the number of rows in the matrix A) is depicted in Figure 2 while the execution time with respect to the number of points, where the results are stored (the number of columns of the matrix A), is depicted in Figure 3. ...```

Also, the hardware specification of the testing machine was appended. Following text was added:

```All procedures presented here were tested on a common PC having Intel Core i5-4690K @ 3.5GHz CPU with 16GB RAM, running on Microsoft Windows 10 64-bit operating system. The first benchmark was designed to measure computational complexity of the SVD decomposition algorithm. Series of 100 random matrices with standard distribution were generated and execution times were recorded and averaged.```

* ... Furthermore I would recommend to redraw the graph in logarithmic scale and add the second x-axis into the graph informing about r-values.

The graphs in figures 5, 7, 8, and 10 were redrawn in logarithmic scale. Also, the secondary x-axis was added to inform about r-values.

* Page 17: I would recommend to merge Figure 7 and 8 into one figure, to use logarithmic scale again and to add second x-axis with r-values.

Unfortunatelly, it is not possible to do the both - merge the figures and add secondary x-axes as each figure has different range of r-values. The figures were therefore kept separated and secondary x-axes were added.

* Page 16: Line 849: this has negligible effect on quality of compression. – what is an impact of this negligible effect? It could be explained in more details.

The effect on quality of compression was quantified in the text:

```this has negligible effect (NRMSD and NME are bellow 1% even for very small r -- 3 out of 22) on quality of compression```

* Page 17: Normalized Maximum Error (NME) is not defined

Definition of Normalized Maximum Error (NME) was added to Section 2.3 (Error estimation)

* Line 1035: Figure 11 summarizes the compression error for all three benchmarks using logarithmic scale – it is confusing as PSNR is defined using logarithm and the reader could expect logarithmic scale on y-axis, explain in more details

The description of Figure 11 was clarified. The sentence was replaced with the following text:

```Figure 11 summarizes the compression error for all three benchmarks using PSNR metric. PSNR is defined using logarithm (see Equation (9) for denition), and is included here mainly as a comparison to other image-related compression methods whose quality is often expressed by PSNR.```

* Figure 11: Figure 12: Comparison of Peek signal to noise ratio value for different randomized decompositions. -> Dependence of PSNR value on c and r calculated for different randomized  SVD decompositions. The graph contains the legend with names of benchmarks, which are not described. Add the second x-axis into the graph informing about r-values.

Names in the legend of Figure 11 and Figure 12 were updated to match the names of benchmarks. However, the secondary x-axes informing about r-values cannot be added, because each series in the graphs has different range of r-values.

* Figure 13: Is it necessary to report 17 times for full SVD decomposition? (blue bars)

We include the columns indicating the full SVD fo Figure 13 in order to show the advantage of randomized SVD and to show the nearly constant computation time for the full SVD.

* Figure 13: Now, there is rank as x-axis, in previous graphs there was c. I would recommend two x-axes: r and c. What should be r=0?

The secondary axis with compression ratio (c) was added to Figure 13. The column for r=0 was removed.


All your other comments were acknowledged and the paper was modified as suggested.


# Reviewer 4

* References to compression ratio are confusing. It is defined as (SVD size) / (original size).

The text in Section Results

```Compression ratio c is an input parameter to the compression algorithm specifying amount of singular values ought to be removed...```

was replaced with the following text:

```In this benchmark, the compression ratio c is an input parameter to the compression algorithm. As follows from the equation (13), the value of compression ratio directly affects the amount of singular values ought to be removed from the SVD decomposition.```

* Line 122: Suggest: summary of sections at the end of Introduction

Following text was added to the end of Introduction:

```The rest of the paper is organized as follows. Section 2 contains math ematical background of the SVD compression (i.e. SVD method itself, its use in low-rank matrix approximation, and description of the randomized SVD method). Implementation of the compression algorithm is presented in Section 3. Section 4 summarizes the results of the benchmarks that were designed to measure quality of the output from the compression algorithm, and also the performance of its the implementation. The paper is concluded in Section 5.```

* Line 154: starting this line, the text does not fit into "Mathematical background", I suggest a new section, e.g. SVD compression.

New subsection "SVD compression" was added to Section 2.


All your other comments were acknowledged and the paper was modified as suggested. The language was completely revised. Missing articles, commas, wrong word order, and misspellings were corrected.
