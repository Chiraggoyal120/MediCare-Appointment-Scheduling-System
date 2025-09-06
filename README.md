# MediCare Appointment Scheduling System

A comprehensive AI-powered appointment scheduling system built with Gradio, featuring patient management, appointment booking, automated reminders, and Excel export functionality.

## Features

- **Patient Registration & Management**: Register new patients or find existing records
- **Appointment Scheduling**: Real-time availability checking and booking
- **Automated Reminder System**: Handle confirmations, cancellations, and status checks
- **Excel Export**: Comprehensive data export with multiple analytics sheets
- **AI Chat Assistant**: Interactive help and guidance system
- **Admin Dashboard**: System statistics and appointment management

## System Architecture

```
├── Patient Management
│   ├── Registration/Lookup
│   ├── Insurance Information
│   └── Patient History Tracking
├── Appointment System
│   ├── Doctor Availability
│   ├── Location Management
│   ├── Real-time Booking
│   └── Conflict Prevention
├── Reminder System
│   ├── Appointment Confirmations
│   ├── Cancellation Handling
│   └── Status Inquiries
└── Analytics & Export
    ├── Excel Report Generation
    ├── Performance Metrics
    └── Schedule Analytics
```

## Quick Start

### Prerequisites

- Python 3.8 or higher
- pip package manager

### Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/medicare-scheduler.git
cd medicare-scheduler
```

2. Create a virtual environment:
```bash
python -m venv medicare_env
source medicare_env/bin/activate  # On Windows: medicare_env\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

4. Run the application:
```bash
python app.py
```

5. Open your browser to `http://localhost:7860`

## Configuration

### Environment Setup

Create a `.env` file in the root directory:

```env
# Application Configuration
APP_NAME=MediCare Appointment Scheduler
APP_VERSION=1.0.0
DEBUG_MODE=True

# Server Configuration
SERVER_HOST=0.0.0.0
SERVER_PORT=7860
SHARE_GRADIO=False

# Database Configuration (Optional - defaults to in-memory SQLite)
DATABASE_URL=sqlite:///medicare.db

# Email Configuration (For Production)
SMTP_SERVER=smtp.gmail.com
SMTP_PORT=587
SMTP_USERNAME=your_email@gmail.com
SMTP_PASSWORD=your_app_password
EMAIL_FROM=appointments@medicare-clinic.com

# Clinic Information
CLINIC_NAME=MediCare Allergy & Wellness Center
CLINIC_PHONE=(555) 123-4567
CLINIC_EMAIL=appointments@medicare-clinic.com
CLINIC_ADDRESS=123 Health Street, Medical District, City, State 12345

# Business Hours
CLINIC_HOURS_WEEKDAY=8:00 AM - 6:00 PM
CLINIC_HOURS_SATURDAY=9:00 AM - 2:00 PM
CLINIC_HOURS_SUNDAY=Closed

# Appointment Configuration
NEW_PATIENT_DURATION=60  # minutes
RETURNING_PATIENT_DURATION=30  # minutes
APPOINTMENT_BUFFER=15  # minutes between appointments
BOOKING_ADVANCE_DAYS=30  # how far ahead patients can book
```

### Docker Setup (Optional)

1. Build the Docker image:
```bash
docker build -t medicare-scheduler .
```

2. Run the container:
```bash
docker run -p 7860:7860 -v $(pwd)/data:/app/data medicare-scheduler
```

## Sample Data

The system automatically generates synthetic test data on startup:

### Test Patients (20 sample records)
- Patient IDs: PAT001 - PAT020
- Realistic names, DOB, contact info
- Mixed insurance carriers
- 50/50 split of new vs returning patients

### Test Appointments
- Pre-scheduled appointments across all doctors
- Various appointment statuses (scheduled, confirmed, cancelled)
- Realistic time distributions

### Doctor Schedules
- 5 doctors with different specialties:
  - Dr. Sarah Smith (Allergy Specialist)
  - Dr. Michael Johnson (Pulmonologist)
  - Dr. Emily Williams (Immunologist)
  - Dr. David Brown (General Practice)
  - Dr. Lisa Davis (Pediatric Allergist)
- 3 locations: Main Clinic, Downtown Branch, Northside Office
- 14-day rolling schedule with 30-minute time slots

## Usage Examples

### 1. Register a New Patient

```python
# Navigate to "Patient Registration" tab
first_name = "John"
last_name = "Doe"
dob = "01/15/1985"
phone = "(555) 123-4567"
email = "john.doe@email.com"
insurance = "Blue Cross Blue Shield"
```

### 2. Book an Appointment

```python
# After patient registration, go to "Schedule Appointment" tab
doctor = "Dr. Sarah Smith (Allergy Specialist)"
location = "Main Clinic"
# Check available slots, then book with patient details
```

### 3. Manage Appointments

```python
# Use "Reminder System" tab
appointment_id = "APT1234"
action = "CONFIRM"  # or "CANCEL reason" or "STATUS"
```

### 4. Export Data

```python
# Use "Admin Dashboard" tab
# Click "Export to Excel" to download comprehensive report
```

## API Reference

### Core Classes

#### `Patient`
```python
@dataclass
class Patient:
    patient_id: str
    first_name: str
    last_name: str
    dob: str
    phone: str
    email: str
    insurance_carrier: str = ""
    member_id: str = ""
    group_number: str = ""
    is_new: bool = True
```

#### `Appointment`
```python
@dataclass
class Appointment:
    appointment_id: str
    patient_id: str
    doctor: str
    date: str
    time: str
    duration: int  # minutes
    location: str
    status: str = "scheduled"
```

### Key Methods

#### `DataManager.search_patient(first_name, last_name, dob)`
Search for existing patient by name and DOB.

#### `DataManager.book_appointment(appointment)`
Book a new appointment and update availability.

#### `DataManager.export_to_excel()`
Generate comprehensive Excel report with multiple sheets.

## Excel Export Details

The Excel export generates a multi-sheet workbook containing:

### Sheet 1: Appointments
- Complete appointment details with patient information
- Appointment status and history
- Doctor and location assignments
- Duration and scheduling information

### Sheet 2: Patients
- Complete patient database
- Contact information and insurance details
- Patient type classification (new/returning)
- Registration timestamps

### Sheet 3: Statistics
- Total appointments by status
- Patient demographics
- System performance metrics
- Export generation timestamp

### Sheet 4: Doctor Performance
- Appointment counts by doctor
- Performance analytics
- Workload distribution

### Sheet 5: Location Performance
- Appointment counts by location
- Location utilization metrics
- Geographic distribution

### Sheet 6: Weekly Schedule
- 7-day schedule overview
- Availability statistics by doctor/location
- Slot utilization rates

## Customization

### Adding New Doctors

Edit the `doctors` list in `AppointmentScheduler.__init__()`:

```python
self.doctors = [
    "Dr. Sarah Smith (Allergy Specialist)",
    "Dr. Michael Johnson (Pulmonologist)", 
    "Dr. Emily Williams (Immunologist)",
    "Dr. David Brown (General Practice)",
    "Dr. Lisa Davis (Pediatric Allergist)",
    "Dr. Your Name (Your Specialty)"  # Add new doctor
]
```

### Adding New Locations

Edit the `locations` list:

```python
self.locations = [
    "Main Clinic", 
    "Downtown Branch", 
    "Northside Office",
    "Your New Location"  # Add new location
]
```

### Modifying Business Hours

Update the schedule generation in `DataManager.generate_synthetic_data()`:

```python
for hour in range(9, 17):  # Change hours as needed
    for minute in [0, 30]:  # Change time slot intervals
```

## Database Schema

### Tables

- `patients`: Patient demographics and contact information
- `appointments`: Appointment details and scheduling
- `doctor_schedule`: Doctor availability and time slots
- `reminders`: Reminder system logs and responses
- `form_status`: Patient form completion tracking

### Relationships

- Appointments → Patients (many-to-one)
- Reminders → Appointments (many-to-one)
- Form Status → Appointments (one-to-one)

## Testing

### Running Tests

```bash
python -m pytest tests/ -v
```

### Test Coverage

```bash
pip install pytest-cov
python -m pytest tests/ --cov=app --cov-report=html
```

### Sample Test Scenarios

1. **Patient Registration**: Test new vs existing patient detection
2. **Appointment Booking**: Test availability checking and conflict prevention
3. **Reminder System**: Test all reminder actions (confirm, cancel, status)
4. **Excel Export**: Test data integrity and sheet generation
5. **AI Chat**: Test intent recognition and response accuracy

## Deployment

### Production Deployment

1. **Set up production environment**:
```bash
export FLASK_ENV=production
export DEBUG_MODE=False
```

2. **Configure email settings** in `.env`

3. **Set up persistent database**:
```bash
export DATABASE_URL=sqlite:///production_medicare.db
```

4. **Use process manager** (PM2, systemd, etc.):
```bash
pm2 start app.py --name medicare-scheduler
```

### Cloud Deployment

#### Heroku
```bash
heroku create your-medicare-app
heroku config:set DEBUG_MODE=False
git push heroku main
```

#### Railway
```bash
railway login
railway init
railway up
```

## Security Considerations

- **Data Privacy**: All patient data is handled according to HIPAA guidelines
- **Authentication**: Add authentication middleware for production use
- **HTTPS**: Always use HTTPS in production
- **Data Backup**: Implement regular database backups
- **Access Logs**: Monitor and log all system access

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

