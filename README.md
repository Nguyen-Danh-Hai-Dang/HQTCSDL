# HQTCSDL

Tài liệu và tiến độ nghiên cứu đề tài:

**MongoDB và MySQL trong cơ sở dữ liệu quản lý bán hàng lớn, kết hợp nghiên cứu các phương pháp Index.**

## Cấu trúc repository

- `docs/theory/`: kiến thức nền về Index, cấu trúc dữ liệu, các loại Index và nguyên tắc thiết kế.
- `docs/research_questions/`: nội dung định hướng cho RQ2, RQ3 và khung tổng hợp RQ7.
- `docs/analysis/`: hướng dẫn đọc execution plan và ghi nhận bằng chứng thực nghiệm.
- `docs/references/`: tài liệu tham khảo chính thức.

## Phân công nhóm

| Thành viên | Phụ trách chính |
|---|---|
| Nguyễn Danh Hải Đăng | Trưởng nhóm; lý thuyết Index; RQ2, RQ3; phân tích execution plan; tổng hợp RQ7, kết luận và rà soát |
| Lê Quang Vinh | MySQL/MongoDB; mô hình dữ liệu; triển khai DB; truy vấn; tạo Index; execution plan |
| Lưu Trung Trực | Dataset, workload, benchmark; đo lường, chuẩn hóa và trực quan hóa kết quả |

## Nguyên tắc nghiên cứu

1. Không điền hoặc suy diễn số liệu benchmark khi chưa có kết quả đo.
2. So sánh trong cùng điều kiện dữ liệu, truy vấn, cấu hình và quy trình đo.
3. Đánh giá cả hiệu năng đọc, ghi, tài nguyên và dung lượng Index.
4. Mọi kết luận phải gắn với workload và điều kiện thực nghiệm cụ thể.

## Trạng thái

Các tài liệu trong `docs/` hiện là khung lý thuyết và hướng dẫn phân tích ban đầu. Các mục `[TODO]` sẽ được hoàn thiện sau khi nhóm có kết quả thực nghiệm đã kiểm chứng.
