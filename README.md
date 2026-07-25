<div align="center">

# 🔗 TaskLink

### *A Hyperlocal Marketplace for Everyday Tasks*

**Connect task posters with skilled taskers in their neighborhood — bid, chat, pay, and get things done.**

[![Flutter](https://img.shields.io/badge/Flutter-3.x-02569B?style=for-the-badge&logo=flutter&logoColor=white)](https://flutter.dev)
[![Dart](https://img.shields.io/badge/Dart-3.10+-0175C2?style=for-the-badge&logo=dart&logoColor=white)](https://dart.dev)
[![Django](https://img.shields.io/badge/Django-5.2-092E20?style=for-the-badge&logo=django&logoColor=white)](https://djangoproject.com)
[![PostgreSQL](https://img.shields.io/badge/PostGIS-PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)](https://postgis.net)
[![Firebase](https://img.shields.io/badge/Firebase-FCM-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)](https://firebase.google.com)
[![Version](https://img.shields.io/badge/Version-1.0.0-brightgreen?style=for-the-badge)](https://github.com)

</div>

---

## 📋 Table of Contents

- [🎯 Project Overview](#-project-overview)
- [🎬 Video Demo](#-video-demo)
- [✨ Key Features](#-key-features)
- [🏗️ Technologies and Architecture](#-technologies-and-architecture)
- [📖 How to Use](#-how-to-use)
- [🔒 Privacy and Security](#-privacy-and-security)
- [📝 Release Notes](#-release-notes)

---

## 🎯 Project Overview

**TaskLink** is a full-stack, hyperlocal task marketplace built as a Final Year Project (FYP). It enables everyday users to post tasks they need done — cleaning, delivery, handyman work, moving, gardening, and more — and connect with nearby taskers who bid on and complete those tasks.

The platform features a **dual-role system**: every user can act as both a **Task Poster** (someone who needs help) and a **Tasker** (someone who offers their services). The entire lifecycle — from posting a task to making a payment, resolving disputes, and leaving reviews — is handled in-app.

> 📍 **Hyperlocal Focus:** Tasks are geo-tagged with PostGIS. Taskers only see tasks within a configurable radius from their current location, ensuring highly relevant local matches.

---

## 🎬 Video Demo

## 🎥 Demo Video



https://github.com/user-attachments/assets/62e8f7b9-3cd1-494a-800f-1218074e5d6d



---

## ✨ Key Features

### 👤 Authentication and User Management
- 📱 **Phone-number-first registration** with OTP verification via SMS
- 📧 **Email verification** for account security
- 🔐 **JWT-based authentication** with rotating refresh tokens
- 🔑 **Forgot password flow** with email-based OTP reset
- 🌓 **Dark / Light theme** toggle stored persistently
- 👤 **Rich user profiles** — name, bio, profile picture, date of birth, gender, and star ratings

### 📋 Task Lifecycle
- ✍️ **Post tasks** with title, description, category, budget, and geo-tagged location
- 🗺️ **Google Maps integration** for precise task location picking
- 🏷️ **9 task categories** — Cleaning, Delivery, Handyman, Moving, Assembly, Gardening, Painting, Plumbing, Electrical, and Other
- 📍 **Radius-based task visibility** (configurable up to 50 km) — only nearby taskers see relevant tasks
- 📸 **Task media uploads** — attach images to task descriptions and completion proofs
- 🔄 **Full task state machine** — BIDDING → ASSIGNED → IN_PROGRESS → COMPLETED → CONFIRMED / DISPUTED / CANCELLED
- ⏰ **Task expiry** — tasks auto-expire after a configured deadline
- 🔔 **Mutual cancellation workflow** — both parties must agree before an in-progress task is cancelled
- 📊 **Task limits** — users may post up to 5 active tasks and take up to 3 tasks simultaneously

### 💰 Bidding System
- 🤝 **Taskers bid on tasks** with a custom price and pitch message
- ✅ **Poster accepts a bid** — all other bids are automatically rejected
- 🔄 **Bid status tracking** — Active, Accepted, Rejected

### 💬 Real-Time Chat
- ⚡ **WebSocket-powered messaging** via Django Channels + Daphne (ASGI)
- 🖼️ **Rich media support** — images, videos, audio, and files in chat
- 📤 **Pre-upload media workflow** — files uploaded first, then attached to messages via WebSocket
- ✔️ **Message read receipts**
- 🗑️ **Soft-delete messages** — messages are marked as deleted, not permanently removed
- 🔄 **Auto-reconnect** when the app resumes from background

### 💳 Payments and Wallet
- 🏦 **In-app wallet** — users top up and spend from their balance
- 🔐 **Escrow system** — payment is held securely when a bid is accepted; released only after task completion is confirmed by the poster
- 🔒 **Escrow locking** during disputes — funds cannot be released until admin resolves the case
- 📲 **JazzCash / EasyPaisa top-up** via TID verification — a Termux SMS gateway polls incoming payment SMSes, parses the Transaction ID, and credits the user wallet automatically
- 💸 **Withdrawal support** — users withdraw earnings to JazzCash, EasyPaisa, or Bank accounts
- 📜 **Full immutable transaction ledger** — every credit and debit logged with balance-after snapshot
- 🏷️ **Human-readable transaction labels** — top-up, task payment, task refund, withdrawal approved/failed

### ⚖️ Dispute Resolution
- 🚩 **Raise disputes** on tasks at any conflicted state
- 📸 **Evidence upload** — attach images as proof when raising a dispute
- 👑 **Admin-only resolution** — admin decides to release payment to tasker or refund to poster
- 🔓 **Automatic escrow unlock** on dispute resolution

### ⭐ Reviews and Ratings
- 🌟 **Mutual reviews** — both poster and tasker can review each other after task confirmation
- 📊 **Dynamic rating average** — each user's star rating is recalculated live
- 💬 **Written comments** — up to 1,000 characters per review

### 🔔 Notifications
- 🔥 **Firebase Cloud Messaging (FCM)** push notifications for all key events
- 🔕 **Background message handling** — notifications arrive even when the app is closed
- 📡 **WebSocket notification stream** — real-time in-app badge counts and banners
- 🏷️ **15 notification types** — bids, task status changes, payments, disputes, chat messages, withdrawals, cancellations, and more
- 🔴 **Unread badge counter** on the bottom navigation bar

### 📍 Location Services
- 📡 **Real-time device location** using Geolocator
- 🗺️ **Google Maps** for interactive, map-based task location picking
- 📏 **Distance-based task filtering** using PostGIS GeoDjango spatial queries

---

## 🏗️ Technologies and Architecture

### 📱 Mobile App — Flutter

| Category | Technology |
|---|---|
| Framework | Flutter 3.x (Dart ≥ 3.10) |
| State Management | GetX (`get ^4.7.3`) |
| Networking | Dio (`dio ^5.9.2`), `http` |
| Real-time | `web_socket_channel ^3.0.3` |
| Push Notifications | Firebase Messaging + `flutter_local_notifications` |
| Maps | Google Maps Flutter |
| Location | Geolocator + Permission Handler |
| Secure Storage | `flutter_secure_storage` |
| Persistent Storage | `shared_preferences` |
| Fonts | Google Fonts — Inter (all 9 weights) |
| Image Handling | `image_picker`, `cached_network_image` |
| Animations | Lottie |
| Icons | Iconsax |
| Theming | Custom Light / Dark themes via `AppTheme` |
| Routing | GetX named routes with `AppPages` |
| Dependency Injection | GetX bindings |

### 🖥️ Backend — Django REST Framework

| Category | Technology |
|---|---|
| Framework | Django 5.2 + Django REST Framework 3.15 |
| Authentication | JWT via `djangorestframework-simplejwt 5.4` |
| Real-time | Django Channels 4.2 + Daphne 4.1 (ASGI) |
| Database | PostgreSQL 14+ with PostGIS (GeoDjango) |
| Caching | Redis via `django-redis` |
| Channel Layer | Redis (Production) / InMemory (Development) |
| Task Queue | Celery 5.4 + Redis |
| Storage | Local filesystem / AWS S3 (Boto3) |
| API Documentation | drf-spectacular (Swagger UI + ReDoc) |
| SMS Gateway | Termux Flask — JazzCash/EasyPaisa SMS polling + parsing |
| Email | Brevo (Sendinblue) SMTP API |
| Push Notifications | Firebase Admin SDK 7.4 |
| Rate Limiting | `django-ratelimit` |
| Phone Validation | `phonenumbers` |
| Geospatial | GDAL 3.10 + GeoDjango + PostGIS |

### 🏛️ Architecture Diagram

```
┌─────────────────────────────────────────────────┐
│               Flutter Mobile App                 │
│   GetX State Mgmt │ Dio HTTP Client               │
│   WebSocket Client │ FCM Push Notifications       │
└──────────┬─────────────────────┬─────────────────┘
           │   REST API (HTTP)   │   WebSocket (WS)
           ▼                     ▼
┌─────────────────────────────────────────────────┐
│         Django + Daphne (ASGI Server)            │
│   DRF REST API    │   Django Channels (WS)        │
│   JWT Auth        │   Chat + Notif Consumers      │
│   GeoDjango       │   Background SMS Poller       │
└────┬──────────────┬──────────────┬───────────────┘
     ▼              ▼              ▼
┌──────────┐  ┌──────────┐  ┌──────────────────────┐
│PostgreSQL│  │  Redis   │  │ External Services     │
│ +PostGIS │  │ Cache +  │  │ Firebase FCM          │
│          │  │ Channels │  │ Brevo Email           │
└──────────┘  └──────────┘  │ Termux SMS Gateway   │
                             └──────────────────────┘
```

---

## 📖 How to Use



### 🧑 As a Task Poster

1. **Register** — Sign up with your phone number, verify via OTP, and complete your profile.
2. **Top Up Your Wallet** — Navigate to **Wallet → Top Up**, make a JazzCash or EasyPaisa payment to the displayed account, and enter your Transaction ID (TID). Your wallet is credited automatically when the SMS is received.
3. **Post a Task** — Tap **Post a Task**, fill in the title, category, description, and budget, then pin your location on the Google Maps interface.
4. **Review Bids** — Browse bids received from nearby taskers, including their offered price, pitch message, and profile rating.
5. **Accept a Bid** — Tap **Accept** on the bid you prefer. The task amount is immediately placed in escrow.
6. **Chat with Tasker** — Use in-app chat to coordinate timing, share directions, or clarify requirements.
7. **Confirm Completion** — Once the tasker marks the task done and uploads proof, review it and tap **Confirm** to release payment to the tasker.
8. **Leave a Review** — Rate and write a review for the tasker after the task is confirmed.

### 🛠️ As a Tasker

1. **Register** — Create your account and fill out your profile to build trust with posters.
2. **Browse Tasks** — The **Home** feed shows tasks posted near your current location.
3. **Filter by Category** — Filter tasks by category: Cleaning, Delivery, Handyman, and more.
4. **Place a Bid** — Open a task, review the details and budget, then submit your bid amount and a short message.
5. **Get Hired** — Receive a push notification when a poster accepts your bid.
6. **Coordinate via Chat** — Discuss logistics with the poster through the built-in chat.
7. **Complete the Task** — Mark the task as **In Progress**, do the work, upload completion proof photos, and mark it **Complete**.
8. **Get Paid** — Once the poster confirms, the escrowed amount is credited to your wallet.
9. **Withdraw Earnings** — Navigate to **Wallet → Withdraw**, choose your payout method (JazzCash, EasyPaisa, or Bank), and submit a withdrawal request.

### ⚖️ Raising a Dispute

If a disagreement arises, either party can raise a dispute directly from the task detail screen. Upload supporting evidence images, describe the issue, and submit. An admin will review the case and decide to either release payment to the tasker or refund the poster.

---

## 🔒 Privacy and Security

- 🔐 **JWT Authentication** — Every API endpoint requires a valid Bearer token. Access tokens expire and refresh tokens rotate on every use.
- 🔑 **OTP Rate Limiting** — Maximum 5 OTPs per hour, 3 verification attempts, and a 30-minute block on excessive failures.
- 🏦 **Escrow Protection** — Funds are never released without explicit poster confirmation or an admin ruling on a dispute.
- 🛡️ **Idempotent TID Redemption** — Each JazzCash/EasyPaisa TID is unique-constrained in the database; it can only be redeemed once, preventing double crediting.
- 🔒 **Secure Token Storage** — JWT tokens are stored in `flutter_secure_storage` on-device (Android Keystore / iOS Keychain), not in plain SharedPreferences.
- 🌐 **CORS** — The backend enforces a strict allowed origins list in production.
- 🚫 **Rate Limiting** — Sensitive endpoints (OTP, login, payment) are protected with `django-ratelimit`.
- 🗑️ **Soft Deletes** — Messages are soft-deleted to preserve chat history integrity.
- 📍 **Location Privacy** — A user's last known location is stored server-side solely for geo-filtering and is never exposed to other users.
- 🔑 **UUID Primary Keys** — All 20 models use UUIDs as primary keys, preventing sequential ID enumeration attacks.
- 🔒 **Dispute Escrow Lock** — Opening a dispute automatically locks the escrow, preventing any unilateral payout until admin resolution.

---

## 📝 Release Notes

See [RELEASE_NOTES.md](./RELEASE_NOTES.md) for the complete version history and changelog.

---

<div align="center">

**Built with ❤️ as a Final Year Project**

*TaskLink — Connecting people. Getting things done.*

</div>
