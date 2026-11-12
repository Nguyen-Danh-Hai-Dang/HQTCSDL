# RQ7. Khung đánh giá lựa chọn Index và hệ quản trị cho workload bán hàng

## 1. Câu hỏi

Trong các điều kiện thực nghiệm của đề tài, chiến lược Index và hệ quản trị nào phù hợp hơn với từng nhóm workload quản lý bán hàng?

## 2. Nguyên tắc trả lời

RQ7 là câu hỏi tổng hợp. Không thể trả lời chỉ bằng việc xếp hạng một hệ quản trị nhanh nhất. Cần chia workload theo nghiệp vụ và đối chiếu hiệu năng đọc, ghi, tài nguyên, dung lượng, tính linh hoạt dữ liệu và độ phức tạp vận hành.

## 3. Ma trận đánh giá

| Nhóm workload | Tiêu chí chính | Kết quả cần dùng |
|---|---|---|
| Tra cứu đơn theo mã | Latency, uniqueness | Q1, execution plan |
| Tra cứu đơn theo khách hàng/thời gian | Latency, keys/rows examined, sort | Q2 |
| Lọc trạng thái và thời gian | Selectivity, latency | Q3 |
| Thống kê/tổng hợp | Latency, CPU, I/O | Q4 |
| Chèn đơn hàng | Throughput, latency ghi | W1 |
| Cập nhật trạng thái | Write latency, index maintenance | W2 |
| Tải hỗn hợp | Throughput, p95/p99 latency | Mixed workload |
| Chi phí lưu trữ | DB size, Index size | Snapshot dung lượng |

## 4. Tiêu chí kết luận

- **Hiệu năng đọc**: median, p95/p99 latency, throughput.
- **Hiệu năng ghi**: tốc độ insert/update/delete và write overhead.
- **Tài nguyên**: CPU, RAM, disk I/O nếu đo được.
- **Dung lượng**: kích thước dữ liệu và Index.
- **Tính phù hợp dữ liệu**: mô hình quan hệ/document, tính nhất quán và khả năng biểu diễn nghiệp vụ.
- **Vận hành**: độ phức tạp thiết kế, bảo trì và khả năng tái lập.

## 5. Cách viết kết luận

Dùng cấu trúc:
1. Với workload [X], phương án [Y] có kết quả [số liệu thực tế].
2. Execution plan cho thấy [bằng chứng].
3. Lợi ích đi kèm chi phí [ghi/dung lượng/tài nguyên].
4. Vì vậy, trong điều kiện [phiên bản, cấu hình, quy mô], phương án [Y] phù hợp hơn cho workload [X].
5. Giới hạn: [phạm vi dữ liệu, số lần đo, môi trường, tính đại diện].

## 6. Chỗ cần điền sau benchmark

- [TODO] Cấu hình máy và phiên bản hệ quản trị.
- [TODO] Dataset và quy mô.
- [TODO] Kết quả từng truy vấn/workload.
- [TODO] Kết quả execution plan.
- [TODO] Chi phí Index và write overhead.
- [TODO] Hạn chế và khuyến nghị có điều kiện.

**Không điền số liệu giả định.** RQ7 chỉ được hoàn thiện sau khi nhận kết quả benchmark đã kiểm tra từ các thành viên phụ trách.
