# 📱 VKU Field Survey PWA — Khảo sát nhu cầu việc làm sinh viên

> **Mini-Project 1** — Môn học: **Cross-Platform Mobile App Development (VKU)**  
> **Sinh viên thực hiện:** Huỳnh Lưu Ly — MSSV: `23IT.B129`  
> **Vai trò:** Full-stack Developer (Đóng góp: 100%)  
> **Demo Live:** [survey-pwa.pages.dev](https://survey-pwa.pages.dev/)  
> **Repository:** [LiLLy3172005/survey-pwa](https://github.com/LiLLy3172005/survey-pwa)  

---

## 📌 Giới thiệu dự án

**VKU Field Survey PWA** là ứng dụng Web lũy tiến (Progressive Web App - PWA) phục vụ việc điều tra, phỏng vấn khảo sát nhu cầu việc làm của sinh viên trực tiếp tại hiện trường.

Ứng dụng được thiết kế theo kiến trúc **Offline-First**, cho phép người khảo sát ghi nhận thông tin, lấy tọa độ GPS thời gian thực và chụp ảnh hiện trường ngay cả khi **không có kết nối mạng Internet**. Toàn bộ dữ liệu được lưu trữ an toàn dưới thiết bị (`IndexedDB`) và **tự động đồng bộ** lên Google Sheet & Google Drive ngay khi kết nối mạng được khôi phục.

---

## 🌟 Tính năng chính

- 📝 **Khảo sát hiện trường:** Form nhập thông tin người phỏng vấn, tự động lấy dấu thời gian (timestamp) và bộ câu hỏi khảo sát chi tiết.
- 📍 **Định vị vị trí (Geolocation):** Tự động thu thập vĩ độ (latitude) và kinh độ (longitude) chính xác thông qua `navigator.geolocation`.
- 📸 **Chụp & Lưu ảnh hiện trường:** Hỗ trợ kích hoạt camera thiết bị (`capture="environment"`), mã hóa ảnh sang chuẩn Base64 để lưu trữ cục bộ.
- 💾 **Lưu trữ Offline Cục bộ:** Sử dụng **IndexedDB** (`sessions` object store) giúp lưu giữ phiên khảo sát ngay tức thì bất kể trạng thái mạng.
- 🔄 **Đồng bộ tự động (Auto-Sync):** Tự động lắng nghe sự kiện `online/offline`. Khi có kết nối mạng, ứng dụng tự động quét và tải các phiên chờ (`synced: false`) lên backend.
- 📊 **Cơ sở dữ liệu Google Sheet & Drive:** Sử dụng Google Apps Script Web App làm backend trung gian để ghi dữ liệu vào Google Sheet và tải ảnh trực tiếp lên Google Drive.
- 🚦 **Thanh trạng thái kết nối:** Thanh hiển thị trạng thái cố định: 🟢 **Online** (Đã kết nối) / 🟠 **Offline** (Chạy ngoại tuyến) / 🔵 **Syncing** (Đang đồng bộ).
- 📲 **Trải nghiệm PWA hoàn chỉnh:** Tích hợp `manifest.json` và Service Worker (`sw.js`) hỗ trợ cache app shell, cho phép cài đặt lên màn hình chính (Add to Home Screen) và chạy độc lập.

---

## 🏗️ Kiến trúc Kỹ thuật (Offline-First 3 Lớp)

```text
┌─────────────────────────────────────────────────────────┐
│                   FRONTEND PWA                          │
│     (HTML5 / CSS3 / Vanilla JS / Service Worker)        │
└──────────────────────────┬──────────────────────────────┘
                           │ (Lưu trữ trực tiếp)
                           ▼
┌─────────────────────────────────────────────────────────┐
│              LOCAL STORAGE (IndexedDB)                  │
│               [Object Store: "sessions"]                │
└──────────────────────────┬──────────────────────────────┘
                           │ (Tự động đồng bộ khi Online)
                           ▼
┌─────────────────────────────────────────────────────────┐
│             BACKEND (Google Apps Script API)            │
│                 [Web App: Code.gs]                      │
└────────────┬─────────────────────────────┬──────────────┘
             │ (Ghi bản ghi)               │ (Tải ảnh Base64)
             ▼                             ▼
┌─────────────────────────┐   ┌───────────────────────────┐
│   GOOGLE SHEETS (DB)    │   │   GOOGLE DRIVE (Photos)   │
└─────────────────────────┘   └───────────────────────────┘
