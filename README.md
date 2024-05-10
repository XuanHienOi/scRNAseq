**Introduction**

Pre-mRNA is synthesized from a DNA template in the cell nucleus by transcription.Pre-mRNA includes both exon and intron.
Introns are nucleotide sequences in DNA and RNA that do not directly code for proteins, and are removed during the precursor messenger RNA (pre-mRNA) stage of maturation of mRNA by RNA splicing.
1. **Difference between Bulk RNA-seq and scRNAseq**

**Bulk RNA-seq**:

- Goal: Obtain average gene expression profile from a population of cells
- Protocols: RNA is extracted from a pool of cells. RNA mixture is converted into cDNA. Sequencing is performed on the cDNA.
- QC: Focuses on RNA extraction and library preparation steps, ensuring high-quality input material with minimal degradation or contamination.
- Analyses: Comparisons of gene expression between conditions or time points. Identification of differentially expressed genes. Focuses on population-level changes.
- It's easier to manipulate and analyse but ignore the variation of cells.

**scRNAseq**:

- Goal: Analyze gene expression at the individual cell level to identify cell types and explore cellular heterogeneity.
- Protocols: Individual cells are isolated using techniques such as microfluidics or droplet-based methods. Each cell's RNA is captured and converted into cDNA, followed by separate sequencing for each cell.
- QC: Evaluation of technical aspects like the number of genes detected per cell and the number of unique molecular identifiers (UMIs) captured.
- Analyses: Characterization of cell-specific gene expression profiles. Inference of cell trajectories. Exploration of gene regulatory networks. Captures cellular heterogeneity and dynamics within a population.
- More expensive and more difficult than bulk RNA-seq

2. **Steps of experiments**:

Cell lysis -> RNA isolation -> Reverse transcription -> Reverse transcription amplification -> Sequencing -> Result analysis.

3. **Fundamental steps of scRNA-seq data analysis**

`Step 1`: Read QC (Quality Control): Cells with low quality will not contribute to further analysis, thus need to be filtered out.
3 parameters to consider:
+ The number of counts per barcode (count depth): how many transcripts were sequenced in a cell, a low count may indicate poor sequencing or dead cells, abnormally high count may indicate doublets
+ The number of genes per barcode
+ The number of mitochondrial genes per barcode(mitochondrial genomes are circular and are present in multiple copies per mitochondrion). A high mitochondrial fraction is an indicator of apoptotic cells or cells with broken membranes during sequencing (if cells are broken, cytoplasmic mRNAs get leaked out and only mitochondrial mRNAs are sequenced)
- A rule of thumb exists: to exclude cells with less than 200 genes and more than 5% of mitochondria counts.
- Depending on how downstream analysis performs, users can re-adjust QC parameters 

`Step 2`: Alignment

`Step 3`: Mapping Quality control

`Step 4`: Reads Quantification

5. **Challenge in scRNAseq**
- Bias amplification: Uneven cDNA amplification
- Gene dropout: Gene dropout refers to the situation where a gene is expressed at moderate or weak levels in one cell but is absent in another cell.

6. **General knowledge about PCA algorithm**
PCA (Principle Component Analysis) is is a dimensionality reduction and machine learning method used to simplify a large data set into a smaller set while still maintaining significant patterns and trends.

`Step 1`: Standardization 
This step is to standardize the range of continuous initial variables so that each of them contributes equally to the analysis.
z = (value - mean)/standard deviation 

`Step 2`: Compute the covariance matrix to identify correlations
The aim is to understand how the variables of the input data set are varying from the mean with respect to each other and then we can see any relationship between them.
So, in order to identify these correlations, we compute the covariance matrix.

| Variable | x | y | z |
|---|---|---|---|
| x | Var(x) | Cov(x, y) | Cov(x, z) |
| y | Cov(y, x) | Var(y) | Cov(y, z) |
| z | Cov(z, x) | Cov(z, y) | Var(z) |

If positive then: the two variables increase or decrease together (correlated)
If negative then: one increases when the other decreases (Inversely correlated)

`Step 3`: Compute the eigenvectors and eigenvalues of the covariance matrix to identify the principal components

Eigenvectors and eigenvalues come in pairs, so that every eigenvector has an eigenvalue. 
their number is equal to the number of dimensions of the data. For example, for a 4-dimensional data set, there are 4 variables, therefore there are 4 eigenvectors with 4 corresponding eigenvalues.

By ranking your eigenvectors in order of their eigenvalues, highest to lowest, you get the principal components in order of significance.

Example: If we rank the eigenvalues in descending order, we get λ1>λ2, which means that the eigenvector that corresponds to the first principal component (PC1) is v1 and the one that corresponds to the second principal component (PC2) is v2.

`Step 4`: Create a feature vector to decide which principal components to keep
it’s up to you to choose whether to keep all the components or discard the ones of lesser significance, depending on what you are looking for. 

`Step 5`: Recast the data along the principal components axes

This can be done by multiplying the transpose of the original data set by the transpose of the feature vector.

Code:
- Preprocessing and visualization.ipynb

**Giới thiệu chung**

DNA sau khi phiên mã sẽ tạo ra Pre-mRNA chứa cả exon bao gồm cả exon (những phần RNA mang thông tin mã hoá amino acid để tổng hợp protein) 
và intron (không mang thông tin mã hóa amino acid). Nhờ cơ chế cắt nối RNA, các intron được cắt ra khỏi pre-mRNA, và các exon được ghép lại với nhau để tạo thành mRNA trưởng thành và được dịch mã thành protein.

Các câu hỏi đặt ra:
1. Bulk RNA-seq và scRNAseq khác nhau như thế nào?
Giải trình tự RNA khối (bulk RNA-Seq):
- Là phương pháp phổ biến hơn và dễ thực hiện hơn. 
- Trộn RNA từ tất cả các tế bào trong mẫu thành một mẫu duy nhất để giải trình tự. 
- Kết quả thu được là biểu hiện trung bình của tất cả các tế bào, dễ phân tích nhưng bỏ qua sự đa dạng giữa các tế bào. 
Ví dụ, một số thuốc hoặc tác động bên ngoài chỉ ảnh hưởng đến các loại tế bào cụ thể hoặc tương tác giữa các loại tế bào.

Giải trình tự RNA đơn bào (single-cell RNA-Seq):
- Cho phép nhìn sâu hơn vào hoạt động của từng tế bào riêng lẻ. 
- Quan trọng để nghiên cứu các mối quan hệ giữa các loại tế bào hoặc các tế bào hiếm.
Ví dụ, trong ung thư, các tế bào ung thư kháng thuốc hiếm có thể gây tái phát bệnh, khó phát hiện bằng phương pháp bulk RNA-Seq đơn giản.
Nhược điểm của scRNA-Seq:
Đắt hơn và khó thực hiện hơn.
Phân tích dữ liệu phức tạp hơn do độ phân giải cao, dễ dẫn đến kết luận sai.

2. Các bước cơ bản trong thí nghiệm scRNA-seq
Ly giải tế bào -> Phân lập RNA -> Phiên mã ngược -> Khếch đại phiên mã ngược -> Giải trình tự -> Phân tích kết quả

3. Các bước cơ bản trong phân tích dữ liệu scRNA-seq
Dataset: sử dụng dataset pbmc10k_multiome (các tế bào đơn lẻ từ mẫu Peripheral Blood Mononuclear Cells) trong thư viện 
- Phân tích dữ liệu scRNAseq với “Expression matrix”. Trong Expression matrix, mỗi hàng biểu diễn một gene và mỗi cột biểu diễn một tế bào. Như vậy, mỗi ô biểu diễn mức độ biểu hiện của một gene trong một tế bào. Các bước xây dựng một “Expression matrix” bao gồm:
Bước 1: Read QC (Quality Control): Đọc và kiểm tra chất lượng đoạn giải trình tự. (Công cụ: FastQC hay Karen)
Bước 2: Alignment: Liên kết đoạn giải trình tự với hệ genome tham khảo (reference genome) để tìm gene tương ứng với đoạn giải trình tự. (Công cụ sử dụng: STAR, TopHat)
Bước 3: Mapping Quality control: Kiểm tra độ chính xác của bước liên kết đoạn giải trình tự với reference genome dựa trên các chỉ số như: tỉ lệ giữa ribosome RNA và transposon RNA, tỉ lệ các đoạn nối đặc hiệu, độ sâu (read depth), đoạn đọc trải dài qua các điểm nối (splice junctions).
Bước 4: Reads Quantification: Tính toán mức độ biểu hiện của mỗi gene trong một tế bào. Với phương pháp scRNA-seq sử dụng kỹ thuật “tag-base”, UMI có thể được dùng để tính số lượng tuyệt đối của một phân tử RNA.
- Sau khi có Expression matrix, dùng Scanpy trong Python để phân tích dữ liệu scRNA-seq

4. Thách thức khi dùng phương pháp scRNA-seq
- Sự nhân lên thiếu đồng đều của cDNA (Bias amplification)
- Gene “bị loại bỏ” (Gene dropout) là trường hợp gene biểu hiện ở mức độ trung bình hoặc yếu trong một tế bào, nhưng không xuất hiện trong tế bào khác.

5. Kết quả cuối cùng cần đạt được sau khi phân tích trong bài code này
- Thông tin về sự biểu hiện gen
- Tương quan giữa các tế bào
- Phân cụm tế bào và/hoặc sự biểu hiện gen khác biệt giữa các nhóm tế bào.
Biểu đồ PCA (Phân tích thành phần chính): Biểu đồ này hiển thị sự biến đổi và phân tách của các tế bào dựa trên thành phần chính. Nó giúp bạn hiểu sự tương quan và sự khác biệt giữa các tế bào trong dữ liệu.
Biểu đồ t-SNE (t-Distributed Stochastic Neighbor Embedding): Biểu đồ t-SNE biểu diễn sự tương quan và sự tách biệt giữa các tế bào trong không gian hai chiều. Nó giúp bạn phát hiện các nhóm tế bào tương tự và những tế bào đơn độc đặc biệt.
Phân cụm tế bào: Kết quả phân tích scRNA-Seq thường bao gồm việc phân cụm các tế bào vào các nhóm dựa trên sự tương đồng trong biểu hiện gen. Kết quả này cho phép bạn xác định các loại tế bào khác nhau trong dữ liệu và tìm hiểu về sự phân bố và tương quan giữa chúng.

Code:
- Preprocessing and visualization.ipynb

