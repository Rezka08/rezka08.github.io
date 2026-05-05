---
layout: post
title: "Vida View Apartment - Full-Stack Apartment Management System"
date: 2025-12-18 14:30:00 +0800
categories: [Web Development, Full-Stack]
tags: [React, Flask, MySQL, TailwindCSS, JWT, Python, JavaScript]
image:
  path: assets/images/projects/vida-view/vida-view.png
  alt: Vida View Apartment Management System
---

## Overview

**Vida View Apartment** adalah sistem manajemen apartemen berbasis web yang komprehensif, dirancang untuk memfasilitasi pengelolaan unit apartemen, booking, pembayaran, dan verifikasi dokumen. Website ini dibangun dengan arsitektur modern menggunakan React.js untuk frontend dan Flask (Python) untuk backend.

> **GitHub Repository:** [github.com/Rezka08/vida-view-apartment-website](#)

---

## 🎯 Problem Statement

Pengelolaan apartemen secara manual sering menghadapi berbagai kendala:
- **Proses booking yang rumit** - Tenant kesulitan mencari dan memesan unit
- **Verifikasi pembayaran manual** - Owner/Admin harus verifikasi pembayaran satu per satu
- **Tidak ada sistem tracking** - Sulit melacak status booking dan pembayaran
- **Manajemen dokumen tidak terorganisir** - Dokumen verifikasi tenant tersebar
- **Tidak ada analytics** - Sulit mendapatkan insight tentang okupansi dan revenue

---

## ✨ Key Features

### 🔐 **Multi-Role Authentication System**
- **3 User Roles**: Admin, Owner (Pemilik Unit), dan Tenant (Penyewa)
- JWT-based authentication dengan refresh token
- Role-based access control (RBAC)
- Secure password hashing dengan bcrypt

### 🏢 **Apartment Management (Owner)**
- CRUD operations untuk unit apartemen
- Multiple photo upload untuk setiap unit
- Custom facilities untuk setiap unit
- Dynamic pricing management
- Availability status tracking (available, occupied, maintenance)
- Unit type classification (1BR, 2BR, 3BR)

### 📅 **Advanced Booking System**
- Real-time availability checking
- Date-based booking dengan validasi
- Automatic price calculation (rent + deposit + utilities + admin fee)
- Promotion code integration dengan usage limit
- Multi-status booking workflow:
  - Pending → Confirmed → Active → Completed
  - Rejected/Cancelled dengan reason tracking

### 💳 **Payment Verification System**
- Multiple payment methods:
  - Bank Transfer (BCA, Mandiri, BNI, BRI)
  - Credit/Debit Card
  - E-Wallet (GoPay, OVO, Dana, LinkAja)
- Receipt upload (mandatory)
- Admin/Owner verification dengan approve/reject
- Payment history tracking
- Automatic notification untuk payment status

### 🎫 **Promotion Management**
- Flexible promotion types:
  - Percentage discount (max 100%)
  - Fixed amount discount (max Rp 1.000.000)
- Usage limit dengan automatic counting
- Date-based validity period
- Real-time promotion validation saat booking

### ⭐ **Favorites System**
- Tenant dapat save apartemen favorit
- Quick access dari dashboard
- Filter pencarian by favorites
- Persistent favorites dengan database storage

### 📄 **Document Verification**
- Upload dokumen untuk verifikasi tenant:
  - KTP (required)
  - Selfie dengan KTP (required)
  - Bukti Penghasilan (optional)
  - Surat Referensi (optional)
- Admin verification workflow
- Document preview dalam modal
- Verification status tracking

### 🔍 **Search & Filter System**
- Advanced search dengan multiple parameters:
  - Unit number/description search
  - Unit type filter (1BR, 2BR, 3BR)
  - Price range filter
  - Bedroom count filter
- Real-time search dari homepage
- URL-based search parameters

### 🔔 **Real-time Notifications**
- In-app notifications untuk semua events:
  - Booking confirmations
  - Payment verifications
  - Document approvals
- Unread count badge
- Auto-refresh setiap 30 detik
- Notification history

### 📊 **Reports & Analytics (Admin)**
- **Occupancy Report**:
  - Total units, occupied, available
  - Occupancy rate by unit type
  - Visual progress bars
- **Revenue Report**:
  - Monthly revenue chart (6 bulan terakhir)
  - Year-over-year comparison
  - Total revenue metrics
- **Top Apartments**:
  - Most viewed units
  - Highest rated units
- Export reports ke PDF dan Excel

### 👤 **User Profile Management**
- Edit profile information
- Upload profile photo
- Change password dengan old password verification
- Update contact details (phone, address, birth date)
- ID Card number management

---

## 🛠️ Technology Stack

### **Frontend**
```javascript
{
  "framework": "React.js 18",
  "build-tool": "Vite",
  "styling": "TailwindCSS 3",
  "routing": "React Router DOM v6",
  "state-management": "Zustand",
  "http-client": "Axios",
  "icons": "Heroicons",
  "notifications": "react-hot-toast",
  "charts": "Custom Chart Components",
  "forms": "Custom Form Components"
}
```

### **Backend**
```python
{
  "framework": "Flask 3.0",
  "database-orm": "SQLAlchemy",
  "authentication": "Flask-JWT-Extended",
  "password-hashing": "bcrypt",
  "file-upload": "Werkzeug",
  "cors": "Flask-CORS",
  "validation": "Custom validators",
  "logging": "Activity Logs"
}
```

### **Database**
```sql
MySQL 8.0
- 15+ normalized tables
- Foreign key constraints
- Indexes on frequently queried columns
- ENUM types for status fields
- JSON columns for flexible data
```

---

## 🏗️ Architecture & Design Patterns

### **Frontend Architecture**
```
frontend/
├── src/
│   ├── api/           # API service layers (axios instances)
│   ├── components/    # Reusable UI components
│   │   ├── common/    # Button, Input, Modal, Badge, etc.
│   │   ├── apartment/ # ApartmentCard, ApartmentFilters
│   │   ├── booking/   # BookingCard, BookingDetailModal
│   │   └── dashboard/ # StatsCard, Chart
│   ├── layouts/       # MainLayout, DashboardLayout, Navbar
│   ├── pages/         # Route-based page components
│   │   ├── admin/     # Admin dashboard, users, reports
│   │   ├── owner/     # Owner dashboard, units, bookings
│   │   ├── tenant/    # Tenant dashboard, bookings, payments
│   │   ├── apartments/# ApartmentList, ApartmentDetail
│   │   └── booking/   # BookingForm, BookingPayment
│   ├── routes/        # ProtectedRoute, RoleRoute guards
│   ├── stores/        # Zustand state stores
│   └── utils/         # Helpers, validators, formatters
```

### **Backend Architecture**
```
backend/
├── routes/            # API endpoints (blueprints)
│   ├── auth.py        # Login, register, logout
│   ├── users.py       # Profile, documents, verification
│   ├── apartments.py  # CRUD apartments, favorites
│   ├── bookings.py    # Booking workflow
│   ├── payments.py    # Payment verification
│   ├── promotions.py  # Promotion management
│   └── admin.py       # Admin reports & analytics
├── models.py          # SQLAlchemy models
├── utils.py           # Helpers, decorators, file upload
├── config.py          # Configuration management
└── app.py             # Application factory
```

### **Design Patterns Used**

1. **Repository Pattern** - Data access through API services
2. **Factory Pattern** - Flask application factory
3. **Decorator Pattern** - `@jwt_required`, `@role_required`
4. **Strategy Pattern** - Payment method handlers
5. **Observer Pattern** - Notification system
6. **State Pattern** - Booking status transitions

---

## 🔒 Security Features

### **Authentication & Authorization**
```python
✅ JWT-based stateless authentication
✅ Access token + Refresh token mechanism
✅ Role-based access control (RBAC)
✅ Password hashing dengan bcrypt (12 rounds)
✅ Protected routes dengan middleware
✅ Session timeout handling
```

### **Data Validation**
```javascript
✅ Frontend validation (React forms)
✅ Backend validation (Flask validators)
✅ SQL injection prevention (SQLAlchemy ORM)
✅ XSS protection (input sanitization)
✅ File upload validation (type, size, extension)
✅ Date range validation untuk booking
```

### **API Security**
```python
✅ CORS configuration
✅ Rate limiting (planned)
✅ Request size limits
✅ Secure file upload paths
✅ Error handling tanpa info leakage
✅ Activity logging untuk audit trail
```

---

## 📊 Database Schema

### **Core Tables**

```sql
users (id, username, email, password_hash, role, full_name, phone,
       address, birth_date, id_card_number, profile_photo,
       id_card_photo, selfie_photo, income_proof, reference_letter,
       document_verified_at, status, created_at, updated_at)

apartments (id, owner_id, unit_number, floor, unit_type, bedrooms,
           bathrooms, area, monthly_rent, deposit, description,
           availability_status, view_count, created_at, updated_at)

apartment_photos (id, apartment_id, photo_url, is_primary,
                 uploaded_at)

facilities (id, name, description, icon, category, status,
           is_custom, created_at)

apartment_facilities (apartment_id, facility_id)

bookings (id, apartment_id, tenant_id, owner_id, booking_code,
         start_date, end_date, total_months, monthly_rent,
         deposit_paid, utility_deposit, admin_fee, promotion_id,
         discount_amount, total_amount, status,
         contract_start_date, contract_end_date, notes,
         created_at, updated_at)

payments (id, booking_id, payment_code, payment_type, amount,
         payment_method, payment_date, payment_status, due_date,
         transaction_id, receipt_file, notes, verified_by,
         verified_at, created_at)

promotions (id, code, title, description, type, value,
           apartment_id, start_date, end_date, min_nights,
           active, usage_limit, usage_count, created_at,
           updated_at)

favorites (id, user_id, apartment_id, created_at)

notifications (id, user_id, title, message, type, related_id,
              is_read, created_at)

activity_logs (id, user_id, action, entity_type, entity_id,
              old_data, new_data, ip_address, user_agent,
              created_at)
```

### **Relationships**
- User **1:N** Apartments (as owner)
- User **1:N** Bookings (as tenant)
- User **1:N** Favorites
- Apartment **N:M** Facilities
- Apartment **1:N** Photos
- Booking **1:N** Payments
- Promotion **1:N** Bookings

---

## 🎨 UI/UX Features

### **Responsive Design**
- ✅ Mobile-first approach dengan TailwindCSS
- ✅ Breakpoints: sm (640px), md (768px), lg (1024px), xl (1280px)
- ✅ Hamburger menu untuk mobile navigation
- ✅ Responsive grid layouts untuk card displays
- ✅ Touch-friendly button sizes

### **User Experience**
- ✅ Loading states dengan skeleton screens
- ✅ Toast notifications untuk feedback
- ✅ Confirmation modals untuk destructive actions
- ✅ Form validation dengan inline error messages
- ✅ Breadcrumb navigation
- ✅ Pagination untuk large datasets
- ✅ Image preview dalam modals
- ✅ Auto-save drafts (booking form)
- ✅ Empty states dengan call-to-action

### **Accessibility**
- ✅ Semantic HTML structure
- ✅ ARIA labels untuk interactive elements
- ✅ Keyboard navigation support
- ✅ Focus states yang jelas
- ✅ Color contrast compliance
- ✅ Alt text untuk semua images

---

## 🚀 Performance Optimizations

### **Frontend**
```javascript
✅ Code splitting dengan React.lazy
✅ Image optimization (lazy loading)
✅ Debounced search input
✅ Memoized components dengan React.memo
✅ Optimistic UI updates
✅ Local state caching dengan Zustand
✅ Vite build optimization (minification, tree-shaking)
```

### **Backend**
```python
✅ Database query optimization (eager loading)
✅ Indexed columns untuk search queries
✅ Pagination untuk large datasets
✅ File compression untuk uploads
✅ Connection pooling
✅ Caching strategies (planned: Redis)
```

---

## 📈 Key Metrics & Statistics

### **Codebase Stats**
```
Total Lines of Code: ~15,000+
Frontend (React):    ~8,000 lines
Backend (Flask):     ~5,000 lines
SQL Migrations:      ~2,000 lines
```

### **Component Breakdown**
```
React Components:    50+ components
API Endpoints:       40+ routes
Database Tables:     15 tables
Reusable Components: 20+ common components
Pages:              30+ pages
```

---

## 🧪 Testing Strategy

### **Manual Testing**
- ✅ User acceptance testing (UAT)
- ✅ Cross-browser testing (Chrome, Firefox, Safari)
- ✅ Mobile responsiveness testing
- ✅ Payment flow end-to-end testing
- ✅ Role-based access testing

### **Planned Automated Testing**
```javascript
// Frontend
- Jest + React Testing Library
- Component unit tests
- Integration tests untuk forms
- E2E tests dengan Cypress

// Backend
- pytest untuk unit tests
- API endpoint testing
- Database migration tests
```

---

## 🔄 Booking & Payment Workflow

### **Booking Lifecycle**
```
1. Tenant searches & filters apartments
2. Views apartment details
3. Clicks "Book Now" → BookingForm
4. Selects dates, applies promo code
5. Reviews booking summary
6. Submits booking (Status: PENDING)
7. Owner reviews & confirms booking (Status: CONFIRMED)
8. Tenant pays deposit → BookingPayment
9. Tenant uploads receipt (Status: VERIFYING)
10. Admin/Owner verifies payment (Status: VERIFIED)
11. Booking becomes active (Status: ACTIVE)
12. Contract ends → (Status: COMPLETED)
```

### **Payment Verification Flow**
```
1. Tenant uploads payment receipt + method details
2. Payment status: PENDING → VERIFYING
3. Admin/Owner reviews receipt
4. If approved:
   - Payment status: VERIFIED
   - Booking status: ACTIVE
   - Apartment status: OCCUPIED
   - Send confirmation notification
5. If rejected:
   - Payment status: PENDING
   - Receipt deleted
   - Booking remains CONFIRMED
   - Apartment remains AVAILABLE
   - Tenant can re-upload (no re-approval needed)
```

---

## 🎯 Business Rules Implemented

### **Booking Rules**
- ✅ Cannot book occupied apartments
- ✅ Minimum 1 month booking duration
- ✅ Start date must be in the future
- ✅ End date must be after start date
- ✅ Promotion usage limit enforcement
- ✅ Promotion date validity checking
- ✅ Automatic deposit calculation (1x monthly rent)
- ✅ Utility deposit: Rp 500,000
- ✅ Admin fee: Rp 300,000

### **Payment Rules**
- ✅ Deposit payment required before monthly rent
- ✅ Receipt upload mandatory
- ✅ Cannot delete verified payments
- ✅ Payment rejection allows re-upload
- ✅ Overdue payment notifications
- ✅ Payment method validation

### **Promotion Rules**
- ✅ Percentage max 100%
- ✅ Fixed amount max Rp 1.000.000
- ✅ Cannot use expired promotions
- ✅ Cannot use promotions that reached usage limit
- ✅ One promotion per booking
- ✅ Usage count auto-increment

---

## 🐛 Challenges & Solutions

### **Challenge 1: Favorite System Race Condition**
**Problem:** Toast notification menampilkan aksi yang salah (Added saat Remove, vice versa)

**Solution:**
```javascript
// Capture state BEFORE API call
const isCurrentlyFavorite = isFavorite;
await apartmentsAPI.toggleFavorite(id);
setIsFavorite(!isCurrentlyFavorite);

toast.success(
  isCurrentlyFavorite ? 'Dihapus dari favorit' : 'Ditambahkan ke favorit'
);
```

### **Challenge 2: Custom Facilities Pollution**
**Problem:** Custom facilities untuk satu unit muncul di unit lain

**Solution:**
- Tambah field `is_custom` di Facility model
- Filter facilities dengan `is_custom: false` saat load
- Set `is_custom: true` saat create custom facility

### **Challenge 3: Payment Rejection Workflow**
**Problem:** Tenant harus re-request booking approval setelah payment ditolak

**Solution:**
```python
# Set booking back to CONFIRMED (not PENDING)
booking.status = 'confirmed'
# Keep apartment AVAILABLE for search
apartment.availability_status = 'available'
# Clear rejected receipt
payment.receipt_file = None
```

### **Challenge 4: Login Redirect Bug**
**Problem:** Tenant login redirect ke `/tenant/dashboard` (route tidak ada), lalu auto-redirect ke home

**Solution:**
```javascript
// Change dari /tenant/dashboard ke /dashboard
navigate('/dashboard');  // ✅ Correct route
```

### **Challenge 5: Promotion Filter Not Working**
**Problem:** Filter "Tidak Aktif" tidak menampilkan promosi yang expired

**Solution:** Frontend filtering berdasarkan fungsi `isPromotionActive()` yang check both `active` flag dan tanggal

---

## 📚 What I Learned

### **Technical Skills**
- ✅ Full-stack development dengan React + Flask
- ✅ JWT authentication implementation
- ✅ Complex state management dengan Zustand
- ✅ File upload handling (multipart/form-data)
- ✅ Database design & normalization
- ✅ RESTful API design best practices
- ✅ Error handling & validation strategies
- ✅ Role-based access control implementation

### **Soft Skills**
- ✅ Requirements analysis & translation to features
- ✅ User flow design untuk complex workflows
- ✅ Code organization & project structure
- ✅ Debugging complex race conditions
- ✅ Documentation writing
- ✅ Version control dengan Git

---

## 🚀 Future Enhancements

### **Planned Features**
```
📱 Mobile App (React Native)
🔔 Push notifications
💬 Chat system (Tenant ↔ Owner)
📧 Email notifications
🌐 Multi-language support (i18n)
📊 Advanced analytics dashboard
🤖 Chatbot untuk customer support
⭐ Review & rating system
📅 Calendar view untuk bookings
🔍 Elasticsearch integration untuk advanced search
💰 Online payment gateway integration
📄 Contract generation & e-signature
🏠 Virtual tour (360° photos)
```

### **Technical Improvements**
```
🧪 Comprehensive test coverage (80%+)
🚀 Redis caching layer
🔐 Rate limiting untuk API
📦 Docker containerization
🌐 CI/CD pipeline (GitHub Actions)
📊 Application monitoring (Sentry)
🗃️ Database backup automation
🔄 WebSocket untuk real-time features
```

---

## 🎓 Key Takeaways

1. **Planning is crucial** - Menghabiskan waktu di awal untuk design database dan API structure menghemat banyak waktu debugging di kemudian hari.

2. **State management matters** - Zustand membuat state management jauh lebih simple dibanding Redux untuk aplikasi scale ini.

3. **Validation at every layer** - Frontend, backend, dan database level validation mencegah banyak bugs.

4. **User feedback is essential** - Toast notifications dan loading states significantly improve UX.

5. **Security cannot be an afterthought** - Implement authentication, authorization, dan validation dari awal.

---

## 💡 Conclusion

Vida View Apartment adalah project yang menantang dan rewarding yang mengajarkan saya tentang full-stack development, dari database design hingga UI/UX implementation. Project ini mendemonstrasikan kemampuan saya dalam:

- 🏗️ Designing scalable architecture
- 💻 Writing clean, maintainable code
- 🔒 Implementing security best practices
- 🎨 Creating intuitive user interfaces
- 🐛 Debugging complex issues
- 📚 Learning new technologies quickly

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

*Last Updated: December 18, 2025*