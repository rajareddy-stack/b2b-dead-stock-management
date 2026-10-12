# DEAD STOCK MARKETPLACE - B2B Surplus Web Application
> **Connecting Sellers | Buyers | Sustainable Supply Chains**  
> Built with **Java 17, Spring Boot 3, Spring Data JPA, Spring Security, MySQL / H2, Thymeleaf, and Bootstrap 5**.

---

## 1. Abstract & Objectives
The **Dead Stock Marketplace** is a B2B web application designed to bridge the gap between businesses with excess or dead inventory (manufacturers, fabricators, retailers, construction firms, restaurants) and buyers seeking discounted industrial supplies.

Rather than discarding dormant materials into landfills, sellers liquidate dead stock to recover tied-up capital. Buyers search by location, negotiate prices in real-time through a dynamic negotiation engine, and coordinate yard pickup.

### Key Objectives:
- Provide a centralized B2B marketplace for industrial surplus.
- Enable sellers to liquidate dead stock and recover tied-up capital.
- Allow buyers to source discounted raw materials locally.
- Implement proximity/location-based search and direct price negotiation.
- Reduce industrial landfill waste and promote circular economy supply chains.

---

## 2. Technology Stack

- **Backend**:
  - Java 17
  - Spring Boot 3.2.5
  - Spring Security (Role-based access control with BCrypt)
  - Spring Data JPA & Hibernate
  - Spring Validation
- **Frontend**:
  - HTML5, CSS3, JavaScript
  - Bootstrap 5.3 & Bootstrap Icons
  - Thymeleaf Template Engine + Spring Security Dialect
- **Database**:
  - Embedded H2 database (zero setup out-of-the-box, console at `/h2-console`)
  - MySQL 8.0 support (configurable in `application-mysql.properties`)
- **API & Docs**:
  - RESTful APIs documented with **Springdoc OpenAPI 3 / Swagger UI**

---

## 3. Project Architecture & Folder Structure

```
ttproject/
├── pom.xml
├── apache-maven-3.9.6/                      # Bundled Maven runtime
└── src/
    ├── main/
    │   ├── java/com/deadstock/marketplace/
    │   │   ├── DeadStockApplication.java
    │   │   ├── config/                      # SecurityConfig, OpenApiConfig, DataInitializer
    │   │   ├── controller/
    │   │   │   ├── api/                     # REST Controllers (Auth, Listings, Offers, Admin)
    │   │   │   └── web/                     # Thymeleaf Web Controllers (Home, Seller, Buyer, Admin, Profile)
    │   │   ├── dto/                         # DTOs & Form Objects
    │   │   ├── model/                       # JPA Entities (User, BusinessProfile, Category, Listing, Offer, Notification)
    │   │   ├── model/enums/                 # Role, OfferStatus, ListingStatus, VerificationStatus, ItemCondition
    │   │   ├── repository/                  # Spring Data JPA Repositories
    │   │   └── service/                     # UserService, ListingService, OfferService, AdminService, NotificationService
    │   └── resources/
    │       ├── application.properties       # Default H2 database configuration
    │       ├── application-mysql.properties # MySQL 8.0 configuration profile
    │       ├── schema.sql                   # Database table DDL script
    │       ├── static/
    │       │   └── css/styles.css           # B2B Portal Custom Styles
    │       └── templates/                   # Bootstrap 5 + Thymeleaf HTML Views
    │           ├── layout/base.html
    │           ├── home.html
    │           ├── auth/login.html & register.html
    │           ├── listings/browse.html & detail.html
    │           ├── seller/dashboard.html, create-listing.html, incoming-offers.html
    │           ├── buyer/dashboard.html
    │           ├── admin/dashboard.html
    │           └── profile/view.html & notifications.html
    └── test/
        └── java/com/deadstock/marketplace/
            └── DeadStockApplicationTests.java
```

---

## 4. Key Modules & Features

### 🏢 Seller Module
- Register and login with commercial credentials.
- Business profile verification (submit Tax ID / GST / EIN).
- Create & manage surplus inventory listings (photos, original price, discounted price, quantity, measurement unit, condition, and yard location).
- Review incoming buyer offers, decline offers, accept deals, or propose counter-offers with custom messages.

### 🛒 Buyer Module
- Search inventory by proximity/location (city) and category.
- View detailed item specifications, discount percentages, and seller verification badge.
- Dynamic Negotiation Engine: Submit purchase offers specifying desired quantity and unit price.
- Deal Tracking: Monitor status (`PENDING`, `COUNTER_OFFERED`, `ACCEPTED`, `REJECTED`).
- Accept or decline seller counter-offers to confirm transactions.

### 🛡️ Admin Module
- Business Verification Queue: Inspect company details, tax registration IDs, and approve/reject businesses.
- Listing Moderation: Flag and delist reported or non-compliant surplus postings.
- Platform Analytics & KPIs:
  - Total registered companies (Sellers vs. Buyers)
  - Active surplus inventory value
  - Total negotiations & accepted deal volume ($)
  - Category-wise inventory counts

### 🔔 Notification Engine
- In-app notification alerts for negotiation events ("New offer received", "Counter-offer proposed", "Deal confirmed", "Business verified").

---

## 5. Pre-configured Demo Accounts

The application automatically seeds realistic data, sample categories, active listings, and test accounts on first launch:

| Role | Email | Password | Business Name | Status |
|---|---|---|---|---|
| **Admin** | `admin@deadstock.com` | `password123` | Dead Stock Marketplace Ops | **VERIFIED** |
| **Seller** | `seller@metrosteel.com` | `password123` | Metro Steel & Alloy Fabricators | **VERIFIED** |
| **Buyer** | `buyer@apexbuild.com` | `password123` | Apex Commercial Builders | **VERIFIED** |
| **Seller (Pending)** | `seller2@novapack.com` | `password123` | Nova Industrial Packaging | **PENDING** |

*(The login page also has 1-click **Demo Autofill buttons** for quick testing!)*

---

## 6. How to Run the Project

### Prerequisites
- **Java 17** or higher installed (`java -version`)
- (Optional) **MySQL 8.0** if you wish to run against MySQL rather than the default embedded H2 database.

### Option A: Run with Embedded H2 Database (Recommended for Grading & Demo)
No database setup is required. The database persists automatically in `./data/deadstockdb`.

```powershell
# In PowerShell inside the project directory:
.\apache-maven-3.9.6\bin\mvn.cmd spring-boot:run
```

Once started:
- **Web Application**: `http://localhost:8080/`
- **Swagger / OpenAPI Documentation**: `http://localhost:8080/swagger-ui.html`
- **H2 Database Console**: `http://localhost:8080/h2-console`
  - JDBC URL: `jdbc:h2:file:./data/deadstockdb`
  - User: `sa`
  - Password: *(leave blank)*

### Option B: Run with Local MySQL Database
1. Create database:
   ```sql
   CREATE DATABASE deadstock_db;
   ```
2. Check/update your MySQL username and password in `src/main/resources/application-mysql.properties`:
   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/deadstock_db?createDatabaseIfNotExist=true&useSSL=false
   spring.datasource.username=root
   spring.datasource.password=YOUR_PASSWORD
   ```
3. Run with the MySQL Spring profile:
   ```powershell
   .\apache-maven-3.9.6\bin\mvn.cmd spring-boot:run -Dspring-boot.run.profiles=mysql
   ```

---

## 7. Main REST API Endpoints (Section 10)

| Method | Endpoint | Description | Access |
|---|---|---|---|
| `POST` | `/api/auth/register` | Register new business user (Seller/Buyer) | Public |
| `POST` | `/api/auth/login` | Authenticate user and create session | Public |
| `GET` | `/api/listings` | Search surplus items by location, category, keyword | Public |
| `POST` | `/api/listings` | Post dead stock listing | Authenticated Seller |
| `POST` | `/api/offers` | Submit purchase offer / negotiation proposal | Authenticated Buyer |
| `POST` | `/api/offers/{id}/counter` | Seller proposes counter-offer price | Authenticated Seller |
| `POST` | `/api/offers/{id}/accept` | Accept offer / counter-offer and lock deal | Authenticated Party |
| `POST` | `/api/offers/{id}/reject` | Decline offer | Authenticated Party |
| `GET` | `/api/admin/stats` | Platform analytics & transaction volume | Admin Only |
| `PUT` | `/api/admin/verify/{id}` | Approve business verification | Admin Only |
| `PUT` | `/api/admin/reject/{id}` | Reject business verification | Admin Only |

---

## 8. Database Tables & ER Architecture (Section 7)

- `users` (user_id, email, password, full_name, role, enabled, created_at)
- `business_profiles` (company_id, user_id, company_name, tax_id, address, city, state, postal_code, phone, industry_type, verification_status, created_at)
- `categories` (category_id, title, description, icon)
- `listings` (listing_id, seller_id, category_id, title, description, original_price, price, quantity, unit, item_condition, location, image_url, status, created_at)
- `offers` (offer_id, listing_id, buyer_id, offer_price, quantity, counter_price, status, buyer_message, seller_message, created_at, updated_at)
- `notifications` (notification_id, user_id, title, message, is_read, link_url, created_at)
