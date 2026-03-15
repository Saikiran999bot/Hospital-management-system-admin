# MediCare+ Admin Panel — README

## Overview

The Admin Panel (`admin.html`) is the management interface for hospital staff. It is a single-file HTML application deployed on **Vercel** that connects to the same backend API as the patient portal.

---

## Accessing the Admin Panel

Navigate to your Vercel URL followed by `/admin.html`

Example: `https://your-site.vercel.app/admin.html`

Login with the admin credentials set in your backend configuration.

---

## Panels & Features

### 📊 Dashboard
- Live stats: total bookings, pending count, confirmed, emails sent today
- 7-day booking trend line chart
- Appointment status donut chart (Confirmed / Pending / Completed / Cancelled)
- Pending appointments list with quick Confirm button
- Doctor availability toggles for today

### 📅 Appointments
- Full table of all appointments with search, date filter, and status filter
- Edit any appointment: change date, time, status, add doctor notes
- **Save & Notify Patient** — on save, patient receives email + WhatsApp (if configured)
- Quick Confirm / Cancel buttons for pending appointments
- Status options: Pending, Confirmed, Rescheduled, Completed, Cancelled

### 👨‍⚕️ Doctors
- Add, edit, deactivate doctors
- Set availability days, consultation hours, fee
- Toggle **Available Today** per doctor (shown live on patient portal)
- Doctor cards show specialization, qualifications, experience

### 👥 Patients
- View all registered patients with their details
- Block or unblock patient accounts
- Click the bell icon to directly send a notification to a specific patient

### 🔔 Notifications (In-App)
- Compose and send in-app notifications to all patients or a specific patient
- Notification types: Announcement, Offer, Appointment Update
- View sent notification history

### 📧 Email Broadcast
- Send bulk emails to **all patients** or a **specific patient**
- Quick templates available: Offer, Health Tip, Reminder, Health Camp, Custom
- Live preview before sending
- Shows recipient count before confirming
- All sends logged in Email Logs

### 📬 Email Logs
- Full history of every email sent, failed, or skipped
- Shows recipient, subject, status, error reason, and timestamp
- Summary bar: sent / failed / skipped count
- Topbar pill shows email failures in real time

### 🏷️ Offers & Health Packages
- Add, edit, delete offers shown on the patient portal homepage
- Toggle each offer visible/hidden without deleting
- Set sort order to control display sequence
- Live Preview strip shows exactly how it looks to patients

### 🔑 API Keys & Email
- Set and update all API keys from the browser — no server restart needed
- Keys stored securely in Supabase, not in code
- Sections: Gemini AI, Brevo Email, Telegram Bot, WhatsApp (Twilio)
- Test any channel (email/telegram/whatsapp) directly from the panel
- **Email Diagnostic Tool** — enter an email and get the exact Brevo API response

---

## Technology Stack

| Layer | Technology |
|---|---|
| Language | HTML, CSS, JavaScript (vanilla) |
| Charts | Chart.js 4.4.1 |
| Icons | Font Awesome 6.5 |
| Fonts | Inter (Google Fonts) |
| Hosting | Vercel |
| API | Connects to Express backend on Render |

---

## Configuration

Open `admin.html` and update the **first line** of the `<script>` block:

```js
const B = "https://your-render-app.onrender.com";
```

Replace with your actual Render deployment URL.

---

## Deployment

1. Push `admin.html` to your GitHub repository
2. Vercel deploys it automatically alongside `user.html`
3. Access via `https://your-site.vercel.app/admin.html`

---

## How API Keys Are Stored

All API keys (Gemini, Brevo, Telegram, Twilio) are stored in the **Supabase `settings` table** — not in environment variables or code files. This means:

- You can update any key at any time from the admin panel
- No redeployment to Render is needed after changing keys
- Keys are never exposed in the frontend HTML

The only environment variables required on Render are the Supabase connection credentials.

---

## Notification Flow

When admin updates an appointment status, the patient is automatically notified via:

| Channel | Trigger |
|---|---|
| In-app notification | Always (all status changes) |
| Email (Brevo) | If Brevo is configured in API Keys |
| WhatsApp (Twilio) | If Twilio is configured in API Keys |

---

## Security Notes

- The admin panel is protected by JWT authentication
- Admin token is stored in browser localStorage
- All admin API routes (`/api/admin/*`) require a valid admin token
- Incorrect credentials show an error — no access is granted
- There is no public registration for admin accounts
