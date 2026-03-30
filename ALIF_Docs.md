<p align="center">
  <img src="https://alifsemi.com/wp-content/uploads/2023/05/ALIF-LOGO-SVG-1.svg" width="400" alt="Alif Semiconductor Logo">
</p>

<h1 align="center"><b>Alif Ensemble E7: Master Documentation Hub</b></h1>

<p align="center">
  <b><i>Tài liệu hướng dẫn toàn diện về hệ sinh thái Alif Ensemble E7 - Từ kiến trúc phần cứng đến thực thi phần mềm.</i></b>
</p>

<p align="center">
  <img src="images/TongQuan/tong-quan.jpg" width="100%" alt="Alif E7 Overview">
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="MIT License"></a>
  <a href="https://www.alifsemi.com/"><img src="https://img.shields.io/badge/Platform-Ensemble_E7-orange?logo=microchip&logoColor=white" alt="Alif E7"></a>
  <a href="#tools"><img src="https://img.shields.io/badge/Tools-Cortex--M55%20%7C%20Ethos--U55-blue" alt="Hardware"></a>
  <a href="#status"><img src="https://img.shields.io/badge/Status-Development-brightgreen" alt="Status"></a>
</p>

---

## 🚀 Giới thiệu

Tài liệu này tập trung vào việc khai thác sức mạnh của dòng **Alif Ensemble E7**, giải pháp MCU tiên phong kết hợp giữa nhân hiệu năng cao **Cortex-M55** và bộ tăng tốc AI **Ethos-U55 NPU**. Nội dung được thiết kế nhằm giúp người dùng nhanh chóng tiếp cận từ lý thuyết kiến trúc đến triển khai ứng dụng thực tế trên các Kit phát triển.

---

## 📖 Cấu trúc tài liệu (Documentation Structure)

Để dễ dàng quản lý và tra cứu, nội dung được chia thành hai phần chuyên biệt:

### 📑 [Phần 1: Tổng quan MCU & Kiến trúc Kit](./docs/01_hardware_overview.md)

_Nắm vững nền tảng phần cứng và lý thuyết vận hành hệ thống._

- **Alif MCU Family:** Giới thiệu tổng quan về các dòng vi điều khiển của Alif Semiconductor.
- **Kiến trúc MCU:** Mô tả chi tiết các hệ thống con (Subsystems), cách thức hoạt động phối hợp giữa các nhân.
- **Luồng dữ liệu (Data Flow):** Phân tích cách dữ liệu di chuyển trong hệ thống để tối ưu hóa hiệu suất AI.
- **Hardware Showcase:** Giới thiệu và so sánh giữa **Ensemble DevKit E7** và **AppKit E7**.

### 🛠️ [Phần 2: Hệ sinh thái Phần mềm & Toolchain](./docs/02_software_ecosystem.md)

_Hướng dẫn thực hành, cài đặt công cụ và quy trình triển khai code._

- **Software Ecosystem:** Giới thiệu các công cụ đặc thù dùng cho việc phát triển trên E7.
- **Setup Guide:** Hướng dẫn chi tiết cài đặt và cấu hình môi trường làm việc chuyên nghiệp.
- **Deployment Workflow:** Cách thức biên dịch, ký file bảo mật (Signing) và quy trình nạp code xuống Kit.
- **Troubleshooting:** Tổng hợp các lỗi thường gặp và giải pháp xử lý nhanh.

---

## 🛠️ Công cụ & Môi trường (Tools)

Dưới đây là các công cụ cốt lõi cần thiết để bắt đầu dự án với Alif E7:

| Công cụ                   | Phiên bản | Chức năng chính                                             |
| :------------------------ | :-------- | :---------------------------------------------------------- |
| **Alif Security Toolkit** | v2.x      | Ký và đóng gói firmware bảo mật (Signing/Provisioning).     |
| **Arm Keil MDK**          | v5.37+    | IDE lập trình, biên dịch và gỡ lỗi (Debug) chính.           |
| **App_Gen Configurator**  | 1.1       | Công cụ cấu hình Memory Map, Pinmux và tài nguyên hệ thống. |
| **Open CMSIS-Pack**       | Mới nhất  | Cung cấp thư viện hỗ trợ thiết bị (Device Support Archive). |

---

## 📬 Liên hệ & Kết nối (Contact)

Tôi luôn sẵn sàng thảo luận, chia sẻ kiến thức hoặc hợp tác trong các dự án khai phá sức mạnh của **Alif Ensemble E7**. Đừng ngần ngại kết nối với tôi qua các nền tảng dưới đây:

<p align="left">
  <a href="mailto:tuannna.work@gmail.com">
    <img src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white" alt="Gmail" />
  </a>
</p>

---

<p align="center">
  <sub>Copyright © 2026 <b>AhaTuna</b> • Thiết kế cho cộng đồng Alif Semiconductor 🦀</sub>
</p>
