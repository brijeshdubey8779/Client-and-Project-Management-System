# Machine Test: Client and Project Management System

This project is a RESTful API-based system for managing **Clients** and **Projects**. It provides functionality for user authentication, client management, and project assignment.

---

## Features

1. **User Authentication**:
   - Token-based authentication using the Django REST Framework (DRF).

2. **Client Management**:
   - Create, read, update, and delete (CRUD) operations for clients.

3. **Project Management**:
   - Assign projects to clients and users.
   - View all projects assigned to the logged-in user.

---

## Installation and Setup

### Prerequisites

- Python 3.x installed on your system.
- MySQL/PostgreSQL (or SQLite for testing).
- Git for version control.
- A virtual environment tool (`venv` or `virtualenv`).

### Installation Steps

1. **Clone the repository:**
   ```bash
   git clone https://github.com/brijeshdubey8779/Client-and-Project-Management-System.git
   cd Client-and-Project-Management-System
   ```

2. **Create a virtual environment:**
   ```bash
   python3 -m venv venv
   source venv/bin/activate  # For Linux/MacOS
   venv\Scripts\activate     # For Windows
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure the database:**
   - Open `settings.py` and update the `DATABASES` section:
     ```python
     DATABASES = {
         'default': {
             'ENGINE': 'django.db.backends.mysql',  # Use 'django.db.backends.postgresql' for PostgreSQL
             'NAME': 'your_database_name',
             'USER': 'your_database_user',
             'PASSWORD': 'your_database_password',
             'HOST': 'localhost',
             'PORT': '3306',  # Use 5432 for PostgreSQL
         }
     }
     ```

5. **Apply migrations to create the database schema:**
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

6. **Create a superuser:**
   ```bash
   python manage.py createsuperuser
   ```

7. **Run the server:**
   ```bash
   python manage.py runserver
   ```

8. Access the application at [http://127.0.0.1:8000](http://127.0.0.1:8000).

---

## API Endpoints

### 1. **Authentication**
   - **Login and obtain token**:
     ```http
     POST /api/token/
     Body:
     {
         "username": "your_username",
         "password": "your_password"
     }
     ```

### 2. **Client Management**
   - **List all clients**:
     ```http
     GET /api/clients/
     Authorization: Token <your-token>
     ```
   - **Create a client**:
     ```http
     POST /api/clients/
     Authorization: Token <your-token>
     Body:
     {
         "client_name": "Client Name"
     }
     ```
   - **Update a client**:
     ```http
     PUT /api/clients/<id>/
     Authorization: Token <your-token>
     Body:
     {
         "client_name": "Updated Client Name"
     }
     ```
   - **Delete a client**:
     ```http
     DELETE /api/clients/<id>/
     Authorization: Token <your-token>
     ```

### 3. **Project Management**
   - **List all projects**:
     ```http
     GET /api/projects/
     Authorization: Token <your-token>
     ```
   - **Create a project**:
     ```http
     POST /api/projects/
     Authorization: Token <your-token>
     Body:
     {
         "project_name": "Project Name",
         "client_id": 1,
         "users": [1, 2]
     }
     ```
   - **View logged-in user’s projects**:
     ```http
     GET /api/user-projects/
     Authorization: Token <your-token>
     ```

---

## Database Design

The project uses the following database schema:

### **User** (Django Default `auth_user`)
- `id`: Primary Key
- `username`, `email`, `password`, etc.

### **Client**
- `id`: Primary Key
- `client_name`: Name of the client.
- `created_by`: Foreign Key to `User`.
- `created_at`: Timestamp.
- `updated_at`: Timestamp.

### **Project**
- `id`: Primary Key
- `project_name`: Name of the project.
- `client`: Foreign Key to `Client`.
- `created_by`: Foreign Key to `User`.
- `users`: Many-to-Many Field with `User`.
- `created_at`: Timestamp.

---

## How to Run the Machine Test

1. **Log in and obtain a token:**
   - Use the `api/token/` endpoint to log in as a superuser or another user and retrieve an authentication token.

2. **Test APIs:**
   - Use tools like **Postman** or **cURL** to test all the endpoints listed in the API section above.
   - Ensure you include the token in the `Authorization` header for all requests.

3. **Verify database changes:**
   - Log in to the Django Admin at [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin).
   - Check if clients and projects are created/updated/deleted correctly.

---

## How the Code Works

1. **Views**:
   - The project uses Django REST Framework (DRF) generic views (`ListCreateAPIView`, `RetrieveUpdateDestroyAPIView`) for CRUD operations.
   - `permission_classes` ensure only authenticated users can access the APIs.

2. **Serializers**:
   - Handle model data validation and serialization for clients and projects.

3. **Authentication**:
   - Uses DRF's built-in token-based authentication for secure access to APIs.

