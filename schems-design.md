# Smart Clinic Management System – Database Schema Design

## 1. MySQL Database Design

Database Name: `smart_clinic`

MySQL is used to store structured data such as patients, doctors, administrators, and appointments. Primary keys uniquely identify records, while foreign keys maintain relationships between tables.

### 1.1 Patients Table

| Column Name | Data Type | Constraints |
|---|---|---|
| patient_id | BIGINT | PRIMARY KEY, AUTO_INCREMENT |
| full_name | VARCHAR(100) | NOT NULL |
| email | VARCHAR(100) | NOT NULL, UNIQUE |
| phone | VARCHAR(15) | NOT NULL |
| date_of_birth | DATE | NOT NULL |
| gender | VARCHAR(20) | NOT NULL |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP |

**Design note:** Each patient has a unique ID and email address. Contact information and basic demographic details are stored here.

### 1.2 Doctors Table

| Column Name | Data Type | Constraints |
|---|---|---|
| doctor_id | BIGINT | PRIMARY KEY, AUTO_INCREMENT |
| full_name | VARCHAR(100) | NOT NULL |
| email | VARCHAR(100) | NOT NULL, UNIQUE |
| phone | VARCHAR(15) | NOT NULL |
| specialization | VARCHAR(100) | NOT NULL |
| availability_status | VARCHAR(20) | NOT NULL, DEFAULT 'AVAILABLE' |

**Design note:** The specialization field helps patients find appropriate doctors. Availability status indicates whether a doctor is accepting appointments.

### 1.3 Admin Table

| Column Name | Data Type | Constraints |
|---|---|---|
| admin_id | BIGINT | PRIMARY KEY, AUTO_INCREMENT |
| full_name | VARCHAR(100) | NOT NULL |
| email | VARCHAR(100) | NOT NULL, UNIQUE |
| password_hash | VARCHAR(255) | NOT NULL |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP |

**Design note:** The admin table stores administrative account details. Passwords must be stored as secure hashes, never as plain text.

### 1.4 Appointments Table

| Column Name | Data Type | Constraints |
|---|---|---|
| appointment_id | BIGINT | PRIMARY KEY, AUTO_INCREMENT |
| patient_id | BIGINT | NOT NULL, FOREIGN KEY |
| doctor_id | BIGINT | NOT NULL, FOREIGN KEY |
| appointment_datetime | DATETIME | NOT NULL |
| status | VARCHAR(20) | NOT NULL, DEFAULT 'SCHEDULED' |
| reason | VARCHAR(255) | NULL |
| created_at | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP |

**Foreign key relationships:**

- `patient_id` references `patients(patient_id)`.
- `doctor_id` references `doctors(doctor_id)`.

**Design note:** Each appointment is linked to one patient and one doctor. These relationships help maintain data integrity and support appointment scheduling.

## 2. MongoDB Collection Design

### Collection Name: `prescriptions`

MongoDB is used to store prescription documents because they may contain variable numbers of medicines, dosage instructions, and follow-up details.

### Example Prescription Document

```json
{
  "prescriptionId": "RX1001",
  "patientId": 101,
  "doctorId": 201,
  "appointmentId": 501,
  "prescriptionDate": "2026-10-16",
  "diagnosis": "Seasonal allergy",
  "medicines": [
    {
      "name": "Medicine A",
      "dosage": "As prescribed",
      "frequency": "As directed by doctor",
      "duration": "5 days"
    },
    {
      "name": "Medicine B",
      "dosage": "As prescribed",
      "frequency": "As directed by doctor",
      "duration": "3 days"
    }
  ],
  "followUp": {
    "required": true,
    "date": "2026-10-23",
    "notes": "Review symptoms during follow-up"
  },
  "createdAt": "2026-10-16T10:00:00Z"
}
```

### Design Justification

1. **MySQL:** Relational tables are suitable for patients, doctors, administrators, and appointments because these records have structured fields and defined relationships.
2. **Primary keys:** Unique identifiers allow each record to be referenced reliably.
3. **Foreign keys:** Appointment relationships prevent references to nonexistent patients or doctors.
4. **MongoDB:** Prescription documents support nested objects and arrays, making it convenient to store multiple medicines and follow-up details in one document.
5. **Data consistency:** Patient, doctor, and appointment identifiers connect prescription documents to the corresponding relational records. These references must be validated by the application because they are not enforced as MySQL foreign keys in MongoDB.
