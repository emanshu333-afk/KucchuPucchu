# PrepPilot - Dynamic Exam Preparation System

An AI-powered exam strategy engine that continuously adapts to your preparation progress. Not just a timetable generator — a dynamic strategy engine that answers: **"Given your current situation and time remaining, what's the most effective way to use every remaining study hour?"**

## 🚀 Key Features

### 1. **Topic Priority Engine** 🎯
Automatically classifies every topic into:
- 🔴 **MUST DO** - High importance + weak preparation
- 🟡 **SHOULD DO** - Important but moderate preparation  
- 🟢 **IF TIME** - Lower priority or lower expected benefit

### 2. **Time Budget Engine** ⏰
Treats study time as a limited resource budget:
- Allocates hours across subjects based on weightage × weakness × difficulty
- Built-in buffers: Revision (10%), Mock Tests (10%), Safety Buffer (5%)
- Recalculates instantly when daily hours change

### 3. **Resource Matcher** 📚
Recommends the ONE best resource per topic per learning objective:
- Theory → Examples → Practice → Test → Analyze → Revise (for weak topics)
- Practice → Revise → Examples (for strong topics)
- Reduces information overload

### 4. **Daily Plan Engine** 📅
Purpose-driven daily blocks:
> "Electrostatics — 60 min theory + 45 min practice + 15 min error review"

### 5. **Recovery/Replanning Engine** 🔄 ⭐ **Signature Feature**
When you fall behind (which everyone does):
- **Does NOT** just push everything forward
- **Analyzes**: What must remain, what can be postponed, what can be removed
- **Options**: Smart Recovery / Intensive Recovery / Minimal Adjustment

### 6. **Exam Readiness Score** 📊
Meaningful 0-100 score based on:
- Syllabus coverage (20%)
- Topic mastery (25%)
- Practice accuracy (20%)
- Mock performance (20%)
- Revision frequency (10%)
- Weak area penalty (-10%)
- Time pressure factor (5%)

### 7. **What-If Simulator** 🔮
Test scenarios before deciding:
- "What if I can only study 3 hrs/day?"
- "What if I skip these 5 topics?"
- "What if I spend 3 days only on Math?"
- "What if exam moves 2 weeks?"

---

## 🏗️ Architecture

```
PrepPilot/
├── config/                 # Django project settings
├── PrepPilot/              # Main application
│   ├── models.py           # 12 core domain models
│   ├── engines/            # 7 business logic engines
│   ├── api/                # REST API (JWT auth, nested routes)
│   ├── management/commands/# 5 automation commands
│   └── tests/              # Unit + integration tests
├── static/                 # CSS, JS, images
├── templates/              # Base templates (Bootstrap 5)
└── Dockerfile              # Multi-stage production build
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|------------|
| Backend | Django 4.2 LTS, DRF 3.14 |
| Database | PostgreSQL 15 |
| Cache/Queue | Redis 7 + Celery 5.3 |
| Auth | JWT (SimpleJWT) with rotation |
| Testing | pytest + pytest-django |
| Frontend | Bootstrap 5 (server-rendered MVP) |
| Deployment | Docker, Docker Compose, Nginx |

---

## 🚀 Quick Start

### Prerequisites
- Docker & Docker Compose
- Git

### Development

```bash
# Clone repository
git clone <repository-url>
cd code_flux

# Copy environment template
cp .env.example .env
# Edit .env with your settings

# Start all services
docker-compose up -d

# Run migrations
docker-compose exec web python manage.py migrate

# Create superuser
docker-compose exec web python manage.py createsuperuser

# Access at http://localhost:8000
```

### Production

```bash
# Build and start
docker-compose -f docker-compose.yml up -d --build

# Run migrations
docker-compose exec web python manage.py migrate --settings=config.settings.production

# Collect static files
docker-compose exec web python manage.py collectstatic --noinput --settings=config.settings.production
```

---

## 📚 API Documentation

Once running, visit:
- **Swagger UI**: `http://localhost:8000/api/docs/`
- **ReDoc**: `http://localhost:8000/api/redoc/`
- **Schema**: `http://localhost:8000/api/schema/`

### Main Endpoints

| Category | Endpoints |
|----------|-----------|
| **Auth** | `POST /api/v1/auth/token/` `POST /api/v1/auth/register/` |
| **Exams** | `GET/POST /api/v1/exams/` `GET /api/v1/exams/{id}/overview/` |
| **Subjects** | `GET/POST /api/v1/exams/{id}/subjects/` |
| **Topics** | `GET/POST /api/v1/exams/{id}/topics/` `POST /api/v1/topics/{id}/update_progress/` |
| **Study Plans** | `GET /api/v1/exams/{id}/plans/` `POST /api/v1/plans/{id}/complete_block/` |
| **Mock Tests** | `GET/POST /api/v1/exams/{id}/mocks/` `POST /api/v1/mocks/{id}/submit/` |
| **Analytics** | `GET /api/v1/analytics/readiness/` `POST /api/v1/analytics/whatif/simulate_hours/` |
| **Recovery** | `POST /api/v1/exams/{id}/check_recovery/` `POST /api/v1/exams/{id}/apply_recovery/` |
| **Notifications** | `GET /api/v1/analytics/notifications/` `POST /api/v1/notifications/{id}/mark_read/` |

---

## 🧪 Running Tests

```bash
# Run all tests
docker-compose exec web pytest

# Run with coverage
docker-compose exec web pytest --cov=PrepPilot --cov-report=html

# Run specific test file
docker-compose exec web pytest PrepPilot/tests/test_engines.py -v
```

---

## 📁 Project Structure

```
code_flux/
├── config/
│   ├── settings/
│   │   ├── base.py         # Shared settings
│   │   ├── development.py  # Dev settings
│   │   └── production.py   # Prod settings
│   ├── urls.py
│   └── wsgi.py / asgi.py
├── PrepPilot/
│   ├── models.py           # User, Exam, Subject, Topic, Resource,
│   │                       # StudyPlan, StudyBlock, MockTest,
│   │                       # TopicPerformance, ReadinessScore,
│   │                       # WhatIfScenario, RecoveryPlan, Notification
│   ├── engines/
│   │   ├── topic_priority.py
│   │   ├── time_budget.py
│   │   ├── resource_matcher.py
│   │   ├── daily_plan.py
│   │   ├── recovery.py
│   │   ├── readiness.py
│   │   └── whatif.py
│   ├── api/
│   │   ├── serializers/
│   │   ├── views.py
│   │   └── *_urls.py
│   ├── management/commands/
│   │   ├── generate_study_plans.py
│   │   ├── calculate_priorities.py
│   │   ├── check_recovery.py
│   │   ├── calculate_readiness.py
│   │   └── send_notifications.py
│   └── tests/
├── static/
├── templates/
├── media/
├── Dockerfile
├── Dockerfile.dev
├── docker-compose.yml
├── docker-compose.override.yml
├── nginx.conf
├── requirements.txt
├── pytest.ini
├── .env.example
├── .gitignore
├── README.md
├── WHAT_IS_DONE.md
├── ARCHITECTURE.md
└── HOW_TO_EXPLAIN.md
```

---

## 🔧 Management Commands

```bash
# Generate study plans for all active exams
python manage.py generate_study_plans

# Calculate topic priorities
python manage.py calculate_priorities

# Check for recovery needs
python manage.py check_recovery --auto-apply

# Calculate readiness scores
python manage.py calculate_readiness

# Send study reminders
python manage.py send_notifications
```

---

## 🐳 Docker Services

| Service | Port | Description |
|---------|------|-------------|
| web | 8000 | Django + Gunicorn |
| celery-worker | - | Background tasks |
| celery-beat | - | Scheduled tasks |
| db | 5432 | PostgreSQL |
| redis | 6379 | Cache + Celery broker |
| nginx | 80/443 | Reverse proxy |

---

## 📖 Documentation

- **[WHAT_IS_DONE.md](WHAT_IS_DONE.md)** - Complete implementation details & reasoning
- **[ARCHITECTURE.md](ARCHITECTURE.md)** - Technical architecture deep-dive
- **[HOW_TO_EXPLAIN.md](HOW_TO_EXPLAIN.md)** - How to pitch/demo the product

---

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgations

- Built for hackathon: **Dynamic Exam Preparation System**
- Inspired by the real struggles of students preparing for competitive exams
- Designed with ❤️ for learners everywhere

---

## 📞 Support

For questions, issues, or feature requests, please open a GitHub issue.

**Built with Django, powered by strategy, designed for students.**