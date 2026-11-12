# RQ2. Index là gì và vì sao ảnh hưởng đến hiệu năng truy vấn?

## 1. Câu hỏi nghiên cứu

Index là gì trong hệ quản trị cơ sở dữ liệu? Index tác động đến hiệu năng truy vấn như thế nào trong hệ thống quản lý bán hàng có quy mô dữ liệu lớn?

## 2. Luận điểm cần trình bày

Index là cấu trúc dữ liệu phụ trợ giúp DBMS định vị bản ghi phù hợp mà không phải quét toàn bộ dữ liệu trong mọi trường hợp. Index có thể giảm lượng dữ liệu phải đọc, hỗ trợ tìm kiếm theo khóa, truy vấn khoảng, sắp xếp và một số phép nối.

Tác động hiệu năng phụ thuộc:
- Kiểu cấu trúc Index và trường được lập chỉ mục.
- Selectivity/cardinality và phân bố dữ liệu.
- Quy mô bảng/collection.
- Mức độ tương thích giữa Index và điều kiện truy vấn.
- Optimizer, thống kê, cache và storage engine.
- Tỷ lệ đọc/ghi của workload.

## 3. Ví dụ theo nghiệp vụ bán hàng

Các truy vấn có thể gồm:
- Tìm đơn hàng theo mã đơn.
- Liệt kê đơn của một khách hàng theo thời gian.
- Lọc đơn theo trạng thái và khoảng ngày.
- Tìm chi tiết đơn theo order_id.
- Thống kê doanh thu theo thời gian hoặc nhóm sản phẩm.

Mỗi truy vấn cần được mô tả bằng điều kiện thực tế và dữ liệu đầu vào. Không nên mặc định cùng một Index tối ưu tất cả truy vấn.

## 4. Cách chứng minh bằng thực nghiệm

Với từng truy vấn:
1. Chạy khi chưa có Index ứng viên.
2. Ghi nhận execution plan và thời gian.
3. Tạo đúng một Index ứng viên.
4. Chạy lại cùng dữ liệu, tham số, cấu hình và số lần lặp.
5. Thu thập median và percentile latency, đồng thời ghi nhận tài nguyên nếu có.
6. So sánh kế hoạch thực thi và số bản ghi/key phải kiểm tra.

MySQL: dùng EXPLAIN và, khi phù hợp, EXPLAIN ANALYZE. Ghi nhận access type, key, rows, filtered và actual timing/rows từ phân tích thực thi.

MongoDB: dùng explain("executionStats"). Ghi nhận winningPlan, stage, nReturned, totalKeysExamined, totalDocsExamined và executionTimeMillis. COLLSCAN thường biểu thị quét collection; IXSCAN biểu thị truy cập qua Index. Cần đọc toàn bộ cây plan, không chỉ một stage.

## 5. Giả thuyết cần kiểm tra

- H1: Với truy vấn lọc chọn lọc trên dữ liệu đủ lớn, Index phù hợp có thể giảm số bản ghi phải kiểm tra và giảm latency.
- H2: Với truy vấn trả về phần lớn dữ liệu hoặc bảng/collection nhỏ, Index có thể không đem lại lợi ích rõ rệt.
- H3: Tăng số lượng Index có thể cải thiện một số truy vấn đọc nhưng làm tăng chi phí ghi và dung lượng.

Đây là giả thuyết, không phải kết quả. Nhóm chỉ chấp nhận hoặc bác bỏ dựa trên số liệu thực nghiệm.

## 6. Dữ liệu cần bổ sung sau benchmark

- [TODO] Phiên bản MySQL/MongoDB và cấu hình máy.
- [TODO] Mô tả dataset, số bản ghi và phân bố dữ liệu.
- [TODO] Truy vấn đại diện và số lần chạy.
- [TODO] Execution plan trước/sau.
- [TODO] Bảng latency và tài nguyên.
- [TODO] Nhận xét có đối chiếu với giả thuyết.
