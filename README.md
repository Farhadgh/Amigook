# Amigook
### 2.9 Game Service (Enhanced - CONTEST SYSTEM)
**Technology**: Rust (Actix-Web) + PostgreSQL + Redis

**Game/Contest Types**:
1. **Beauty Contest**: Judged by appearance, health, grooming
2. **Weight Contest**: Highest weight gain in period
3. **Profit Contest**: Best ROI/profit margin
4. **Consistency Contest**: Best care consistency
5. **Custom Contests**: Admin-defined rules

**Responsibilities**:
- Game/Contest creation and management
- Participant registration
- Automatic scoring and ranking
- Prize distribution
- Leaderboard management
- Daily challenges and tasks
- Achievement system

**Admin Contest Management Endpoints**:
```
POST   /api/v1/admin/contests (Create contest)
PUT    /api/v1/admin/contests/{id}
DELETE /api/v1/admin/contests/{id}
POST   /api/v1/admin/contests/{id}/start
POST   /api/v1/admin/contests/{id}/end
POST   /api/v1/admin/contests/{id}/judge (For beauty contests)
GET    /api/v1/admin/contests/{id}/participants
POST   /api/v1/admin/contests/{id}/announce-winners
POST   /api/v1/admin/contests/{id}/distribute-prizes
```

**User Contest Endpoints**:
```
GET    /api/v1/contests (Browse active contests)
GET    /api/v1/contests/{id}
POST   /api/v1/contests/{id}/register (Register livestock)
GET    /api/v1/contests/{id}/leaderboard
GET    /api/v1/contests/{id}/my-ranking
GET    /api/v1/my-contests
GET    /api/v1/contests/{id}/rules
```

**Pilot Farming Endpoints**:
```
GET    /api/v1/games/pilot-farming/dashboard
GET    /api/v1/games/pilot-farming/tasks
POST   /api/v1/games/pilot-farming/tasks/{id}/complete
GET    /api/v1/games/pilot-farming/achievements
GET    /api/v1/games/pilot-farming/leaderboard
POST   /api/v1/games/pilot-farming/challenges/{id}/join
```

**Database Schema**:
```sql
CREATE TYPE contest_type_enum AS ENUM ('beauty', 'weight_gain', 'profit', 'consistency', 'custom');
CREATE TYPE contest_status_enum AS ENUM ('draft', 'registration_open', 'active', 'judging', 'completed', 'cancelled');
CREATE TYPE prize_type_enum AS ENUM ('cash', 'discount', 'free_service', 'badge', 'custom');

CREATE TABLE contests (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    creator_id UUID REFERENCES users(id) NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    contest_type contest_type_enum NOT NULL,
    status contest_status_enum DEFAULT 'draft',
    
    -- Eligibility# Amigok Platform - Complete Backend Architecture
## Smart Farm Management & Livestock Investment Platform

## 1. System Overview

A comprehensive microservices platform for livestock investment, farm management, gaming, and marketplace with real-time video streaming, wallet management, and payment processing.

### Core Business Models
1. **Profit Farming**: Users invest in livestock for ROI
2. **Gaming/Pilot Farming**: Interactive farm management with gamification
3. **Marketplace**: Buy/sell livestock, feed packages, and farm products

### Architecture Diagram
```
┌─────────────────────────────────────────────────────────────────┐
│                    API Gateway (Kong/Traefik)                    │
│         Rate Limiting | Auth | Routing | Load Balancing          │
└───────────────────────────────┬─────────────────────────────────┘
                                │
┌───────────────────────────────┴─────────────────────────────────┐
│                        Service Mesh Layer                        │
└───────────────────────────────┬─────────────────────────────────┘
                                │
        ┌───────────────────────┼───────────────────┐
        │                       │                   │
┌───────▼────────┐  ┌──────────▼──────┐   ┌───────▼────────┐
│  Auth Service  │  │  User Service   │   │ Farm Service   │
│   (Identity)   │  │   (Profiles)    │   │  (Livestock)   │
└───────┬────────┘  └──────────┬──────┘   └───────┬────────┘
        │                      │                   │
        └──────────────────────┼───────────────────┘
                               │
        ┌──────────────────────┼──────────────────┐
        │                      │                  │
┌───────▼────────┐  ┌─────────▼─────────┐ ┌─────▼──────────┐
│ Wallet Service │  │  Payment Service  │ │ Game Service   │
│   (Balance)    │  │   (Transactions)  │ │ (Gamification) │
└───────┬────────┘  └─────────┬─────────┘ └─────┬──────────┘
        │                     │                  │
        └─────────────────────┼──────────────────┘
                              │
        ┌─────────────────────┼─────────────────┐
        │                     │                 │
┌───────▼────────┐  ┌────────▼────────┐ ┌─────▼──────────┐
│ Marketplace    │  │  Feed Package   │ │ Care Service   │
│   Service      │  │    Service      │ │  (Medicine)    │
└───────┬────────┘  └────────┬────────┘ └─────┬──────────┘
        │                    │                 │
        └────────────────────┼─────────────────┘
                            │
        ┌───────────────────┼───────────────┐
        │                   │               │
┌───────▼────────┐  ┌──────▼──────┐ ┌─────▼──────────┐
│ Stream Service │  │  Analytics  │ │  Notification  │
│  (Video/IoT)   │  │   Service   │ │    Service     │
└────────────────┘  └─────────────┘ └────────────────┘
```

---

## 2. Enhanced Service Details

### 2.1 User Management Service (Enhanced)
**Technology**: Rust (Actix-Web) + PostgreSQL + Redis

**Responsibilities**:
- User authentication and authorization
- Multi-factor authentication (2FA)
- KYC integration for investors
- Role management (Investor, Gamer, Admin, Farm Staff)
- Session management
- **Admin user management (CRUD)**
- User dashboard configuration

**Authentication Endpoints**:
```
POST   /api/v1/auth/register
POST   /api/v1/auth/login
POST   /api/v1/auth/logout
POST   /api/v1/auth/refresh
POST   /api/v1/auth/verify-email
POST   /api/v1/auth/verify-phone
POST   /api/v1/auth/2fa/enable
POST   /api/v1/auth/2fa/verify
POST   /api/v1/auth/kyc/submit
GET    /api/v1/auth/kyc/status
```

**Admin User Management Endpoints**:
```
GET    /api/v1/admin/users (List all users with filters)
GET    /api/v1/admin/users/{id}
POST   /api/v1/admin/users (Create user)
PUT    /api/v1/admin/users/{id}
DELETE /api/v1/admin/users/{id}
PUT    /api/v1/admin/users/{id}/role
PUT    /api/v1/admin/users/{id}/status (activate/deactivate)
POST   /api/v1/admin/users/{id}/reset-password
GET    /api/v1/admin/users/{id}/activity-log
GET    /api/v1/admin/users/statistics
```

**Database Schema**:
```sql
CREATE TYPE user_role_enum AS ENUM ('investor', 'gamer', 'admin', 'farm_staff', 'veterinarian');
CREATE TYPE kyc_status_enum AS ENUM ('not_submitted', 'pending', 'approved', 'rejected');

CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    phone VARCHAR(20) UNIQUE,
    password_hash VARCHAR(255),
    username VARCHAR(100) UNIQUE,
    full_name VARCHAR(255),
    role user_role_enum DEFAULT 'investor',
    kyc_status kyc_status_enum DEFAULT 'not_submitted',
    kyc_data JSONB,
    two_factor_enabled BOOLEAN DEFAULT FALSE,
    two_factor_secret VARCHAR(255),
    email_verified BOOLEAN DEFAULT FALSE,
    phone_verified BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    last_login TIMESTAMPTZ,
    is_active BOOLEAN DEFAULT TRUE,
    created_by UUID REFERENCES users(id), -- Admin who created this user
    notes TEXT -- Admin notes about user
);

CREATE TABLE user_activity_logs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) ON DELETE CASCADE,
    action VARCHAR(100) NOT NULL, -- login, purchase, withdrawal, etc.
    ip_address INET,
    user_agent TEXT,
    metadata JSONB,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_activity_logs_user ON user_activity_logs(user_id, created_at DESC);
```

---

### 2.2 Farm Service (NEW)
**Technology**: Rust (Actix-Web) + PostgreSQL + Redis

**Responsibilities**:
- Livestock inventory management (Admin only can add from outside)
- Farm location and facility management
- Animal health tracking
- Assignment of livestock to users
- Lifecycle management (birth, growth, sale, withdrawal)
- Livestock type and breed management (Admin only)

**Admin-Only Endpoints**:
```
POST   /api/v1/admin/livestock-types
PUT    /api/v1/admin/livestock-types/{id}
DELETE /api/v1/admin/livestock-types/{id}
POST   /api/v1/admin/livestock-breeds
PUT    /api/v1/admin/livestock-breeds/{id}
DELETE /api/v1/admin/livestock-breeds/{id}
POST   /api/v1/admin/livestock (Add livestock from outside to system)
PUT    /api/v1/admin/livestock/{id}/price
```

**Public/User Endpoints**:
```
GET    /api/v1/livestock-types (List available types)
GET    /api/v1/livestock-breeds (List breeds by type)
GET    /api/v1/farms
GET    /api/v1/farms/{id}
GET    /api/v1/farms/{id}/livestock (Available livestock in farm)
GET    /api/v1/livestock/{id}
GET    /api/v1/livestock/{id}/health
POST   /api/v1/livestock/{id}/health
GET    /api/v1/livestock/{id}/owner-history
POST   /api/v1/livestock/{id}/withdraw-request
GET    /api/v1/my-livestock (User's owned livestock)
```

**Database Schema**:
```sql
CREATE TABLE farms (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    location GEOGRAPHY(POINT),
    address TEXT,
    manager_id UUID REFERENCES users(id),
    capacity INT NOT NULL,
    current_occupancy INT DEFAULT 0,
    facilities JSONB,
    certifications JSONB,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Admin-managed livestock types (dynamic)
CREATE TABLE livestock_types (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) UNIQUE NOT NULL, -- sheep, cow, ostrich, etc.
    description TEXT,
    icon_url TEXT,
    is_active BOOLEAN DEFAULT TRUE,
    created_by UUID REFERENCES users(id),
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Insert default type
INSERT INTO livestock_types (name, description, is_active) 
VALUES ('sheep', 'Sheep livestock type', TRUE);

-- Admin-managed breeds per livestock type
CREATE TABLE livestock_breeds (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    livestock_type_id UUID REFERENCES livestock_types(id) ON DELETE CASCADE,
    breed_name VARCHAR(100) NOT NULL,
    description TEXT,
    typical_weight_kg DECIMAL(6, 2),
    characteristics JSONB,
    is_active BOOLEAN DEFAULT TRUE,
    created_by UUID REFERENCES users(id),
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    UNIQUE(livestock_type_id, breed_name)
);

CREATE TYPE gender_enum AS ENUM ('male', 'female');
CREATE TYPE livestock_status_enum AS ENUM ('available', 'assigned', 'in_care', 'ready_for_sale', 'sold', 'withdrawn', 'deceased');
CREATE TYPE ownership_type_enum AS ENUM ('profit', 'gaming');

CREATE TABLE livestock (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    farm_id UUID REFERENCES farms(id) NOT NULL,
    owner_id UUID REFERENCES users(id), -- NULL means owned by system/admin
    ownership_type ownership_type_enum,
    livestock_type_id UUID REFERENCES livestock_types(id) NOT NULL,
    breed_id UUID REFERENCES livestock_breeds(id),
    tag_number VARCHAR(50) UNIQUE NOT NULL, -- Physical ID card/RFID tag
    unique_id_card VARCHAR(100) UNIQUE NOT NULL, -- ID for Jetson Nano recognition
    date_of_birth DATE,
    gender gender_enum NOT NULL,
    weight_kg DECIMAL(8, 2) NOT NULL,
    price_per_kg DECIMAL(10, 2) NOT NULL, -- Price per kilogram
    total_price DECIMAL(12, 2) GENERATED ALWAYS AS (weight_kg * price_per_kg) STORED,
    health_status VARCHAR(50) DEFAULT 'healthy',
    status livestock_status_enum DEFAULT 'available',
    purchase_price DECIMAL(12, 2), -- Original purchase price by user
    current_value DECIMAL(12, 2), -- Current market value
    assigned_at TIMESTAMPTZ,
    jetson_camera_id UUID, -- Connected Jetson Nano device
    last_detected_at TIMESTAMPTZ, -- Last time detected by camera
    metadata JSONB,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE livestock_health_records (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    livestock_id UUID REFERENCES livestock(id) ON DELETE CASCADE,
    recorded_by UUID REFERENCES users(id),
    record_type VARCHAR(50) NOT NULL, -- checkup, vaccination, treatment
    description TEXT,
    diagnosis TEXT,
    treatment TEXT,
    medication JSONB,
    cost DECIMAL(10, 2),
    next_checkup_date DATE,
    veterinarian_notes TEXT,
    recorded_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE livestock_ownership_history (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    livestock_id UUID REFERENCES livestock(id),
    user_id UUID REFERENCES users(id),
    ownership_type ownership_type_enum,
    started_at TIMESTAMPTZ DEFAULT NOW(),
    ended_at TIMESTAMPTZ,
    purchase_price DECIMAL(12, 2),
    sale_price DECIMAL(12, 2),
    reason VARCHAR(100)
);
```

---

### 2.3 Wallet Service (NEW)
**Technology**: Rust (Actix-Web) + PostgreSQL + Redis

**Responsibilities**:
- User wallet management
- Balance tracking (main balance, locked balance)
- Transaction history
- Internal transfers
- Withdrawal processing
- Deposit handling

**Key Endpoints**:
```
GET    /api/v1/wallet
GET    /api/v1/wallet/balance
GET    /api/v1/wallet/transactions
POST   /api/v1/wallet/deposit
POST   /api/v1/wallet/withdraw
POST   /api/v1/wallet/transfer
GET    /api/v1/wallet/withdraw/requests
PUT    /api/v1/wallet/withdraw/{id}/status
```

**Database Schema**:
```sql
CREATE TABLE wallets (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) UNIQUE NOT NULL,
    balance DECIMAL(15, 2) DEFAULT 0.00,
    locked_balance DECIMAL(15, 2) DEFAULT 0.00,
    currency VARCHAR(3) DEFAULT 'USD',
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    CONSTRAINT positive_balance CHECK (balance >= 0),
    CONSTRAINT positive_locked CHECK (locked_balance >= 0)
);

CREATE TYPE transaction_type_enum AS ENUM (
    'deposit', 'withdrawal', 'purchase', 'refund', 
    'feeding_payment', 'care_payment', 'sale_revenue',
    'game_reward', 'transfer_in', 'transfer_out'
);

CREATE TYPE transaction_status_enum AS ENUM ('pending', 'completed', 'failed', 'cancelled');

CREATE TABLE wallet_transactions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    wallet_id UUID REFERENCES wallets(id) NOT NULL,
    transaction_type transaction_type_enum NOT NULL,
    amount DECIMAL(15, 2) NOT NULL,
    balance_before DECIMAL(15, 2),
    balance_after DECIMAL(15, 2),
    status transaction_status_enum DEFAULT 'pending',
    reference_id UUID, -- ID of related entity (livestock, package, etc.)
    reference_type VARCHAR(50),
    description TEXT,
    metadata JSONB,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    completed_at TIMESTAMPTZ
);

CREATE TYPE withdrawal_status_enum AS ENUM ('pending', 'processing', 'completed', 'rejected');

CREATE TABLE withdrawal_requests (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) NOT NULL,
    wallet_id UUID REFERENCES wallets(id) NOT NULL,
    amount DECIMAL(15, 2) NOT NULL,
    withdrawal_method VARCHAR(50), -- bank_transfer, paypal, crypto
    account_details JSONB,
    status withdrawal_status_enum DEFAULT 'pending',
    admin_notes TEXT,
    processed_by UUID REFERENCES users(id),
    requested_at TIMESTAMPTZ DEFAULT NOW(),
    processed_at TIMESTAMPTZ
);

CREATE INDEX idx_wallet_transactions_wallet ON wallet_transactions(wallet_id, created_at DESC);
CREATE INDEX idx_withdrawal_requests_user ON withdrawal_requests(user_id, requested_at DESC);
```

---

### 2.4 Payment Service (NEW)
**Technology**: Rust (Actix-Web) + PostgreSQL + Stripe/PayPal API

**Responsibilities**:
- Payment gateway integration (Stripe, PayPal, Crypto)
- Payment processing
- Refund handling
- Invoice generation
- Payment webhook handling
- PCI compliance

**Key Endpoints**:
```
POST   /api/v1/payments/create
POST   /api/v1/payments/confirm
GET    /api/v1/payments/{id}
POST   /api/v1/payments/{id}/refund
GET    /api/v1/payments/history
POST   /api/v1/payments/webhook/stripe
POST   /api/v1/payments/webhook/paypal
GET    /api/v1/invoices/{id}
POST   /api/v1/invoices/{id}/download
```

**Database Schema**:
```sql
CREATE TYPE payment_method_enum AS ENUM ('credit_card', 'paypal', 'bank_transfer', 'crypto', 'wallet');
CREATE TYPE payment_status_enum AS ENUM ('pending', 'processing', 'completed', 'failed', 'refunded');

CREATE TABLE payments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) NOT NULL,
    wallet_id UUID REFERENCES wallets(id),
    amount DECIMAL(15, 2) NOT NULL,
    currency VARCHAR(3) DEFAULT 'USD',
    payment_method payment_method_enum NOT NULL,
    payment_gateway VARCHAR(50), -- stripe, paypal, etc.
    gateway_transaction_id VARCHAR(255),
    status payment_status_enum DEFAULT 'pending',
    reference_type VARCHAR(50), -- livestock_purchase, feed_package, care_service
    reference_id UUID,
    metadata JSONB,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    completed_at TIMESTAMPTZ
);

CREATE TABLE refunds (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    payment_id UUID REFERENCES payments(id) NOT NULL,
    amount DECIMAL(15, 2) NOT NULL,
    reason TEXT,
    status payment_status_enum DEFAULT 'pending',
    processed_by UUID REFERENCES users(id),
    created_at TIMESTAMPTZ DEFAULT NOW(),
    processed_at TIMESTAMPTZ
);

CREATE TABLE invoices (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) NOT NULL,
    payment_id UUID REFERENCES payments(id),
    invoice_number VARCHAR(50) UNIQUE NOT NULL,
    amount DECIMAL(15, 2) NOT NULL,
    tax_amount DECIMAL(15, 2) DEFAULT 0,
    total_amount DECIMAL(15, 2) NOT NULL,
    items JSONB NOT NULL,
    pdf_url TEXT,
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

---

### 2.5 Feed Package Service (NEW)
**Technology**: Rust (Actix-Web) + PostgreSQL

**Responsibilities**:
- Feed package catalog management (Admin creates packages)
- Package subscription/purchase (Users select from packages)
- Feeding schedule management
- Automatic payment processing
- Feed delivery tracking
- **Note**: Feed packages are NOT in marketplace, they are in user's feed management menu

**Admin Endpoints**:
```
POST   /api/v1/admin/feed-packages (Create new package)
PUT    /api/v1/admin/feed-packages/{id}
DELETE /api/v1/admin/feed-packages/{id}
PUT    /api/v1/admin/feed-packages/{id}/pricing
```

**User Endpoints**:
```
GET    /api/v1/feed-packages (View available packages)
GET    /api/v1/feed-packages/{id}
POST   /api/v1/livestock/{livestock_id}/feed-packages/{package_id}/subscribe
GET    /api/v1/my-feed-subscriptions
POST   /api/v1/subscriptions/{id}/cancel
POST   /api/v1/subscriptions/{id}/upgrade
GET    /api/v1/livestock/{id}/feeding-schedule
GET    /api/v1/livestock/{id}/feeding-history
```

**Farm Staff Endpoints**:
```
GET    /api/v1/staff/feeding-tasks (Daily feeding tasks)
POST   /api/v1/staff/feeding-log (Log feeding completion)
POST   /api/v1/staff/feeding-log/bulk (Bulk feeding log)
```

**Database Schema**:
```sql
CREATE TYPE package_type_enum AS ENUM ('basic', 'standard', 'premium', 'organic');
CREATE TYPE subscription_status_enum AS ENUM ('active', 'paused', 'cancelled', 'expired');

CREATE TABLE feed_packages (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    description TEXT,
    package_type package_type_enum NOT NULL,
    livestock_types VARCHAR(50)[] NOT NULL,
    daily_cost DECIMAL(10, 2) NOT NULL,
    weekly_cost DECIMAL(10, 2) NOT NULL,
    monthly_cost DECIMAL(10, 2) NOT NULL,
    ingredients JSONB,
    nutritional_info JSONB,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE livestock_feed_subscriptions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    livestock_id UUID REFERENCES livestock(id) NOT NULL,
    user_id UUID REFERENCES users(id) NOT NULL,
    package_id UUID REFERENCES feed_packages(id) NOT NULL,
    status subscription_status_enum DEFAULT 'active',
    start_date DATE NOT NULL,
    end_date DATE,
    billing_cycle VARCHAR(20) DEFAULT 'monthly', -- daily, weekly, monthly
    next_billing_date DATE,
    auto_renew BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE feeding_logs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    livestock_id UUID REFERENCES livestock(id) NOT NULL,
    subscription_id UUID REFERENCES livestock_feed_subscriptions(id),
    fed_by UUID REFERENCES users(id),
    feeding_time TIMESTAMPTZ NOT NULL,
    feed_type VARCHAR(100),
    quantity_kg DECIMAL(6, 2),
    notes TEXT,
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

---

### 2.6 Care Service (Enhanced)
**Technology**: Rust (Actix-Web) + PostgreSQL

**Responsibilities**:
- Veterinary service management
- Medicine inventory and prescription
- Care scheduling
- Health monitoring
- Care cost tracking
- Emergency care requests
- **Admin assigns care services to users**
- **Admin adds medicines/treatments to user dashboard**

**Admin Care Management Endpoints**:
```
POST   /api/v1/admin/care-services (Create care service type)
PUT    /api/v1/admin/care-services/{id}
DELETE /api/v1/admin/care-services/{id}
POST   /api/v1/admin/medicines (Add medicine to inventory)
PUT    /api/v1/admin/medicines/{id}
DELETE /api/v1/admin/medicines/{id}
POST   /api/v1/admin/users/{user_id}/assign-care (Assign care to user's livestock)
POST   /api/v1/admin/users/{user_id}/assign-medicine (Add medicine to user dashboard)
GET    /api/v1/admin/care-requests (View all care requests)
PUT    /api/v1/admin/care-requests/{id}/assign-vet
```

**User Care Endpoints**:
```
GET    /api/v1/care-services (Browse available services)
POST   /api/v1/livestock/{id}/care-request (Request care for livestock)
GET    /api/v1/livestock/{id}/care-history
GET    /api/v1/my-care-requests
GET    /api/v1/my-dashboard/assigned-care (Care assigned by admin)
GET    /api/v1/my-dashboard/assigned-medicines
POST   /api/v1/care-requests/{id}/accept (Accept admin-assigned care)
POST   /api/v1/care-requests/{id}/pay
```

**Veterinarian Endpoints**:
```
GET    /api/v1/vet/care-requests (Assigned to me)
PUT    /api/v1/vet/care-requests/{id}/start
POST   /api/v1/vet/care-requests/{id}/complete
POST   /api/v1/vet/care-requests/{id}/prescribe-medicine
```

**Database Schema**:
```sql
CREATE TYPE care_type_enum AS ENUM ('vaccination', 'checkup', 'treatment', 'emergency', 'grooming', 'surgery');
CREATE TYPE care_status_enum AS ENUM ('scheduled', 'in_progress', 'completed', 'cancelled');
CREATE TYPE care_assignment_type_enum AS ENUM ('user_requested', 'admin_assigned');

CREATE TABLE care_services (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    care_type care_type_enum NOT NULL,
    description TEXT,
    base_cost DECIMAL(10, 2) NOT NULL,
    duration_minutes INT,
    is_active BOOLEAN DEFAULT TRUE,
    created_by UUID REFERENCES users(id),
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE medicines (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    description TEXT,
    manufacturer VARCHAR(255),
    dosage_info TEXT,
    unit_price DECIMAL(10, 2) NOT NULL,
    stock_quantity INT DEFAULT 0,
    expiry_date DATE,
    storage_requirements TEXT,
    requires_prescription BOOLEAN DEFAULT TRUE,
    created_by UUID REFERENCES users(id),
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE care_requests (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    livestock_id UUID REFERENCES livestock(id) NOT NULL,
    user_id UUID REFERENCES users(id) NOT NULL,
    service_id UUID REFERENCES care_services(id),
    veterinarian_id UUID REFERENCES users(id),
    assignment_type care_assignment_type_enum DEFAULT 'user_requested',
    assigned_by UUID REFERENCES users(id), -- Admin who assigned
    status care_status_enum DEFAULT 'scheduled',
    scheduled_at TIMESTAMPTZ,
    completed_at TIMESTAMPTZ,
    diagnosis TEXT,
    treatment_notes TEXT,
    total_cost DECIMAL(10, 2),
    payment_id UUID REFERENCES payments(id),
    is_emergency BOOLEAN DEFAULT FALSE,
    is_paid BOOLEAN DEFAULT FALSE,
    admin_notes TEXT, -- Notes from admin when assigning
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE care_request_medicines (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    care_request_id UUID REFERENCES care_requests(id),
    medicine_id UUID REFERENCES medicines(id),
    quantity INT NOT NULL,
    dosage VARCHAR(100),
    frequency VARCHAR(100), -- "twice daily", "once weekly"
    duration_days INT,
    cost DECIMAL(10, 2) NOT NULL,
    prescribed_by UUID REFERENCES users(id),
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Admin can assign care/medicine directly to users
CREATE TABLE user_assigned_care (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) NOT NULL,
    livestock_id UUID REFERENCES livestock(id) NOT NULL,
    service_id UUID REFERENCES care_services(id),
    medicine_id UUID REFERENCES medicines(id),
    assigned_by UUID REFERENCES users(id) NOT NULL, -- Admin
    reason TEXT,
    quantity INT,
    is_mandatory BOOLEAN DEFAULT FALSE,
    cost DECIMAL(10, 2),
    due_date DATE,
    status VARCHAR(50) DEFAULT 'pending', -- pending, accepted, completed, rejected
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_care_requests_user ON care_requests(user_id, created_at DESC);
CREATE INDEX idx_care_requests_vet ON care_requests(veterinarian_id, status);
CREATE INDEX idx_user_assigned_care ON user_assigned_care(user_id, status);
```

---

### 2.7 Marketplace Service (NEW - RESTRICTED)
**Technology**: Rust (Actix-Web) + PostgreSQL + Elasticsearch

**Important Business Rules**:
1. **Admin can list new livestock** (from outside ecosystem) with system ownership
2. **Users can ONLY list livestock they OWN within the ecosystem**
3. Users CANNOT add livestock from outside
4. **Feed packages are NOT in marketplace** (they're in feed management menu)
5. Marketplace is ONLY for buying/selling livestock owned within platform

**Responsibilities**:
- System livestock listings (Admin-added)
- User livestock re-sale
- Buy/sell operations
- Order management
- Rating and review system
- Search and filtering

**Admin Endpoints**:
```
POST   /api/v1/admin/marketplace/livestock (List new livestock from outside)
DELETE /api/v1/admin/marketplace/listings/{id}
PUT    /api/v1/admin/marketplace/listings/{id}/featured
GET    /api/v1/admin/marketplace/orders
```

**User Endpoints**:
```
GET    /api/v1/marketplace/livestock (Browse available livestock)
GET    /api/v1/marketplace/livestock/{id}
POST   /api/v1/marketplace/livestock/{id}/buy
POST   /api/v1/marketplace/my-livestock/{id}/list (Can only list owned livestock)
DELETE /api/v1/marketplace/listings/{id} (Can only delete own listings)
GET    /api/v1/marketplace/my-listings
GET    /api/v1/marketplace/my-orders
POST   /api/v1/marketplace/orders/{id}/review
GET    /api/v1/marketplace/search
```

**Validation Logic**:
```rust
pub async fn list_livestock(
    user_id: Uuid,
    livestock_id: Uuid,
    price: Decimal,
    repo: &Repository,
) -> Result<MarketplaceListing, ServiceError> {
    // CRITICAL: Verify user owns this livestock
    let livestock = repo.get_livestock_by_id(livestock_id).await?;
    
    // Check ownership
    if livestock.owner_id != Some(user_id) {
        return Err(ServiceError::Unauthorized(
            "You can only list livestock you own in this ecosystem".to_string()
        ));
    }
    
    // Check status
    if livestock.status != LivestockStatus::Assigned {
        return Err(ServiceError::BadRequest(
            "Livestock must be in 'assigned' status to list".to_string()
        ));
    }
    
    // Create listing
    repo.create_marketplace_listing(MarketplaceListing {
        seller_id: user_id,
        livestock_id,
        price,
        listing_type: ListingType::UserResale,
        status: ListingStatus::Active,
        ..Default::default()
    }).await
}
```

**Database Schema**:
```sql
CREATE TYPE listing_type_enum AS ENUM ('system_livestock', 'user_resale');
CREATE TYPE listing_status_enum AS ENUM ('active', 'sold', 'expired', 'removed');

CREATE TABLE marketplace_listings (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    seller_id UUID REFERENCES users(id) NOT NULL, -- Can be admin for system livestock
    livestock_id UUID REFERENCES livestock(id) NOT NULL,
    listing_type listing_type_enum NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    price DECIMAL(12, 2) NOT NULL,
    negotiable BOOLEAN DEFAULT FALSE,
    images TEXT[],
    status listing_status_enum DEFAULT 'active',
    views_count INT DEFAULT 0,
    featured BOOLEAN DEFAULT FALSE, -- Only admin can set featured
    metadata JSONB,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW(),
    expires_at TIMESTAMPTZ,
    UNIQUE(livestock_id, status) -- One active listing per livestock
);

-- Constraint: Users can only list livestock they own
CREATE OR REPLACE FUNCTION check_listing_ownership()
RETURNS TRIGGER AS $
BEGIN
    -- If seller is not admin
    IF NOT EXISTS (SELECT 1 FROM users WHERE id = NEW.seller_id AND role = 'admin') THEN
        -- Check if seller owns the livestock
        IF NOT EXISTS (
            SELECT 1 FROM livestock 
            WHERE id = NEW.livestock_id 
            AND owner_id = NEW.seller_id
            AND status = 'assigned'
        ) THEN
            RAISE EXCEPTION 'You can only list livestock you own in this ecosystem';
        END IF;
        
        -- Set listing type to user_resale
        NEW.listing_type := 'user_resale';
    ELSE
        -- Admin can list system livestock
        NEW.listing_type := 'system_livestock';
    END IF;
    
    RETURN NEW;
END;
$ LANGUAGE plpgsql;

CREATE TRIGGER enforce_listing_ownership
BEFORE INSERT ON marketplace_listings
FOR EACH ROW EXECUTE FUNCTION check_listing_ownership();

CREATE TYPE order_status_enum AS ENUM ('pending', 'confirmed', 'processing', 'completed', 'cancelled', 'refunded');

CREATE TABLE marketplace_orders (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    order_number VARCHAR(50) UNIQUE NOT NULL,
    buyer_id UUID REFERENCES users(id) NOT NULL,
    seller_id UUID REFERENCES users(id) NOT NULL,
    listing_id UUID REFERENCES marketplace_listings(id),
    total_amount DECIMAL(12, 2) NOT NULL,
    payment_id UUID REFERENCES payments(id),
    status order_status_enum DEFAULT 'pending',
    delivery_address TEXT,
    notes TEXT,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    completed_at TIMESTAMPTZ
);

CREATE TABLE marketplace_reviews (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    order_id UUID REFERENCES marketplace_orders(id) NOT NULL,
    reviewer_id UUID REFERENCES users(id) NOT NULL,
    reviewee_id UUID REFERENCES users(id) NOT NULL,
    rating INT CHECK (rating >= 1 AND rating <= 5),
    comment TEXT,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_marketplace_listings_status ON marketplace_listings(status, created_at DESC);
CREATE INDEX idx_marketplace_orders_buyer ON marketplace_orders(buyer_id, created_at DESC);
```

---

### 2.8 Stream Service (NEW - JETSON NANO INTEGRATION)
**Technology**: Rust (Actix-Web) + WebRTC + RTMP + PostgreSQL + Jetson Nano Edge AI

**Important Architecture Notes**:
1. **Cameras are connected to Jetson Nano devices on-site (NOT online directly)**
2. **Jetson Nano performs edge AI detection** (sheep recognition by ID card)
3. **Jetson Nano streams to local RTMP server** on-site
4. **Local RTMP relays to cloud streaming infrastructure**
5. **Each sheep has unique ID card** for Jetson detection
6. **Real-time detection events** are sent to backend

**Responsibilities**:
- Manage Jetson Nano device registrations
- Receive detection events from Jetson devices
- Stream relay management
- Video recording and storage
- Stream authentication and authorization
- Historical footage retrieval
- Multiple camera management per farm

**Jetson Nano Endpoints** (Device-to-Server):
```
POST   /api/v1/jetson/devices/register
POST   /api/v1/jetson/devices/{id}/heartbeat
POST   /api/v1/jetson/detection-events (Sheep detected with ID)
POST   /api/v1/jetson/stream/start
POST   /api/v1/jetson/stream/stop
POST   /api/v1/jetson/health-status
```

**User Endpoints**:
```
GET    /api/v1/streams/livestock/{id}
GET    /api/v1/streams/livestock/{id}/token
GET    /api/v1/streams/livestock/{id}/recordings
GET    /api/v1/streams/recordings/{id}/play
POST   /api/v1/streams/snapshot/{livestock_id}
GET    /api/v1/livestock/{id}/detection-history
GET    /api/v1/livestock/{id}/last-seen
```

**Admin Endpoints**:
```
POST   /api/v1/admin/jetson/devices
PUT    /api/v1/admin/jetson/devices/{id}
DELETE /api/v1/admin/jetson/devices/{id}
GET    /api/v1/admin/jetson/devices
POST   /api/v1/admin/cameras
GET    /api/v1/admin/cameras
PUT    /api/v1/admin/cameras/{id}
DELETE /api/v1/admin/cameras/{id}
POST   /api/v1/admin/cameras/{id}/assign-livestock
```

**Technology Stack**:
```
- RTMP Server: Nginx-RTMP or MediaMTX
- Video Processing: FFmpeg
- Streaming Protocol: HLS (HTTP Live Streaming) or WebRTC
- CDN: CloudFront or Cloudflare Stream
- Storage: S3 for recordings
- IoT Protocol: MQTT for sensor data
```

**Database Schema**:
```sql
-- Jetson Nano devices on-site
CREATE TABLE jetson_devices (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    device_name VARCHAR(255) NOT NULL,
    farm_id UUID REFERENCES farms(id) NOT NULL,
    device_serial VARCHAR(100) UNIQUE NOT NULL,
    ip_address INET,
    local_rtmp_url TEXT NOT NULL, -- Local RTMP server address
    relay_rtmp_url TEXT, -- Cloud RTMP relay URL
    model_version VARCHAR(50), -- AI model version
    status VARCHAR(50) DEFAULT 'offline', -- online, offline, maintenance
    last_heartbeat TIMESTAMPTZ,
    capabilities JSONB, -- {detection: true, night_vision: false, etc}
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TYPE camera_status_enum AS ENUM ('online', 'offline', 'maintenance', 'error');

CREATE TABLE cameras (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    jetson_device_id UUID REFERENCES jetson_devices(id) ON DELETE CASCADE,
    farm_id UUID REFERENCES farms(id) NOT NULL,
    camera_name VARCHAR(255) NOT NULL,
    camera_location VARCHAR(255), -- Area/barn/pen location
    camera_angle VARCHAR(50), -- front, side, top, etc.
    rtmp_input_url TEXT NOT NULL, -- Input from Jetson
    hls_output_url TEXT, -- HLS output for users
    status camera_status_enum DEFAULT 'offline',
    resolution VARCHAR(20) DEFAULT '1080p',
    fps INT DEFAULT 30,
    is_recording BOOLEAN DEFAULT TRUE,
    is_active BOOLEAN DEFAULT TRUE,
    detection_enabled BOOLEAN DEFAULT TRUE, -- AI detection on/off
    assigned_area VARCHAR(100), -- Pen/barn area for this camera
    metadata JSONB,
    last_online_at TIMESTAMPTZ,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Sheep detection events from Jetson
CREATE TABLE livestock_detection_events (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    livestock_id UUID REFERENCES livestock(id) NOT NULL,
    jetson_device_id UUID REFERENCES jetson_devices(id) NOT NULL,
    camera_id UUID REFERENCES cameras(id) NOT NULL,
    unique_id_card VARCHAR(100) NOT NULL, -- ID card detected
    confidence_score DECIMAL(5, 4), -- AI confidence 0.0000 to 1.0000
    detection_image_url TEXT, -- Snapshot of detection
    bounding_box JSONB, -- {x, y, width, height}
    location_area VARCHAR(100), -- Where detected
    detected_at TIMESTAMPTZ NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Create hypertable for detection events (time-series data)
SELECT create_hypertable('livestock_detection_events', 'detected_at');

CREATE TABLE stream_recordings (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    camera_id UUID REFERENCES cameras(id) NOT NULL,
    jetson_device_id UUID REFERENCES jetson_devices(id),
    start_time TIMESTAMPTZ NOT NULL,
    end_time TIMESTAMPTZ,
    duration_seconds INT,
    file_size_mb DECIMAL(10, 2),
    storage_url TEXT NOT NULL,
    thumbnail_url TEXT,
    detected_livestock_ids UUID[], -- Array of detected livestock IDs
    is_available BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE stream_access_tokens (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) NOT NULL,
    livestock_id UUID REFERENCES livestock(id) NOT NULL,
    camera_id UUID REFERENCES cameras(id) NOT NULL,
    token VARCHAR(500) UNIQUE NOT NULL,
    expires_at TIMESTAMPTZ NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- IoT sensor data (temperature, humidity, etc. from Jetson)
CREATE TABLE iot_sensor_data (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    jetson_device_id UUID REFERENCES jetson_devices(id) NOT NULL,
    farm_id UUID REFERENCES farms(id) NOT NULL,
    sensor_type VARCHAR(50) NOT NULL, -- temperature, humidity, air_quality
    value DECIMAL(10, 2) NOT NULL,
    unit VARCHAR(20),
    location_area VARCHAR(100),
    recorded_at TIMESTAMPTZ NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Create hypertable for IoT sensor data
SELECT create_hypertable('iot_sensor_data', 'recorded_at');

CREATE INDEX idx_jetson_devices_farm ON jetson_devices(farm_id, status);
CREATE INDEX idx_cameras_jetson ON cameras(jetson_device_id, status);
CREATE INDEX idx_detection_livestock ON livestock_detection_events(livestock_id, detected_at DESC);
CREATE INDEX idx_detection_jetson ON livestock_detection_events(jetson_device_id, detected_at DESC);
CREATE INDEX idx_recordings_camera ON stream_recordings(camera_id, start_time DESC);
CREATE INDEX idx_iot_jetson_time ON iot_sensor_data(jetson_device_id, recorded_at DESC);
```

**Stream Service Implementation**:
```rust
use actix_web::{web, HttpResponse, Result};
use uuid::Uuid;

pub struct StreamService {
    rtmp_server_url: String,
    cdn_base_url: String,
    jwt_secret: String,
}

impl StreamService {
    pub async fn generate_stream_token(
        &self,
        user_id: Uuid,
        livestock_id: Uuid,
    ) -> Result<String, ServiceError> {
        // Verify user owns this livestock
        // Generate JWT token with expiration
        // Return token for authenticated stream access
    }

    pub async fn get_hls_playlist(
        &self,
        livestock_id: Uuid,
        token: String,
    ) -> Result<String, ServiceError> {
        // Validate token
        // Return HLS playlist URL
    }

    pub async fn create_snapshot(
        &self,
        livestock_id: Uuid,
    ) -> Result<String, ServiceError> {
        // Capture current frame from stream
        // Upload to S3
        // Return snapshot URL
    }
}
```

---

### 2.9 Game Service (Enhanced)
**Technology**: Rust (Actix-Web) + PostgreSQL + Redis

**Additional Features for Pilot Farming**:
- Daily challenges and tasks
- Achievement system
- Virtual currency and rewards
- Leaderboards
- Social features (friends, competitions)

**New Endpoints**:
```
GET    /api/v1/games/pilot-farming/dashboard
GET    /api/v1/games/pilot-farming/tasks
POST   /api/v1/games/pilot-farming/tasks/{id}/complete
GET    /api/v1/games/pilot-farming/achievements
GET    /api/v1/games/pilot-farming/leaderboard
POST   /api/v1/games/pilot-farming/challenges/{id}/join
```

**Additional Schema**:
```sql
CREATE TABLE pilot_farming_tasks (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) NOT NULL,
    livestock_id UUID REFERENCES livestock(id) NOT NULL,
    task_type VARCHAR(50) NOT NULL, -- feed, health_check, clean, exercise
    description TEXT,
    reward_coins INT DEFAULT 0,
    reward_xp INT DEFAULT 0,
    status VARCHAR(20) DEFAULT 'pending',
    due_date DATE,
    completed_at TIMESTAMPTZ,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE user_achievements (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) NOT NULL,
    achievement_type VARCHAR(50) NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    icon_url TEXT,
    reward_coins INT DEFAULT 0,
    unlocked_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE user_game_stats (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) UNIQUE NOT NULL,
    level INT DEFAULT 1,
    xp_points INT DEFAULT 0,
    total_coins INT DEFAULT 0,
    tasks_completed INT DEFAULT 0,
    livestock_raised INT DEFAULT 0,
    perfect_care_streak INT DEFAULT 0,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

---

### 2.10 Analytics Service (Enhanced)
**Technology**: Rust (Actix-Web) + PostgreSQL + ClickHouse

**Responsibilities**:
- Financial analytics (ROI, profit/loss)
- Livestock performance tracking
- User behavior analytics
- Market trends analysis
- Custom reports generation

**Key Endpoints**:
```
GET    /api/v1/analytics/financial/roi
GET    /api/v1/analytics/financial/profit-loss
GET    /api/v1/analytics/livestock/performance
