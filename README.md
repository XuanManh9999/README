# 📘 MÔ TẢ DỰ ÁN CHI TIẾT - HỆ THỐNG CỔNG THÔNG TIN ĐIỆN TỬ

## 📋 MỤC LỤC

1. [Tổng quan dự án](#1-tổng-quan-dự-án)
2. [Kiến trúc hệ thống](#2-kiến-trúc-hệ-thống)
3. [Các thành phần chính](#3-các-thành-phần-chính)
4. [Tính năng chi tiết](#4-tính-năng-chi-tiết)
5. [Công nghệ sử dụng](#5-công-nghệ-sử-dụng)
6. [Quy trình làm việc](#6-quy-trình-làm-việc)
7. [Cấu trúc dữ liệu](#7-cấu-trúc-dữ-liệu)
8. [Hướng dẫn sử dụng cơ bản](#8-hướng-dẫn-sử-dụng-cơ-bản)

---

## 1. TỔNG QUAN DỰ ÁN

### 1.1. Dự án là gì?

**Hệ thống Cổng Thông Tin Điện Tử** là một hệ thống quản lý và xuất bản nội dung (CMS - Content Management System) được xây dựng cho các tổ chức, đặc biệt là các trường học, cơ quan nhà nước. Hệ thống cho phép:

- **Quản lý nội dung**: Tạo, chỉnh sửa, xuất bản các bài viết, tin tức, văn bản
- **Hiển thị công khai**: Người dùng có thể xem nội dung trên website công khai
- **Quản trị nội dung**: Quản trị viên có thể quản lý toàn bộ nội dung qua hệ thống admin

### 1.2. Mục đích sử dụng

Hệ thống được thiết kế để:

1. **Xuất bản thông tin**: Đăng tải tin tức, sự kiện, thông báo của tổ chức
2. **Quản lý tài liệu**: Lưu trữ và chia sẻ văn bản, tài liệu, hướng dẫn
3. **Tương tác với người dùng**: Nhận phản hồi, góp ý từ người dùng
4. **Quản lý đa phương tiện**: Lưu trữ và hiển thị hình ảnh, video
5. **Cung cấp thông tin**: Giới thiệu về tổ chức, lịch sử, cơ cấu tổ chức

### 1.3. Đối tượng sử dụng

- **Người dùng cuối**: Xem thông tin trên website công khai (không cần đăng nhập)
- **Biên tập viên**: Tạo và chỉnh sửa nội dung
- **Người phê duyệt**: Duyệt và xuất bản nội dung
- **Quản trị viên**: Quản lý toàn bộ hệ thống, cấu hình website

---

## 2. KIẾN TRÚC HỆ THỐNG

### 2.1. Tổng quan kiến trúc

Hệ thống được xây dựng theo mô hình **3 tầng** (3-tier architecture):

```
┌─────────────────────────────────────────────────────────┐
│                    FRONTEND LAYER                        │
│  (Giao diện người dùng - User Interface)                 │
├─────────────────────────────────────────────────────────┤
│  • Website công khai (Vue.js)                           │
│  • Hệ thống quản trị (Angular)                          │
└─────────────────────────────────────────────────────────┘
                          ↕
┌─────────────────────────────────────────────────────────┐
│                    BACKEND LAYER                         │
│  (Xử lý logic nghiệp vụ - Business Logic)                │
├─────────────────────────────────────────────────────────┤
│  • API Gateway (Điểm vào chính)                          │
│  • CMS API (Quản lý nội dung)                            │
│  • Identity API (Xác thực người dùng)                    │
│  • Identity Authentication (Xác thực OAuth)                │
└─────────────────────────────────────────────────────────┘
                          ↕
┌─────────────────────────────────────────────────────────┐
│                    DATABASE LAYER                        │
│  (Lưu trữ dữ liệu - Data Storage)                       │
├─────────────────────────────────────────────────────────┤
│  • SQL Server Database                                   │
│  • Lưu trữ nội dung, người dùng, cấu hình...            │
└─────────────────────────────────────────────────────────┘
```

### 2.2. Luồng hoạt động

**Khi người dùng truy cập website:**

1. Người dùng mở trình duyệt → Truy cập website công khai (Vue Frontend)
2. Frontend gửi yêu cầu → API Gateway
3. API Gateway định tuyến → CMS API hoặc Identity API
4. Backend xử lý → Truy vấn Database
5. Database trả dữ liệu → Backend xử lý và trả về
6. API Gateway trả kết quả → Frontend hiển thị cho người dùng

**Khi quản trị viên đăng nhập:**

1. Quản trị viên truy cập → Hệ thống quản trị (Angular CMS)
2. Hệ thống yêu cầu đăng nhập → Identity Authentication
3. Sau khi đăng nhập → Nhận token xác thực
4. Sử dụng token → Gọi các API để quản lý nội dung
5. Thực hiện thao tác → Tạo, sửa, xóa nội dung

---

## 3. CÁC THÀNH PHẦN CHÍNH

### 3.1. Frontend - Giao diện người dùng

#### 3.1.1. Website Công Khai (Vue.js)

**Vị trí**: `congthongtindientu/`

**Mô tả**: Website công khai cho phép mọi người xem thông tin mà không cần đăng nhập.

**Tính năng chính**:

- Hiển thị trang chủ với các tin tức mới nhất
- Xem danh sách bài viết, tin tức theo danh mục
- Xem chi tiết bài viết, văn bản, tài liệu
- Xem album ảnh, video
- Tìm kiếm nội dung
- Xem lịch công tác, lịch giảng dạy
- Gửi phản hồi, góp ý
- Đa ngôn ngữ (Tiếng Việt, Tiếng Anh)

**Các trang chính**:

- Trang chủ
- Giới thiệu (Lịch sử phát triển, Tổ chức bộ máy, Chức năng nhiệm vụ)
- Tin tức & Sự kiện
- Đảng và Đoàn thể
- Hợp tác quốc tế
- Khoa học - Thông tin - Tư liệu
- Đào tạo - Bồi dưỡng
- Nghiên cứu - Trao đổi
- Tài liệu
- Album ảnh/video
- Lịch công tác
- Liên hệ

#### 3.1.2. Hệ Thống Quản Trị (Angular CMS)

**Vị trí**: `congthongtindientu-cms/`

**Mô tả**: Hệ thống quản trị cho phép quản trị viên và biên tập viên quản lý nội dung.

**Tính năng chính**:

- Đăng nhập/đăng xuất
- Quản lý bài viết, tin tức
- Quản lý văn bản, tài liệu
- Quản lý đa phương tiện (ảnh, video)
- Quản lý cấu trúc website (menu, footer, trang)
- Quản lý người dùng
- Quy trình phê duyệt bài viết
- Quản lý phản hồi từ người dùng

**Các module chính**:

1. **Quản trị Cấu trúc website**

   - Quản lý Menu (Danh mục chức năng)
   - Quản lý Footer (Chân trang)
   - Quản lý Trang (Page)
   - Quản lý Khối nội dung (Section)
   - Quản lý Danh mục tiện ích

2. **Quản lý Thông tin & hỗ trợ**

   - Quản lý Thông tin trường
   - Quản lý Tài liệu hướng dẫn
   - Quản lý Liên kết website

3. **Quản lý Bài viết**

   - Quản lý Chuyên mục tin
   - Quản lý Tin bài
   - Quản lý Quy trình xuất bản

4. **Quản lý Nội dung số**

   - Quản lý đa phương tiện (ảnh, video)
   - Quản lý RSS
   - Quản lý Văn bản

5. **Tương tác & phản hồi**
   - Quản lý Hòm thư góp ý

### 3.2. Backend - Xử lý logic nghiệp vụ

#### 3.2.1. API Gateway

**Vị trí**: `infoportal/Services/LHP.Gateway/`

**Mô tả**: Điểm vào chính của hệ thống, định tuyến các yêu cầu đến các service phù hợp.

**Chức năng**:

- Nhận tất cả yêu cầu từ frontend
- Định tuyến đến service phù hợp (CMS API, Identity API, ...)
- Xử lý xác thực
- Tổng hợp Swagger documentation

**Port**: 5200

#### 3.2.2. CMS API

**Vị trí**: `infoportal/Services/LHP.Cms/`

**Mô tả**: Service chính xử lý tất cả logic liên quan đến quản lý nội dung.

**Chức năng**:

- Quản lý bài viết, tin tức
- Quản lý văn bản, tài liệu
- Quản lý đa phương tiện
- Quản lý cấu trúc website
- Quản lý phản hồi
- Quy trình phê duyệt

**Port**: 5203

**Các Controller chính**:

- `BlogController`: Quản lý bài viết
- `ArticleController`: Quản lý nội dung trang
- `DocumentController`: Quản lý văn bản, tài liệu
- `AlbumController`: Quản lý album ảnh/video
- `MenuController`: Quản lý menu
- `PageController`: Quản lý trang
- `SectionController`: Quản lý khối nội dung
- `CategoryController`: Quản lý danh mục
- `CommentController`: Quản lý bình luận
- `FeedbackMailboxController`: Quản lý phản hồi
- `RssFeedController`: Quản lý RSS feed

#### 3.2.3. Identity API

**Vị trí**: `infoportal/Services/LHP.Identity/LHP.Identity.Api/`

**Mô tả**: Service quản lý người dùng và phân quyền.

**Chức năng**:

- Quản lý tài khoản người dùng
- Quản lý vai trò (roles) và quyền (permissions)
- Xác thực người dùng
- Quản lý thông tin cá nhân

**Port**: 5202

#### 3.2.4. Identity Authentication

**Vị trí**: `infoportal/Services/LHP.Identity/LHP.Identity.Authentication/`

**Mô tả**: Service xác thực sử dụng OAuth 2.0 / OpenID Connect.

**Chức năng**:

- Xác thực đăng nhập
- Cấp token (JWT) cho người dùng
- Quản lý session
- Single Sign-On (SSO)

**Port**: 5213

### 3.3. Database - Lưu trữ dữ liệu

**Công nghệ**: SQL Server

**Các database chính**:

1. **ag.env-dev.cms**: Database chính lưu trữ nội dung

   - Bài viết, tin tức
   - Văn bản, tài liệu
   - Album ảnh/video
   - Cấu trúc website (menu, footer, trang, section)
   - Phản hồi, bình luận

2. **lhp-dev.user-service**: Database quản lý người dùng

   - Tài khoản người dùng
   - Vai trò và quyền
   - Thông tin cá nhân

3. **ag.env-dev.user-service**: Database cho Identity Server
   - Cấu hình OAuth clients
   - Tokens và sessions

---

## 4. TÍNH NĂNG CHI TIẾT

### 4.1. Quản lý Bài viết và Tin tức

#### 4.1.1. Tạo bài viết

**Quy trình**:

1. Biên tập viên đăng nhập vào hệ thống quản trị
2. Chọn "Quản lý Bài viết" → "Quản lý Tin bài"
3. Click "Tạo mới"
4. Điền thông tin:
   - Tiêu đề
   - Chọn chuyên mục
   - Nội dung (sử dụng rich text editor)
   - Ảnh đại diện
   - File đính kèm (nếu có)
5. Chọn trạng thái:
   - **Nháp**: Lưu để chỉnh sửa sau
   - **Chờ duyệt**: Gửi để người phê duyệt xem xét
6. Lưu bài viết

#### 4.1.2. Quy trình phê duyệt

**Các trạng thái bài viết**:

- **Nháp (Draft)**: Bài viết đang được soạn thảo
- **Chờ duyệt (Pending)**: Đã gửi để phê duyệt
- **Đã duyệt (Approved)**: Đã được phê duyệt, có thể xuất bản
- **Từ chối (Rejected)**: Bị từ chối, cần chỉnh sửa
- **Chỉnh sửa (Edit)**: Đang chỉnh sửa sau khi bị từ chối

**Quy trình**:

1. Biên tập viên tạo bài viết → Chọn "Chờ duyệt"
2. Người phê duyệt xem danh sách bài viết chờ duyệt
3. Người phê duyệt xem chi tiết và quyết định:
   - **Duyệt**: Bài viết chuyển sang trạng thái "Đã duyệt"
   - **Từ chối**: Bài viết chuyển sang trạng thái "Từ chối", kèm lý do
4. Biên tập viên nhận thông báo và chỉnh sửa (nếu bị từ chối)
5. Sau khi duyệt, bài viết có thể được xuất bản lên website công khai

#### 4.1.3. Quản lý chuyên mục

- Tạo, sửa, xóa chuyên mục tin tức
- Sắp xếp thứ tự hiển thị
- Cấu hình hiển thị trên website

### 4.2. Quản lý Văn bản và Tài liệu

#### 4.2.1. Phân loại văn bản

**Các loại văn bản**:

1. **Văn bản trường**: Văn bản nội bộ của tổ chức
2. **Văn bản quy phạm pháp luật**: Văn bản của Nhà nước
3. **Văn bản dự thảo**: Văn bản đang trong quá trình soạn thảo, cần góp ý

#### 4.2.2. Quản lý văn bản

**Tính năng**:

- Tải lên file văn bản (PDF, Word, Excel, ...)
- Phân loại theo loại văn bản
- Gắn thẻ, mô tả
- Tìm kiếm văn bản
- Tải xuống văn bản

#### 4.2.3. Góp ý dự thảo

**Quy trình**:

1. Quản trị viên tải lên văn bản dự thảo
2. Văn bản được hiển thị trên website công khai
3. Người dùng có thể xem và gửi góp ý
4. Quản trị viên xem và xử lý các góp ý

### 4.3. Quản lý Đa phương tiện

#### 4.3.1. Album ảnh

**Tính năng**:

- Tạo album ảnh theo chủ đề
- Tải lên nhiều ảnh cùng lúc
- Quản lý thông tin ảnh (tiêu đề, mô tả)
- Hiển thị dạng gallery trên website
- Xem ảnh dạng slideshow

#### 4.3.2. Album video

**Tính năng**:

- Tạo album video
- Tải lên video hoặc nhúng link YouTube/Vimeo
- Quản lý thông tin video
- Hiển thị danh sách video trên website

### 4.4. Quản lý Cấu trúc Website

#### 4.4.1. Quản lý Menu

**Tính năng**:

- Tạo menu đa cấp (menu chính, menu con)
- Sắp xếp thứ tự hiển thị
- Liên kết đến trang hoặc URL bên ngoài
- Hiển thị/ẩn menu
- Quản lý menu theo ngôn ngữ

#### 4.4.2. Quản lý Footer

**Tính năng**:

- Cấu hình nội dung chân trang
- Thêm thông tin liên hệ
- Thêm liên kết hữu ích
- Quản lý theo cột

#### 4.4.3. Quản lý Trang (Page)

**Tính năng**:

- Tạo trang tĩnh (ví dụ: Giới thiệu, Liên hệ)
- Sử dụng rich text editor để soạn nội dung
- Cấu hình URL của trang
- Quản lý SEO (meta title, description)

#### 4.4.4. Quản lý Khối nội dung (Section)

**Tính năng**:

- Tạo các khối nội dung động trên trang
- Cấu hình layout (lưới, danh sách, carousel)
- Chọn nội dung hiển thị (bài viết, văn bản, ảnh, ...)
- Cấu hình số lượng hiển thị
- Sắp xếp thứ tự

**Các loại Section**:

- **DetailedHighlightWithSidebar**: Nội dung chính + sidebar
- **MixedHighlightList**: Danh sách bài viết theo danh mục
- **TableList**: Danh sách dạng bảng (văn bản, tài liệu)
- **MediaShowcase**: Trình diễn đa phương tiện (ảnh, video)

### 4.5. Quản lý Tương tác

#### 4.5.1. Hòm thư góp ý

**Tính năng**:

- Người dùng gửi phản hồi qua form trên website
- Quản trị viên xem danh sách phản hồi
- Trả lời phản hồi
- Đánh dấu đã xử lý

#### 4.5.2. Bình luận

**Tính năng**:

- Người dùng bình luận trên bài viết
- Quản trị viên quản lý bình luận
- Duyệt/ẩn bình luận
- Xóa bình luận không phù hợp

### 4.6. Tìm kiếm

**Tính năng**:

- Tìm kiếm bài viết, tin tức
- Tìm kiếm văn bản, tài liệu
- Lọc theo danh mục
- Lọc theo ngày tháng
- Hiển thị kết quả có phân trang

### 4.7. Đa ngôn ngữ

**Tính năng**:

- Hỗ trợ Tiếng Việt và Tiếng Anh
- Chuyển đổi ngôn ngữ dễ dàng
- Quản lý nội dung theo từng ngôn ngữ

### 4.8. RSS Feed

**Tính năng**:

- Tạo RSS feed cho các danh mục
- Người dùng đăng ký RSS để nhận tin mới
- Tích hợp với các RSS reader

---

## 5. CÔNG NGHỆ SỬ DỤNG

### 5.1. Frontend

#### 5.1.1. Vue.js 3 (Website công khai)

**Tại sao chọn Vue.js?**

- Dễ học, dễ sử dụng
- Hiệu suất cao
- Cộng đồng lớn
- Phù hợp cho website công khai

**Các thư viện chính**:

- **Vue Router**: Điều hướng giữa các trang
- **Vue I18n**: Hỗ trợ đa ngôn ngữ
- **Axios**: Gọi API
- **Bootstrap 5**: Framework CSS cho giao diện

#### 5.1.2. Angular 20 (Hệ thống quản trị)

**Tại sao chọn Angular?**

- Framework mạnh mẽ, phù hợp cho ứng dụng phức tạp
- TypeScript - an toàn kiểu dữ liệu
- Có sẵn nhiều component
- Phù hợp cho hệ thống quản trị

**Các thư viện chính**:

- **NG-ZORRO**: Component library
- **Angular Material**: Material Design components
- **NgxEditor**: Rich text editor
- **Angular Auth OIDC Client**: Xác thực OAuth

### 5.2. Backend

#### 5.2.1. .NET 7 / C#

**Tại sao chọn .NET?**

- Hiệu suất cao
- An toàn, bảo mật tốt
- Hỗ trợ tốt cho enterprise applications
- Cộng đồng lớn

**Các công nghệ chính**:

- **ASP.NET Core**: Framework web
- **Entity Framework Core**: ORM (Object-Relational Mapping) - làm việc với database
- **MediatR**: Pattern CQRS (Command Query Responsibility Segregation)
- **AutoMapper**: Mapping giữa các object
- **Identity Server 4**: Xác thực OAuth/OpenID Connect
- **Ocelot**: API Gateway
- **MassTransit**: Message queue (RabbitMQ)

#### 5.2.2. Kiến trúc Backend

**Domain-Driven Design (DDD)**:

- Tổ chức code theo domain (lĩnh vực nghiệp vụ)
- Dễ bảo trì và mở rộng

**CQRS Pattern**:

- Tách biệt Command (thao tác ghi) và Query (thao tác đọc)
- Tối ưu hiệu suất

**Repository Pattern**:

- Tách biệt logic truy cập database
- Dễ test và bảo trì

### 5.3. Database

**SQL Server**:

- Database quan hệ mạnh mẽ
- Hỗ trợ tốt cho enterprise
- Bảo mật cao
- Có sẵn nhiều công cụ quản lý

### 5.4. Công cụ phát triển

- **Vite**: Build tool cho Vue.js (nhanh hơn Webpack)
- **Angular CLI**: Công cụ phát triển Angular
- **Entity Framework Migrations**: Quản lý thay đổi database
- **Swagger**: Tài liệu API tự động

---

## 6. QUY TRÌNH LÀM VIỆC

### 6.1. Quy trình xuất bản bài viết

```
┌─────────────┐
│ Biên tập    │
│ viên tạo    │
│ bài viết    │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ Lưu nháp    │
│ (Draft)     │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ Gửi duyệt   │
│ (Pending)   │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ Người phê   │
│ duyệt xem   │
│ xét         │
└──────┬──────┘
       │
   ┌───┴───┐
   │       │
   ▼       ▼
┌─────┐ ┌─────┐
│Duyệt│ │Từ  │
│     │ │chối│
└──┬──┘ └──┬─┘
   │       │
   ▼       ▼
┌─────┐ ┌─────┐
│Đã   │ │Chỉnh│
│duyệt│ │sửa  │
└──┬──┘ └──┬──┘
   │       │
   │       └───┐
   │           │
   ▼           ▼
┌─────────────┐
│ Xuất bản    │
│ lên website │
└─────────────┘
```

### 6.2. Quy trình quản lý văn bản

1. **Tải lên văn bản**

   - Quản trị viên tải file lên hệ thống
   - Điền thông tin (tên, loại, mô tả)
   - Phân loại theo danh mục

2. **Hiển thị trên website**

   - Văn bản được hiển thị trong danh sách
   - Người dùng có thể tìm kiếm, lọc
   - Người dùng có thể tải xuống

3. **Góp ý (nếu là văn bản dự thảo)**
   - Người dùng xem và gửi góp ý
   - Quản trị viên xem và xử lý góp ý

### 6.3. Quy trình quản lý phản hồi

1. Người dùng gửi phản hồi qua form trên website
2. Phản hồi được lưu vào database
3. Quản trị viên xem danh sách phản hồi trong hệ thống quản trị
4. Quản trị viên trả lời hoặc xử lý phản hồi
5. Đánh dấu đã xử lý

---

## 7. CẤU TRÚC DỮ LIỆU

### 7.1. Các loại nội dung chính

#### 7.1.1. Blog (Bài viết/Tin tức)

**Thông tin lưu trữ**:

- Tiêu đề
- Nội dung (HTML)
- Ảnh đại diện
- Chuyên mục
- Trạng thái (Nháp, Chờ duyệt, Đã duyệt, Từ chối)
- Người tạo, ngày tạo
- Người duyệt, ngày duyệt
- File đính kèm
- Tags, mô tả ngắn

#### 7.1.2. Article (Nội dung trang)

**Thông tin lưu trữ**:

- Tiêu đề
- Mã trang (Code)
- Nội dung
- Danh mục
- File đính kèm

#### 7.1.3. Document (Văn bản/Tài liệu)

**Thông tin lưu trữ**:

- Tên văn bản
- Loại văn bản (Văn bản trường, Văn bản quy phạm, Văn bản dự thảo)
- File đính kèm
- Số hiệu, ngày ban hành
- Cơ quan ban hành
- Mô tả
- Trạng thái

#### 7.1.4. Album (Album ảnh/video)

**Thông tin lưu trữ**:

- Tên album
- Mô tả
- Danh sách ảnh/video
- Thông tin từng ảnh/video (tên, mô tả, file)

### 7.2. Cấu trúc website

#### 7.2.1. Menu

**Thông tin lưu trữ**:

- Tên menu
- Link (URL hoặc Page ID)
- Thứ tự hiển thị
- Menu cha (để tạo menu đa cấp)
- Hiển thị/ẩn

#### 7.2.2. Page (Trang)

**Thông tin lưu trữ**:

- Tên trang
- URL
- Nội dung
- Các Section (khối nội dung) trên trang

#### 7.2.3. Section (Khối nội dung)

**Thông tin lưu trữ**:

- Tên section
- Loại layout
- Cấu hình nội dung (chọn bài viết, văn bản, ...)
- Số lượng hiển thị
- Thứ tự

#### 7.2.4. Footer

**Thông tin lưu trữ**:

- Nội dung các cột
- Thông tin liên hệ
- Liên kết

### 7.3. Người dùng và phân quyền

#### 7.3.1. User (Người dùng)

**Thông tin lưu trữ**:

- Tên đăng nhập
- Mật khẩu (đã mã hóa)
- Email
- Họ tên
- Vai trò (Role)
- Quyền (Permissions)

#### 7.3.2. Role (Vai trò)

**Các vai trò chính**:

- **Administrator**: Quản trị viên - toàn quyền
- **Editor**: Biên tập viên - tạo, sửa bài viết
- **Approver**: Người phê duyệt - duyệt bài viết
- **Viewer**: Người xem - chỉ xem

#### 7.3.3. Permission (Quyền)

**Các quyền chính**:

- View: Xem
- Add: Thêm mới
- Update: Chỉnh sửa
- Delete: Xóa
- Approve: Phê duyệt
- Reject: Từ chối

---

## 8. HƯỚNG DẪN SỬ DỤNG CƠ BẢN

### 8.1. Cho người dùng cuối (Xem website)

1. **Truy cập website**: Mở trình duyệt, vào địa chỉ website
2. **Xem tin tức**: Click vào "Tin tức & Sự kiện"
3. **Xem chi tiết**: Click vào tiêu đề bài viết
4. **Tìm kiếm**: Sử dụng ô tìm kiếm ở header
5. **Xem album**: Click vào "Album ảnh" hoặc "Album video"
6. **Gửi phản hồi**: Click "Liên hệ" và điền form

### 8.2. Cho biên tập viên

1. **Đăng nhập**: Vào hệ thống quản trị, nhập tên đăng nhập và mật khẩu
2. **Tạo bài viết mới**:
   - Vào "Quản lý Bài viết" → "Quản lý Tin bài"
   - Click "Tạo mới"
   - Điền thông tin và lưu
3. **Gửi duyệt**: Chọn bài viết, chọn trạng thái "Chờ duyệt"
4. **Chỉnh sửa**: Chọn bài viết, click "Sửa"

### 8.3. Cho người phê duyệt

1. **Đăng nhập**: Vào hệ thống quản trị
2. **Xem bài viết chờ duyệt**:
   - Vào "Quản lý Bài viết" → "Quản lý Quy trình xuất bản"
   - Xem danh sách bài viết chờ duyệt
3. **Duyệt/Từ chối**:
   - Click vào bài viết để xem chi tiết
   - Click "Duyệt" hoặc "Từ chối"
   - Nếu từ chối, nhập lý do

### 8.4. Cho quản trị viên

1. **Quản lý cấu trúc website**:

   - Vào "Quản trị Cấu trúc website"
   - Quản lý Menu, Footer, Trang, Section

2. **Quản lý nội dung**:

   - Quản lý bài viết, văn bản, đa phương tiện
   - Xem và xử lý phản hồi

3. **Quản lý người dùng**:
   - Vào "Quản lý người dùng"
   - Tạo, sửa, xóa tài khoản
   - Phân quyền

---

## 9. BẢO MẬT

### 9.1. Xác thực người dùng

- **OAuth 2.0 / OpenID Connect**: Chuẩn xác thực hiện đại
- **JWT Token**: Token để xác thực các request
- **Mật khẩu mã hóa**: Mật khẩu được mã hóa trong database

### 9.2. Phân quyền

- **Role-based Access Control (RBAC)**: Phân quyền theo vai trò
- **Permission-based**: Kiểm soát quyền chi tiết đến từng chức năng
- **API Authorization**: Kiểm tra quyền ở tầng API

### 9.3. Bảo mật dữ liệu

- **HTTPS**: Mã hóa dữ liệu truyền tải
- **SQL Injection Protection**: Entity Framework tự động bảo vệ
- **XSS Protection**: Làm sạch dữ liệu đầu vào
- **CORS**: Kiểm soát truy cập từ domain khác

---

## 10. MỞ RỘNG VÀ TÍCH HỢP

### 10.1. API

Hệ thống cung cấp RESTful API để:

- Tích hợp với hệ thống khác
- Mobile app
- Third-party services

### 10.2. RSS Feed

- Tạo RSS feed cho các danh mục
- Người dùng đăng ký để nhận tin mới

### 10.3. Message Queue

- Sử dụng RabbitMQ để xử lý bất đồng bộ
- Gửi email thông báo
- Xử lý tác vụ nền

---

## 11. KẾT LUẬN

Hệ thống Cổng Thông Tin Điện Tử là một giải pháp toàn diện cho việc quản lý và xuất bản nội dung. Với kiến trúc hiện đại, tính năng phong phú và giao diện thân thiện, hệ thống đáp ứng nhu cầu của cả người dùng cuối và quản trị viên.

**Điểm mạnh**:

- ✅ Kiến trúc hiện đại, dễ mở rộng
- ✅ Tính năng phong phú, đáp ứng đầy đủ nhu cầu
- ✅ Giao diện thân thiện, dễ sử dụng
- ✅ Bảo mật cao
- ✅ Hiệu suất tốt
- ✅ Hỗ trợ đa ngôn ngữ

**Hướng phát triển**:

- 📱 Mobile app
- 🔍 Tìm kiếm nâng cao
- 📊 Analytics và báo cáo
- 🤖 AI hỗ trợ biên tập
- 🌐 Tích hợp mạng xã hội

---

## 12. 🔒 ĐÁNH GIÁ BẢO MẬT VÀ RỦI RO

### 12.1. Tổng quan đánh giá

**Mức độ rủi ro tổng thể**: ⚠️ **TRUNG BÌNH - CAO**

**Phạm vi đánh giá**:

- Mã nguồn Backend (.NET)
- Mã nguồn Frontend (Vue.js, Angular)
- Dependencies và thư viện
- Cấu hình bảo mật
- Xác thực và phân quyền

---

### 12.2. 🔴 RỦI RO NGHIÊM TRỌNG (CRITICAL)

#### 12.2.1. Hardcoded Credentials trong appsettings.json

**Vị trí**:

- `infoportal/Services/LHP.Cms/LHP.Cms.Api/appsettings.json`
- `infoportal/Services/LHP.Identity/LHP.Identity.Api/appsettings.json`
- `infoportal/Services/LHP.Identity/LHP.Identity.Authentication/appsettings.json`

**Vấn đề**:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=localhost\\SQLEXPRESS;Initial Catalog=ag.env-dev.cms;User Id=sa;Password=0328063079Manh;TrustServerCertificate=true;"
  },
  "Jwt": {
    "SecretKey": "JsonWebApiTokenWithSwaggerAuthorizationAuthenticationAspNetCore"
  },
  "Authentication": {
    "OpenId": {
      "ClientSecret": "test.fsel.swagger_secret"
    }
  }
}
```

**Mức độ rủi ro**: 🔴 **CRITICAL**

**Tác động**:

- Kẻ tấn công có thể truy cập database
- Có thể tạo JWT token giả mạo
- Có thể bypass authentication

**Khuyến nghị**:

- ✅ Sử dụng User Secrets cho development
- ✅ Sử dụng Azure Key Vault hoặc AWS Secrets Manager cho production
- ✅ Sử dụng Environment Variables
- ✅ Tạo JWT Secret Key mạnh (ít nhất 32 ký tự, random)
- ✅ Không commit file appsettings.json có chứa credentials

---

#### 12.2.2. CORS Configuration Quá Rộng

**Vị trí**:

- `infoportal/Services/LHP.Identity/LHP.Identity.Authentication/Program.cs`

**Vấn đề**:

```csharp
app.UseCors(options =>
{
    options.AllowAnyOrigin();
    options.AllowAnyHeader();
    options.AllowAnyMethod();
});
```

**Mức độ rủi ro**: 🔴 **CRITICAL** (trong Production)

**Tác động**:

- Website khác có thể gọi API từ trình duyệt người dùng
- Có thể bị tấn công CSRF
- Lộ thông tin nhạy cảm

**Khuyến nghị**:

- ✅ Chỉ cho phép các origin cụ thể:
  ```csharp
  options.WithOrigins("https://yourdomain.com", "https://www.yourdomain.com");
  ```
- ✅ Sử dụng `AllowCredentials()` chỉ khi cần thiết
- ✅ Cấu hình riêng cho từng môi trường

---

#### 12.2.3. JWT Secret Key Yếu

**Vị trí**: Tất cả file `appsettings.json`

**Vấn đề**:

```json
"Jwt": {
  "SecretKey": "JsonWebApiTokenWithSwaggerAuthorizationAuthenticationAspNetCore"
}
```

**Mức độ rủi ro**: 🔴 **CRITICAL**

**Mô tả**:

- Secret key quá ngắn và dễ đoán
- Không phải là random string
- Có thể bị brute force

**Khuyến nghị**:

- ✅ Tạo secret key mạnh (ít nhất 256 bits):
  ```csharp
  var key = new byte[32];
  using (var rng = RandomNumberGenerator.Create())
  {
      rng.GetBytes(key);
  }
  var secretKey = Convert.ToBase64String(key);
  ```
- ✅ Sử dụng RSA keys cho production
- ✅ Rotate keys định kỳ

---

#### 12.2.4. XSS Risk - Sử dụng innerHTML

**Vị trí**:

- `congthongtindientu/src/views/DetailArticleView.vue`
- `congthongtindientu/src/views/DetailCommentDraftView.vue`

**Vấn đề**:

```javascript
return el.body.innerHTML;
```

**Mức độ rủi ro**: 🔴 **CRITICAL**

**Mô tả**:

- Sử dụng `innerHTML` để render nội dung từ server
- Nếu nội dung không được sanitize, có thể bị XSS attack

**Tác động**:

- Kẻ tấn công có thể inject JavaScript độc hại
- Có thể đánh cắp session, cookies
- Có thể redirect người dùng đến website độc hại

**Khuyến nghị**:

- ✅ Sanitize HTML trước khi render:
  ```javascript
  import DOMPurify from "dompurify";
  return DOMPurify.sanitize(el.body.innerHTML);
  ```
- ✅ Sử dụng `v-html` với sanitization
- ✅ Validate và sanitize nội dung ở backend trước khi lưu

---

### 12.3. 🟠 RỦI RO CAO (HIGH)

#### 12.3.1. TrustServerCertificate = true

**Vị trí**: Connection strings trong `appsettings.json`

**Vấn đề**:

```json
"DefaultConnection": "...;TrustServerCertificate=true;"
```

**Mức độ rủi ro**: 🟠 **HIGH**

**Mô tả**:

- Bỏ qua việc xác thực SSL certificate của SQL Server
- Có thể bị tấn công Man-in-the-Middle

**Khuyến nghị**:

- ✅ Chỉ sử dụng trong development
- ✅ Trong production, sử dụng certificate hợp lệ
- ✅ Cấu hình SQL Server với SSL certificate

---

#### 12.3.2. AllowedHosts = "\*"

**Vị trí**: Tất cả file `appsettings.json`

**Vấn đề**:

```json
"AllowedHosts": "*"
```

**Mức độ rủi ro**: 🟠 **HIGH**

**Mô tả**:

- Cho phép tất cả host headers
- Có thể bị tấn công Host Header Injection

**Khuyến nghị**:

- ✅ Chỉ định các host cụ thể:
  ```json
  "AllowedHosts": "yourdomain.com;www.yourdomain.com"
  ```

---

#### 12.3.3. MaxRequestBodySize = long.MaxValue

**Vị trí**:

- `infoportal/Services/LHP.Gateway/LHP.Gateway.Api/Program.cs`

**Vấn đề**:

```csharp
options.Limits.MaxRequestBodySize = long.MaxValue;
x.MaxRequestBodySize = long.MaxValue;
```

**Mức độ rủi ro**: 🟠 **HIGH**

**Mô tả**:

- Không giới hạn kích thước request body
- Có thể bị tấn công DoS (Denial of Service)

**Khuyến nghị**:

- ✅ Đặt giới hạn hợp lý (ví dụ: 100MB):
  ```csharp
  options.Limits.MaxRequestBodySize = 100 * 1024 * 1024; // 100MB
  ```
- ✅ Validate file size trước khi upload
- ✅ Sử dụng rate limiting

---

#### 12.3.4. SQL Injection Risk - String Interpolation

**Vị trí**:

- `infoportal/Services/LHP.Identity/LHP.Identity.Application/Queries/UserQuery/ExecuteUsersQuery.cs`

**Vấn đề**:

```csharp
query = query.Where(p => !string.IsNullOrEmpty(p.FullName) &&
    EF.Functions.Like(p.FullName, $"%{request.Keyword}%"));
```

**Mức độ rủi ro**: 🟠 **HIGH** (nếu không được parameterized đúng cách)

**Mô tả**:

- Sử dụng string interpolation trong EF.Functions.Like
- Entity Framework thường parameterize tự động, nhưng cần kiểm tra kỹ

**Khuyến nghị**:

- ✅ Kiểm tra SQL được generate có parameterized không
- ✅ Validate và sanitize input trước khi query
- ✅ Sử dụng parameterized queries rõ ràng

**Lưu ý**: Entity Framework Core thường tự động parameterize, nhưng cần verify trong production.

---

### 12.4. 🟡 RỦI RO TRUNG BÌNH (MEDIUM)

#### 12.4.1. Sentry DSN Exposed

**Vị trí**:

- `infoportal/Services/LHP.Cms/LHP.Cms.Api/appsettings.json`

**Vấn đề**:

```json
"SentryConfig": {
  "Dsn": "https://3c246735e89d4aebfe43ad11ff86ca2e@o4506116054384640.ingest.sentry.io/4506116055302144"
}
```

**Mức độ rủi ro**: 🟡 **MEDIUM**

**Mô tả**:

- Sentry DSN có thể bị lộ
- Kẻ tấn công có thể gửi error logs giả mạo

**Khuyến nghị**:

- ✅ Rotate DSN nếu bị lộ
- ✅ Sử dụng environment variables
- ✅ Giới hạn IP có thể gửi logs

---

#### 12.4.2. Swagger Exposed trong Production

**Mức độ rủi ro**: 🟡 **MEDIUM** (nếu cấu hình sai)

**Mô tả**:

- Swagger UI có thể bị expose nếu cấu hình sai
- Có thể lộ thông tin về API structure

**Khuyến nghị**:

- ✅ Đảm bảo Swagger chỉ enable trong development/staging
- ✅ Sử dụng authentication cho Swagger trong staging
- ✅ Disable hoàn toàn trong production

---

#### 12.4.3. Thiếu Input Validation ở một số nơi

**Mô tả**:

- Một số input không được validate đầy đủ
- Cần kiểm tra kỹ hơn các endpoint nhận input từ user

**Khuyến nghị**:

- ✅ Sử dụng Data Annotations cho validation
- ✅ Implement custom validators
- ✅ Validate ở cả client và server side
- ✅ Sanitize HTML input (đặc biệt cho rich text editor)

---

### 12.5. RỦI RO BẢO MẬT TRONG THƯ VIỆN

#### 12.5.1. Đánh giá Dependencies Frontend (Vue.js)

**File**: `congthongtindientu/package.json`

**✅ Thư viện an toàn**:

- `vue@^3.5.17` - ✅ Phiên bản mới, an toàn
- `vue-router@^4.5.1` - ✅ Phiên bản mới
- `axios@^1.10.0` - ✅ Phiên bản mới
- `bootstrap@^5.3.7` - ✅ Phiên bản mới
- `vue-i18n@^11.1.7` - ✅ Phiên bản mới

**⚠️ Cần kiểm tra**:

- `file-saver@^2.0.5` - ⚠️ Cần kiểm tra CVE
- `jszip@^3.10.1` - ⚠️ Cần kiểm tra CVE (đã có một số lỗ hổng trong quá khứ)

**Khuyến nghị**:

- ✅ Chạy `npm audit` để kiểm tra vulnerabilities
- ✅ Update các package có lỗ hổng
- ✅ Sử dụng `npm audit fix` để tự động fix

---

#### 12.5.2. Đánh giá Dependencies Frontend (Angular)

**File**: `congthongtindientu-cms/package.json`

**✅ Thư viện an toàn**:

- `@angular/core@^20.0.6` - ✅ Phiên bản mới nhất
- `ng-zorro-antd@^20.0.0` - ✅ Component library uy tín
- `angular-auth-oidc-client@^19.0.2` - ✅ OAuth client library
- `rxjs@~7.8.0` - ✅ Phiên bản mới

**⚠️ Cần kiểm tra**:

- `crypto-es@3.0.2` - ⚠️ Fork của crypto-js, cần verify
- `ngx-editor@^19.0.0-beta.1` - ⚠️ Beta version, có thể có lỗ hổng
- `jwt-decode@^4.0.0` - ⚠️ Cần kiểm tra CVE

**Khuyến nghị**:

- ✅ Chạy `npm audit` để kiểm tra vulnerabilities
- ✅ Cân nhắc sử dụng `crypto-js` chính thức thay vì `crypto-es`
- ✅ Cập nhật `ngx-editor` lên stable version nếu có

---

#### 12.5.3. Đánh giá Dependencies Backend (.NET)

**File**: `infoportal/Services/LHP.Cms/LHP.Cms.Api/LHP.Cms.Api.csproj`

**✅ Thư viện an toàn**:

- `Microsoft.AspNetCore.OpenApi@7.0.20` - ✅ Phiên bản mới
- `Swashbuckle.AspNetCore@6.5.0` - ✅ Phiên bản mới

**Khuyến nghị**:

- ✅ Chạy `dotnet list package --vulnerable` để kiểm tra
- ✅ Update các package có lỗ hổng
- ✅ Sử dụng `dotnet outdated` để kiểm tra package cũ

---

### 12.6. PHÁT HIỆN BACKDOOR

#### 12.6.1. Kết quả kiểm tra

**✅ KHÔNG PHÁT HIỆN BACKDOOR RÕ RÀNG**

**Các điểm đã kiểm tra**:

- ✅ Không có code đáng ngờ (eval, exec, Function)
- ✅ Không có kết nối đến server lạ
- ✅ Không có code obfuscated
- ✅ Không có hardcoded backdoor credentials
- ✅ Không có suspicious network calls

---

#### 12.6.2. Các điểm cần lưu ý

**WorkFlowApiUrl trỏ đến domain bên ngoài**

**Vị trí**:

- `infoportal/Services/LHP.Cms/LHP.Cms.Api/appsettings.json`

**Vấn đề**:

```json
"Services": {
  "WorkFlowApiUrl": "https://lhp-gateway.fsel.edu.vn/workflow-gateway"
}
```

**Đánh giá**:

- ⚠️ Cần verify đây là service hợp lệ
- ⚠️ Đảm bảo service này được bảo mật
- ⚠️ Kiểm tra SSL certificate của domain này

**Khuyến nghị**:

- ✅ Verify domain `fsel.edu.vn` là domain chính thức
- ✅ Sử dụng mTLS nếu có thể
- ✅ Implement retry và timeout cho external calls

---

### 12.7. ĐÁNH GIÁ THƯ VIỆN ĐÁNG NGỜ

#### 12.7.1. crypto-es

**Vị trí**: `congthongtindientu-cms/package.json`

**Mô tả**:

- Fork của `crypto-js`
- Không phải là package chính thức
- Có thể có rủi ro bảo mật

**Đánh giá**: 🟡 **MEDIUM RISK**

**Khuyến nghị**:

- ✅ Cân nhắc sử dụng `crypto-js` chính thức
- ✅ Nếu phải dùng, verify source code của package
- ✅ Kiểm tra GitHub repository của package

---

#### 12.7.2. ngx-editor (Beta)

**Vị trí**: `congthongtindientu-cms/package.json`

**Mô tả**:

- Đang ở phiên bản beta
- Có thể có lỗ hổng chưa được phát hiện

**Đánh giá**: 🟡 **MEDIUM RISK**

**Khuyến nghị**:

- ✅ Cập nhật lên stable version nếu có
- ✅ Kiểm tra changelog và security advisories
- ✅ Cân nhắc sử dụng alternative như Quill hoặc TinyMCE

---

#### 12.7.3. Thư viện hợp lệ

Tất cả các thư viện khác đều là:

- ✅ Package chính thức từ npm/nuget
- ✅ Có nhiều downloads và stars
- ✅ Được maintain tích cực
- ✅ Không có dấu hiệu đáng ngờ

---

### 12.8. KHUYẾN NGHỊ KHẮC PHỤC

#### 12.8.1. Ưu tiên cao (CRITICAL)

1. **Di chuyển credentials ra khỏi code**

   - Sử dụng User Secrets cho development
   - Sử dụng Azure Key Vault cho production
   - Không commit credentials lên Git

2. **Tạo JWT Secret Key mạnh**

   - Sử dụng ít nhất 256 bits
   - Generate random key
   - Rotate định kỳ

3. **Cấu hình CORS đúng cách**

   - Chỉ cho phép các origin cụ thể
   - Không dùng `AllowAnyOrigin()` trong production

4. **Giới hạn MaxRequestBodySize**

   - Đặt giới hạn hợp lý (100MB)
   - Validate file size trước khi upload

5. **Sanitize HTML trước khi render**
   - Sử dụng DOMPurify hoặc tương tự
   - Validate và sanitize nội dung ở backend

---

#### 12.8.2. Ưu tiên trung bình (HIGH)

1. **Cấu hình AllowedHosts**

   - Chỉ định các host cụ thể

2. **Sử dụng SSL Certificate hợp lệ**

   - Không dùng `TrustServerCertificate=true` trong production

3. **Kiểm tra và update dependencies**
   - Chạy `npm audit` và `dotnet list package --vulnerable`
   - Update các package có lỗ hổng

---

#### 12.8.3. Ưu tiên thấp (MEDIUM)

1. **Rotate Sentry DSN nếu cần**
2. **Đảm bảo Swagger không expose trong production**
3. **Cải thiện input validation**

---

### 12.9. KẾT LUẬN ĐÁNH GIÁ BẢO MẬT

**Mức độ rủi ro tổng thể**: ⚠️ **TRUNG BÌNH - CAO**

**Các vấn đề chính**:

- 🔴 Hardcoded credentials (CRITICAL)
- 🔴 CORS configuration quá rộng (CRITICAL)
- 🔴 JWT Secret Key yếu (CRITICAL)
- 🔴 XSS Risk - innerHTML (CRITICAL)
- 🟠 TrustServerCertificate = true (HIGH)
- 🟠 MaxRequestBodySize không giới hạn (HIGH)

**Backdoor**: ✅ **KHÔNG PHÁT HIỆN**

**Thư viện đáng ngờ**:

- 🟡 `crypto-es` - Cần cân nhắc
- 🟡 `ngx-editor` (beta) - Cần cập nhật

**Khuyến nghị tổng thể**:

1. **Ngay lập tức**:

   - Di chuyển tất cả credentials ra khỏi code
   - Tạo JWT Secret Key mạnh
   - Cấu hình CORS đúng cách
   - Sanitize HTML trước khi render

2. **Trong tuần này**:

   - Giới hạn MaxRequestBodySize
   - Cấu hình AllowedHosts
   - Kiểm tra và update dependencies

3. **Trong tháng này**:
   - Security audit đầy đủ
   - Penetration testing
   - Implement security monitoring

**Lưu ý**: Đây là đánh giá tự động. Cần có security expert review kỹ hơn trước khi deploy production.

---

**Phiên bản**: 1.0

**Liên hệ hỗ trợ**: Xem file `HUONG_DAN_START.md` để biết cách cài đặt và chạy dự án.
