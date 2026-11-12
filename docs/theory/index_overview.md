# Tổng quan về Index trong hệ quản trị cơ sở dữ liệu

## 1. Khái niệm

Index (chỉ mục) là cấu trúc dữ liệu phụ trợ được DBMS duy trì nhằm rút ngắn quá trình tìm kiếm bản ghi. Thay vì phải đọc và kiểm tra lần lượt toàn bộ bản ghi trong bảng hoặc collection (full scan), hệ quản trị có thể đi qua cấu trúc chỉ mục để xác định những vị trí có khả năng chứa dữ liệu cần tìm.

Có thể hình dung Index như mục lục của một cuốn sách: mục lục giúp tìm nhanh chương cần đọc, nhưng bản thân mục lục chiếm thêm không gian và phải được cập nhật khi nội dung thay đổi.

## 2. Index hoạt động ở mức khái quát

1. Người dùng gửi truy vấn có điều kiện lọc, sắp xếp, nối bảng hoặc truy cập theo một trường.
2. Bộ tối ưu truy vấn (query optimizer) xem xét các kế hoạch thực thi khả thi.
3. Nếu có Index phù hợp và chi phí ước lượng thấp hơn, DBMS có thể chọn truy cập qua Index.
4. DBMS đọc các mục chỉ mục liên quan, sau đó có thể truy xuất bản ghi tương ứng.
5. Kết quả được lọc, sắp xếp hoặc kết hợp tiếp theo yêu cầu truy vấn.

Có Index không đồng nghĩa mọi truy vấn đều nhanh hơn. Optimizer có thể chọn full scan khi bảng nhỏ, điều kiện không chọn lọc, thống kê không phù hợp hoặc chi phí truy cập Index cộng thêm đọc bản ghi lớn hơn.

## 3. Thành phần và chi phí

- **Search key**: trường hoặc tổ hợp trường được lập chỉ mục.
- **Index entry**: thông tin khóa và tham chiếu đến bản ghi hoặc vị trí dữ liệu.
- **Index structure**: cấu trúc tổ chức các entry, ví dụ B-tree hoặc hash.
- **Statistics**: thông tin như cardinality và phân bố dữ liệu giúp optimizer ước lượng chi phí.

Index tạo ra các lợi ích: giảm số trang dữ liệu phải đọc, hỗ trợ truy vấn theo khoảng, hỗ trợ sắp xếp/nối trong một số trường hợp và có thể tránh đọc bảng nếu Index bao phủ đủ dữ liệu cần thiết.

Đổi lại, Index tiêu tốn dung lượng lưu trữ, làm tăng công việc INSERT/UPDATE/DELETE, có chi phí tạo và bảo trì, đồng thời có thể làm tăng độ phức tạp của thiết kế.

## 4. Những khái niệm cần phân biệt

- **Selectivity**: mức độ một điều kiện lọc thu hẹp tập bản ghi. Điều kiện càng chọn lọc thường càng có khả năng tận dụng Index hiệu quả.
- **Cardinality**: số lượng giá trị phân biệt của một cột hoặc nhóm cột, tùy cách DBMS định nghĩa thống kê.
- **Clustered index**: cách tổ chức dữ liệu bảng gắn với thứ tự khóa chỉ mục trong các hệ quản trị hỗ trợ mô hình này.
- **Secondary index**: chỉ mục bổ sung ngoài cấu trúc tổ chức dữ liệu chính.
- **Covering index**: Index chứa đủ các trường cần thiết cho một truy vấn, có thể giúp tránh truy cập thêm vào dữ liệu gốc.
- **Composite index**: Index trên nhiều trường; thứ tự trường có ý nghĩa đối với khả năng sử dụng.

## 5. Phạm vi nghiên cứu của nhóm

Đề tài so sánh MySQL và MongoDB trong bối cảnh cơ sở dữ liệu quản lý bán hàng lớn, tập trung vào cách thiết kế và sử dụng Index. Phần lý thuyết cần được nối với workload thực nghiệm: truy vấn tra cứu đơn hàng, lọc theo khách hàng/trạng thái/thời gian, thống kê và ghi dữ liệu.

Không kết luận một hệ quản trị hoặc một loại Index tốt nhất trong mọi trường hợp. Kết luận phải dựa trên workload, cấu hình, phiên bản, dữ liệu và phép đo được ghi nhận.
