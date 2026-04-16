# Dự án Multi-Maven Spring Boot - Clean Architecture & DDD

Dự án này là một ví dụ minh họa về việc áp dụng **Clean Architecture** và **Domain-Driven Design (DDD)** trong một ứng dụng Spring Boot sử dụng cấu trúc Multi-module Maven.

## 🏗 Kiến trúc dự án

Dự án được chia thành các module riêng biệt để đảm bảo tính độc lập và dễ bảo trì:

1.  **Domain**: Lớp trung tâm chứa các thực thể (Entities), Aggregate và các logic nghiệp vụ cốt lõi. Đây là nơi định nghĩa các Interface cho Repository và Service.
2.  **Service**: Lớp triển khai (Implementation) các logic nghiệp vụ. Nó điều phối dữ liệu giữa lớp Domain và các thành phần bên ngoài.
3.  **Controller**: Lớp Adapter xử lý các yêu cầu HTTP (REST API). Đây là cửa ngõ giao tiếp giữa máy khách và ứng dụng.
4.  **Application**: Chứa lớp Main và các cấu hình để khởi chạy ứng dụng Spring Boot.
5.  **AuthServer**: Module xử lý xác thực và bảo mật (Authentication & Authorization).

---

## 🚀 Luồng hoạt động (Execution Flow)

Khi một yêu cầu (Request) được gửi đến hệ thống, nó sẽ đi qua các lớp sau:

1.  **Client -> Controller**: Người dùng gửi request (ví dụ: `GET /customers`). Spring Boot định tuyến đến phương thức tương ứng trong `CustomerController`.
2.  **Controller -> Service**: Controller tiếp nhận tham số, log thông tin và gọi đến Service Interface (ví dụ: `CustomerService`).
3.  **Service -> Repository**: Bản triển khai `CustomerServiceImpl` thực hiện logic nghiệp vụ và gọi xuống `CustomerRepository` để tương tác với cơ sở dữ liệu.
4.  **Repository -> Database**: Spring Data JPA tự động thực thi các truy vấn SQL và ánh xạ kết quả vào các Entity trong lớp Domain.
5.  **Response**: Dữ liệu từ Domain được trả ngược lại Service -> Controller. Cuối cùng, Controller trả về JSON cho Client.

---

## 🛠 Công nghệ sử dụng

- **Java 11**
- **Spring Boot 2.3.1**
- **Maven** (Multi-module)
- **Spring Data JPA** (H2/Database persistence)
- **Logback** (Logging)

---

## 📂 Cấu trúc thư mục chính

```text
├── Application (Main app & Config)
├── AuthServer (Security)
├── Controller (REST Endpoints)
├── Domain (Core Logic, Entities, Repo Interfaces)
├── Service (Business Implementation)
└── pom.xml (Parent POM)
```

---

## 📖 Cách chạy dự án

1. Đảm bảo bạn đã cài đặt JDK 11 và Maven.
2. Build toàn bộ dự án từ thư mục gốc:
   ```bash
   mvn clean install
   ```
3. Chạy ứng dụng từ module `Application`:
   ```bash
   mvn spring-boot:run -pl Application
   ```

---

## 🛡 Đặc điểm Clean Architecture
- **Độc lập Framework**: Mặc dù có phụ thuộc vào Spring JPA trong Domain (theo hướng thực dụng), nhưng cấu trúc tổng thể giúp bạn dễ dàng thay đổi các thành phần giao diện hoặc DB mà không ảnh hưởng đến logic lõi.
- **Dễ kiểm thử**: Các logic trong Domain và Service có thể được Unit Test một cách độc lập.
- **Tường minh**: Mỗi module có một trách nhiệm duy nhất.
