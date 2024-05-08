**Giới thiệu chung **

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