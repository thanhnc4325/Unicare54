# UniCareHaui2026
ỨNG DỤNG ĐĂNG KÝ KHÁM - ĐIỀU TRỊ BỆNH CHO BỆNH VIỆN

# Uni Care - Clean MVVM Architecture (Production Grade)

## 1. Tổng kiến trúc 

```
                        ┌──────────────────────┐
                        │        UI LAYER      │
                        │ Activity / Fragment  │
                        └─────────┬────────────┘
                                  │ observes
                        ┌─────────▼────────────┐
                        │     VIEWMODEL LAYER  │
                        │   State + Logic UI   │
                        └─────────┬────────────┘
                                  │ calls
                        ┌─────────▼────────────┐
                        │     DOMAIN LAYER     │
                        │   UseCases (Business)│
                        └─────────┬────────────┘
                                  │ uses
                        ┌─────────▼────────────┐
                        │      DATA LAYER      │
                        │ Repository Interface │
                        └─────────┬────────────┘
                                  │ implements
            ┌─────────────────────┴─────────────────────┐
            │                                           │
┌───────────▼───────────┐                 ┌─────────────▼────────────┐
│   REMOTE DATA SOURCE   │                 │  LOCAL DATA SOURCE (opt) │
│  Retrofit / API MySQL  │                 │ SQLite / Cache / Session │
└───────────┬───────────┘                 └─────────────┬────────────┘
            │                                           │
            └─────────────────────┬─────────────────────┘
                                  ▼
                        ┌──────────────────────┐
                        │   BACKEND API        │
                        │  (Spring / PHP / etc)│
                        │     + MySQL DB       │
                        └──────────────────────┘
```

---

## 2. Cấu trúc Project (Android Java)

```
com.example.uncare
```

---

## 3. CORE MODULE 

```
core/
├── base/
│   ├── BaseActivity.java
│   ├── BaseFragment.java
│   └── BaseViewModel.java
│
├── network/
│   ├── ApiService.java
│   ├── RetrofitClient.java
│   └── NetworkResult.java
│
├── session/
│   └── SessionManager.java
│
├── utils/
│   ├── Constants.java
│   ├── Validator.java
│   └── DateUtils.java
│
├── ui/
│   ├── dialog/
│   │   ├── LoadingDialog.java
│   │   └── ErrorDialog.java
│   └── adapter/
│       └── BaseAdapter.java
```

---

## 4. DOMAIN LAYER 

```
domain/
├── model/
│   ├── User.java
│   ├── Doctor.java
│   ├── Patient.java
│   ├── Appointment.java
│   ├── MedicalRecord.java
│
├── repository/
│   ├── AuthRepository.java
│   ├── AppointmentRepository.java
│   ├── PatientRepository.java
│   ├── DoctorRepository.java
│
└── usecase/
    ├── auth/
    │   ├── LoginUseCase.java
    │   └── RegisterUseCase.java
    │
    ├── appointment/
    │   ├── GetAppointmentsUseCase.java
    │   ├── BookAppointmentUseCase.java
    │
    ├── patient/
    ├── doctor/
```

---

## 5. DATA LAYER 

```
data/
├── repository/
│   ├── AuthRepositoryImpl.java
│   ├── AppointmentRepositoryImpl.java
│
├── remote/
│   ├── ApiService.java
│   ├── AuthApi.java
│   ├── AppointmentApi.java
│
├── datasource/
│   ├── remote/
│   │   ├── AuthRemoteDataSource.java
│   │   ├── AppointmentRemoteDataSource.java
│
├── mapper/
│   ├── UserMapper.java
│   ├── AppointmentMapper.java
```

---

## 6. FEATURE LAYER 

### Auth Feature

```
feature/auth/
├── ui/
│   ├── LoginActivity.java
│   └── RegisterActivity.java
│
├── viewmodel/
│   └── AuthViewModel.java
```

### Patient Feature

```
feature/patient/
├── home/
├── appointment/
├── doctor/
├── record/
├── profile/
├── notification/
```

### Doctor Feature

```
feature/doctor/
├── home/
├── schedule/
├── patient/
├── record/
├── treatment/
├── profile/
├── notification/
```

### Common Feature

```
feature/common/
└── splash/
    └── SplashActivity.java
```

---

## 7. Navigation Layer

```
navigation/
└── nav_graph.xml
```

---

## 8. Luồng hoạt động 

### Book Appointment Flow

```
UI (Fragment)
   ↓
ViewModel
   ↓
UseCase
   ↓
Repository
   ↓
RemoteDataSource
   ↓
Backend API
   ↓
MySQL
```

---

## 9. Role Handling

```
SessionManager.getRole()

if (PATIENT)
    → PatientHomeActivity
else if (DOCTOR)
    → DoctorHomeActivity
```

---

###
````
com.haui.unicare
│
├── core
│   ├── base
│   │   ├── BaseActivity.java
│   │   │   # Activity cha: setup ViewModel, loading, common UI logic
│   │   │
│   │   ├── BaseFragment.java
│   │   │   # Fragment cha: binding, lifecycle, observe LiveData
│   │   │
│   │   └── BaseViewModel.java
│   │       # Base ViewModel: xử lý loading, error chung
│   │
│   ├── network
│   │   ├── ApiService.java
│   │   │   # Interface Retrofit: khai báo toàn bộ API endpoint
│   │   │
│   │   ├── RetrofitClient.java
│   │   │   # Singleton tạo Retrofit instance (baseUrl, interceptor)
│   │   │
│   │   └── ApiHelper.java
│   │       # Wrapper gọi API (xử lý response + error chung)
│   │
│   ├── session
│   │   └── SessionManager.java
│   │       # Lưu login, token, userId, role (SharedPreferences)
│   │
│   ├── utils
│   │   ├── Constants.java
│   │   │   # BASE_URL, KEY_SHARED_PREF, ROLE, config app
│   │   │
│   │   ├── DateUtils.java
│   │   │   # format date/time
│   │   │
│   │   └── Validator.java
│   │       # validate input (email, phone, password)
│   │
│   └── common_ui
│       ├── LoadingDialog.java
│       │   # dialog loading khi call API
│       │
│       ├── ErrorDialog.java
│       │   # dialog hiển thị lỗi hệ thống/API
│       │
│       └── BaseAdapter.java
│           # RecyclerView base adapter dùng lại
│
├── data
│   ├── model
│   │   ├── User.java
│   │   │   # ánh xạ bảng users
│   │   │
│   │   ├── Doctor.java
│   │   │   # ánh xạ bảng doctors
│   │   │
│   │   ├── Patient.java
│   │   │   # ánh xạ bảng patients
│   │   │
│   │   ├── Appointment.java
│   │   │   # ánh xạ bảng appointments
│   │   │
│   │   ├── MedicalRecord.java
│   │   │   # ánh xạ bảng medical_records
│   │   │
│   │   ├── TreatmentPlan.java
│   │   │   # ánh xạ bảng treatment_plans
│   │   │
│   │   └── Notification.java
│   │       # ánh xạ bảng notifications
│   │
│   └── repository
│       ├── AuthRepository.java
│       │   # login/register
│       │
│       ├── DoctorRepository.java
│       │   # lấy danh sách bác sĩ
│       │
│       ├── PatientRepository.java
│       │   # thông tin bệnh nhân
│       │
│       ├── AppointmentRepository.java
│       │   # CRUD lịch hẹn
│       │
│       ├── MedicalRecordRepository.java
│       │   # CRUD bệnh án
│       │
│       └── NotificationRepository.java
│           # lấy thông báo hệ thống
│
├── feature
│
│   ├── auth
│   │   ├── ui
│   │   │   ├── LoginActivity.java
│   │   │   │   # màn đăng nhập
│   │   │   │
│   │   │   └── RegisterActivity.java
│   │   │       # màn đăng ký tài khoản
│   │   │
│   │   └── viewmodel
│   │       └── AuthViewModel.java
│   │           # xử lý login/register + LiveData User
│   │
│   ├── patient
│   │   ├── home
│   │   │   ├── PatientHomeActivity.java
│   │   │   │   # màn chính patient (BottomNavigation)
│   │   │   │
│   │   │   └── HomeFragment.java
│   │   │       # dashboard tổng quan
│   │   │
│   │   ├── appointment
│   │   │   ├── ui
│   │   │   │   ├── AppointmentFragment.java
│   │   │   │   │   # danh sách lịch khám
│   │   │   │   │
│   │   │   │   ├── BookAppointmentActivity.java
│   │   │   │   │   # đặt lịch khám
│   │   │   │   │
│   │   │   │   └── AppointmentDetailActivity.java
│   │   │   │       # chi tiết lịch khám
│   │   │   │
│   │   │   ├── viewmodel
│   │   │   │   └── AppointmentViewModel.java
│   │   │   │       # xử lý logic lịch khám
│   │   │   │
│   │   │   └── adapter
│   │   │       └── AppointmentAdapter.java
│   │   │           # RecyclerView danh sách lịch
│   │   │
│   │   ├── doctor
│   │   │   ├── ui
│   │   │   │   └── DoctorListFragment.java
│   │   │   │       # danh sách bác sĩ
│   │   │   │
│   │   │   └── viewmodel
│   │   │       └── DoctorViewModel.java
│   │   │
│   │   ├── record
│   │   │   ├── ui
│   │   │   │   ├── MedicalRecordFragment.java
│   │   │   │   │   # danh sách bệnh án
│   │   │   │   │
│   │   │   │   └── RecordDetailActivity.java
│   │   │   │       # chi tiết bệnh án
│   │   │   │
│   │   │   └── viewmodel
│   │   │       └── MedicalRecordViewModel.java
│   │   │
│   │   ├── profile
│   │   │   ├── ui
│   │   │   │   ├── ProfileFragment.java
│   │   │   │   └── EditProfileActivity.java
│   │   │   │
│   │   │   └── viewmodel
│   │   │       └── PatientViewModel.java
│   │   │
│   │   └── notification
│   │       ├── ui
│   │       │   └── NotificationFragment.java
│   │       │
│   │       └── viewmodel
│   │           └── NotificationViewModel.java
│   │
│   ├── doctor
│   │   ├── home
│   │   │   └── DoctorHomeActivity.java
│   │   │       # dashboard bác sĩ
│   │   │
│   │   ├── schedule
│   │   │   ├── ui
│   │   │   │   └── AppointmentManageFragment.java
│   │   │   │       # quản lý lịch khám
│   │   │   │
│   │   │   └── viewmodel
│   │   │       └── AppointmentViewModel.java
│   │   │
│   │   ├── patient
│   │   │   ├── ui
│   │   │   │   ├── PatientListFragment.java
│   │   │   │   └── PatientDetailActivity.java
│   │   │   │
│   │   │   └── viewmodel
│   │   │       └── PatientViewModel.java
│   │   │
│   │   ├── record
│   │   │   ├── ui
│   │   │   │   ├── RecordListFragment.java
│   │   │   │   └── CreateRecordActivity.java
│   │   │   │
│   │   │   └── viewmodel
│   │   │       └── MedicalRecordViewModel.java
│   │   │
│   │   ├── treatment
│   │   │   ├── ui
│   │   │   │   └── TreatmentPlanActivity.java
│   │   │   │
│   │   │   └── viewmodel
│   │   │       └── TreatmentViewModel.java
│   │   │
│   │   ├── profile
│   │   │   └── DoctorProfileFragment.java
│   │   │
│   │   └── notification
│   │       └── NotificationFragment.java
│   │
│   └── common
│       └── splash
│           └── SplashActivity.java
│               # kiểm tra login → điều hướng theo role
│
└── navigation
└── nav_graph.xml
