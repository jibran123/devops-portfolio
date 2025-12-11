# Warehouse Management System

A full-stack web application for warehouse inventory management built with Python Flask and Angular.JS.

## 🎯 Project Overview

This system provides comprehensive warehouse management capabilities including:
- Real-time inventory tracking
- Stock movement logging
- User authentication and role-based access
- RESTful API backend
- Modern Angular.JS frontend

## 🛠️ Technical Stack

**Backend**
- Python 3.x
- Flask web framework
- SQLAlchemy ORM
- PostgreSQL/MongoDB database
- JWT authentication

**Frontend**
- Angular.JS
- Bootstrap for responsive UI
- RESTful API integration

**Deployment**
- Docker containerization
- Nginx reverse proxy
- Gunicorn WSGI server

## 📋 Key Features

### Inventory Management
- Add, update, delete inventory items
- Track quantities and locations
- Generate inventory reports
- Low-stock alerts

### User Management
- Role-based access control (Admin, Manager, Staff)
- Secure authentication
- Activity logging

### API Endpoints
```
GET    /api/v1/inventory          # List all items
POST   /api/v1/inventory          # Create new item
GET    /api/v1/inventory/:id      # Get item details
PUT    /api/v1/inventory/:id      # Update item
DELETE /api/v1/inventory/:id      # Delete item

POST   /api/v1/auth/login         # User login
POST   /api/v1/auth/register      # User registration
```

## 🚀 Deployment

### Using Docker
```bash
# Clone repository
git clone https://github.com/jibran123/warehouse.git
cd warehouse

# Build Docker image
docker build -t warehouse-app .

# Run container
docker run -p 5000:5000 warehouse-app
```

### Manual Setup
```bash
# Install dependencies
pip install -r requirements.txt

# Set environment variables
export FLASK_APP=app.py
export DATABASE_URL=postgresql://user:pass@localhost/warehouse

# Initialize database
flask db upgrade

# Run application
flask run
```

## 📊 Architecture
```
┌─────────────┐
│   Angular   │
│   Frontend  │
└──────┬──────┘
       │ HTTP/REST
       │
┌──────▼──────┐      ┌──────────┐
│   Flask     │◄────►│ Database │
│   Backend   │      │PostgreSQL│
└─────────────┘      └──────────┘
```

## 💡 Technical Highlights

- **Modular Architecture**: Separation of concerns with blueprints
- **API-First Design**: RESTful API with proper HTTP methods and status codes
- **Security**: JWT tokens, password hashing, CSRF protection
- **Error Handling**: Comprehensive error handling and logging
- **Testing**: Unit and integration tests included

## 🔧 Development Setup
```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate

# Install dev dependencies
pip install -r requirements-dev.txt

# Run tests
pytest

# Run with auto-reload
flask run --reload --debug
```

## 📈 Future Enhancements

- [ ] Real-time notifications with WebSockets
- [ ] Barcode scanning integration
- [ ] Advanced reporting and analytics
- [ ] Multi-warehouse support
- [ ] Mobile application

## 🔗 Links

- [Live Demo](#) (if available)
- [API Documentation](#)
- [GitHub Repository](https://github.com/jibran123/warehouse)

---

**Note**: This project was built as a demonstration of full-stack development capabilities and modern web application patterns.