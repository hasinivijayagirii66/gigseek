# gigseek
**GigSeek** is a full-stack gig marketplace web app built with Flask, HTML, CSS, and JS. It streamlines freelance hiring by combining real-time, geolocation-based notifications with an AI-powered applicant ranking system. Designed to bridge the gap between local talent and businesses, it delivers a robust, automated matching experience.
# GigSeek 🎯

**GigSeek** is a full-stack gig economy web platform that connects **Hosts** (employers/clients) with **Gigsters** (freelancers/gig workers). Hosts can post short-term job listings, Gigsters can browse and apply, and when a match is made, contact details are revealed — enabling direct communication.

Built with **Flask**, **SQLAlchemy**, and **SQLite**, GigSeek features role-based access control, resume uploads, application status management, and an admin panel.

---

## ✨ Features

- **Role-based user system** — three roles: `Gigster`, `Host`, and `Admin`
- **Gig Feed** — Gigsters can browse and filter gigs by keyword and location
- **Gig Posting** — Hosts can post gigs with category, salary, work type, and experience level
- **Apply & Track** — Gigsters apply with a cover note; Hosts manage applications from their dashboard
- **Match Reveal** — Accepting an application reveals the Gigster's contact details to the Host
- **Resume Upload** — Gigsters upload a PDF resume during registration
- **Admin Panel** — View and manage all users and gigs
- **Notifications** — Hosts see a count of pending applications on the dashboard
- **Seed Data** — Pre-loaded 25 gig listings across 15 categories for quick demos

---

## 🛠️ Tech Stack

| Layer      | Technology                        |
|------------|-----------------------------------|
| Backend    | Python, Flask, Flask-SQLAlchemy   |
| Auth       | Flask-Login, Flask-Bcrypt         |
| Database   | SQLite (via SQLAlchemy ORM)       |
| Frontend   | Jinja2 Templates, HTML/CSS, JS    |
| File Upload| Werkzeug                          |

---

## 📁 Project Structure

```
gigseek/
├── app.py                  # Main Flask app, routes & config
├── models.py               # SQLAlchemy models (User, Gig, Application)
├── seed_data.py            # Database seeder with sample gigs & hosts
├── instance/
│   └── gigseek.db          # SQLite database (auto-created)
├── static/
│   └── uploads/
│       └── resumes/        # Uploaded resume PDFs
└── templates/
    ├── base.html
    ├── index.html
    ├── login.html
    ├── register_gigster.html
    ├── register_host.html
    ├── Dashboard.html
    ├── gig_feed.html
    ├── post_gig.html
    ├── applicants.html
    └── admin.html
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- pip

### 1. Clone the repository

```bash
git clone https://github.com/your-username/gigseek.git
cd gigseek
```

### 2. Create a virtual environment

```bash
python -m venv venv
source venv/bin/activate        # On Windows: venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install flask flask-sqlalchemy flask-login flask-bcrypt werkzeug
```

### 4. Run the app

```bash
python app.py
```

The app will start at `http://127.0.0.1:5000`.

### 5. (Optional) Seed the database

To populate the database with 25 sample gigs and 6 host accounts:

```bash
python seed_data.py
```

> **Default password for all seeded host accounts:** `host1234`

---

## 👥 User Roles

| Role     | Capabilities                                                        |
|----------|---------------------------------------------------------------------|
| Gigster  | Register with resume, browse gig feed, apply for gigs, track status |
| Host     | Post gigs, view applicants, accept/reject applications              |
| Admin    | View all users and gigs via admin panel                             |

---

## 📌 Routes Overview

| Route                         | Description                          | Access      |
|-------------------------------|--------------------------------------|-------------|
| `/`                           | Landing page                         | Public      |
| `/register/<role>`            | Registration (`gigster` or `host`)   | Public      |
| `/login`                      | Login                                | Public      |
| `/logout`                     | Logout                               | Logged in   |
| `/dashboard`                  | Role-based dashboard                 | Logged in   |
| `/gig_feed`                   | Browse and search gigs               | Gigster     |
| `/post_gig`                   | Post a new gig                       | Host        |
| `/apply/<gig_id>`             | Apply for a gig                      | Gigster     |
| `/update_status/<app_id>/<status>` | Accept or reject an application | Host        |
| `/admin`                      | Admin panel                          | Admin       |

---

## 🗄️ Database Schema

### `users`
| Column        | Type    | Description                          |
|---------------|---------|--------------------------------------|
| id            | Integer | Primary key                          |
| name          | String  | Full name                            |
| email         | String  | Unique email (used for login)        |
| password_hash | String  | Bcrypt-hashed password               |
| role          | String  | `gigster`, `host`, or `admin`        |
| mobile        | String  | Contact number (Gigsters)            |
| portfolio     | String  | Portfolio URL (Gigsters)             |
| resume        | String  | Uploaded PDF filename (Gigsters)     |

### `gigs`
| Column      | Type    | Description                           |
|-------------|---------|---------------------------------------|
| id          | Integer | Primary key                           |
| host_id     | FK      | References `users.id`                 |
| title       | String  | Gig title                             |
| description | Text    | Full job description                  |
| location    | String  | City or "Remote"                      |
| salary      | String  | Pay rate / range                      |
| experience  | String  | Required experience level             |
| work_type   | String  | `Remote`, `On-site`, `Hybrid`         |
| category    | String  | Gig category (e.g. "Online Tutor")    |
| is_active   | Boolean | Whether the gig is active             |

### `applications`
| Column     | Type    | Description                                          |
|------------|---------|------------------------------------------------------|
| id         | Integer | Primary key                                          |
| gigster_id | FK      | References `users.id`                               |
| gig_id     | FK      | References `gigs.id`                                |
| status     | String  | `pending`, `reviewed`, `Accepted`, `Rejected`        |
| cover_note | Text    | Optional message from the Gigster                   |
| applied_at | DateTime| Timestamp of application                            |

---

## ⚙️ Configuration

Key settings in `app.py`:

```python
app.config['SECRET_KEY'] = 'your-secret-key'         # Change in production
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///gigseek.db'
app.config['UPLOAD_FOLDER'] = 'static/uploads/resumes'
app.config['ALLOWED_EXTENSIONS'] = {'pdf'}
```

> ⚠️ **Important:** Change `SECRET_KEY` before deploying to production.

---

## 🌱 Gig Categories (Seeded)

Online Tutor · Delivery Partner · Content Creator · Freelance Developer · Data Entry · Event Staff · Photography · Social Media Manager · Home Services · Pet Care · Virtual Assistant · Graphic Designer · Customer Support · Transcription · Translation

---

## 🔮 Future Enhancements

- [ ] Email notifications on application status change
- [ ] Gigster profile page with ratings and reviews
- [ ] In-app messaging between matched Host and Gigster
- [ ] Payment integration for gig payouts
- [ ] AI-based gig recommendation for Gigsters
- [ ] REST API + React frontend migration

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).

---

## 🙋‍♀️ Author

Built by **Hasini** — CSE Final Year Project, 2026.
