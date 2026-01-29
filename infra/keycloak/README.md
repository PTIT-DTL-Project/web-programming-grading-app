# Keycloak Realm Setup (DEV)

This folder is used to store the exported Keycloak realm configuration for local development.

## Steps (first time on your machine)

1. Start Keycloak without auto-import (one time only):

   ```bash
   docker compose up -d keycloak
   ```

2. Open Keycloak Admin Console at http://localhost:8080

3. Login with:
   - Username: `admin`
   - Password: `admin` (or the values you configured in docker-compose)

4. Create realm, roles, clients and demo users:
   - Realm: `web-programming-grading`
   - Realm roles: `STUDENT`, `TEACHER`, `ADMIN`
   - Clients, for example:
     - `grading-frontend` (public, for React)
     - `grading-api` (confidential/bearer-only, for Spring Boot API)
   - Demo users (optional, but recommended for DEV):
     - `student1` with role `STUDENT`
     - `teacher1` with role `TEACHER`
     - `admin1` with role `ADMIN`
 
 Đăng nhập bằng user mình tạo phải bằng đường dẫn : http://localhost:8080/realms/tên realm mình tạo/account



