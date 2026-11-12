# Các loại Index trong MySQL và MongoDB

## 1. Index theo số trường

### Single-field index
Index trên một trường. Phù hợp khi workload thường xuyên lọc hoặc sắp xếp theo một trường độc lập.

### Compound index
Index trên nhiều trường. Thứ tự trường quyết định các tiền tố có thể sử dụng và ảnh hưởng đến sort/range. Không thể coi Index (A, B) tương đương (B, A).

## 2. Một số loại Index trong MySQL

- **PRIMARY KEY**: duy nhất, không NULL; với InnoDB là clustered index.
- **UNIQUE**: đảm bảo tính duy nhất theo quy tắc của cột/nhóm cột.
- **Secondary INDEX**: chỉ mục B-tree bổ sung.
- **Composite index**: chỉ mục nhiều cột.
- **FULLTEXT**: phục vụ tìm kiếm văn bản theo cơ chế full-text, khác với tìm kiếm bằng tiền tố B-tree.
- **SPATIAL**: phục vụ dữ liệu không gian trong các kiểu dữ liệu và điều kiện được hỗ trợ.

MySQL/InnoDB còn có các đặc tính tối ưu như prefix index và index condition pushdown trong những điều kiện phù hợp. Cần kiểm tra đúng phiên bản MySQL và storage engine của thí nghiệm trước khi mô tả tính năng.

## 3. Một số loại Index trong MongoDB

- **Single field**: chỉ mục một trường.
- **Compound**: chỉ mục nhiều trường, thứ tự tăng/giảm có thể ảnh hưởng khả năng đáp ứng sort.
- **Multikey**: tự động áp dụng khi lập chỉ mục trường chứa mảng; một document có thể tạo nhiều index key.
- **Text**: tìm kiếm văn bản theo cơ chế text index.
- **Geospatial (2d, 2dsphere)**: truy vấn dữ liệu không gian theo mô hình được hỗ trợ.
- **Hashed**: lập chỉ mục giá trị băm, thường dùng trong một số truy vấn bằng và thiết kế sharding.
- **Wildcard**: hỗ trợ lập chỉ mục các trường linh hoạt trong document theo phạm vi wildcard.
- **TTL**: tự động loại bỏ document hết hạn theo cấu hình thời gian.
- **Unique**: ràng buộc giá trị duy nhất.
- **Partial**: chỉ lập chỉ mục những document thỏa bộ lọc.
- **Sparse**: bỏ qua document không có trường được lập chỉ mục, theo quy tắc của MongoDB.

Các loại trên không có nghĩa đều cần đưa vào benchmark. Chỉ chọn loại liên quan trực tiếp đến workload bán hàng và khả năng triển khai công bằng giữa hai hệ thống.

## 4. Bảng đối chiếu định hướng

| Nhu cầu | MySQL | MongoDB |
|---|---|---|
| Lọc bằng một trường | B-tree secondary index | Single-field index |
| Lọc nhiều trường | Composite index | Compound index |
| Duy nhất | PRIMARY KEY / UNIQUE | Unique index |
| Tìm kiếm văn bản | FULLTEXT | Text index |
| Trường mảng | Không tương đương trực tiếp trong mô hình quan hệ | Multikey |
| Dữ liệu không gian | SPATIAL | 2d / 2dsphere |
| Tự xóa theo thời gian | Thường dùng tác vụ/partition/event tùy thiết kế | TTL index |

## 5. Lưu ý khi so sánh

Chỉ so sánh trực tiếp những phương án có mục tiêu truy vấn tương đương. Cần mô tả khác biệt về mô hình dữ liệu, semantics, tính năng, cấu hình và phiên bản. Không gộp mọi Index vào một nhóm duy nhất rồi kết luận từ một phép đo.
