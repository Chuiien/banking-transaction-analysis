Banking Transaction Analysis
Data Analyst Project – Core Banking (T24 Context)
Dự án phân tích dữ liệu giao dịch ngân hàng nhằm hiểu rõ hành vi khách hàng, xác định nhóm khách hàng giá trị cao và theo dõi xu hướng giao dịch theo thời gian, phục vụ cho các quyết định kinh doanh và quản trị trong ngân hàng.

**1. Bối cảnh và mục tiêu phân tích (Background & Objectives)**
Trong hoạt động ngân hàng, dữ liệu giao dịch phản ánh trực tiếp hành vi sử dụng sản phẩm dịch vụ và giá trị của khách hàng. Việc phân tích dữ liệu này giúp ngân hàng đánh giá hiệu quả hoạt động, nhận diện khách hàng trọng tâm và tối ưu hóa chiến lược kinh doanh.
  **Mục tiêu chính của dự án:**
- Hiểu hành vi giao dịch của khách hàng
- Xác định nhóm khách hàng giá trị cao
- Phân tích xu hướng và chu kỳ giao dịch theo thời gian

  **Ứng dụng kết quả phân tích:**
- Ưu tiên chăm sóc và giữ chân khách hàng giá trị cao
- Tối ưu hóa danh mục sản phẩm và chính sách tài khoản
- Hỗ trợ ra quyết định kinh doanh dựa trên dữ liệu thực tế

Lưu ý: Dữ liệu sử dụng trong dự án là **dữ liệu mô phỏng**, phục vụ mục đích học tập và minh họa nghiệp vụ Core Banking.



**2. Tổng quan cấu trúc dữ liệu (Data Structure Overview)**
Bộ dữ liệu mô phỏng cấu trúc dữ liệu giao dịch trong hệ thống Core Banking.
  **Các thực thể chính:**
- Khách hàng (Customer): Thông tin định danh và đặc điểm cơ bản
- Tài khoản (Account): Mỗi khách hàng được giả định có một tài khoản chính
- Giao dịch (Transaction): Các giao dịch tài chính phát sinh từ tài khoản

  **Mối quan hệ dữ liệu:**
Customer → Account → Transaction
NOTE: Do dữ liệu không tách riêng bảng tài khoản, CustomerID được sử dụng như đại diện cho account_id nhằm đơn giản hóa mô hình và phù hợp với mục tiêu phân tích.


**3. Tóm tắt điều hành (Executive Summary)**
Dự án phân tích dữ liệu giao dịch ngân hàng nhằm làm rõ hành vi khách hàng, hiệu quả tài khoản và xu hướng giao dịch theo thời gian.

  **Phạm vi phân tích:**
- Thông tin khách hàng
- Số dư tài khoản
- Giá trị và thời điểm giao dịch

  **Kết quả chính:**
Hành vi khách hàng:
- Một nhóm nhỏ khách hàng đóng góp phần lớn tổng giá trị giao dịch
- Khách hàng có số dư cao thường phát sinh giao dịch giá trị lớn

Cơ cấu tài khoản
- Có sự khác biệt rõ rệt về tần suất và giá trị giao dịch giữa các tài khoản
- Một số tài khoản hoạt động hiệu quả hơn và cần được ưu tiên

Xu hướng giao dịch
- Hoạt động giao dịch có tính chu kỳ theo thời gian
- Một số giai đoạn ghi nhận sự tăng trưởng rõ rệt về khối lượng và giá trị giao dịch

Giá trị mang lại:
- Hỗ trợ quyết định kinh doanh dựa trên dữ liệu
- Nâng cao hiệu quả quản trị khách hàng và tài khoản
- Thể hiện khả năng xử lý dữ liệu thực tế trong bối cảnh ngân hàng

**4. Phân tích chuyên sâu (Insights Deep Dive)**
**4.1 Phân tích khách hàng (Customer Analysis)**
  **Mục tiêu:**
- Đánh giá giá trị khách hàng
- Xếp hạng khách hàng theo giá trị giao dịch
- Xác định nhóm khách hàng trọng tâm

  **Chỉ số chính:**
- Tổng giá trị giao dịch
- Số lượng giao dịch
- Thứ hạng khách hàng

  **Phương pháp:**
- Tổng hợp dữ liệu ở cấp độ khách hàng
- Sử dụng hàm RANK() để xếp hạng theo giá trị giao dịch
WITH customer_summary AS (
    SELECT
        CustomerID AS customer_id,
        COUNT(TransactionID) AS total_transactions,
        SUM(`TransactionAmount (INR)`) AS total_transaction_amount
    FROM transactions
    GROUP BY CustomerID
)
SELECT
    customer_id,
    total_transactions,
    total_transaction_amount,
    RANK() OVER (ORDER BY total_transaction_amount DESC) AS customer_rank
FROM customer_summary
ORDER BY customer_rank;

  **Insight chính:**
- Một tỷ lệ nhỏ khách hàng tạo ra phần lớn giá trị giao dịch
- Phần lớn khách hàng có giá trị thấp nhưng chiếm số lượng lớn

NOTE: Việc xếp hạng giúp ngân hàng ưu tiên nguồn lực chăm sóc hiệu quả hơn.

**4.2 Phân tích tài khoản (Account Analysis)**
  **Mục tiêu:**
- Đánh giá mức độ hoạt động của tài khoản
- Phân tích giá trị giao dịch theo tài khoản

NOTE: CustomerID được sử dụng như account_id trong phạm vi dự án.
SELECT
    CustomerID AS account_id,
    COUNT(TransactionID) AS total_transactions,
    SUM(`TransactionAmount (INR)`) AS total_transaction_amount
FROM transactions
GROUP BY CustomerID
ORDER BY total_transaction_amount DESC;

  **Insight chính:**
- Một số tài khoản có tần suất và giá trị giao dịch vượt trội
- Phần lớn tài khoản có mức độ hoạt động thấp

**4.3 Phân tích giao dịch (Transaction Analysis)**
  **Mục tiêu:**
- Phân tích giao dịch theo thời gian
- Nhận diện xu hướng và chu kỳ giao dịch

NOTE: Ngày giao dịch được chuyển đổi từ dạng dd-mm-yyyy sang kiểu date trước khi phân tích.

WITH parsed_transactions AS (
    SELECT
        TransactionID,
        STR_TO_DATE(TransactionDate, '%d-%m-%Y') AS transaction_date,
        `TransactionAmount (INR)` AS transaction_amount
    FROM transactions
)
SELECT
    DATE_FORMAT(transaction_date, '%Y-%m') AS transaction_month,
    COUNT(TransactionID) AS total_transactions,
    SUM(transaction_amount) AS total_transaction_amount
FROM parsed_transactions
GROUP BY DATE_FORMAT(transaction_date, '%Y-%m')
ORDER BY transaction_month;

  **Insight chính:**
- Giao dịch có tính chu kỳ theo thời gian
- Một số giai đoạn ghi nhận mức tăng trưởng rõ rệt

**5. Khuyến nghị (Recommendations)**
- Ưu tiên chăm sóc nhóm khách hàng giá trị cao
- Tối ưu hóa danh mục và chính sách tài khoản
- Lập kế hoạch kinh doanh theo chu kỳ giao dịch
- Chuẩn hóa dữ liệu để nâng cao chất lượng phân tích
- Mở rộng sang các phân tích dự báo trong tương lai
