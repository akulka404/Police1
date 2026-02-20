# Police1

A Django REST Framework-based API for police management, providing endpoints to manage criminal records, criminal sightings, and police alerts.

## Table of Contents

- [Overview](#overview)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Data Models](#data-models)
- [API Endpoints](#api-endpoints)
- [Installation & Setup](#installation--setup)
- [Running the Application](#running-the-application)
- [Deployment](#deployment)

## Overview

Police1 is a RESTful API built with Django and Django REST Framework. It provides three core modules:

- **Criminal Data** – Store and retrieve detailed criminal records including personal information, known crimes, custody status, and latest sightings.
- **Criminal Sightings** – Log and query sightings of known criminals by date, time, and location.
- **Alerts** – Create and manage police alerts with subject descriptions and reference links.

## Technology Stack

| Technology | Version |
|---|---|
| Python | 3.6.13 |
| Django | 3.1.5 |
| Django REST Framework | 3.12.2 |
| Gunicorn | 20.0.4 |
| django-multiselectfield | 0.1.12 |
| django-cors-headers | 3.7.0 |
| WhiteNoise | 5.2.0 |

## Project Structure

```
Police1/
├── API/                    # Django project configuration (settings, URLs, WSGI/ASGI)
├── alerts/                 # Alerts app – models, views, serializers, URLs
├── criminal_data/          # Criminal Data app – models, views, serializers, URLs
├── criminal_sightings/     # Criminal Sightings app – models, views, serializers, URLs
├── manage.py               # Django management script
├── requirements.txt        # Python dependencies
├── Procfile                # Heroku process configuration
└── runtime.txt             # Python runtime version for Heroku
```

## Data Models

### AlertAPI (`alerts`)

| Field | Type | Description |
|---|---|---|
| `name` | CharField (max 100) | Name associated with the alert |
| `date` | DateField | Date of the alert |
| `time` | TimeField | Time of the alert |
| `subject` | TextField | Subject/description of the alert |
| `link` | URLField | Optional reference URL |

### CriminalDataAPI (`criminal_data`)

| Field | Type | Description |
|---|---|---|
| `name` | CharField (max 100) | Full name of the criminal |
| `dob` | DateField | Date of birth |
| `known_crimes` | TextField | Description of known crimes |
| `registered` | CharField (Y/N) | Whether the criminal is registered |
| `registered_at` | CharField (max 100) | Location of registration |
| `custody` | CharField (Y/N) | Whether the criminal is currently in custody |
| `latest_sightings_time` | TimeField | Time of the latest sighting |
| `latest_sightings_date` | DateField | Date of the latest sighting |
| `latest_sightings_place` | CharField (max 100) | Location of the latest sighting |

### CriminalSightingsAPI (`criminal_sightings`)

| Field | Type | Description |
|---|---|---|
| `name` | CharField (max 100) | Name of the criminal sighted |
| `date` | DateField | Date of the sighting |
| `time` | TimeField | Time of the sighting |
| `location` | TextField | Location of the sighting |
| `description` | TextField | Description of the sighting |

## API Endpoints

### Alerts

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/alerts/` | Retrieve all alerts |
| `POST` | `/alerts/` | Create a new alert |
| `DELETE` | `/alerts/` | Delete all alerts |
| `GET` | `/alerts/<name>/` | Retrieve alerts by name |
| `DELETE` | `/alerts/<name>/` | Delete alert by name |
| `GET` | `/alerts/<subject>/` | Retrieve alerts by subject |
| `DELETE` | `/alerts/<subject>/` | Delete alert by subject |

> **Note:** The `<name>` and `<subject>` path parameters share the same URL pattern, which may cause routing ambiguity. Use the collection endpoint with query parameters where possible.

### Criminal Data

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/criminal_data/` | Retrieve all criminal records |
| `POST` | `/criminal_data/` | Create a new criminal record |
| `DELETE` | `/criminal_data/` | Delete all criminal records |
| `GET` | `/criminal_data/<name>/` | Retrieve criminal record by name |
| `DELETE` | `/criminal_data/<name>/` | Delete criminal record by name |

### Criminal Sightings

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/criminal_sightings/` | Retrieve all criminal sightings |
| `POST` | `/criminal_sightings/` | Log a new criminal sighting |
| `DELETE` | `/criminal_sightings/` | Delete all criminal sightings |
| `GET` | `/criminal_sightings/<name>/` | Retrieve sightings by criminal name |
| `DELETE` | `/criminal_sightings/<name>/` | Delete sightings by criminal name |
| `GET` | `/criminal_sightings/<location>/` | Retrieve sightings by location |
| `DELETE` | `/criminal_sightings/<location>/` | Delete sightings by location |

> **Note:** The `<name>` and `<location>` path parameters share the same URL pattern, which may cause routing ambiguity. Use the collection endpoint with query parameters where possible.

## Installation & Setup

### Prerequisites

- Python 3.6.13
- pip

### Steps

1. **Clone the repository:**

   ```bash
   git clone https://github.com/akulka404/Police1.git
   cd Police1
   ```

2. **Create and activate a virtual environment:**

   ```bash
   python -m venv venv
   source venv/bin/activate   # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies:**

   ```bash
   pip install -r requirements.txt
   ```

4. **Apply database migrations:**

   ```bash
   python manage.py migrate
   ```

5. **Create a superuser (optional, for Django admin access):**

   ```bash
   python manage.py createsuperuser
   ```

## Running the Application

Start the development server:

```bash
python manage.py runserver
```

The API will be available at `http://127.0.0.1:8000/`.

The Django admin panel is accessible at `http://127.0.0.1:8000/admin/`.

## Deployment

This project is configured for deployment on **Heroku** using Gunicorn as the WSGI server and WhiteNoise for static file serving.

The `Procfile` defines the web process:

```
web: gunicorn API.wsgi --log-file -
```

To deploy to Heroku:

```bash
heroku create
git push heroku main
heroku run python manage.py migrate
```