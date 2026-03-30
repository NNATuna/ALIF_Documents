<p align="center">
  <img src="https://alifsemi.com/wp-content/uploads/2023/05/ALIF-LOGO-SVG-1.svg" width="400" alt="Alif Semiconductor Logo">
</p>

<h1 align="center">Phần 2: Hệ sinh thái Phần mềm & Toolchain</h1>

<p align="center">
  <b>Hướng dẫn thực hành, cài đặt công cụ, bảo mật và quy trình triển khai code trên E7.</b>
</p>

---
# 🛠️ 1. Môi trường & Công cụ

## 1.1. Tổng quan hệ sinh thái

Phần này cung cấp cái nhìn tổng quan về nền tảng E7, kiến trúc bảo mật cốt lõi và vai trò của hệ thống quản lý an toàn (CISS-M). Đây là các thành phần nền tảng cần hiểu trước khi thực hiện build và nạp firmware.

---

### 1.1.1. E7 Platform

E7 là một nền tảng SoC (System-on-Chip) tích hợp đa lõi, được thiết kế cho các ứng dụng nhúng hiệu năng cao và tiết kiệm năng lượng. Hệ thống bao gồm:

- **Application Core (A32):** xử lý các tác vụ hệ điều hành hoặc logic chính
- **Real-time Core (M55):** xử lý tín hiệu, điều khiển thời gian thực
- **NPU (Ethos-U55):** tăng tốc xử lý AI/ML
- **MRAM:** bộ nhớ không bay hơi dùng để lưu firmware
- **Peripheral subsystem:** GPIO, UART, SPI, I2C, DMA...

Điểm đặc biệt của E7 là toàn bộ hệ thống được thiết kế xoay quanh **bảo mật phần cứng (hardware security)** và khả năng xác thực firmware ngay từ khi khởi động.

---

### 1.1.2. Kiến trúc Secure Enclave (SE)

Secure Enclave (SE) là thành phần bảo mật trung tâm, chịu trách nhiệm kiểm soát toàn bộ quá trình khởi động và xác thực firmware.

Các chức năng chính:

- **Secure Boot:** đảm bảo chỉ firmware hợp lệ (đã ký) mới được phép chạy
- **Authentication:** xác minh tính toàn vẹn của firmware thông qua chữ ký số
- **Key Management:** quản lý khóa mã hóa và chứng chỉ
- **Isolation:** tách biệt vùng bảo mật với các lõi CPU chính

Trong quá trình nạp code, tất cả firmware đều phải đi qua SE dưới dạng một gói đặc biệt (**ATOC**). Nếu firmware không hợp lệ, hệ thống sẽ từ chối khởi động.

---

### 1.1.3. Vai trò của CISS-M

CISS-M (Chip Internal Secure State Machine) là cơ chế điều khiển trạng thái bảo mật nội bộ của chip.

Vai trò chính:

- **Quản lý trạng thái hệ thống:** điều phối các chế độ như boot, maintenance, secure mode
- **Kiểm soát quyền truy cập:** đảm bảo chỉ các thành phần hợp lệ mới được truy cập tài nguyên
- **Điều phối Secure Enclave:** hỗ trợ SE trong quá trình xác thực và khởi động
- **Xử lý lỗi bảo mật:** phát hiện và ngăn chặn các hành vi bất thường

Trong thực tế, CISS-M hoạt động như một “bộ điều phối trung tâm”, đảm bảo toàn bộ quy trình từ nạp firmware → xác thực → chạy chương trình diễn ra an toàn và đúng thứ tự.

---

### ✅ Tóm tắt

- **E7 Platform:** phần cứng đa lõi + bảo mật tích hợp
- **Secure Enclave:** kiểm soát boot và xác thực firmware
- **CISS-M:** quản lý trạng thái và điều phối bảo mật toàn hệ thống

👉 Ba thành phần này kết hợp với nhau tạo thành nền tảng bảo mật hoàn chỉnh, là lý do vì sao firmware bắt buộc phải được đóng gói và ký trước khi nạp bằng SETOOLS.

## 1.2. Công cụ chính

Phần này giới thiệu các công cụ cốt lõi cần sử dụng trong quá trình phát triển và nạp firmware lên E7. Đây là bộ công cụ tối thiểu để thực hiện workflow: build → đóng gói → flash.

---

### 1.2.1. SETOOLS (Security Toolkit)

SETOOLS là bộ công cụ chính thức từ Alif Semiconductor dùng để cấu hình, đóng gói và nạp firmware vào thiết bị E7.

Các đặc điểm chính:

- Cung cấp dưới dạng **executables độc lập** (Windows / Linux / macOS), không bắt buộc cài Python :contentReference[oaicite:0]{index=0}  
- Giao tiếp với thiết bị thông qua **SE-UART (ISP - In System Programming)** :contentReference[oaicite:1]{index=1}  
- Hỗ trợ toàn bộ quy trình bảo mật: tạo key, ký firmware, xác thực và flash

Các tool quan trọng:

- `tools-config`  
  → Cấu hình Part Number, Revision và kết nối thiết bị  

- `app-gen-rot`  
  → Tạo Root of Trust (key + certificate) cho hệ thống  

- `app-gen-toc`  
  → Đóng gói các file `.bin` thành **ATOC package** (định dạng firmware hợp lệ) :contentReference[oaicite:2]{index=2}  

- `app-write-mram`  
  → Nạp firmware vào MRAM qua SE-UART :contentReference[oaicite:3]{index=3}  

- `maintenance`  
  → Debug, kiểm tra trạng thái hệ thống và xử lý lỗi  

👉 Vai trò cốt lõi:  
SETOOLS là **cầu nối bắt buộc** giữa firmware và phần cứng E7.  
Không thể nạp trực tiếp `.bin`, mà phải thông qua **ATOC + signing**.

---

### 1.2.2. VSCode (IDE chính)

VSCode được sử dụng làm môi trường phát triển chính:

- Viết và quản lý source code
- Build project (qua Makefile / CMake / task)
- Tích hợp terminal để chạy SETOOLS
- Debug cơ bản (log UART, output)

👉 Lưu ý:  
VSCode **không trực tiếp flash firmware**, mà chỉ đóng vai trò:
- Editor + build system
- Gọi các lệnh SETOOLS qua terminal

---

### 1.2.3. Compiler (GCC / armclang)

Compiler dùng để biên dịch source code thành file `.bin` để nạp lên E7.

Hai lựa chọn chính:

- **Arm GNU Toolchain (GCC)**  
  - Miễn phí, phổ biến  
  - Dễ tích hợp với VSCode  

- **Arm Compiler 6 (armclang)**  
  - Tối ưu tốt hơn cho Cortex-M55 (đặc biệt với Helium)  
  - Phù hợp khi cần hiệu năng cao  

Output sau khi build:

- File `.elf` (debug)
- File `.bin` (dùng để đóng gói ATOC)

👉 Vai trò trong workflow:
Compiler chỉ tạo **firmware thô**,  
SETOOLS sẽ xử lý phần **bảo mật + đóng gói + nạp**.

---

### ✅ Tóm tắt

- **SETOOLS:** công cụ bắt buộc để đóng gói và flash firmware  
- **VSCode:** môi trường phát triển + build + chạy tool  
- **Compiler:** tạo file `.bin` từ source code  

👉 Ba thành phần này kết hợp tạo thành workflow hoàn chỉnh để nạp code lên E7.

## 1.3. Thành phần quan trọng trong workflow

Trong quá trình nạp firmware lên E7, có một số thành phần bắt buộc phải hiểu rõ. Các thành phần này liên kết với nhau theo chuỗi:  
`.bin → cấu hình → đóng gói (ATOC) → ký bảo mật → flash`

---

### 1.3.1. File `.bin` (firmware)

Đây là file nhị phân được tạo ra sau khi biên dịch source code bằng compiler (GCC hoặc armclang).

**Đặc điểm:**
- Là firmware thô, chưa có thông tin bảo mật  
- Mỗi lõi CPU có thể có một file `.bin` riêng (A32, M55...)  
- Được tạo từ file `.elf` sau khi build  

**Vai trò:**
- Là đầu vào chính cho quá trình đóng gói ATOC  
- Không thể nạp trực tiếp lên E7 nếu chưa qua xử lý của SETOOLS  

---

### 1.3.2. File cấu hình `app-cfg.json`

Đây là file cấu hình trung tâm dùng để mô tả cách firmware sẽ được đóng gói và nạp vào hệ thống.

**Nội dung chính:**
- Khai báo các file `.bin`  
- Định nghĩa địa chỉ nạp (memory address)  
- Gán firmware cho từng lõi CPU  
- Liên kết với file cấu hình phần cứng (từ Conductor)  

**Vai trò:**
- Là “bản thiết kế” cho ATOC package  
- Quyết định firmware sẽ được tổ chức và load như thế nào khi boot  

---

### 1.3.3. ATOC Package

ATOC (Application Table of Contents) là định dạng firmware cuối cùng mà E7 chấp nhận.

**Đặc điểm:**
- Là file đã được đóng gói và ký bảo mật  
- Chứa toàn bộ firmware và metadata  
- Được tạo ra bằng tool `app-gen-toc`  

**Vai trò:**
- Là file duy nhất được phép nạp vào MRAM  
- Được Secure Enclave kiểm tra trước khi cho phép chạy  

👉 Hiểu đơn giản:  
`.bin` là code → `ATOC` là firmware hợp lệ để chạy trên E7  

---

### 1.3.4. Root of Trust (RoT)

Root of Trust là nền tảng bảo mật dùng để xác thực firmware.

**Bao gồm:**
- Private key  
- Public key / certificate  

**Tạo bằng lệnh:**
    app-gen-rot

**Vai trò:**
- Dùng để ký firmware trong quá trình tạo ATOC  
- Secure Enclave sẽ dùng key này để xác minh firmware khi boot  

**Lưu ý:**
- Chỉ cần tạo 1 lần cho mỗi project  
- Nếu mất key → firmware cũ có thể không còn hợp lệ  

---

### Tóm tắt

- `.bin` → firmware thô sau khi build  
- `app-cfg.json` → mô tả cách đóng gói firmware  
- `ATOC` → firmware hoàn chỉnh để nạp  
- `RoT` → đảm bảo firmware là hợp lệ và an toàn  

👉 Workflow chuẩn:
Build → Config → Sign → Package → Flash

## 1.4. Tính năng bảo mật cần biết

E7 được thiết kế với cơ chế bảo mật phần cứng chặt chẽ. Trước khi nạp và chạy firmware, cần hiểu các cơ chế này để tránh lỗi trong quá trình deploy.

---

### 1.4.1. Secure Boot

Secure Boot là cơ chế đảm bảo thiết bị chỉ chạy firmware hợp lệ.

**Nguyên lý hoạt động:**
- Khi khởi động, Secure Enclave sẽ kiểm tra firmware trong MRAM
- Firmware phải có chữ ký hợp lệ
- Nếu xác thực thất bại → hệ thống sẽ không chạy code

**Ý nghĩa:**
- Ngăn chặn firmware giả mạo hoặc bị sửa đổi
- Đảm bảo code chạy trên thiết bị là “chính chủ”

**Lưu ý khi sử dụng:**
- Không thể bypass Secure Boot
- Mọi firmware đều phải được đóng gói qua ATOC và ký bằng key hợp lệ

---

### 1.4.2. Firmware Signing

Firmware Signing là quá trình ký số firmware trước khi nạp vào thiết bị.

**Quy trình:**
- Tạo Root of Trust (key + certificate)
- Dùng key để ký firmware khi tạo ATOC
- Secure Enclave dùng public key để xác minh

**Vai trò:**
- Đảm bảo firmware không bị thay đổi sau khi build
- Là điều kiện bắt buộc để firmware được chấp nhận

**Liên quan tool:**
- `app-gen-rot` → tạo key  
- `app-gen-toc` → thực hiện signing  

**Lưu ý:**
- Nếu key không khớp → firmware sẽ bị từ chối
- Không nên thay đổi key giữa chừng trong cùng project

---

### 1.4.3. MRAM Protection

MRAM là bộ nhớ không bay hơi dùng để lưu firmware trên E7, có cơ chế bảo vệ riêng.

**Đặc điểm:**
- Chỉ cho phép ghi theo block (alignment)
- Không cho phép ghi tùy ý vào mọi vùng nhớ
- Được kiểm soát bởi Secure Enclave

**Cơ chế bảo vệ:**
- Chỉ cho phép ghi thông qua SETOOLS
- Firmware phải đúng định dạng (ATOC)
- Có kiểm tra địa chỉ và kích thước khi ghi

**Lỗi thường gặp:**
- NACK / sai địa chỉ → do ghi sai alignment
- Ghi trực tiếp `.bin` → bị từ chối

**Lưu ý khi sử dụng:**
- Luôn flash qua `app-write-mram`
- Không ghi thủ công vào MRAM
- Đảm bảo firmware đã được đóng gói hợp lệ

---

### Tóm tắt

- **Secure Boot:** kiểm tra firmware khi khởi động  
- **Firmware Signing:** đảm bảo firmware hợp lệ  
- **MRAM Protection:** kiểm soát việc ghi firmware  

👉 Đây là lý do bắt buộc phải dùng SETOOLS trong toàn bộ workflow nạp code trên E7.

---

# ⚙️ 2. Cài đặt & Workflow trên VSCode

## 2.1. Chuẩn bị môi trường

Phần này hướng dẫn chi tiết cách cài đặt môi trường phát triển (VSCode, toolchain, SETOOLS, driver...).

👉 Xem hướng dẫn đầy đủ tại video:

[Xem video setup môi trường](https://www.youtube.com/watch?v=fns9HmVv0hs)
## 2.2. Kết nối phần cứng
### 2.3.1. Kết nối SE-UART
### 2.3.2. Cấu hình COM / Baudrate
### 2.3.3. Kiểm tra kết nối

## 2.4. Quy trình build & nạp code

---

# 🔧 3. Lỗi thường gặp & cách xử lý

## 3.1. Lỗi kết nối
### 3.1.1. Không nhận COM Port
### 3.1.2. Target không phản hồi
### 3.1.3. Sai baudrate

## 3.2. Lỗi trong quá trình flash
### 3.2.1. NACK / ISP error
### 3.2.2. Sai địa chỉ MRAM
### 3.2.3. Lỗi ATOC package

## 3.3. Lỗi hệ thống
### 3.3.1. Thiết bị bị treo
### 3.3.2. STOP mode
### 3.3.3. Boot loop

## 3.4. Lỗi bảo mật
### 3.4.1. Sai key / certificate
### 3.4.2. Lỗi signing
### 3.4.3. Secure boot fail

## 3.5. Cách khôi phục nhanh
### 3.5.1. Reset thiết bị
### 3.5.2. Xóa firmware (`erase`)
### 3.5.3. Dùng maintenance tool