# UniCare
ỨNG DỤNG ĐĂNG KÝ KHÁM - ĐIỀU TRỊ BỆNH CHO BỆNH VIỆN

# Uni Care - Clean MVVM Architecture (Production Grade)

## 1. Overall architecture

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

## 2. Project Structure (Android Java)

```
com.example.uncare
```

---

## 3. Core module

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

## 4. Domain layer

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

## 5. Data layer

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

## 6. Feature layer

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

## 8. Workflow

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
│   │   │
│   │   ├── BaseFragment.java
│   │   │
│   │   └── BaseViewModel.java
│   │
│   ├── network
│   │   ├── ApiService.java
│   │   │
│   │   ├── RetrofitClient.java
│   │   │
│   │   └── ApiHelper.java
│   │
│   ├── session
│   │   └── SessionManager.java
│   │
│   ├── utils
│   │   ├── Constants.java
│   │   │
│   │   ├── DateUtils.java
│   │   │
│   │   └── Validator.java
│   │
│   └── common_ui
│       ├── LoadingDialog.java
│       │
│       ├── ErrorDialog.java
│       │
│       └── BaseAdapter.java
│
├── data
│   ├── model
│   │   ├── User.java
│   │   │
│   │   ├── Doctor.java
│   │   │
│   │   ├── Patient.java
│   │   │
│   │   ├── Appointment.java
│   │   │
│   │   ├── MedicalRecord.java
│   │   │
│   │   ├── TreatmentPlan.java
│   │   │
│   │   └── Notification.java
│   │
│   └── repository
│       ├── AuthRepository.java
│       │
│       ├── DoctorRepository.java
│       │
│       ├── PatientRepository.java
│       │
│       ├── AppointmentRepository.java
│       │
│       ├── MedicalRecordRepository.java
│       │
│       └── NotificationRepository.java
│
├── feature
│
│   ├── auth
│   │   ├── ui
│   │   │   ├── LoginActivity.java
│   │   │   │
│   │   │   └── RegisterActivity.java
│   │   │
│   │   └── viewmodel
│   │       └── AuthViewModel.java
│   │
│   ├── patient
│   │   ├── home
│   │   │   ├── PatientHomeActivity.java
│   │   │   │
│   │   │   └── HomeFragment.java
│   │   │
│   │   ├── appointment
│   │   │   ├── ui
│   │   │   │   ├── AppointmentFragment.java
│   │   │   │   │
│   │   │   │   ├── BookAppointmentActivity.java
│   │   │   │   │
│   │   │   │   └── AppointmentDetailActivity.java
│   │   │   │
│   │   │   ├── viewmodel
│   │   │   │   └── AppointmentViewModel.java
│   │   │   │
│   │   │   └── adapter
│   │   │       └── AppointmentAdapter.java
│   │   │
│   │   ├── doctor
│   │   │   ├── ui
│   │   │   │   └── DoctorListFragment.java
│   │   │   │
│   │   │   └── viewmodel
│   │   │       └── DoctorViewModel.java
│   │   │
│   │   ├── record
│   │   │   ├── ui
│   │   │   │   ├── MedicalRecordFragment.java
│   │   │   │   │
│   │   │   │   └── RecordDetailActivity.java
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
│   │   │
│   │   ├── schedule
│   │   │   ├── ui
│   │   │   │   └── AppointmentManageFragment.java
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
│
└── navigation
└── nav_graph.xml
