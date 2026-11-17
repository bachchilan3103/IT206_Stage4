# BÀI TẬP: KHẮC PHỤC LỖI KHAI BÁO BIẾN SỐ TRONG PROMPT MẪU

## BƯỚC 1: PHÂN TÍCH LỖI

**Prompt bị lỗi:**

Hãy viết một bài chia sẻ về Kỹ thuật Prompting Nâng cao (Logic & Chuỗi) bằng ngôn ngữ [language]. Dành cho đối tượng {audience}.

**Nguyên nhân gây lỗi:**

- `[language]` sử dụng ngoặc vuông, còn `{audience}` sử dụng ngoặc nhọn nên không thống nhất cách khai báo biến.
- Khi tự động thay thế dữ liệu, hệ thống có thể khó xác định chính xác vị trí và tên biến.
- Việc sử dụng nhiều kiểu ký hiệu khác nhau làm prompt khó quản lý và dễ xảy ra lỗi khi tái sử dụng.

## BƯỚC 2: ĐỒNG BỘ BIẾN SỐ

**Prompt sau khi chỉnh sửa:**

Hãy viết một bài chia sẻ về Kỹ thuật Prompting Nâng cao (Logic & Chuỗi) bằng ngôn ngữ [NGÔN_NGỮ]. Dành cho đối tượng [ĐỐI_TƯỢNG].

**Các biến đã đồng bộ:**

- `[NGÔN_NGỮ]`: Ngôn ngữ sử dụng để viết nội dung.
- `[ĐỐI_TƯỢNG]`: Nhóm người đọc mà bài viết hướng đến.

## BƯỚC 3: KẾT QUẢ CHẠY THỬ

**Dữ liệu thay thế:**

- `[NGÔN_NGỮ]` = Tiếng Việt.
- `[ĐỐI_TƯỢNG]` = Sinh viên ngành Công nghệ thông tin.

**Prompt sau khi điền dữ liệu:**

Hãy viết một bài chia sẻ về Kỹ thuật Prompting Nâng cao (Logic & Chuỗi) bằng tiếng Việt. Dành cho đối tượng là sinh viên ngành Công nghệ thông tin.

**Kết quả AI trả về:**

### Kỹ thuật Prompting Nâng cao (Logic & Chuỗi)

Kỹ thuật Prompting Nâng cao giúp người dùng khai thác AI hiệu quả hơn thông qua việc xây dựng câu lệnh có cấu trúc và logic rõ ràng.

- **Chain-of-Thought:** Giúp AI phân tích vấn đề theo từng bước logic để đưa ra kết quả phù hợp.
- **Prompt Chaining:** Chia nhiệm vụ lớn thành nhiều bước nhỏ, sử dụng kết quả của bước trước làm đầu vào cho bước tiếp theo.
- **Xử lý đứt gãy:** Kiểm tra và điều chỉnh những bước gặp lỗi trong chuỗi prompt để đảm bảo kết quả cuối cùng chính xác.

Các kỹ thuật này giúp sinh viên sử dụng AI có hệ thống hơn, đặc biệt khi giải quyết những nhiệm vụ phức tạp.