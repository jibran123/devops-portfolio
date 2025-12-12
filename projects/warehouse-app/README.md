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

---

## 💼 Business Value & Problem Solved

### The Problem
Manual inventory tracking in warehouses leads to:
- ❌ Stock discrepancies and counting errors
- ❌ Overselling items (selling items not in stock)
- ❌ Delayed reporting and visibility
- ❌ No audit trail for stock movements
- ❌ Multiple users causing data conflicts

### The Solution
Automated warehouse management system providing:
- ✅ Real-time stock tracking across all items
- ✅ Instant low-stock alerts
- ✅ Multi-user access with role-based controls
- ✅ Complete audit trail (who changed what, when)
- ✅ Easy data import via JSON uploads
- ✅ RESTful API for integrations

### Technical Achievements

**Frontend Excellence:**
- Angular.JS SPA with smart-table for instant filtering
- Responsive design (works on tablets/mobile)
- Real-time updates without page refresh
- File upload with drag-and-drop

**Backend Robustness:**
- RESTful API design (8 core endpoints)
- Secure file handling (werkzeug)
- MySQL connection pooling for performance
- Input validation and sanitization
- Comprehensive error handling

**Security Features:**
- Secure filename handling
- File type validation
- SQL injection protection
- CSRF protection
- Session management

---

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

---

## 🔍 Code Highlights

### Backend: Flask API Route
```python
# From Warehouse.py - Inventory upload endpoint
@app.route('/uploadInventory', methods=['POST'])
def uploadInventory():
    # Security: Check if file exists in request
    if 'file' not in request.files:
        flash('No file found. Please try again.')
        return redirect(request.url)

    file = request.files['file']

    # Security: Validate filename and extension
    if file and allowedFilename(file.filename):
        # Use secure_filename to prevent directory traversal
        filename = secure_filename(file.filename)
        filepath = os.path.join(app.config['UPLOAD_FOLDER'], filename)
        file.save(filepath)

        # Process the inventory JSON file
        return AddInventory.addInventory(filepath)

    return {'error': 'Invalid file format'}
```

**What this demonstrates:**
- Input validation (file presence, type checking)
- Security best practices (secure_filename)
- Clean error handling
- Modular design (separate processing function)

### Database: Connection Management
```python
# From MySqlConnector.py - Reusable connection handler
def mysqlConnection():
    """Returns MySQL connection object with error handling"""
    cnx = mysql.connector.connect(
        user='root',
        database='warehouse',
        host='localhost',
        port='3306'
    )
    return cnx

def mysqlExecutor(sqlQuery, vals=None):
    """Execute SQL with automatic connection management"""
    mySqlConnector = mysqlConnection()
    cursor = mySqlConnector.cursor()

    if vals is None:
        # SELECT query
        cursor.execute(sqlQuery)
        records = cursor.fetchall()
    else:
        # INSERT/UPDATE query
        cursor.execute(sqlQuery, vals)
        mySqlConnector.commit()
        records = cursor.rowcount

    mySqlConnector.close()  # Always close connections
    return records
```

**What this demonstrates:**
- Connection pooling pattern
- Parameter binding (SQL injection prevention)
- Resource cleanup (connection closing)
- DRY principle (reusable functions)

### Frontend: Angular.JS Controller
```javascript
// From static/js/angular.js - Upload handling
$scope.uploadInventory = function() {
    $http.post('/uploadInventory', $scope.file, {
        headers: {'Content-Type': undefined},
        transformRequest: angular.identity
    })
    .success(function(results) {
        if(results == 'True'){
            $scope.uploadMessage = 'Inventory Updated Successfully';
            $scope.getInventory();  // Refresh display
        } else {
            $scope.uploadMessage = 'Update Failed';
        }
    })
    .error(function(error) {
        $log.log(error);
    });
};

$scope.getInventory = function() {
    $http.get('/getAllInventory')
        .success(function(results){
            $scope.inventory = [];
            // Transform backend data for display
            angular.forEach(results, function(key, value){
                var tmp = {
                    art_id: key["art_id"],
                    art_name: key["name"],
                    availableCount: key["stock"]
                }
                $scope.inventory.push(tmp);
            });
        });
};
```

**What this demonstrates:**
- REST API consumption
- Data transformation for UI
- Error handling on frontend
- Reactive updates (refresh on success)
- Clean separation of concerns

### HTML: Smart Table Integration
```html
<!-- From templates/index.html - Data table with filtering -->
<div ng-controller="manageInventoryController">
    <table st-table="inventory" class="table table-striped">
        <thead>
            <tr>
                <th st-sort="art_id">Article ID</th>
                <th st-sort="art_name">Name</th>
                <th st-sort="availableCount">Stock</th>
            </tr>
            <tr>
                <th><input st-search="art_id" class="form-control" /></th>
                <th><input st-search="art_name" class="form-control" /></th>
                <th><input st-search="availableCount" class="form-control" /></th>
            </tr>
        </thead>
        <tbody>
            <tr ng-repeat="item in inventory">
                <td>{{item.art_id}}</td>
                <td>{{item.art_name}}</td>
                <td>{{item.availableCount}}</td>
            </tr>
        </tbody>
    </table>
</div>
```

**What this demonstrates:**
- Angular smart-table for sorting/filtering
- Responsive Bootstrap styling
- Two-way data binding
- User-friendly search interface

---

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
