# Service Boundary của nhóm

## 1. Thông tin nhóm

- Tên nhóm: Đinh Ngọc Sơn
- Lớp: CNNTT17-10
- Thành viên: Đinh Ngọc Sơn, Hoàng Tuấn Hải, Đinh Thế Mạnh
- Service nhóm phụ trách: Camera stream
- Sản phẩm tổng thể của lớp: Hệ thống giám sát camera thông minh với các dịch vụ microservices

## 2. Actor

Ai tương tác với hệ thống/service?

- Người dùng cuối (End User): Xem luồng video từ camera
- Quản trị viên (Administrator): Quản lý camera, cấu hình stream
- Hệ thống giám sát (Monitoring System): Nhận dữ liệu stream để xử lý

## 3. System Boundary

Nhóm em xây phần nào?

Phần nhóm kiểm soát:

- Dịch vụ Camera Stream: Xử lý và cung cấp luồng video từ camera
- API Gateway cho camera
- Cơ sở dữ liệu metadata của camera

Phần nhóm chỉ tích hợp:

- Dịch vụ xác thực người dùng
- Dịch vụ lưu trữ video dài hạn
- Dịch vụ phân tích AI

## 4. Service Boundary

Service của nhóm có trách nhiệm gì?

- Nhận và xử lý luồng video từ camera vật lý
- Cung cấp API để truy cập luồng video theo thời gian thực
- Quản lý metadata của camera (vị trí, trạng thái, cấu hình)
- Đảm bảo bảo mật và xác thực cho truy cập stream

Service KHÔNG làm gì?

- Không xử lý phân tích video (AI detection)
- Không lưu trữ video dài hạn
- Không quản lý camera vật lý (hardware)

## 5. Input / Output

### Input

- Dữ liệu video từ camera (RTSP stream)
- Yêu cầu truy cập stream từ client
- Thông tin cấu hình camera
- Token xác thực

### Output

- Luồng video HLS/MPEG-DASH
- Metadata camera (JSON)
- Trạng thái kết nối camera
- Log hoạt động

## 6. API dự kiến

| Method | Endpoint | Mục đích |
|---|---|---|
| GET | /health | Kiểm tra trạng thái service |
| GET | /cameras | Lấy danh sách camera |
| GET | /cameras/{id}/stream | Lấy luồng video của camera |
| POST | /cameras | Thêm camera mới |
| PUT | /cameras/{id} | Cập nhật thông tin camera |
| DELETE | /cameras/{id} | Xóa camera |

## 7. Phụ thuộc service khác

Service này gọi đến service nào?

- Authentication Service: Để xác thực người dùng
- Database Service: Để lưu metadata camera

Service nào gọi đến service này?

- Frontend Web App: Để hiển thị video
- Monitoring Service: Để theo dõi trạng thái camera
- Analytics Service: Để nhận dữ liệu video cho phân tích

## 8. Sơ đồ minh họa

Có thể vẽ bằng Mermaid, draw.io, Ludichart hoặc ảnh chụp sơ đồ.
https://lucid.app/lucidchart/91e13ba9-91da-4c17-a93a-1ceeb7b76d40/edit?viewport_loc=-1588%2C-379%2C3488%2C1911%2C0_0&invitationId=inv_5f6f8537-e8d1-49b4-9e63-27ef05970c36
```mermaid
flowchart LR
    User[Actor] --> Service[Service của nhóm]
    Service --> DB[(Database)]
    Service --> Other[Service khác]
