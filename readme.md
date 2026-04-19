# 📱 HỆ THỐNG ĐẶT LỊCH KHÁM BỆNH

**Flutter + Dart + SQLite (Android & Web)**

---

## I. Giới thiệu

Hệ thống đặt lịch khám bệnh là ứng dụng hỗ trợ bệnh nhân đặt lịch khám với bác sĩ thông qua nền tảng **Android** và **Web**.

* **Android (Mobile):** Dành cho bệnh nhân (User)
* **Web:** Dành cho quản trị viên (Admin)

Ứng dụng được xây dựng theo mô hình **MVC (Model - View - Controller)**, sử dụng **SQLite** để lưu trữ dữ liệu.

---

## II. Công nghệ sử dụng

* **Flutter** (Android + Web)
* **Dart**
* **SQLite**

  * `sqflite` (mobile)
  * `sqlite3 / drift / web adapter` (web)
* **MVC Architecture (không tách frontend/backend)**

---

## III. Chức năng hệ thống

---

### 1. Mobile (User)

#### 1.1. Xác thực

* Đăng ký tài khoản
* Đăng nhập

#### 1.2. Xem danh sách bác sĩ

* Hiển thị:

  * Tên
  * Chuyên khoa
  * Kinh nghiệm
  * Ảnh

#### 1.3. Xem lịch khám

* Chọn bác sĩ
* Xem danh sách lịch trống theo ngày

#### 1.4. Đặt lịch khám

* Chọn:

  * Bác sĩ
  * Ngày
  * Giờ
* Nhập triệu chứng

#### 1.5. Quản lý lịch đã đặt

* Xem danh sách lịch
* Hủy lịch

---

### 2. Web (Admin)

#### 2.1. Dashboard

* Tổng số:

  * Bệnh nhân
  * Bác sĩ
  * Lịch khám

#### 2.2. Quản lý bác sĩ

* Thêm / sửa / xóa
* Quản lý chuyên khoa

#### 2.3. Quản lý lịch làm việc

* Tạo lịch:

  * Ngày
  * Giờ bắt đầu – kết thúc
* Gán cho bác sĩ

#### 2.4. Quản lý lịch đặt

* Xem tất cả lịch
* Xác nhận / hủy

---

## IV. Cấu trúc thư mục (MVC)

```plaintext
lib/
│
├── models/
│   ├── user.dart
│   ├── doctor.dart
│   ├── schedule.dart
│   └── appointment.dart
│
├── views/
│   ├── auth/
│   ├── user/
│   ├── admin/
│   └── widgets/
│
├── controllers/
│   ├── auth_controller.dart
│   ├── doctor_controller.dart
│   ├── schedule_controller.dart
│   └── appointment_controller.dart
│
├── database/
│   └── db_helper.dart
│
└── main.dart
```

---

## V. Thiết kế Database (SQLite)

### 1. Bảng users

```sql
CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT,
  email TEXT UNIQUE,
  password TEXT,
  phone TEXT,
  role TEXT
);
```

---

### 2. Bảng doctors

```sql
CREATE TABLE doctors (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT,
  specialty TEXT,
  experience INTEGER,
  description TEXT,
  image TEXT
);
```

---

### 3. Bảng schedules

```sql
CREATE TABLE schedules (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  doctor_id INTEGER,
  date TEXT,
  start_time TEXT,
  end_time TEXT,
  is_booked INTEGER DEFAULT 0,
  FOREIGN KEY (doctor_id) REFERENCES doctors(id)
);
```

---

### 4. Bảng appointments

```sql
CREATE TABLE appointments (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER,
  doctor_id INTEGER,
  schedule_id INTEGER,
  symptom TEXT,
  status TEXT,
  created_at TEXT,
  FOREIGN KEY (user_id) REFERENCES users(id),
  FOREIGN KEY (doctor_id) REFERENCES doctors(id),
  FOREIGN KEY (schedule_id) REFERENCES schedules(id)
);
```

---

## VI. Luồng hoạt động chính

### 1. Đặt lịch

1. User chọn bác sĩ
2. Hệ thống hiển thị lịch trống
3. User chọn slot
4. Lưu vào bảng `appointments`
5. Cập nhật `schedules.is_booked = 1`

---

### 2. Hủy lịch

1. User chọn lịch
2. Cập nhật:

   * `appointments.status = cancelled`
   * `schedules.is_booked = 0`

---

### 3. Kiểm tra trùng lịch

```sql
SELECT * FROM schedules 
WHERE id = ? AND is_booked = 0
```

---

## VII. Hướng dẫn cài đặt

### 1. Yêu cầu môi trường

* Flutter SDK >= 3.x
* Dart >= 3.x
* Android Studio / VS Code
* Chrome (để chạy web)

---

### 2. Clone project

```bash
git clone <repo_url>
cd project_name
```

---

### 3. Cài dependencies

```bash
flutter pub get
```

---

### 4. Chạy ứng dụng

#### Android

```bash
flutter run
```

---

#### Web (Admin)

```bash
flutter run -d chrome
```

---

## VIII. Tài khoản mẫu

### Admin (Web)

```
email: admin@gmail.com
password: 123456
```

---

### User (Mobile)

* Đăng ký trực tiếp trong app

---

## IX. Giao diện đề xuất

* Màu chủ đạo:

  * Xanh y tế (#2EC4B6)
  * Trắng
  * Xám nhạt

* Thiết kế:

  * Card layout
  * Bo góc nhẹ
  * Button rõ ràng

---

## X. Tính năng mở rộng (không bắt buộc)

* Tìm kiếm bác sĩ
* Lọc theo chuyên khoa
* Dark mode
* Thông báo (fake)

---

## XI. Đánh giá

Dự án đáp ứng:

* Kiến trúc MVC
* CRUD đầy đủ
* Phân quyền rõ ràng (User / Admin)
* Đặt lịch có kiểm soát
* Chạy đa nền tảng (Android + Web)

---

## XII. Tác giả

* Sinh viên thực hiện: ...
* Môn học: ...
* Giảng viên hướng dẫn: ...

---
