# DANH MỤC SƠ ĐỒ HỆ THỐNG: RESEARCH THESIS PORTAL
*(Tài liệu phục vụ Báo cáo Đồ án / Khóa luận)*

Dưới đây là tập hợp đầy đủ và chi tiết các sơ đồ (Mermaid) dành riêng cho hệ thống **Research Thesis Portal**. Các sơ đồ được thiết kế với độ chi tiết cao để phục vụ báo cáo đồ án / khóa luận.

---

## 1. HỆ THỐNG SƠ ĐỒ USE CASE

### 1.1. Sơ đồ Use Case Tổng Quát

```mermaid
graph TD
    A["👤 Sinh viên"]
    B["👨‍🏫 Giảng viên"]
    C["⚙️ Quản trị viên"]

    D["🔐 Đăng nhập & Xác thực"]
    E["📋 Quản lý Đề tài"]
    F["✍️ Đăng ký & Xét duyệt"]
    G["📊 Quản lý Tiến độ & Báo cáo"]
    H["⭐ Đánh giá & Nghiệm thu"]
    I["🛠️ Quản trị Hệ thống"]

    A --> D
    A --> F
    A --> G
    A --> H

    B --> D
    B --> E
    B --> F
    B --> G
    B --> H

    C --> D
    C --> E
    C --> F
    C --> I
    C --> H

    style D fill:#e1f5ff
    style E fill:#f3e5f5
    style F fill:#e8f5e9
    style G fill:#fff3e0
    style H fill:#fce4ec
    style I fill:#f1f8e9
```

### 1.2. Sơ đồ Use Case - Tác nhân Sinh viên

```mermaid
graph LR
    A["👤 Sinh viên"]

    B["Đăng nhập hệ thống"]
    C["Xem danh sách đề tài"]
    D["Xem chi tiết đề tài"]
    E["Đăng ký tham gia"]
    F["Hủy đăng ký"]
    G["Nộp báo cáo"]
    H["Xem phản hồi"]
    I["Xem lịch bảo vệ"]
    J["Xem kết quả"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F
    A --> G
    A --> H
    A --> I
    A --> J

    E -.Cần.-> B
    G -.Cần.-> B

    style A fill:#bbdefb,stroke:#1976d2,stroke-width:2px
    style B fill:#c8e6c9,stroke:#388e3c
    style E fill:#fff9c4,stroke:#f57f17
    style G fill:#ffccbc,stroke:#d84315
```

### 1.3. Sơ đồ Use Case - Tác nhân Giảng viên

```mermaid
graph LR
    A["👨‍🏫 Giảng viên"]

    B["Đề xuất đề tài"]
    C["Cập nhật/Xóa đề tài"]
    D["Duyệt đăng ký SV"]
    E["Xem danh sách nhóm"]
    F["Theo dõi tiến độ"]
    G["Chấm & Nhận xét"]
    H["Tham gia hội đồng"]
    I["Chấm điểm nghiệm thu"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F
    A --> G
    A --> H
    A --> I

    style A fill:#c5cae9,stroke:#3f51b5,stroke-width:2px
    style B fill:#c8e6c9,stroke:#388e3c
    style D fill:#fff9c4,stroke:#f57f17
    style G fill:#ffccbc,stroke:#d84315
```

### 1.4. Sơ đồ Use Case - Tác nhân Quản trị viên

```mermaid
graph LR
    A["⚙️ Quản trị viên"]

    B["Quản lý Tài khoản"]
    C["Quản lý Danh mục"]
    D["Thiết lập Đợt DK"]
    E["Duyệt Đề tài"]
    F["Phân công GVHD"]
    G["Thành lập Hội đồng"]
    H["Phân công vào HĐ"]
    I["Lập lịch bảo vệ"]
    J["Công bố điểm"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F
    A --> G
    A --> H
    A --> I
    A --> J

    style A fill:#f8bbd0,stroke:#c2185b,stroke-width:2px
    style B fill:#ffe0b2,stroke:#e65100
    style E fill:#c8e6c9,stroke:#388e3c
    style J fill:#f8bbd0,stroke:#c2185b
```

---

## 2. HỆ THỐNG SƠ ĐỒ HOẠT ĐỘNG (STATE DIAGRAM)

### 2.1. Quy trình Đề xuất và Xét duyệt Đề tài

```mermaid
stateDiagram-v2
    [*] --> GV_NhapDeTai
    GV_NhapDeTai --> GV_Submit: Lưu & gửi duyệt
    GV_Submit --> Admin_Review: Chờ duyệt

    Admin_Review --> TuChoi: Từ chối
    Admin_Review --> YeuCauSua: Cần chỉnh sửa
    Admin_Review --> PheDuyet: Phê duyệt

    TuChoi --> [*]
    YeuCauSua --> GV_NhapDeTai: Trả lại sửa
    PheDuyet --> ChoDangKy: Sẵn sàng
    ChoDangKy --> [*]
```

### 2.2. Quy trình Sinh viên Đăng ký Đề tài

```mermaid
stateDiagram-v2
    [*] --> XemDanhSach
    XemDanhSach --> ChonDeTai
    ChonDeTai --> SubmitDangKy: Đăng ký
    SubmitDangKy --> HeThongCheck: Kiểm tra

    HeThongCheck --> LoiDieuKien: Lỗi điều kiện
    HeThongCheck --> HopLe: Hợp lệ

    LoiDieuKien --> XemDanhSach
    HopLe --> ChoGVDuyet: Chờ GV duyệt
    ChoGVDuyet --> GV_TuChoi: GV từ chối
    ChoGVDuyet --> GV_DongY: GV đồng ý

    GV_TuChoi --> [*]
    GV_DongY --> ThucHienDeTai: Thực hiện
    ThucHienDeTai --> [*]
```

---

## 3. HỆ THỐNG SƠ ĐỒ TUẦN TỰ (SEQUENCE DIAGRAM)

### 3.1. Luồng Xác thực và Đăng nhập (JWT Auth)

```mermaid
sequenceDiagram
    participant User as Người dùng
    participant UI as Frontend
    participant API as Backend
    participant DB as Database

    User->>UI: Nhập tài khoản/mật khẩu
    UI->>API: POST /api/auth/login
    API->>DB: Query user

    alt Mật khẩu sai
        DB-->>API: Error
        API-->>UI: 401 Unauthorized
        UI-->>User: Báo lỗi
    else Mật khẩu đúng
        DB-->>API: User data
        API->>API: Tạo JWT Token
        API-->>UI: 200 OK + Tokens
        UI->>UI: Lưu Token
        UI-->>User: Chuyển Dashboard
    end
```

### 3.2. Luồng Nộp Báo cáo Tiến độ

```mermaid
sequenceDiagram
    participant SV as Sinh viên
    participant UI as Frontend
    participant API as Backend
    participant Store as Storage
    participant DB as Database
    participant GV as Giảng viên

    SV->>UI: Chọn file & nộp
    UI->>API: POST /api/reports + File
    API->>Store: Lưu file
    Store-->>API: File URL
    API->>DB: Insert báo cáo
    DB-->>API: OK
    API-->>UI: 201 Created
    UI-->>SV: Thành công
    API->>GV: Gửi thông báo
```

---

## 4. SƠ ĐỒ KIẾN TRÚC HỆ THỐNG

```mermaid
graph TB
    subgraph Users["👥 Người Dùng"]
        U1["Sinh viên"]
        U2["Giảng viên"]
        U3["Quản trị viên"]
    end

    subgraph Frontend["📱 Frontend - Angular"]
        F1["Browser/SPA"]
        F2["Auth Guards"]
        F3["HTTP Services"]
    end

    subgraph Backend["⚙️ Backend - FastAPI"]
        B1["API Routers"]
        B2["JWT Auth"]
        B3["Controllers"]
        B4["ORM Layer"]
    end

    subgraph Database["🗄️ Database"]
        DB["PostgreSQL"]
    end

    Users -->|HTTPS| Frontend
    F1 --> F2
    F2 --> F3
    F3 -->|REST API| Backend
    B1 --> B2
    B2 --> B3
    B3 --> B4
    B4 -->|SQL| Database

    style Users fill:#e3f2fd,stroke:#1565c0,stroke-width:2px
    style Frontend fill:#f3e5f5,stroke:#6a1b9a,stroke-width:2px
    style Backend fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
    style Database fill:#fff3e0,stroke:#ef6c00,stroke-width:2px
```

---

## 5. SƠ ĐỒ THỰC THỂ LIÊN KẾT (ERD - ENTITY RELATIONSHIP)

```mermaid
erDiagram
    USERS ||--o{ TOPICS : proposed_by
    USERS ||--o{ TOPIC_REGISTRATIONS : creates
    USERS ||--o{ REPORTS : submits
    USERS ||--o{ COUNCIL_MEMBERS : is_lecturer
    DEPARTMENTS ||--o{ USERS : contains
    REGISTRATION_PERIODS ||--o{ TOPICS : contains
    TOPICS ||--o{ TOPIC_REGISTRATIONS : has
    TOPICS ||--o{ REPORTS : tracks
    COUNCILS ||--o{ COUNCIL_MEMBERS : has

    USERS {
        int id
        string username
        string email
        string role
        string full_name
    }

    DEPARTMENTS {
        int id
        string name
        string code
    }

    REGISTRATION_PERIODS {
        int id
        string name
        datetime start_date
        datetime end_date
    }

    TOPICS {
        int id
        string title
        int lecturer_id
        int period_id
        string status
    }

    TOPIC_REGISTRATIONS {
        int id
        int topic_id
        int student_id
        string status
    }

    REPORTS {
        int id
        int topic_id
        int student_id
        string file_url
        float score
    }

    COUNCILS {
        int id
        string name
        datetime defense_date
    }

    COUNCIL_MEMBERS {
        int id
        int council_id
        int lecturer_id
        string role
    }
```

---

## 6. BẢNG TÓM TẮT CÁC ENDPOINT API

| Phương thức | Endpoint | Mô tả | Quyền hạn |
|---|---|---|---|
| POST | `/api/auth/login` | Đăng nhập | Public |
| POST | `/api/auth/refresh` | Làm mới Token | User |
| GET | `/api/topics` | Danh sách đề tài | Student, Lecturer |
| POST | `/api/topics` | Tạo đề tài | Lecturer |
| PUT | `/api/topics/{id}` | Cập nhật đề tài | Lecturer |
| DELETE | `/api/topics/{id}` | Xóa đề tài | Lecturer, Admin |
| POST | `/api/registrations` | Đăng ký đề tài | Student |
| PUT | `/api/registrations/{id}/approve` | Duyệt đăng ký | Lecturer |
| PUT | `/api/registrations/{id}/reject` | Từ chối đăng ký | Lecturer |
| POST | `/api/reports` | Nộp báo cáo | Student |
| GET | `/api/reports/{id}` | Xem báo cáo | Student, Lecturer |
| PUT | `/api/reports/{id}/feedback` | Nhận xét báo cáo | Lecturer |
| GET | `/api/councils` | Danh sách hội đồng | Lecturer, Admin |
| POST | `/api/councils` | Tạo hội đồng | Admin |

---

## 7. LUỒNG LỰA VỤ CHỦ YẾU

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
6. Sinh viên Nộp Báo cáo Định kỳ
   ↓
7. Giảng viên Chấm & Phản hồi
   ↓
8. Admin Thành lập Hội đồng
   ↓
9. Sinh viên Bảo vệ trước Hội đồng
   ↓
10. Công bố Điểm Cuối cùng
```

---

**Lưu ý:** Tất cả các sơ đồ đã được tối ưu hóa để hiển thị đúng trong bảng xem trước GitHub.
