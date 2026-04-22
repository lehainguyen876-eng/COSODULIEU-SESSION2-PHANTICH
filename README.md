[ Phân tích ] Khủng hoảng nâng cấp Dữ liệu Số Điện Thoại
## 1. Phân tích vấn đề & Đề xuất
**Hiện trạng:** Bảng `USERS` có 2 triệu bản ghi, cột `Phone` đang để kiểu `INT` làm mất số 0 ở đầu (ví dụ: 098 -> 98), khiến khách hàng không thể đăng nhập.

Em đề xuất 2 giải pháp DDL để xử lý:
* **Giải pháp 1:** Sửa đổi trực tiếp (Direct Modify) bằng lệnh `ALTER TABLE`.
* **Giải pháp 2:** Chiến thuật cột tạm (Shadow Column) - Tạo cột mới, copy dữ liệu rồi đổi tên.

## 2. So sánh & Lựa chọn (Trade-off)

| Tiêu chí | Giải pháp 1: Sửa trực tiếp | Giải pháp 2: Chiến thuật cột tạm |
| :--- | :--- | :--- |
| **Độ khó** | **Rất dễ:** Chỉ cần 1 dòng code. | **Cao:** Cần nhiều bước thực hiện. |
| **Rủi ro** | **Cao:** Có thể gây khóa bảng (Table Lock) lâu trên 2 triệu dòng, làm gián đoạn ứng dụng. | **Thấp:** Không gây gián đoạn hệ thống do xử lý dữ liệu song song. |
| **An toàn** | Dễ hỏng dữ liệu nếu xảy ra lỗi giữa chừng. | Rất an toàn, luôn có cột cũ để dự phòng. |



**=> Lựa chọn:** Em chọn **Giải pháp 1 (Sửa trực tiếp)** để tối ưu thời gian thực thi.

