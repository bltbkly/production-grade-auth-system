# Security Demo (Spring Boot)

A Spring Boot 4 demo application with a strong focus on security, combining classic web authentication (Thymeleaf + form login) with stateless JWT APIs.

The project demonstrates practical security controls such as role-based access, password policy validation, account lockout, request rate limiting, JWT access/refresh token flow, and audit logging.

## Security Highlights

- Role-based authorization for both web pages and API routes
- Strong password policy and validation rules
- Account lockout after repeated failed attempts
- API rate limiting to reduce abuse/brute-force pressure
- JWT access/refresh token model with refresh token rotation and revocation
- Security event auditing (login success/failure, registration, refresh, logout)
- Hardened HTTP headers (HSTS, CSP, frame rules)

## Tech Stack

- Java 21
- Spring Boot 4.0.5
- Spring Security
- Spring MVC + Thymeleaf
- Spring Data JPA
- H2 (default) and MySQL (optional)
- JWT (`jjwt`)
- Gradle

## Key Features

- Form login for web pages (`/sign-in`) with role-aware authorization
- JWT authentication for API endpoints under `/api/**`
- Access + refresh token flow with refresh token rotation
- Password policy checks (length, uppercase/lowercase, digit, special char)
- Temporary account lockout after repeated failed logins
- Simple per-client rate limiting filter
- Security audit records for login/register/refresh/logout events
- Seeded demo data on startup (except in `test` profile)

## Prerequisites

- JDK 21
- (Optional) MySQL if you want to run with external DB instead of H2

## Getting Started

1. Clone/open the project.
2. Run the application:

```bash
./gradlew bootRun
```

3. Open the app:
   - Web UI: [http://localhost:8000](http://localhost:8000)
   - H2 Console: [http://localhost:8000/h2-console](http://localhost:8000/h2-console)

### Default Demo Users

Created automatically on first startup:

- `user@example.com` / `user123`
- `manager@example.com` / `manager123`
- `admin@example.com` / `admin123`

## Running Tests

```bash
./gradlew test
```

## Database Configuration

By default, the app runs with in-memory H2 (`src/main/resources/application.properties`).

To use MySQL, comment out H2 datasource lines and uncomment the MySQL block in `application.properties`.

## Main Web Routes

- Public: `/`, `/index`, `/sign-in`, `/sign-up`
- Authenticated: `/profile`, `/change-password`
- Manager/Admin: `/posts`
- Admin only: `/users`

## Main API Routes

Base path: `/api`

- `POST /api/auth/register`
- `POST /api/auth/login`
- `POST /api/auth/refresh`
- `POST /api/auth/logout`
- `GET /api/users/me` (requires Bearer token)

## Security Configuration Notes

Important properties are under `app.security.*` in `application.properties`, including:

- `app.security.jwt.secret`
- JWT lifetimes
- password policy flags
- lockout settings
- rate-limit settings

For production:

- Set a strong and unique `APP_SECURITY_JWT_SECRET` environment variable
- Use MySQL or another persistent database (not in-memory H2)
- Review and tighten `app.security.lock.*` and `app.security.rate-limit.*`
- Keep security headers enabled and terminate TLS correctly at your edge/proxy

