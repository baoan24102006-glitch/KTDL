# BÁO CÁO KHAI THÁC DỮ LIỆU
## Đề tài: Book Recommender System

### 1. Giới thiệu

Trong đề tài Book Recommender System, khai thác dữ liệu là bước đầu tiên nhằm tìm hiểu, kiểm tra và chuẩn bị dữ liệu phục vụ xây dựng hệ thống gợi ý sách. Phần công việc này sử dụng bộ dữ liệu Book-Crossing, bao gồm thông tin sách, thông tin người dùng và các lượt đánh giá sách.

Mục tiêu là kiểm tra chất lượng dữ liệu, xử lý các vấn đề về dữ liệu thiếu và trùng lặp, phân tích đặc điểm của tập dữ liệu, trực quan hóa kết quả và xuất dữ liệu đã xử lý để bàn giao cho các thành viên xây dựng mô hình và ứng dụng.

### 2. Mục tiêu thực hiện

- Tìm hiểu cấu trúc của bộ dữ liệu Book-Crossing.
- Kiểm tra số lượng bản ghi, số cột, dữ liệu thiếu và dữ liệu trùng lặp.
- Phân tích phân bố điểm đánh giá sách.
- Làm sạch và chuẩn hóa dữ liệu.
- Xây dựng tối thiểu ba biểu đồ phân tích dữ liệu.
- Xuất các tệp dữ liệu đã xử lý phục vụ những giai đoạn tiếp theo.

### 3. Công cụ sử dụng

- Python: ngôn ngữ lập trình dùng để xử lý và phân tích dữ liệu.
- Pandas: đọc CSV, kiểm tra, làm sạch và tổng hợp dữ liệu.
- NumPy: hỗ trợ xử lý dữ liệu số và giá trị không hợp lệ.
- Matplotlib: trực quan hóa dữ liệu bằng biểu đồ.
- Jupyter Notebook trong VS Code: thực hiện các bước phân tích và lưu lại kết quả.

### 4. Mô tả dữ liệu ban đầu

Bộ dữ liệu được sử dụng gồm ba bảng chính:

| Tên bảng | Số dòng ban đầu | Số cột | Nội dung |
|---|---:|---:|---|
| Books | 271.379 | 8 | Thông tin sách, tác giả, năm xuất bản, nhà xuất bản và đường dẫn ảnh |
| Ratings | 1.149.780 | 3 | Mã người dùng, ISBN và điểm đánh giá sách |
| Users | 278.858 | 3 | Mã người dùng, địa điểm và tuổi |

Bảng Books chứa thông tin mô tả sách. Bảng Ratings ghi nhận các lượt đánh giá của người dùng. Bảng Users cung cấp thông tin bổ sung về người dùng.

### 5. Kiểm tra chất lượng dữ liệu

#### 5.1. Dữ liệu thiếu

Kết quả kiểm tra ban đầu cho thấy bảng Books có một số giá trị thiếu ở trường tác giả và nhà xuất bản. Bảng Users có nhiều giá trị tuổi chưa được cung cấp. Đối với năm xuất bản, một số giá trị cần được chuyển đổi và kiểm tra tính hợp lệ.

Các giá trị thiếu được xử lý tùy theo ý nghĩa của từng cột. Thông tin tác giả và nhà xuất bản bị thiếu được thay bằng nhãn “Unknown”. Năm xuất bản không hợp lệ được chuyển thành giá trị thiếu thay vì tự suy đoán.

#### 5.2. Dữ liệu trùng lặp

Kết quả kiểm tra ban đầu không phát hiện dòng trùng lặp hoàn toàn trong ba bảng. Tuy nhiên, trong quá trình làm sạch, dữ liệu sách còn được kiểm tra theo ISBN và dữ liệu đánh giá được kiểm tra theo cặp User-ID và ISBN để hạn chế các bản ghi lặp theo tiêu chí đã chọn.

#### 5.3. Phân tích điểm đánh giá

Điểm đánh giá trong bộ Book-Crossing nằm trong khoảng từ 0 đến 10. Giá trị 0 thường biểu thị lượt đánh giá không có điểm số thực, do đó cần phân biệt với điểm đánh giá từ 1 đến 10.

Trong bước chuẩn bị dữ liệu cho mô hình, các lượt đánh giá có điểm từ 1 đến 10 được giữ lại. Các lượt có điểm bằng 0 không được sử dụng như đánh giá có điểm số thực.

### 6. Quy trình làm sạch dữ liệu

Quá trình làm sạch được thực hiện theo các bước:

1. Đọc các tệp CSV và kiểm tra cấu trúc dữ liệu.
2. Chuẩn hóa khoảng trắng ở các trường văn bản.
3. Loại bỏ bản ghi sách không có ISBN hoặc tên sách.
4. Điền nhãn “Unknown” cho tác giả và nhà xuất bản bị thiếu.
5. Chuyển đổi năm xuất bản sang kiểu dữ liệu số và đánh dấu các năm không hợp lệ là giá trị thiếu.
6. Giữ một bản ghi cho mỗi ISBN trong bảng Books.
7. Loại bỏ các lượt đánh giá thiếu thông tin cần thiết hoặc có điểm không thuộc khoảng 1–10.
8. Loại bỏ bản ghi đánh giá trùng theo cặp User-ID và ISBN.
9. Chỉ giữ các lượt đánh giá có ISBN tồn tại trong bảng sách.

Các bước trên giúp dữ liệu nhất quán hơn trước khi chuyển sang giai đoạn xây dựng mô hình gợi ý.

### 7. Kết quả sau làm sạch

Theo kết quả thực nghiệm trong Notebook:

| Bảng dữ liệu | Số dòng ban đầu | Số dòng sau làm sạch |
|---|---:|---:|
| Books | 271.379 | 271.378 |
| Ratings | 1.149.780 | 383.853 |

Số lượt đánh giá giảm đáng kể do các lượt có điểm bằng 0, các bản ghi trùng theo cặp người dùng–sách và các ISBN không có trong bảng Books được xử lý theo quy trình đã mô tả.

Sau làm sạch, bảng Books vẫn còn một số giá trị năm xuất bản bị thiếu. Các giá trị này được giữ ở dạng thiếu nhằm tránh đưa thông tin không có căn cứ vào dữ liệu.

### 8. Trực quan hóa và nhận xét

#### 8.1. Biểu đồ phân bố điểm đánh giá sách

Biểu đồ thể hiện số lượt đánh giá tương ứng với từng mức điểm từ 1 đến 10 sau khi loại bỏ các lượt đánh giá có điểm bằng 0. Qua biểu đồ, có thể thấy điểm 8 có số lượt đánh giá cao nhất, tiếp theo là điểm 10 và điểm 7. Trong khi đó, các mức điểm thấp từ 1 đến 3 có số lượt đánh giá tương đối ít.

Kết quả cho thấy các mức điểm cao xuất hiện nhiều hơn các mức điểm thấp trong tập dữ liệu đã xử lý. Đây là thông tin hữu ích để tìm hiểu hành vi đánh giá của người dùng và chuẩn bị dữ liệu cho hệ thống gợi ý sách. Tuy nhiên, kết quả chỉ phản ánh tập dữ liệu đang phân tích, không đại diện cho toàn bộ độc giả.

#### 8.2. Biểu đồ Top 10 sách có nhiều lượt đánh giá nhất

Biểu đồ thể hiện 10 cuốn sách có số lượt đánh giá cao nhất trong tập dữ liệu sau làm sạch. Trong đó, *The Lovely Bones: A Novel* đứng đầu, tiếp theo là *Wild Animus* và *The Da Vinci Code*. Các cuốn sách còn lại trong danh sách cũng nhận được nhiều lượt đánh giá từ người dùng.

Kết quả cho thấy mức độ tương tác giữa người dùng và các đầu sách có sự khác biệt. Một số cuốn sách thu hút nhiều lượt đánh giá hơn những cuốn khác, do đó có thể được sử dụng để xây dựng phương pháp gợi ý dựa trên mức độ phổ biến. Tuy nhiên, số lượt đánh giá nhiều không đồng nghĩa với điểm đánh giá trung bình cao hoặc chất lượng sách tốt hơn.

#### 8.3. Biểu đồ phân bố năm xuất bản sách

Biểu đồ thể hiện số lượng sách theo năm xuất bản sau khi các giá trị năm không hợp lệ được chuyển thành giá trị thiếu. Có thể quan sát thấy số lượng sách tập trung chủ yếu ở giai đoạn cuối thập niên 1990 và đầu những năm 2000. Trong khi đó, số lượng sách ở các năm xa hơn thấp hơn đáng kể.

Kết quả giúp nhận biết đặc điểm thời gian của tập dữ liệu. Tuy nhiên, cần lưu ý rằng dữ liệu năm xuất bản bị thiếu hoặc không hợp lệ không được thể hiện trong biểu đồ. Vì vậy, biểu đồ phản ánh phân bố của các bản ghi có năm xuất bản hợp lệ, không nhất thiết phản ánh đầy đủ lịch sử xuất bản của tất cả sách trong bộ dữ liệu.

#### 8.4. Tổng kết kết quả trực quan hóa

Ba biểu đồ cung cấp cái nhìn tổng quan về đặc điểm của dữ liệu Book-Crossing sau khi làm sạch. Biểu đồ thứ nhất cho thấy phân bố điểm đánh giá; biểu đồ thứ hai xác định những cuốn sách có nhiều lượt đánh giá; biểu đồ thứ ba mô tả phân bố năm xuất bản. Các kết quả này hỗ trợ quá trình tìm hiểu dữ liệu và là cơ sở cho các bước xây dựng, kiểm thử hệ thống gợi ý sách tiếp theo.

### 9. Sản phẩm bàn giao

Các sản phẩm của phần Data Mining gồm:

- `data/books.csv`: thông tin sách đã làm sạch.
- `data/ratings.csv`: dữ liệu đánh giá đã làm sạch.
- `01_data_analysis.ipynb`: Notebook ghi lại các bước đọc, kiểm tra, làm sạch và phân tích dữ liệu.
- Ba biểu đồ phân tích và phần nhận xét tương ứng.

Hai tệp CSV sau xử lý được bàn giao cho thành viên Machine Learning để xây dựng các thuật toán gợi ý, đồng thời cung cấp dữ liệu cho thành viên Demo và tích hợp ứng dụng.

### 10. Kết luận

Qua quá trình khai thác dữ liệu, bộ Book-Crossing đã được kiểm tra, làm sạch và phân tích bằng Python. Các vấn đề về dữ liệu thiếu, năm xuất bản không hợp lệ, điểm đánh giá bằng 0 và bản ghi lặp theo tiêu chí đã chọn được xem xét trong quá trình tiền xử lý.

Các biểu đồ cung cấp cái nhìn tổng quan về phân bố điểm đánh giá, những sách có nhiều lượt đánh giá và phân bố năm xuất bản. Dữ liệu sau xử lý là cơ sở để nhóm tiếp tục xây dựng, kiểm thử và đánh giá hệ thống Book Recommender System.