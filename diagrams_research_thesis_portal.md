# DANH MỤC SƠ ĐỒ HỆ THỐNG: RESEARCH THESIS PORTAL
*(Tài liệu phục vụ Báo cáo Đồ án / Khóa luận)*

Dưới đây là tập hợp đầy đủ và chi tiết các sơ đồ (Mermaid) dành riêng cho hệ thống **Research Thesis Portal**. Các sơ đồ được thiết kế với độ chi tiết cao, giúp sinh viên, giảng viên và quản trị viên hiểu rõ quy trình hoạt động.

---

## 1. HỆ THỐNG SƠ ĐỒ USE CASE

### 1.1. Sơ đồ Use Case Tổng Quát

```mermaid
usecaseDiagram
    actor "Sinh viên" as SV
    actor "Giảng viên" as GV
    actor "Quản trị viên" as Admin

    package "Research Thesis Portal" {
        usecase "Phân hệ Đăng nhập & Xác thực" as Auth
        usecase "Phân hệ Quản lý Đề tài" as QLDeTai
        usecase "Phân hệ Đăng ký & Xét duyệt" as QLDangKy
        usecase "Phân hệ Quản lý Tiến độ & Báo cáo" as QLBaoCao
        usecase "Phân hệ Đánh giá & Nghiệm thu" as QLDanhGia
        usecase "Phân hệ Quản trị Hệ thống" as QLHeThong
    }

    SV --> Auth
    SV --> QLDangKy
    SV --> QLBaoCao
    SV --> QLDanhGia

    GV --> Auth
    GV --> QLDeTai
    GV --> QLDangKy
    GV --> QLBaoCao
    GV --> QLDanhGia

    Admin --> Auth
    Admin --> QLDeTai
    Admin --> QLDangKy
    Admin --> QLHeThong
    Admin --> QLDanhGia
```

### 1.2. Sơ đồ Use Case - Tác nhân Sinh viên

```mermaid
usecaseDiagram
    actor "Sinh viên" as SV

    package "Phân hệ Sinh viên" {
        usecase "Đăng nhập hệ thống" as UC1
        usecase "Xem danh sách đề tài mở" as UC2
        usecase "Xem chi tiết đề tài" as UC3
        usecase "Đăng ký tham gia đề tài" as UC4
        usecase "Hủy đăng ký đề tài" as UC5
        usecase "Nộp báo cáo định kỳ" as UC6
        usecase "Xem phản hồi/nhận xét từ GV" as UC7
        usecase "Xem lịch bảo vệ hội đồng" as UC8
        usecase "Xem kết quả điểm nghiệm thu" as UC9
    }

    SV --> UC1
    SV --> UC2
    SV --> UC3
    SV --> UC4
    SV --> UC5
    SV --> UC6
    SV --> UC7
    SV --> UC8
    SV --> UC9

    UC4 ..> UC1 : <<include>>
    UC6 ..> UC1 : <<include>>
```

### 1.3. Sơ đồ Use Case - Tác nhân Giảng viên

```mermaid
usecaseDiagram
    actor "Giảng viên" as GV

    package "Phân hệ Giảng viên" {
        usecase "Đề xuất đề tài mới" as UC1
        usecase "Cập nhật/Xóa đề tài đề xuất" as UC2
        usecase "Duyệt/Từ chối SV đăng ký" as UC3
        usecase "Xem danh sách nhóm SV hướng dẫn" as UC4
        usecase "Theo dõi tiến độ báo cáo" as UC5
        usecase "Nhận xét & Chấm điểm định kỳ" as UC6
        usecase "Tham gia hội đồng đánh giá" as UC7
        usecase "Chấm điểm nghiệm thu KLTN/NCKH" as UC8
    }

    GV --> UC1
    GV --> UC2
    GV --> UC3
    GV --> UC4
    GV --> UC5
    GV --> UC6
    GV --> UC7
    GV --> UC8
```

### 1.4. Sơ đồ Use Case - Tác nhân Quản trị viên (Admin)

```mermaid
usecaseDiagram
    actor "Quản trị viên" as Admin

    package "Phân hệ Quản trị" {
        usecase "Quản lý Tài khoản (CRUD)" as UC1
        usecase "Quản lý Danh mục (Khoa, Ngành)" as UC2
        usecase "Thiết lập Đợt đăng ký (Mở/Đóng)" as UC3
        usecase "Duyệt đề tài từ Giảng viên" as UC4
        usecase "Phân công Giảng viên hướng dẫn" as UC5
        usecase "Thành lập Hội đồng bảo vệ" as UC6
        usecase "Phân công Giảng viên vào Hội đồng" as UC7
        usecase "Lập lịch & Phòng bảo vệ" as UC8
        usecase "Công bố điểm/Kết quả tổng hợp" as UC9
    }

    Admin --> UC1
    Admin --> UC2
    Admin --> UC3
    Admin --> UC4
    Admin --> UC5
    Admin --> UC6
    Admin --> UC7
    Admin --> UC8
    Admin --> UC9
```

---

## 2. HỆ THỐNG SƠ ĐỒ HOẠT ĐỘNG (STATE DIAGRAM)

### 2.1. Quy trình Đề xuất và Xét duyệt Đề tài

```mermaid
stateDiagram-v2
    [*] --> GV_NhapDeTai : Giảng viên tạo đề tài mới
    GV_NhapDeTai --> GV_Submit : Lưu và gửi duyệt
    GV_Submit --> Admin_Review : Hệ thống chuyển sang trạng thái "Chờ duyệt"

    state Admin_Review {
        [*] --> KiemTraThongTin
        KiemTraThongTin --> TuChoi : Thiếu thông tin / Không phù hợp
        KiemTraThongTin --> YeuCauSua : Cần chỉnh sửa
        KiemTraThongTin --> PheDuyet : Đạt yêu cầu
    }

    TuChoi --> [*] : Hủy đề tài
    YeuCauSua --> GV_NhapDeTai : Trả lại cho Giảng viên
    PheDuyet --> ChoDangKy : Cập nhật trạng thái "Sẵn sàng"

    ChoDangKy --> MoDotDangKy : Admin mở đợt đăng ký
    MoDotDangKy --> [*]
```

### 2.2. Quy trình Sinh viên Đăng ký Đề tài

```mermaid
stateDiagram-v2
    [*] --> XemDanhSach : SV đăng nhập và xem đề tài
    XemDanhSach --> ChonDeTai
    ChonDeTai --> SubmitDangKy : Nhấn nút Đăng ký
    SubmitDangKy --> HeThongCheck : Kiểm tra điều kiện (Tín chỉ, Số lượng)

    state HeThongCheck {
        [*] --> KiemTraSL
        KiemTraSL --> LoiDieuKien : Đã đủ nhóm/Hết hạn/Chưa đủ TC
        KiemTraSL --> HopLe : Điều kiện thỏa mãn
    }

    LoiDieuKien --> XemDanhSach : Báo lỗi, yêu cầu chọn lại
    HopLe --> ChoGVDuyet : Trạng thái "Chờ GV duyệt"

    ChoGVDuyet --> GVDanhGia : Giảng viên nhận thông báo

    state GVDanhGia {
        [*] --> XemThongTinSV
        XemThongTinSV --> GV_TuChoi
        XemThongTinSV --> GV_DongY
    }

    GV_TuChoi --> XemDanhSach : Hệ thống thông báo SV bị từ chối
    GV_DongY --> ThucHienDeTai : Chốt danh sách nhóm
    ThucHienDeTai --> [*]
```

---

## 3. HỆ THỐNG SƠ ĐỒ TUẦN TỰ (SEQUENCE DIAGRAM)

### 3.1. Luồng Xác thực và Đăng nhập (JWT Auth)

```mermaid
sequenceDiagram
    actor User as Người dùng (SV/GV/Admin)
    participant UI as Angular Frontend
    participant API as FastAPI Backend
    participant DB as PostgreSQL

    User->>UI: 1. Nhập Username & Password
    UI->>API: 2. POST /api/auth/login
    API->>DB: 3. Lấy thông tin User theo Username
    DB-->>API: 4. Trả về thông tin & Hash Password

    alt Sai mật khẩu
        API-->>UI: 5a. Return 401 Unauthorized
        UI-->>User: 6a. Báo lỗi sai tài khoản/mật khẩu
    else Đúng mật khẩu
        API->>API: 5b. Tạo JWT Access Token & Refresh Token
        API-->>UI: 6b. Return 200 OK + JWT Tokens + User Info
        UI->>UI: 7. Lưu Token vào LocalStorage / Cookie
        UI-->>User: 8. Chuyển hướng (Redirect) vào Dashboard tương ứng
    end
```

### 3.2. Luồng Nộp Báo cáo Tiến độ (Của Sinh viên)

```mermaid
sequenceDiagram
    actor SV as Sinh viên
    participant UI as Angular Frontend
    participant API as FastAPI Backend
    participant Storage as File Storage (Local/S3)
    participant DB as PostgreSQL
    actor GV as Giảng viên

    SV->>UI: 1. Vào form nộp báo cáo, chọn File (.pdf, .docx)
    UI->>API: 2. POST /api/reports (Kèm Token JWT + File + Data)
    API->>API: 3. Middleware xác thực Token (Verify JWT)
    API->>Storage: 4. Lưu File vào ổ cứng/S3
    Storage-->>API: 5. Trả về File_URL
    API->>DB: 6. Insert bản ghi Báo cáo (gắn File_URL, Topic_ID, Student_ID)
    DB-->>API: 7. Xác nhận thành công
    API-->>UI: 8. Return 201 Created (Kèm Dữ liệu Báo cáo)
    UI-->>SV: 9. Hiển thị thông báo "Nộp thành công"

    API->>GV: 10. (Background Task) Gửi Notification/Email cho GVHD

    GV->>UI: 11. Đăng nhập, mở mục "Chấm báo cáo"
    UI->>API: 12. GET /api/reports/{id}
    API->>DB: 13. Query thông tin báo cáo
    DB-->>API: 14. Dữ liệu báo cáo
    API-->>UI: 15. Dữ liệu JSON (Kèm File_URL)
    UI-->>GV: 16. Hiển thị File báo cáo cho GV đọc
```

---

## 4. SƠ ĐỒ KIẾN TRÚC HỆ THỐNG (SYSTEM ARCHITECTURE)

Thiết kế dựa trên mô hình Single Page Application (SPA) kết nối với RESTful API, chạy độc lập các Container trên Docker.

```mermaid
graph TB
    subgraph Users [Người Dùng Cuối]
        U_SV(Sinh viên)
        U_GV(Giảng viên)
        U_AD(Quản trị viên)
    end

    subgraph Frontend [Angular Application - Docker Container 1]
        UI_SPA[Trình duyệt (Browser) - SPA]
        Guard[Auth Guards]
        Services[Angular HTTP Services]
        UI_SPA --> Guard
        Guard --> Services
    end

    subgraph Backend [FastAPI Application - Docker Container 2]
        Router[API Routers (Endpoints)]
        Auth[JWT Authentication & Middleware]
        Controllers[Business Logic / Services]
        ORM[SQLAlchemy ORM]

        Router --> Auth
        Auth --> Controllers
        Controllers --> ORM
    end

    subgraph Database [Database - Docker Container 3]
        PG[(PostgreSQL)]
    end

    Users -->|HTTPS / Trình duyệt| Frontend
    Services -->|HTTP/REST JSON| Router
    ORM -->|TCP (Port 5432)| PG

    classDef frontend fill:#ddf3ff,stroke:#0075b0,stroke-width:2px;
    classDef backend fill:#e1ffe5,stroke:#008b1a,stroke-width:2px;
    classDef db fill:#ffe1e1,stroke:#b00000,stroke-width:2px;

    class Frontend frontend;
    class Backend backend;
    class Database db;
```

---

## 5. SƠ ĐỒ THỰC THỂ LIÊN KẾT (ERD - ENTITY RELATIONSHIP)

Sơ đồ mô phỏng cấu trúc bảng cơ sở dữ liệu cốt lõi (Core Schema) bằng SQLAlchemy/PostgreSQL.

```mermaid
erDiagram
    USERS {
        int id PK
        string username
        string password_hash
        string full_name
        string email
        string role
        boolean is_active
    }

    DEPARTMENTS {
        int id PK
        string name
        string code
    }

    REGISTRATION_PERIODS {
        int id PK
        string name
        datetime start_date
        datetime end_date
        string status
    }

    TOPICS {
        int id PK
        string title
        text description
        int lecturer_id FK
        int period_id FK
        int max_students
        string status
    }

    TOPIC_REGISTRATIONS {
        int id PK
        int topic_id FK
        int student_id FK
        datetime registered_at
        string status
    }

    REPORTS {
        int id PK
        int topic_id FK
        int student_id FK
        string file_url
        text description
        datetime submitted_at
        string feedback
        float score
    }

    COUNCILS {
        int id PK
        string name
        datetime defense_date
        string room
    }

    COUNCIL_MEMBERS {
        int id PK
        int council_id FK
        int lecturer_id FK
        string role
    }

    USERS ||--o{ TOPICS : proposed_by
    TOPICS ||--o{ TOPIC_REGISTRATIONS : has
    TOPICS ||--o{ REPORTS : tracks
    USERS ||--o{ TOPIC_REGISTRATIONS : creates
    USERS ||--o{ REPORTS : submits
    USERS ||--o{ COUNCIL_MEMBERS : is_lecturer
    DEPARTMENTS ||--o{ USERS : contains
    REGISTRATION_PERIODS ||--o{ TOPICS : contains
    COUNCILS ||--o{ COUNCIL_MEMBERS : has_members
```

---

## 6. BẢNG TÓM TẮT CÁC ENDPOINT API

| Phương thức | Endpoint | Mô tả | Quyền hạn |
|---|---|---|---|
| POST | `/api/auth/login` | Đăng nhập | Public |
| POST | `/api/auth/refresh` | Làm mới JWT Token | User |
| GET | `/api/topics` | Lấy danh sách đề tài | Student, Lecturer |
| POST | `/api/topics` | Tạo đề tài mới | Lecturer |
| PUT | `/api/topics/{id}` | Cập nhật đề tài | Lecturer |
| DELETE | `/api/topics/{id}` | Xóa đề tài | Lecturer, Admin |
| POST | `/api/registrations` | Đăng ký đề tài | Student |
| GET | `/api/registrations/{id}` | Xem chi tiết đăng ký | Student, Lecturer |
| PUT | `/api/registrations/{id}/approve` | Duyệt đăng ký | Lecturer |
| PUT | `/api/registrations/{id}/reject` | Từ chối đăng ký | Lecturer |
| POST | `/api/reports` | Nộp báo cáo | Student |
| GET | `/api/reports/{id}` | Xem báo cáo | Student, Lecturer |
| PUT | `/api/reports/{id}/feedback` | Nhận xét báo cáo | Lecturer |
| GET | `/api/councils` | Xem danh sách hội đồng | Lecturer, Admin |
| POST | `/api/councils` | Tạo hội đồng | Admin |
| GET | `/api/users` | Xem danh sách người dùng | Admin |
| POST | `/api/users` | Tạo tài khoản | Admin |

---

## 7. LUỒNG LỰA VỤ CHỦ YẾU (MAIN WORKFLOWS)

### Luồng 1: Chu kỳ Một Đề tài Từ Đầu Đến Cuối

```
1. Giảng viên Đề xuất Đề tài
   ↓
2. Admin Duyệt Đề tài (APPROVED)
   ↓
3. Admin Mở Đợt Đăng ký
   ↓
4. Sinh viên Xem & Đăng ký Đề tài
   ↓
5. Giảng viên Duyệt Danh sách Sinh viên (Chốt nhóm)
   ↓
6. Sinh viên Nộp Báo cáo Định kỳ (Hàng tuần/Hàng tháng)
   ↓
7. Giảng viên Chấm & Phản hồi Báo cáo
   ↓
8. Admin Thành lập Hội đồng Bảo vệ
   ↓
9. Sinh viên Bảo vệ trước Hội đồng
   ↓
10. Công bố Điểm Cuối cùng (KLTN/NCKH)
```

---

Lưu ý: Tất cả các sơ đồ được thiết kế để hỗ trợ quá trình quản lý khóa luận/đồ án toàn diện, từ khâu đề xuất đề tài cho đến khi công bố kết quả cuối cùng.
