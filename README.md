# Mật Mã Số

Tool mã hóa chuỗi chữ (tiếng Trung, tiếng Việt, bất kỳ ngôn ngữ nào) thành một dãy số ngắn bằng key tự nhập, và giải mã lại bằng đúng key đó.

## Cách dùng

Mở `mat-ma-so.html` bằng trình duyệt. Không cần cài đặt, chạy được offline, chữ và key không gửi đi đâu.

- **Mã hóa:** nhập key và chuỗi, dãy số hiện ngay, bấm Copy.
- **Giải mã:** dán dãy số, nhập đúng key là ra chuỗi gốc. Dấu cách và dấu gạch được bỏ qua.

## Hai chế độ

| Chế độ | Độ dài | Đặc điểm |
|---|---|---|
| Ngắn gọn | `你好世界` → 20 chữ số | Cùng chuỗi và cùng key luôn ra cùng dãy số |
| Bảo mật | dài thêm khoảng 38 chữ số | AES-GCM, salt ngẫu nhiên, mỗi lần ra dãy khác nhau |

## Cách hoạt động

1. Mỗi ký tự được đánh số theo bảng chữ ngắn nhất chứa nó: ASCII, tiếng Việt, 3755 chữ Hán thông dụng (GB2312 cấp 1), hoặc bảng Hán mở rộng. Chữ ngoài bảng được ghi kèm mã Unicode. Văn bản dài thì tự nén (deflate) nếu ngắn hơn.
2. Cả chuỗi được gộp thành một số nguyên lớn theo cơ số hỗn hợp.
3. Chế độ Ngắn gọn: key qua PBKDF2-SHA256, rồi XOR từng bit với keystream HMAC-SHA256, kèm mã kiểm tra 10 bit. Chế độ Bảo mật: AES-GCM với key dẫn xuất từ salt ngẫu nhiên 8 byte.
4. Kết quả được viết ra hệ thập phân.

## Lưu ý

- Key 4 chữ số rất dễ bị dò. Cần giữ bí mật thật thì dùng key dài có cả chữ và số.
- Ở chế độ Ngắn gọn, khoảng 1/1000 trường hợp nhập sai key vẫn ra chuỗi vô nghĩa thay vì báo lỗi.
