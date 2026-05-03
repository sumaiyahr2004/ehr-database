# Electronic Health Record (EHR) Database
This is a full-stack web application for managing clinical data across patients, providers, visits, diagnoses, prescriptions, and medications. We built this with Flask and PostgreSQL, with raw SQL queries handling all database interactions. We included an allergy conflict detection system that flags patients who have been prescribed medications they are allergic to.

The website covers the core data management needs of a clinical environment. Every major entity in the system has its own page with full search, create, edit, and delete functionality where applicable. There is also a reports section that runs more complex queries across multiple tables to surface clinically meaningful insights.

## Features

**Patient Managemen** 
- Search patients by name, email, or phone number
- Add new patients with full demographic and emergency contact information
- Edit existing patient records
- Delete patients from the system

**Clinical Data**
- Browse all visits with associated diagnoses pulled in via JOIN
- View prescriptions linked to the ordering provider and patient
- View medications tied to prescriptions
- Browse provider directory by specialty

**Allergy Conflict Detection**
- Automatically flags cases where a patient has been prescribed a medication that conflicts with a known allergy
- Covers common conflict pairs including Penicillin, NSAIDs, Sulfa drugs, ACE inhibitors, and more

**Reports**
- Patients who received a diagnosis but no prescription during a visit
- Providers ranked by how frequently they prescribe each medication
- Prescription count per patient with a configurable minimum threshold filter

## Project Structure
```
ehr-database/
    server.py                  # All Flask routes and SQL queries
    requirements.txt           # Python dependencies
    templates/
        _base.html             # Shared layout and navigation
        home.html              # Landing page
        patient.html           # Patient list and search
        patient_new.html       # Add new patient form
        patient_edit.html      # Edit patient form
        visit.html             # Visit list with diagnoses
        provider.html          # Provider directory
        diagnosis.html         # Diagnosis records
        prescription.html      # Prescription records
        medication.html        # Medication records
        patient_allergy.html   # Allergy records
        allergy_conflict.html  # Allergy conflict report
        report.html            # Clinical reports
        report_rx_counts.html  # Prescription count report
```

## How to Run
1. Install dependencies:
`pip install -r requirements.txt` 
2. Run the server:
`python server.py` 
3. Open your browser and go to http://localhost:8111

## Schema 
The app connects to a PostgreSQL database hosted on Google Cloud using SQLAlchemy as the connection layer. All queries are written in raw SQL using parameterized inputs to prevent SQL injection. The schema covers the following tables:
- patient (demographic and contact information)
- provider (clinical staff and specialties)
- visit (appointment records linking patients and providers)
- diagnosis (ICD-style diagnosis codes and names)
- visit_diagnosis (many-to-many between visits and diagnoses)
- prescription (medication orders tied to a visit and provider)
- medication (drug reference table with generic and brand names)
- prescription_medication (many-to-many between prescriptions and medications)
- patient_allergy (known patient allergies with reaction and severity)
- allergyconflict (flags allergy-medication conflicts)

## Error Handling
The app includes custom error pages for 400, 404, and 500 responses instead of exposing raw Flask error output. Database operations are wrapped in try/except blocks with rollback on failed writes.

## Tools Used: 
Backend: Python, Flask, SQLAlchemy
Database: PostgreSQL
Frontend: HTML, Jinja2
Hosting: Google Cloud SQL
