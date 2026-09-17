# Payment Matching AI

Hệ thống quản lý khách hàng, giao dịch chuyển khoản và đơn thanh toán bằng AI, hoạt động trên Angular frontend + Express backend + PostgreSQL/Neon + Google Gemini.

## Tổng quan

Dự án cho phép:

- tải ảnh giao dịch chuyển khoản
- trích xuất thông tin giao dịch bằng Gemini AI
- đối chiếu giao dịch với khách hàng phù hợp theo số tài khoản và độ tương đồng tên
- quản lý danh sách khách hàng và thông tin ngân hàng
- tạo và theo dõi đơn thanh toán
- xác nhận giao dịch đã khớp với đơn thanh toán
- hiển thị dashboard nghiệp vụ trên giao diện Angular

## Công nghệ sử dụng

- Frontend: Angular 18
- Backend: Node.js + Express
- Database: PostgreSQL trên Neon
- AI: Google Gemini
- Upload: Multer
- CORS và env config: dotenv, cors

## Demo và repo

- Frontend repo: https://github.com/hoangphuc2k5/LinKQ.Ai_Demo_Fontend
- Backend repo: https://github.com/hoangphuc2k5/LinKQ.Ai_Demo_backend
- Production frontend: https://demo-ai-theta.vercel.app
- Production backend API: https://demo-ai-api.vercel.app/api

## Kiến trúc hệ thống

```text
Angular Frontend
      |
      v
Express API Server
      |
      +--> Controllers
      +--> Services
      +--> Repositories
      +--> PostgreSQL (Neon)
      +--> Gemini AI
```

### Cấu trúc thư mục chính

```text
project/
├─ backend/
│  ├─ src/
│  │  ├─ config/
│  │  ├─ controllers/
│  │  ├─ dtos/
│  │  ├─ repositories/
│  │  ├─ routes/
│  │  ├─ services/
│  │  ├─ models/
│  │  └─ server.js
│  ├─ public/
│  ├─ .env.example
│  ├─ package.json
│  └─ package-lock.json
├─ frontend/
│  ├─ src/
│  ├─ angular.json
│  ├─ package.json
│  └─ tsconfig*.json
├─ README.md
└─ .gitignore
```

## Tính năng chính

- Quản lý khách hàng: thêm, sửa, xóa, xem danh sách
- Quản lý giao dịch: upload ảnh, trích xuất nội dung, gắn khách hàng phù hợp
- Quản lý thanh toán: tạo đơn, cập nhật trạng thái, đối soát với giao dịch thực tế
- Tự động khớp: dựa trên số tài khoản, tên khách hàng, nội dung giao dịch và mã thanh toán
- Hệ thống REST API rõ ràng cho frontend và quản trị

## Yêu cầu môi trường

- Node.js 18+
- npm hoặc pnpm
- Một database PostgreSQL trên Neon
- Một API key Google Gemini

## Cài đặt local

### 1) Clone dự án

```bash
git clone https://github.com/hoangphuc2k5/LinKQ.Ai_Demo_backend.git backend
git clone https://github.com/hoangphuc2k5/LinKQ.Ai_Demo_Fontend.git frontend
```

Nếu bạn đang làm việc trong workspace local hiện tại, bạn chỉ cần mở folder `backend` và `frontend` như 2 project riêng.

### 2) Cài đặt dependency backend

```bash
cd backend
npm install
```

### 3) Cấu hình biến môi trường backend

Tạo file `.env` dựa trên `.env.example`:

```bash
cp .env.example .env
```

Nội dung tham khảo:

```env
PORT=3000
GEMINI_API_KEY=your_gemini_api_key_here
GEMINI_MODEL=gemini-2.0-flash
DATABASE_URL=postgresql://user:password@host:5432/dbname?sslmode=require
MATCH_THRESHOLD=70
```

Hoặc nếu bạn dùng cấu hình từng biến riêng:

```env
DB_USER=your_neon_user
DB_PASSWORD=your_neon_password
DB_SERVER=your_neon_host
DB_NAME=neondb
DB_PORT=5432
DB_ENCRYPT=true
DB_TRUST_SERVER_CERTIFICATE=false
```

> Lưu ý: file `.env` không nên commit lên Git; hãy giữ riêng trên máy local. File `.env.example` dùng để làm template.

### 4) Khởi động backend

```bash
npm run dev
```

Backend sẽ chạy tại:

```text
http://localhost:3000
```

Kiểm tra sức khỏe API:

```bash
curl http://localhost:3000/api/health
```

### 5) Cài đặt và chạy frontend

```bash
cd ../frontend
npm install
npm start
```

Frontend thường chạy tại:

```text
http://localhost:4200
```

## API chính

### Health check

- GET /api/health

### Customers

- GET /api/customers
- POST /api/customers
- PUT /api/customers/:id
- DELETE /api/customers/:id

### Transactions

- GET /api/transactions
- POST /api/transactions/analyze
- PUT /api/transactions/:id/confirm

### Payment orders

- GET /api/payment-orders
- POST /api/payment-orders
- PATCH /api/payment-orders/:id/status
- POST /api/payment-orders/:id/settle

## Database

Dự án hiện sử dụng Neon PostgreSQL và tự tạo schema khi khởi động nếu chưa tồn tại. Các bảng chính bao gồm:

- Customers
- Transactions
- PaymentOrders

Cấu hình connect đang ưu tiên đọc `DATABASE_URL`, nếu không có thì sẽ build từ các biến `DB_USER`, `DB_PASSWORD`, `DB_SERVER`, `DB_PORT`, `DB_NAME`.

## Quy tắc an toàn

- Không commit file `.env` lên repository
- Chỉ dùng `.env.example` làm template để chia sẻ cấu hình
- Nếu database hoặc API key đã bị lộ, hãy đổi ngay khóa tương ứng trên nền tảng tương ứng

## Deploy

### Frontend (Vercel)

- Build Angular app với `ng build`
- Publish thư mục `dist` cho project Angular
- Có thể dùng Vercel với project `frontend`

### Backend (Vercel)

- Dùng Node.js server trong project `backend`
- Cấu hình biến môi trường trên Vercel: `DATABASE_URL`, `GEMINI_API_KEY`, `PORT`
- Dùng `npm start` hoặc chạy server trên runtime Node

## Kịch bản nghiệp vụ

1. Người dùng nhập khách hàng vào hệ thống.
2. Upload ảnh giao dịch chuyển khoản.
3. Backend gửi ảnh tới Gemini để trích xuất thông tin như số tài khoản, số tiền, nội dung giao dịch.
4. Hệ thống đối chiếu với danh sách khách hàng.
5. Người dùng xác nhận khách hàng phù hợp.
6. Tạo đơn thanh toán hoặc đối soát với giao dịch đã xác nhận.
7. Hệ thống cập nhật trạng thái thanh toán.

## Lưu ý phát triển

- Nếu bạn thấy lỗi tương tự `pool.request is not a function`, hãy kiểm tra rằng backend đang dùng `pg` chứ không phải `mssql`.
- Nếu frontend không gọi được backend, hãy kiểm tra `API_BASE` trong file `frontend/src/app/core/services/api.service.ts`.
- Nếu database không kết nối, hãy kiểm tra `DATABASE_URL` hoặc các biến `DB_*` trên môi trường.

## License

Dự án này sử dụng cho mục đích demo và phát triển nội bộ.

## Liên hệ

Nếu cần hỗ trợ xây dựng, mở rộng tính năng hoặc sửa lỗi production, bạn có thể liên hệ với người phát triển của dự án.

    MatchService --> MatchCandidate
    MatchService --> Customer

    Customer "1" --> "*" PaymentOrder
    Customer "1" --> "*" Transaction : matched
    PaymentOrder --> Transaction : settle
    AnalyzeResponse --> Transaction
    AnalyzeResponse --> ExtractedPaymentInfo
    AnalyzeResponse --> MatchCandidate
    MatchCandidate --> Customer
    UploadComponent --> AnalyzeResponse
```

## Yêu cầu môi trường

- Node.js và npm.
- SQL Server đang chạy.
- Gemini API key.

## Cấu hình database

Repo hiện không chứa file `database/schema.sql`. Hãy tạo database và các bảng
`Customers`, `Transactions`, `PaymentOrders` theo môi trường SQL Server của bạn.

Bảng `PaymentOrders` phải có cột mã thanh toán. Nếu bảng đã tồn tại, chạy:

```sql
IF COL_LENGTH('dbo.PaymentOrders', 'PaymentCode') IS NULL
BEGIN
  ALTER TABLE dbo.PaymentOrders
    ADD PaymentCode NVARCHAR(100) NULL;
END;

IF NOT EXISTS (
  SELECT 1
  FROM sys.indexes
  WHERE name = 'UX_PaymentOrders_PaymentCode'
    AND object_id = OBJECT_ID('dbo.PaymentOrders')
)
BEGIN
  CREATE UNIQUE INDEX UX_PaymentOrders_PaymentCode
    ON dbo.PaymentOrders (PaymentCode)
    WHERE PaymentCode IS NOT NULL;
END;
```

## Cấu hình backend

```powershell
cd backend
npm install
Copy-Item .env.example .env
```

Mở `backend/.env` và điền thông tin SQL Server cùng Gemini API key:

```dotenv
PORT=3000
GEMINI_API_KEY=your_gemini_api_key_here
GEMINI_MODEL=gemini-2.0-flash
DB_USER=sa
DB_PASSWORD=your_password
DB_SERVER=localhost
DB_NAME=PaymentMatchingDB
DB_PORT=1433
DB_ENCRYPT=true
DB_TRUST_SERVER_CERTIFICATE=true
MATCH_THRESHOLD=70
```

Chạy backend:

```powershell
npm run dev
```

Backend mặc định lắng nghe tại `http://localhost:3000`.

## Cấu hình frontend

```powershell
cd frontend
npm install
npm run dev
```

Frontend Angular mặc định chạy tại `http://localhost:4200`. URL backend được
khai báo trong `frontend/src/app/core/services/api.service.ts`:

```ts
const API_BASE = 'http://localhost:3000/api';
```

Build production:

```powershell
npm run build
```

## API chính

### Transactions

| Method | Endpoint | Mô tả |
|---|---|---|
| `POST` | `/api/transactions/analyze` | Upload ảnh với field `file`, gọi Gemini, đối chiếu và lưu giao dịch |
| `GET` | `/api/transactions` | Lấy danh sách giao dịch |
| `GET` | `/api/transactions/:id` | Lấy chi tiết giao dịch |
| `PUT` | `/api/transactions/:id/confirm` | Xác nhận khách hàng với body `{ "customerId": 1 }` |

### Customers

| Method | Endpoint | Mô tả |
|---|---|---|
| `GET` | `/api/customers` | Lấy danh sách khách hàng |
| `GET` | `/api/customers/:id` | Lấy chi tiết khách hàng |
| `POST` | `/api/customers` | Tạo khách hàng |
| `PUT` | `/api/customers/:id` | Cập nhật khách hàng |
| `DELETE` | `/api/customers/:id` | Xóa khách hàng |

### Payment orders

| Method | Endpoint | Mô tả |
|---|---|---|
| `GET` | `/api/payment-orders?customerId=1` | Lấy đơn, có thể lọc theo khách hàng |
| `POST` | `/api/payment-orders` | Tạo đơn với `customerId`, `paymentCode`, `amount`, `currency` |
| `PATCH` | `/api/payment-orders/:id/status` | Cập nhật trạng thái đơn |
| `POST` | `/api/payment-orders/:id/settle` | Chuyển đơn sang `PAID` và gắn `transactionId` |

## Luồng tự động xác nhận thanh toán

Sau khi ảnh được phân tích và khách hàng được xác định:

1. Frontend tải các đơn `PENDING` của khách hàng.
2. Hệ thống kiểm tra `PaymentCode` có xuất hiện trong phần `content` của kết quả Gemini hay không.
3. Hệ thống so sánh `amount` của giao dịch với `Amount` của đơn hàng.
4. Nếu cả mã thanh toán và số tiền đều khớp, frontend gọi endpoint `settle`.
5. Backend cập nhật đơn thành `PAID` và lưu `PaidTransactionId`.
6. Giao diện hiển thị hộp thông báo xác nhận thành công.

## Logic đối chiếu khách hàng

Điểm đối chiếu nằm trong `backend/src/services/matchService.js`:

- Tối đa 70 điểm cho số tài khoản người chuyển trùng khớp.
- Tối đa 30 điểm cho độ tương đồng tên.
- Ngưỡng mặc định là `MATCH_THRESHOLD=70`.

Kết quả có thể là `MATCHED`, `NEEDS_REVIEW` hoặc `UNMATCHED`. Khi chưa chắc
chắn, người dùng có thể chọn khách hàng thủ công trên màn hình phân tích ảnh.

## Gemini AI

Prompt và phần gửi ảnh tới Gemini nằm trong:

`backend/src/services/geminiService.js`

Gemini được yêu cầu trả về JSON gồm ngân hàng, tài khoản, số tiền, mã giao
dịch, ngày giao dịch và nội dung chuyển khoản.

Có thể tạo Gemini API key tại:

https://aistudio.google.com/app/apikey

## Lưu ý bảo mật và triển khai

- Không commit `backend/.env`, API key hoặc mật khẩu SQL Server.
- Không commit `node_modules`, Angular cache và build output.
- Nên thêm xác thực và phân quyền trước khi triển khai production.
- Nên giới hạn rate limit cho endpoint phân tích ảnh vì mỗi lần gọi có thể phát sinh chi phí Gemini.
- CORS hiện được bật cho mục đích phát triển; cần giới hạn origin khi triển khai production.
- Ảnh giao dịch hiện được xử lý trong bộ nhớ; production nên dùng object storage nếu cần lưu lại ảnh gốc.
