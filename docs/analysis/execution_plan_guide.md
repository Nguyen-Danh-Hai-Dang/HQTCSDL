# Hướng dẫn đọc Execution Plan

## 1. Mục tiêu

Execution plan cho biết DBMS dự định hoặc đã thực thi truy vấn bằng cách nào. Đây là bằng chứng để giải thích vì sao truy vấn nhanh/chậm, thay vì chỉ so sánh thời gian cuối cùng.

## 2. MySQL

### EXPLAIN
Dùng để xem kế hoạch mà optimizer chọn:

```sql
EXPLAIN
SELECT order_id, order_date, status
FROM orders
WHERE customer_id = 1001
ORDER BY order_date DESC;
```

Các cột thường cần chú ý:
- **type**: access method; cần phân biệt scan và truy cập theo Index.
- **possible_keys**: Index optimizer có thể cân nhắc.
- **key**: Index được chọn.
- **rows**: số dòng ước lượng cần đọc.
- **filtered**: tỷ lệ phần trăm dòng dự kiến còn lại sau điều kiện lọc.
- **Extra**: thông tin bổ sung như Using where, Using index, Using filesort.

### EXPLAIN ANALYZE
Thực thi truy vấn và cung cấp thông tin thực tế về iterator, số dòng và thời gian. Chỉ chạy trên môi trường thử nghiệm phù hợp vì câu lệnh thực sự thực thi truy vấn.

## 3. MongoDB

Ví dụ:

```javascript
db.orders.find(
  { customer_id: 1001 },
  { order_id: 1, order_date: 1, status: 1, _id: 0 }
).sort({ order_date: -1 }).explain("executionStats")
```

Các trường/stage cần chú ý:
- **winningPlan**: kế hoạch được chọn.
- **COLLSCAN**: quét collection.
- **IXSCAN**: quét Index.
- **FETCH**: đọc document sau khi lấy candidate từ Index.
- **SORT**: stage sắp xếp riêng, có thể chỉ ra Index chưa đáp ứng thứ tự sort.
- **nReturned**: số document trả về.
- **totalKeysExamined**: số index key đã kiểm tra.
- **totalDocsExamined**: số document đã kiểm tra.
- **executionTimeMillis**: thời gian thực thi được báo cáo.

## 4. Cách so sánh công bằng

- Giữ nguyên dataset, truy vấn, tham số, cấu hình máy và phiên bản.
- Ghi rõ trạng thái Index trước/sau.
- Chạy warm-up và nhiều lần đo; báo cáo median và percentile thay vì chỉ một lần.
- Phân biệt estimated plan với actual execution.
- Không chỉ nhìn có IXSCAN/key hay không; phải xem keys/docs examined và nReturned.
- Ghi nhận việc optimizer có thể chọn plan khác khi thống kê hoặc quy mô dữ liệu thay đổi.

## 5. Mẫu bảng ghi nhận

| DB | Truy vấn | Index | Plan/access | Rows/keys examined | Docs/rows returned | Median latency | Ghi chú |
|---|---|---|---|---:|---:|---:|---|
| MySQL | Q1 | Không | [TODO] | [TODO] | [TODO] | [TODO] | |
| MySQL | Q1 | Có | [TODO] | [TODO] | [TODO] | [TODO] | |
| MongoDB | Q1 tương đương | Không | [TODO] | [TODO] | [TODO] | [TODO] | |
| MongoDB | Q1 tương đương | Có | [TODO] | [TODO] | [TODO] | [TODO] | |

## 6. Cảnh báo diễn giải

Không so sánh trực tiếp executionTimeMillis với MySQL actual time nếu cách đo, phạm vi thời gian và overhead khác nhau mà không giải thích. Không khẳng định IXSCAN luôn nhanh hơn COLLSCAN. Optimizer chọn kế hoạch dựa trên ước lượng chi phí; cần đặt plan trong ngữ cảnh workload và dữ liệu.
