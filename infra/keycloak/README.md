# Keycloak Realm Setup (DEV)
# Keycloak Realm Setup (DEV)

Thư mục này dùng để lưu file export realm của Keycloak (bao gồm realms, roles, users, clients) phục vụ cho phát triển local và chia sẻ qua Git.

Docker Compose đã được cấu hình để **tự động import** tất cả các file realm nằm trong thư mục này khi container Keycloak start.

## Cách bạn (máy A) export realm ra file và đẩy lên GitHub

1. Start Keycloak (máy bạn):

   ```bash
   docker compose up -d keycloak
   ```

2. Mở Keycloak Admin Console tại:

   - http://localhost:8080

3. Đăng nhập:

   - Username: `admin`
   - Password: `admin` (hoặc đúng với docker-compose của bạn)

4. Tạo realm, roles, clients, users theo ý bạn (ví dụ):

   - Realm: `web-programming-grading`
   - Realm roles: `STUDENT`, `TEACHER`, `ADMIN`
   - Clients:
     - `grading-frontend` (public, cho frontend)
     - `grading-api` (confidential/bearer-only, cho backend)
   - Demo users (tuỳ chọn):
     - `student1` role `STUDENT`
     - `teacher1` role `TEACHER`
     - `admin1` role `ADMIN`

5. Export realm (kèm users) ra file JSON trong thư mục này:

   ```bash
   # Đảm bảo container keycloak đang chạy
   docker compose ps

   # Export realm "web-programming-grading" ra file JSON (kèm users)
   docker exec -it keycloak \
     /opt/keycloak/bin/kc.sh export \
     --realm web-programming-grading \
     --file /opt/keycloak/data/import/web-programming-grading-realm.json \
     --users realm_file
   ```

   Sau lệnh này, trong repo của bạn sẽ có file:

   - infra/keycloak/web-programming-grading-realm.json

6. Commit & push file JSON lên GitHub:

   ```bash
   git add infra/keycloak/web-programming-grading-realm.json
   git commit -m "Add Keycloak realm export"
   git push
   ```

## Cách bạn của bạn (máy B) dùng lại realm/roles/users đã export
   Nếu đã có image + volume rồi thì chạy lệnh : docker compose down -v
1. Pull code từ GitHub (đã có file JSON realm trong infra/keycloak).


2. Start Keycloak:

   ```bash
   docker compose up -d keycloak
   ```

   - Docker Compose đã được cấu hình với:
     - command: ["start-dev", "--import-realm"]
     - Mount thư mục infra/keycloak vào trong container tại /opt/keycloak/data/import
   - Nên khi Keycloak start lần đầu, nó sẽ **tự động import** tất cả realm JSON trong thư mục đó (bao gồm roles, users, clients bạn đã tạo).

3. Bạn của bạn có thể đăng nhập bằng user bạn đã tạo (ví dụ student1, teacher1, admin1) theo đường dẫn:

   - http://localhost:8080/realms/<ten-realm>/account

   Ví dụ với realm web-programming-grading:

   - http://localhost:8080/realms/web-programming-grading/account

> Lưu ý: Import realm chỉ tự động chạy khi Keycloak start với --import-realm. Nếu sau này bạn đổi file JSON, nên xoá volume/data cũ hoặc tạo realm mới rồi import lại tuỳ nhu cầu DEV.



