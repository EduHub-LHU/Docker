# Docker
# 🐳 Hướng Dẫn Sử Dụng Docker Cho Windows (.NET)

## 1. 📌 Docker Là Gì? Dùng Để Làm Gì?

**Docker** là một nền tảng giúp đóng gói ứng dụng và các thành phần phụ thuộc (database, service, config...) vào một môi trường gọi là **container**. 

### ✅ Lợi ích:
- Chạy ứng dụng nhất quán trên mọi máy (dev, test, production)
- Không cần cài từng phần mềm riêng lẻ
- Dễ deploy, dễ scale
- Nhẹ hơn máy ảo (VM)

### 📦 Ví dụ:
- Thay vì cài .NET SDK, SQL Server, Redis,... → chỉ cần chạy `docker-compose up` là có tất cả trong container.

---

## 2. 🧰 Cách Setup Docker Trên Windows (WSL2)

### Bước 1: Cài Docker Desktop
- Tải tại: https://www.docker.com/products/docker-desktop/
- Cài đặt như phần mềm bình thường
- Chọn **WSL 2 backend** trong quá trình cài

### Bước 2: Cài WSL + Ubuntu (nếu chưa có)
```powershell
wsl --install -d Ubuntu
```
Hoặc cài từ Microsoft Store → tìm **"Ubuntu"**

### Bước 3: Kiểm tra Docker hoạt động
```bash
docker --version
docker run hello-world
```
## 3. 🧠 Các Khái Niệm Cần Biết Về Docker

| Khái niệm            | Giải thích                                                                                                                                       |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| **Image**            | Một bản mẫu chỉ đọc (read-only) dùng để tạo container. Nó chứa tất cả mã nguồn, thư viện và thiết lập cần thiết để ứng dụng chạy được.            |
| **Container**        | Là một phiên bản đang chạy (instance) của image. Container hoạt động độc lập, nhẹ, nhanh và có thể tạo/sửa/xóa dễ dàng.                        |
| **Dockerfile**       | Là tập tin cấu hình (dạng script) dùng để định nghĩa cách tạo ra một image. Nó chứa các lệnh như `FROM`, `COPY`, `RUN`, `CMD`, v.v.             |
| **Volume**           | Cơ chế lưu trữ dữ liệu bên ngoài container. Volume giúp dữ liệu không bị mất khi container bị xóa hoặc tạo lại.                                 |
| **Bind Mount**       | Gắn trực tiếp một thư mục từ máy host vào container, phù hợp cho môi trường dev khi cần chỉnh sửa code theo thời gian thực.                     |
| **Network**          | Docker cung cấp các mạng ảo (bridge, host, overlay...) để các container có thể giao tiếp với nhau hoặc với máy chủ.                             |
| **Port Mapping**     | Cho phép ánh xạ cổng trong container (ví dụ: 80) ra cổng trên máy host (ví dụ: 8080), dùng cú pháp `-p 8080:80`.                                |
| **docker-compose**   | Là công cụ cho phép định nghĩa và chạy nhiều container cùng lúc thông qua file `docker-compose.yml`. Giúp orchestrate các dịch vụ phức tạp.     |
| **Registry**         | Nơi lưu trữ và phân phối các Docker image. Ví dụ: Docker Hub (mặc định), GitHub Container Registry, hoặc private registry của doanh nghiệp.     |
| **Tag**              | Phiên bản của image. Ví dụ: `nginx:1.25`, `mcr.microsoft.com/dotnet/sdk:7.0`. Nếu không chỉ rõ tag, Docker mặc định dùng `latest`.              |
| **Layers**           | Docker image được xây dựng từ nhiều lớp (layers), mỗi lệnh trong Dockerfile tạo ra 1 layer. Điều này giúp cache và tái sử dụng hiệu quả hơn.     |
| **Build Context**    | Là tập tin và thư mục được gửi vào Docker daemon trong quá trình build. Tất cả nội dung bên dưới thư mục hiện tại sẽ nằm trong build context.     |
| **Entrypoint / CMD** | Dùng để chỉ định chương trình chính chạy khi container khởi động. `CMD` có thể bị ghi đè khi chạy container, `ENTRYPOINT` thì không.             |
### ⚠️ Lưu Ý Khi Cài Docker Trên Windows

Docker trên Windows không chạy trực tiếp như trên Linux. Nó sử dụng một lớp trung gian – **WSL 2 (Windows Subsystem for Linux)** hoặc máy ảo Hyper-V – để tạo môi trường Linux bên dưới. Vì vậy cần lưu ý:

| Vấn đề / lưu ý                         | Mô tả                                                                                      |
|----------------------------------------|---------------------------------------------------------------------------------------------|
| **Cần bật WSL2**                       | Docker Desktop yêu cầu WSL2 hoặc Hyper-V để hoạt động. Bạn cần bật các tính năng này trong Windows Features. |
| **Chậm hơn Linux native**             | Do Docker trên Windows chạy trong lớp ảo hóa (WSL2), hiệu suất có thể thấp hơn so với chạy Docker trực tiếp trên Linux. |
| **File system khác biệt**             | Truy cập file từ Windows (`C:\Users\...`) bên trong WSL sẽ chậm hơn. Nên để project trong thư mục Linux nội bộ (`/home/...`). |
| **Networking khác nhau**              | Trong một số trường hợp, Docker trên Windows sẽ không chia sẻ port giống hệt Linux, cần kiểm tra `localhost` kỹ hơn. |
| **Permission và Volume Mapping**      | Một số permission (chmod, chown) có thể không hoạt động đúng nếu file nằm trên ổ Windows được mount. |
| **Docker Desktop chiếm RAM/CPU**     | Docker Desktop chạy nền sẽ chiếm RAM/CPU ngay cả khi không sử dụng. Có thể tắt autostart trong settings. |

🔸 Nếu bạn làm việc nhiều với Docker, có thể cân nhắc dùng **dual boot Linux** hoặc **máy ảo nhẹ (VM)** để có trải nghiệm sát với production hơn.
