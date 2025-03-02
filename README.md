Loan Management System REST API

Description

The Loan Management System is a Django-based REST API that facilitates loan management with user-defined monthly compound interest calculations. It provides role-based authentication, automatic interest computation, and loan repayment schedules. The system also supports loan foreclosure, enabling users to settle loans before tenure completion with adjusted interest.

Tech Stack

Backend: Django, Django REST Framework

Authentication: JWT (Simple JWT)

OTP Email Service: Nodemailer (via SMTP)

Database: PostgreSQL (Preferred) or MongoDB (Optional with Djongo)

Deployment: Render (Free Tier)

Features

1. Authentication & Role-Based Access Control

JWT authentication using Simple JWT.

User roles: Admin & User.

OTP verification for user registration.

Role-based access control for API endpoints.

2. Loan Management

For Users:

Apply for a loan (specify amount, tenure, interest rate).

View active and past loans.

Get loan details with monthly installments and interest breakdown.

Foreclose loans with adjusted interest calculations.

For Admins:

View all loans in the system.

Access all user loan details.

Delete loan records.

3. Loan Calculation

Monthly installments computed automatically based on user-defined yearly compound interest.

Foreclosure option to settle the loan early with adjusted interest.

Stores total amount payable, interest amount, and payment schedules.

API Endpoints

1. Add a Loan

Endpoint: POST /api/loans/

Request:

{
  "amount": 10000,
  "tenure": 12,
  "interest_rate": 10
}

Response:

{
  "status": "success",
  "data": {
    "loan_id": "LOAN001",
    "amount": 10000,
    "tenure": 12,
    "interest_rate": "10% yearly",
    "monthly_installment": 879.16,
    "total_interest": 1549.92,
    "total_amount": 11549.92,
    "payment_schedule": [
      {
        "installment_no": 1,
        "due_date": "2024-03-24",
        "amount": 879.16
      }
    ]
  }
}

2. List Loans

Endpoint: GET /api/loans/

Response:

{
  "status": "success",
  "data": {
    "loans": [
      {
        "loan_id": "LOAN001",
        "amount": 10000,
        "tenure": 12,
        "monthly_installment": 879.16,
        "total_amount": 11549.92,
        "amount_paid": 1758.32,
        "amount_remaining": 9791.60,
        "next_due_date": "2024-04-24",
        "status": "ACTIVE",
        "created_at": "2024-02-24T10:30:00Z"
      }
    ]
  }
}

3. Loan Foreclosure

Endpoint: POST /api/loans/{loan_id}/foreclose/

Request:

{
  "loan_id": "LOAN001"
}

Response:

{
  "status": "success",
  "message": "Loan foreclosed successfully.",
  "data": {
    "loan_id": "LOAN001",
    "amount_paid": 11000.00,
    "foreclosure_discount": 500.00,
    "final_settlement_amount": 10500.00,
    "status": "CLOSED"
  }
}

Installation & Setup

1. Clone the Repository

git clone https://github.com/your-username/loan-management-api.git
cd loan-management-api

2. Set Up Virtual Environment (Windows)

python -m venv venv
venv\Scripts\activate

3. Install Dependencies

pip install -r requirements.txt

4. Set Up Environment Variables

Create a .env file and add:

SECRET_KEY=your-secret-key
DEBUG=True
DATABASE_URL=your-database-url
EMAIL=your-email@example.com
PASSWORD=your-email-password

5. Apply Migrations

python manage.py makemigrations
python manage.py migrate

6. Create a Superuser

python manage.py createsuperuser

7. Run the Server

python manage.py runserver

The API will be accessible at:

http://127.0.0.1:8000/

Deployment (Render Free Tier)

1. Set Up GitHub Repository

Push your project to GitHub.

2. Create Render Web Service

Select Python runtime.

Set start command: gunicorn loan_management.wsgi:application.

Add environment variables in the Render dashboard.

3. Set Up PostgreSQL Database on Render

Create a PostgreSQL database in Render.

Copy the database URL and set it as DATABASE_URL.

4. Deploy the Web Service

Click Deploy, and your API will be live.

Testing

Use Postman or Thunder Client to test the API.

Verify authentication, loan creation, foreclosure, and role-based access.

Contributing

Fork the repository.

Create a feature branch.

Commit changes and push to GitHub.

Open a pull request.

License

MIT License
