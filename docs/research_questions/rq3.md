# RQ3. Các phương pháp Index hoạt động như thế nào và khi nào nên sử dụng?

## 1. Mục tiêu

Trình bày cơ chế và phạm vi sử dụng của các phương pháp Index có liên quan đến MySQL và MongoDB, sau đó chọn các phương án phù hợp để đưa vào thí nghiệm quản lý bán hàng.

## 2. Nhóm phương pháp chính

### B-tree và compound B-tree
Phù hợp với equality, range và một số truy vấn sort. Compound index phục vụ nhiều điều kiện khi thứ tự trường phù hợp với truy vấn. Cần đánh giá leftmost prefix và thứ tự equality/sort/range.

### Unique index
Dùng khi nghiệp vụ yêu cầu không trùng lặp, ví dụ mã đơn hàng hoặc email khách hàng nếu quy tắc nghiệp vụ quy định duy nhất. Ngoài tác động truy vấn, nó còn thực thi ràng buộc dữ liệu.

### Covering index
Có thể giảm bước truy cập bản ghi gốc nếu Index chứa các trường cần thiết. Phải đánh giá kích thước Index và chi phí cập nhật.

### Full-text/text index
Dành cho tìm kiếm văn bản theo cơ chế full-text/text của từng hệ quản trị, không phải phương án thay thế chung cho B-tree. Chỉ benchmark nếu đề tài có truy vấn tìm kiếm mô tả sản phẩm hoặc nội dung văn bản.

### Multikey index (MongoDB)
Dùng cho trường mảng trong document. Cần xem xét số lượng key được tạo, giới hạn của compound multikey và đặc điểm truy vấn mảng.

### Hashed index (MongoDB)
Phù hợp với một số truy vấn equality và trường hợp thiết kế sharding. Không phù hợp để thay thế B-tree cho range hoặc sort theo thứ tự giá trị gốc.

### Partial index (MongoDB)
Chỉ lập chỉ mục các document thỏa bộ lọc, có thể giảm kích thước khi workload thường truy vấn một tập con. Query phải tương thích với filter để optimizer sử dụng.

### TTL index (MongoDB)
Phục vụ tự động xóa document hết hạn. Đây là cơ chế vòng đời dữ liệu, không phải Index tổng quát để tăng tốc mọi truy vấn bán hàng.

### Spatial index
Phục vụ truy vấn không gian. Chỉ cần đưa vào nghiên cứu nếu hệ thống có nghiệp vụ vị trí/giao hàng và dataset hỗ trợ.

## 3. Phạm vi nên ưu tiên cho benchmark bán hàng

Ưu tiên các trường hợp có thể ánh xạ nghiệp vụ giữa hai DB:
1. Tra cứu theo khóa định danh.
2. Lọc theo một trường có chọn lọc.
3. Lọc theo nhiều trường.
4. Lọc và sắp xếp theo thời gian.
5. Truy vấn có thể tận dụng covering index.
6. Đo workload ghi khi có và không có Index.

Các loại đặc thù như multikey, TTL, text, geospatial cần được trình bày như khác biệt tính năng hoặc thử nghiệm riêng nếu nhóm có đủ thời gian và dữ liệu tương ứng. Không ép tạo phép so sánh tương đương khi semantics khác nhau.

## 4. Ma trận lựa chọn sơ bộ

| Workload | Hướng Index cần thử | Câu hỏi cần kiểm chứng |
|---|---|---|
| Tra cứu mã đơn duy nhất | PRIMARY/UNIQUE; unique index | Latency và đảm bảo uniqueness |
| Đơn theo khách hàng | B-tree/compound | Tác động của customer_id và order_date |
| Lọc trạng thái + thời gian | Composite/compound | Thứ tự trường nào phù hợp |
| Lấy một số cột từ đơn | Covering/compound phù hợp | Có giảm đọc bản ghi gốc không |
| Ghi đơn hàng liên tục | So sánh ít/nhiều Index | Write overhead và dung lượng |

## 5. Kết luận RQ3 cần có sau thực nghiệm

Với từng phương pháp được thử, nhóm cần nêu: cấu trúc, workload phù hợp, execution plan quan sát được, lợi ích đo được, chi phí ghi/dung lượng và giới hạn. Không kết luận “Index X tốt nhất” nếu chỉ thử một kiểu truy vấn hoặc một cấu hình.
