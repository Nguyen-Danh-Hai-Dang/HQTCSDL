# Nguyên tắc thiết kế Index

## 1. Bắt đầu từ workload

Không nên tạo Index chỉ vì một cột “có vẻ quan trọng”. Hãy liệt kê truy vấn thường chạy, tần suất, điều kiện WHERE, JOIN, ORDER BY, GROUP BY, số trường trả về và tỷ lệ đọc/ghi. Index cần được thiết kế để phục vụ workload cụ thể.

## 2. Selectivity và cardinality

Điều kiện lọc có selectivity cao thường giảm được nhiều bản ghi và có khả năng làm Index hữu ích hơn. Tuy nhiên, cardinality cao không tự động đảm bảo Index sẽ được chọn. Optimizer còn cân nhắc số dòng, phân bố dữ liệu, chi phí đọc ngẫu nhiên, khả năng cache và thống kê.

Ví dụ: cột trạng thái đơn hàng chỉ có vài giá trị có thể có selectivity thấp nếu truy vấn lấy phần lớn đơn hàng. Nhưng nếu workload thường lọc một trạng thái hiếm, Index vẫn có thể hữu ích.

## 3. Thứ tự cột trong composite index

Với B-tree composite index, nguyên tắc leftmost prefix có ý nghĩa: Index (customer_id, order_date) thường hỗ trợ truy vấn theo customer_id hoặc theo customer_id kết hợp order_date; nó không tương đương Index bắt đầu bằng order_date.

Khi thiết kế, xem xét:
1. Các điều kiện bằng (equality).
2. Trường phục vụ sắp xếp (sort).
3. Điều kiện khoảng (range).
4. Các trường cần trả về để cân nhắc covering index.

Đây là hướng dẫn phân tích, không phải công thức cứng cho mọi truy vấn. Cần kiểm tra execution plan thực tế.

## 4. Covering index

Nếu Index chứa tất cả trường cần cho truy vấn, DBMS có thể trả kết quả từ Index mà không phải đọc thêm bản ghi gốc trong một số trường hợp. Lợi ích cần cân đối với kích thước Index và chi phí ghi tăng thêm.

## 5. Chi phí ghi và dung lượng

Mỗi Index liên quan phải được cập nhật khi dữ liệu thay đổi. Nhiều Index có thể:
- Tăng dung lượng lưu trữ.
- Tăng thời gian INSERT/UPDATE/DELETE.
- Tăng chi phí tạo Index và bảo trì.
- Làm tăng áp lực cache và I/O.

Vì vậy benchmark cần đo cả workload đọc lẫn ghi, không chỉ thời gian SELECT.

## 6. Tránh Index dư thừa

Hai Index có tiền tố hoặc phạm vi gần nhau có thể chồng lấn. Tuy nhiên, không xóa chỉ dựa vào tên hoặc cảm giác trùng lặp: khác biệt về thứ tự cột, uniqueness, collation, partial filter, sort direction và workload có thể khiến chúng có vai trò khác nhau.

## 7. Quy trình thiết kế đề xuất

1. Thu thập truy vấn và phân loại workload.
2. Xác định điều kiện lọc, join, sort và trường đầu ra.
3. Đề xuất Index tối thiểu có cơ sở.
4. Kiểm tra execution plan trước và sau khi tạo Index.
5. Đo latency và tài nguyên với dữ liệu/workload cố định.
6. Đo ảnh hưởng đến thao tác ghi và dung lượng.
7. Lặp lại với từng phương án, chỉ thay đổi một yếu tố tại một thời điểm.
8. Ghi lại phiên bản, cấu hình, lệnh chạy và seed dữ liệu để tái lập.

## 8. Các lỗi diễn giải cần tránh

- Index tồn tại không có nghĩa optimizer chắc chắn sử dụng.
- Index được sử dụng không đồng nghĩa truy vấn tối ưu.
- So sánh hai DB với schema hoặc workload khác bản chất sẽ không công bằng.
- Chỉ đo một lần dễ bị ảnh hưởng bởi cache và nhiễu hệ thống.
- Không được kết luận từ dữ liệu mẫu nhỏ rồi suy rộng thành “luôn nhanh hơn”.
