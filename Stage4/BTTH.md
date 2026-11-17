# BÀI TẬP THỰC HÀNH TỔNG HỢP
## SESSION 04: KỸ THUẬT PROMPTING NÂNG CAO (LOGIC & CHUỖI)

## 1. BỐI CẢNH BÀI TOÁN

Một công ty công nghệ thường xuyên nhận được các yêu cầu hỗ trợ kỹ thuật từ nhân viên. Nội dung yêu cầu có thể không đầy đủ, sai định dạng hoặc viết không rõ ràng.

Nếu xử lý thủ công sẽ mất nhiều thời gian. Vì vậy, em đề xuất sử dụng AI kết hợp Prompt Chaining để tự động tiếp nhận, phân loại và đưa ra hướng xử lý cho từng yêu cầu.

**Mục tiêu:**

- Tiếp nhận thông tin từ người dùng.
- Phân tích và phân loại lỗi.
- Đề xuất hướng giải quyết phù hợp.
- Kiểm tra kết quả trước khi trả lời.
- Xử lý dữ liệu thiếu hoặc sai định dạng mà không làm gián đoạn hệ thống.

## 2. THIẾT KẾ GIẢI PHÁP

Sử dụng Prompt Chaining để chia bài toán thành 4 bước liên kết với nhau.

### Sơ đồ hoạt động

```text
Dữ liệu đầu vào
      |
      v
Bước 1: Tiếp nhận và chuẩn hóa dữ liệu
      |
      v
Bước 2: Phân tích và phân loại lỗi
      |
      v
Bước 3: Đề xuất hướng xử lý
      |
      v
Bước 4: Kiểm tra và hoàn thiện kết quả
      |
      v
Kết quả trả về cho người dùng
```

### Vai trò của các kỹ thuật

| Kỹ thuật | Cách áp dụng |
|---|---|
| Chain-of-Thought | Yêu cầu AI phân tích theo từng bước và đưa ra lý do ngắn gọn cho kết quả phân loại. |
| Prompt Chaining | Chia nhiệm vụ thành 4 prompt, đầu ra bước trước làm đầu vào bước sau. |
| Xử lý đứt gãy | Kiểm tra dữ liệu thiếu, sai định dạng và cho phép thực hiện lại bước bị lỗi. |

## 3. PROMPT HỆ THỐNG

```text
Bạn là AI hỗ trợ kỹ thuật IT.

Nhiệm vụ:
Tiếp nhận, phân tích và đề xuất cách xử lý
các yêu cầu hỗ trợ kỹ thuật.

Quy định:
- Làm việc theo từng bước.
- Không tự ý tạo thông tin khi dữ liệu bị thiếu.
- Nếu dữ liệu không hợp lệ, phải thông báo rõ.
- Ưu tiên giải pháp đơn giản, dễ thực hiện.
- Không đưa ra kết luận khi chưa đủ thông tin.
- Trả kết quả đúng định dạng JSON được yêu cầu.
- Nếu gặp lỗi, trả về trạng thái ERROR
  và mô tả nguyên nhân.
```

## 4. XÂY DỰNG CHUỖI PROMPT THỰC THI

### Bước 1: Tiếp nhận và chuẩn hóa dữ liệu

**Prompt:**

```text
Bạn là trợ lý tiếp nhận yêu cầu IT.

Hãy đọc thông tin người dùng cung cấp,
chuẩn hóa nội dung và kiểm tra dữ liệu.

Yêu cầu:
- Xác định tiêu đề lỗi.
- Tóm tắt nội dung.
- Kiểm tra thông tin còn thiếu.
- Không tự suy đoán dữ liệu.

Đầu ra JSON:
{
  "status": "SUCCESS hoặc NEED_INFO",
  "title": "",
  "description": "",
  "missing_info": []
}

Dữ liệu đầu vào:
[THONG_TIN_NGUOI_DUNG]
```

### Bước 2: Phân tích và phân loại lỗi

**Prompt:**

```text
Dựa trên dữ liệu đã chuẩn hóa ở bước 1,
hãy phân tích và phân loại lỗi.

Yêu cầu:
- Xác định loại lỗi.
- Đánh giá mức độ ảnh hưởng.
- Nêu nguyên nhân có thể xảy ra.
- Đưa ra lý do phân loại ngắn gọn.
- Không khẳng định nguyên nhân khi chưa đủ bằng chứng.

Đầu ra JSON:
{
  "status": "SUCCESS hoặc NEED_INFO",
  "error_type": "",
  "severity": "",
  "possible_cause": "",
  "reason": ""
}

Dữ liệu bước 1:
[DU_LIEU_BUOC_1]
```

### Bước 3: Đề xuất hướng xử lý

**Prompt:**

```text
Dựa trên kết quả phân tích ở bước 2,
hãy đề xuất cách khắc phục lỗi.

Yêu cầu:
- Đưa ra các bước xử lý cụ thể.
- Sắp xếp từ dễ đến khó.
- Có phương án khác nếu cách đầu tiên thất bại.
- Không đề xuất thao tác nguy hiểm khi chưa có xác nhận.

Đầu ra JSON:
{
  "status": "SUCCESS hoặc NEED_INFO",
  "solutions": [],
  "alternative": "",
  "warning": ""
}

Dữ liệu bước 2:
[DU_LIEU_BUOC_2]
```

### Bước 4: Kiểm tra và hoàn thiện kết quả

**Prompt:**

```text
Bạn là người kiểm tra kết quả hỗ trợ IT.

Hãy kiểm tra thông tin từ bước 1, 2 và 3.

Yêu cầu:
- Kiểm tra tính đầy đủ và logic.
- Loại bỏ thông tin không có căn cứ.
- Nếu thiếu dữ liệu, ghi rõ cần bổ sung.
- Nếu có lỗi, yêu cầu thực hiện lại bước liên quan.
- Chỉ xuất kết quả cuối cùng khi dữ liệu hợp lệ.

Đầu ra:
1. Trạng thái xử lý.
2. Nội dung lỗi.
3. Nguyên nhân có thể xảy ra.
4. Hướng khắc phục.
5. Thông tin cần bổ sung (nếu có).

Dữ liệu:
[DU_LIEU_BUOC_1]
[DU_LIEU_BUOC_2]
[DU_LIEU_BUOC_3]
```

## 5. CƠ CHẾ XỬ LÝ ĐỨT GÃY

Để tránh hệ thống bị dừng khi gặp dữ liệu không hợp lệ, thiết lập các quy tắc:

| Tình huống | Cách xử lý |
|---|---|
| Thiếu thông tin | Yêu cầu người dùng bổ sung dữ liệu. |
| Sai định dạng JSON | Yêu cầu AI xuất lại đúng định dạng. |
| Kết quả không đầy đủ | Thực hiện lại bước bị lỗi. |
| Nguyên nhân chưa rõ | Không tự kết luận, yêu cầu thêm thông tin. |
| AI trả về lỗi liên tục | Dừng xử lý và thông báo cho người quản lý. |

**Nguyên tắc:** Mỗi bước chỉ được chuyển dữ liệu sang bước tiếp theo khi kết quả đã được kiểm tra hợp lệ. Giới hạn tối đa 2 lần thử lại cho mỗi bước để tránh lặp vô hạn.

## 6. KẾT QUẢ CHẠY THỬ

### 6.1. Dữ liệu đầu vào

Nhân viên gửi yêu cầu:

```text
Máy tính của tôi không mở được VS Code.
Khi mở chương trình thì xuất hiện lỗi
"Unable to write to user settings".
Tôi đã thử khởi động lại nhưng vẫn không được.
```

### 6.2. Kết quả bước 1: Chuẩn hóa dữ liệu

```json
{
  "status": "SUCCESS",
  "title": "Không mở được VS Code",
  "description": "VS Code xuất hiện lỗi Unable to write to user settings khi khởi động. Khởi động lại máy nhưng chưa khắc phục được.",
  "missing_info": [
    "Phiên bản VS Code",
    "Hệ điều hành đang sử dụng"
  ]
}
```

### 6.3. Kết quả bước 2: Phân tích lỗi

```json
{
  "status": "SUCCESS",
  "error_type": "Lỗi quyền truy cập tệp cấu hình",
  "severity": "Trung bình",
  "possible_cause": "VS Code có thể không có quyền ghi vào tệp cài đặt người dùng hoặc tệp cấu hình đang bị khóa.",
  "reason": "Thông báo lỗi liên quan trực tiếp đến việc ghi dữ liệu vào user settings."
}
```

### 6.4. Kết quả bước 3: Đề xuất xử lý

```json
{
  "status": "SUCCESS",
  "solutions": [
    "Kiểm tra quyền truy cập thư mục cấu hình VS Code.",
    "Kiểm tra tệp settings.json có đang bị đặt chế độ chỉ đọc hay không.",
    "Đóng các tiến trình VS Code đang chạy rồi thử mở lại."
  ],
  "alternative": "Nếu vẫn gặp lỗi, kiểm tra quyền truy cập thư mục User trên hệ điều hành.",
  "warning": "Không xóa toàn bộ thư mục cấu hình khi chưa sao lưu dữ liệu."
}
```

### 6.5. Kết quả bước 4: Kiểm tra và hoàn thiện

```text
TRẠNG THÁI: ĐÃ PHÂN TÍCH

LỖI:
VS Code không thể ghi vào tệp cài đặt người dùng.

NGUYÊN NHÂN CÓ THỂ:
Thiếu quyền truy cập hoặc tệp cấu hình
đang ở chế độ chỉ đọc.

HƯỚNG KHẮC PHỤC:
1. Kiểm tra quyền truy cập thư mục cấu hình.
2. Kiểm tra thuộc tính của settings.json.
3. Đóng tiến trình VS Code và mở lại.

THÔNG TIN CẦN BỔ SUNG:
- Phiên bản VS Code.
- Hệ điều hành đang sử dụng.

LƯU Ý:
Nguyên nhân hiện tại mới là giả thuyết,
cần kiểm tra thêm để xác định chính xác.
```

## 7. KIỂM TRA KHẢ NĂNG XỬ LÝ DỮ LIỆU LỖI

**Trường hợp thử nghiệm:** Người dùng chỉ nhập:

```text
VS Code bị lỗi.
```

**Kết quả mong đợi:**

```json
{
  "status": "NEED_INFO",
  "message": "Thông tin chưa đủ để xác định lỗi.",
  "missing_info": [
    "Thông báo lỗi cụ thể",
    "Thời điểm xảy ra lỗi",
    "Hệ điều hành đang sử dụng"
  ]
}
```

Hệ thống không tự suy đoán nguyên nhân mà yêu cầu người dùng bổ sung thông tin. Nhờ đó, dữ liệu không đầy đủ không làm gián đoạn toàn bộ chuỗi xử lý.

## 8. ĐÁNH GIÁ KẾT QUẢ

- Các bước được thực hiện theo đúng thứ tự.
- Dữ liệu đầu ra của bước trước được sử dụng cho bước tiếp theo.
- AI có thể phân tích, phân loại và đề xuất hướng xử lý.
- Có cơ chế phát hiện dữ liệu thiếu và sai định dạng.
- Kết quả cuối cùng được kiểm tra trước khi trả về người dùng.