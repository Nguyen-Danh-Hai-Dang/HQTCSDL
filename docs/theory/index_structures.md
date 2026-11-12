# Các cấu trúc dữ liệu Index

## 1. B-tree và B+tree

B-tree là họ cấu trúc cây cân bằng được dùng rộng rãi để hỗ trợ tìm kiếm, chèn và xóa với chiều cao cây được kiểm soát. Các thao tác thường có độ phức tạp theo số mức của cây, xấp xỉ logarithmic theo số entry trong điều kiện mô hình lý tưởng.

B+tree là biến thể phổ biến trong hệ quản trị cơ sở dữ liệu: các khóa tìm kiếm được tổ chức ở các nút trong, còn dữ liệu/định vị bản ghi nằm ở tầng lá; các nút lá thường được liên kết để hỗ trợ duyệt tuần tự và truy vấn khoảng. Cách tổ chức này phù hợp với:
- So sánh bằng (=).
- Truy vấn khoảng (>, >=, <, <=, BETWEEN).
- Sắp xếp theo thứ tự khóa.
- Một số truy vấn tiền tố và truy vấn kết hợp nhiều điều kiện, tùy thứ tự cột và optimizer.

**Trong MySQL InnoDB:** chỉ mục chính (PRIMARY KEY) là clustered index, dữ liệu hàng được tổ chức theo khóa chính. Secondary index lưu khóa phụ cùng giá trị khóa chính để định vị hàng trong clustered index. Vì vậy khóa chính dài có thể làm các secondary index lớn hơn.

**Trong MongoDB:** các Index thông thường trên trường đơn hoặc nhiều trường sử dụng cấu trúc B-tree. Khả năng tận dụng compound index phụ thuộc thứ tự trường, điều kiện truy vấn và sort.

## 2. Hash index

Hash index dùng hàm băm để ánh xạ khóa sang vị trí lưu trữ. Cấu trúc này phù hợp với tra cứu bằng chính xác trong mô hình hỗ trợ, nhưng không tự nhiên hỗ trợ truy vấn khoảng hay thứ tự khóa.

Không nên mặc định rằng mọi bảng MySQL InnoDB đều có hash index do người dùng tạo. InnoDB có Adaptive Hash Index là cơ chế nội bộ có điều kiện, khác với việc tạo một chỉ mục HASH tùy ý. MongoDB cung cấp hashed index với mục đích và giới hạn riêng, bao gồm hỗ trợ một số kiểu shard key; hashed index không thay thế B-tree cho truy vấn khoảng.

## 3. Clustered và secondary index

Clustered index quyết định cách các bản ghi được tổ chức vật lý hoặc logic theo khóa của cấu trúc lưu trữ. Trong InnoDB, bảng được tổ chức theo PRIMARY KEY; nếu không khai báo khóa chính, InnoDB chọn khóa phù hợp theo quy tắc của nó.

Secondary index là cấu trúc phụ. Trong InnoDB, leaf entry của secondary index chứa giá trị khóa phụ và khóa chính. Truy vấn có thể cần bước tra cứu tiếp vào clustered index nếu các cột cần lấy không nằm trong secondary index.

MongoDB không dùng thuật ngữ clustered/secondary theo đúng mô hình InnoDB trong thiết kế Index thông thường; cần tránh áp dụng máy móc khái niệm giữa hai hệ thống.

## 4. Những yếu tố ảnh hưởng hiệu quả cấu trúc

- Số lượng entry và chiều cao cây.
- Kích thước khóa và độ rộng bản ghi chỉ mục.
- Mức độ phân biệt của giá trị.
- Tính tuần tự hay ngẫu nhiên của thao tác ghi.
- Mức độ phù hợp giữa thứ tự trường trong Index và truy vấn.
- Cache, RAM, storage engine, cấu hình và phân bố dữ liệu.

## 5. Gắn với thực nghiệm

Khi đọc execution plan, không chỉ nhìn tên cấu trúc Index. Cần ghi nhận số dòng ước lượng/thực tế, số key và document/row được kiểm tra, số lần truy cập dữ liệu, thời gian, sort và scan. Một Index được sử dụng vẫn có thể kém hiệu quả nếu phải đọc quá nhiều entry hoặc quá nhiều bản ghi.
