# Tongji Contactless Palmprint Dataset

**Bộ dữ liệu ảnh lòng bàn tay không tiếp xúc (Tongji University, Trung Quốc)**

## 1. Giới thiệu

**Tongji Contactless Palmprint Dataset** là bộ dữ liệu sinh trắc học phục vụ nghiên cứu **nhận dạng lòng bàn tay không tiếp xúc** (*contactless palmprint recognition*). Dữ liệu được xây dựng bởi nhóm nghiên cứu tại **Đại học Tongji (Trung Quốc)** nhằm phục vụ các thử nghiệm nhận dạng sinh trắc học không tiếp xúc.

Bộ dữ liệu chứa ảnh bề mặt **lòng bàn tay** (các đường chỉ chính, nếp gấp và kết cấu da), **không phải** dữ liệu vân đầu ngón tay (*fingerprint*) hay ảnh tĩnh mạch lòng bàn tay (*palm vein*).

## 2. Quy mô và đặc điểm

| Thuộc tính | Thông tin |
|---|---|
| Đơn vị phát triển | Tongji University, Trung Quốc |
| Số người tham gia | **300** |
| Số lòng bàn tay | **600** (hai bàn tay của mỗi người) |
| Tổng số ảnh | **12.000** |
| Số phiên thu thập | **2** (`session1`, `session2`) |
| Số ảnh mỗi phiên | **6.000** |
| Số ảnh mỗi lòng bàn tay trong một phiên | **10** |
| Số ảnh mỗi lòng bàn tay trong cả hai phiên | **20** |
| Khoảng cách trung bình giữa hai phiên | **61 ngày** (nhỏ nhất 21 ngày; lớn nhất 106 ngày) |
| Kích thước ảnh gốc được mô tả | **600 × 800 pixel** (cần kiểm tra cách biểu diễn chiều rộng/chiều cao trên tệp thực tế) |
| Dữ liệu được phát hành | Ảnh gốc (**Original Images**) và ảnh vùng lòng bàn tay (**ROI Images**) |

**Lưu ý:** *600 lòng bàn tay* là **600 lớp mẫu lòng bàn tay**, không phải 600 người khác nhau.

## 3. Phương pháp thu nhận dữ liệu

Ảnh được thu bằng thiết bị không tiếp xúc do nhóm nghiên cứu phát triển, gồm camera CCD **JAI AD-80 GE**, thấu kính, nguồn sáng LED trắng dạng vòng và màn hình hướng dẫn người dùng. Khi chụp, người tham gia đưa bàn tay phía trên vùng thu nhận ảnh, duỗi tay tự nhiên để các khe ngón hiện rõ. Hình ảnh được chụp trên **nền tối** với điều kiện chiếu sáng được thiết kế tương đối ổn định.

Việc thu nhận trong hai phiên cách nhau nhiều ngày hỗ trợ đánh giá khả năng nhận dạng khi tư thế, kích thước hiển thị và tình trạng bề mặt bàn tay thay đổi giữa các lần chụp.

## 4. Cấu trúc thư mục và quy tắc gán nhãn

Dữ liệu được tổ chức theo hai phiên thu nhận. Ví dụ đối với bộ ảnh gốc đã giải nén:

```text
Original Images/
├── session1/
│   ├── 00001.<đuôi_ảnh>
│   ├── ...
│   └── 06000.<đuôi_ảnh>
└── session2/
    ├── 00001.<đuôi_ảnh>
    ├── ...
    └── 06000.<đuôi_ảnh>
```

Trong mỗi phiên:

| Dãy số trong tên ảnh | Nhãn lòng bàn tay (*Palm ID*) |
|---|---:|
| `00001`–`00010` | 1 |
| `00011`–`00020` | 2 |
| `00021`–`00030` | 3 |
| ... | ... |
| `05991`–`06000` | 600 |

Với số thứ tự `n` lấy từ tên ảnh (1 ≤ `n` ≤ 6000), nhãn một-based được suy ra bằng:

```text
Palm_ID = floor((n - 1) / 10) + 1
```

**Hai tệp có cùng số thứ tự ở `session1` và `session2` thuộc cùng một lòng bàn tay**, nhưng được chụp trong hai phiên khác nhau. Bộ dữ liệu không nhất thiết có tệp nhãn CSV/JSON riêng vì có thể xác định nhãn dựa trên quy tắc đặt tên tệp.

Quy tắc trên dùng để suy ra **Palm ID**. Không tự suy ra **Person ID** hoặc cặp tay trái/phải của cùng một người nếu chưa có ánh xạ được xác nhận từ nguồn dữ liệu.

## 5. Ảnh gốc và ảnh ROI

- **Original Images:** Ảnh bàn tay đầy đủ, có ngón tay và nền ảnh. Thích hợp cho nghiên cứu phân đoạn bàn tay, tìm mốc hình học và trích xuất vùng quan tâm.
- **ROI Images:** Ảnh vùng lòng bàn tay (*Region of Interest*) đã được nhóm tác giả cắt và chuẩn hóa bằng thuật toán của họ. Thích hợp cho nghiên cứu trích xuất đặc trưng và nhận dạng.

Mỗi ảnh gốc có **một ảnh ROI tương ứng** trong bộ dữ liệu được phát hành. Hai dạng ảnh này là **hai biểu diễn của cùng một mẫu**, không phải hai bộ mẫu độc lập.

## 6. Bài toán nghiên cứu mà dữ liệu hỗ trợ

Bộ dữ liệu phù hợp với **nhận dạng lòng bàn tay 1:N** (*identification*): từ một ảnh truy vấn, xác định mẫu thuộc lòng bàn tay nào trong tập các lòng bàn tay đã đăng ký. Với cách coi mỗi lòng bàn tay là một lớp, đây là bài toán **phân loại nhiều lớp (600 lớp)**.

Dữ liệu cũng có thể được sử dụng cho **xác minh 1:1** (*verification*), tức so sánh xem hai ảnh có thuộc cùng một lòng bàn tay hay không. Hai bài toán này cần giao thức đánh giá và chỉ số riêng.

Một cách tổ chức thử nghiệm phù hợp là dùng ảnh của **phiên thứ nhất** làm tập mẫu đăng ký (*gallery*) và ảnh của **phiên thứ hai** làm tập truy vấn (*probe*).

## 7. Đặc điểm và giới hạn cần lưu ý

- Ảnh không tiếp xúc có thể biến đổi về **vị trí, góc xoay, tỷ lệ và tư thế các ngón tay**; do đó, trích xuất và căn chỉnh ROI là bước quan trọng.
- Hai phiên chụp tạo điều kiện đánh giá **sự biến thiên giữa các lần thu nhận**, thay vì chỉ kiểm tra ảnh trong cùng một phiên.
- Dữ liệu được thu bằng **thiết bị chuyên dụng và nền tối có kiểm soát**; không nên mặc định kết quả sẽ giữ nguyên khi chụp bằng điện thoại hoặc webcam trong môi trường bất kỳ.
- Dữ liệu là **thông tin sinh trắc học**; cần tuân thủ điều kiện sử dụng của nguồn phát hành và tránh công bố các bản sao dữ liệu cá nhân không cần thiết.

## 8. Liên kết truy cập và tải dữ liệu

**Trang giới thiệu bộ dữ liệu:**  
https://cslinzhang.github.io/ContactlessPalm/

**Ảnh gốc (Original Images):**  
https://drive.google.com/file/d/15hEsOm0fZKUHpFNChPSjwiRfMczxcnVQ/view

**Ảnh ROI (ROI Images):**  
https://drive.google.com/file/d/1KZCXi6zAk5mZ1nQHFdeYHboAII3DOjls/view
