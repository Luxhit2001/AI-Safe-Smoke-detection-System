[README.md](https://github.com/user-attachments/files/34033935/README.md)
# AI-SAFE — Smoke Detection & Air Quality Monitoring System

AI-SAFE is a college project that combines a Flask web dashboard with Arduino-based smoke/gas sensing. It demonstrates sensor monitoring, alerts, incident tracking, user accounts, and basic camera-based smoke/fire detection features.

> **Project/demo note:** This is an educational prototype, not a certified fire alarm or regulatory air-quality measurement system. Do not rely on it as the only safety system.

## Features

- **Flask web application** with login, registration, email OTP verification, password reset, and a dashboard.
- **Air-quality/smoke monitoring** using an MQ-135 sensor connected to an Arduino.
- **Live sensor updates** through USB serial communication between Arduino and the Flask application.
- **Alert and notification interface** for configured sensor thresholds.
- **Incident and station information** with demo records clearly distinguished from live readings.
- **CSV export** for sensor history when signed in.
- **Camera-based vision features** using OpenCV, where supported by the local setup.
- **SQLite database** for local application data.
- **Arduino sketches** and wiring notes in the `hardware/` folder.

## Technology Stack

- Python
- Flask
- SQLite
- HTML, CSS, and JavaScript
- pySerial for Arduino USB serial communication
- OpenCV for camera-related vision features
- Arduino Uno and MQ-135 sensor

## Project Structure

```text
dashboard_ui_final/
├── app.py
├── requirements.txt
├── START_AI_SAFE.bat
├── setup_email.py
├── import_air_quality.py
├── .env.example
├── templates/
│   ├── login.html
│   ├── register.html
│   ├── verify_otp.html
│   ├── forgot_password.html
│   ├── reset_password.html
│   └── dashboard.html
├── static/
│   ├── css/
│   ├── js/
│   └── img/
├── data/
│   ├── file1.csv
│   ├── file2.csv
│   └── README.txt
└── hardware/
    ├── AI_SAFE_Arduino_MQ135.ino
    ├── smoke_monitor_advanced_AI_SAFE.ino
    └── README.md
```

Some optional files or features may require additional configuration depending on the machine and connected hardware.

## Requirements

- Windows 10/11 or another Python-supported operating system
- Python 3.10+ recommended
- pip
- Arduino IDE (only if using the Arduino hardware)
- Arduino Uno and MQ-135 sensor (for live hardware readings)
- Gmail App Password (if email OTP/password-reset features are to be used)
- A webcam (only for camera-based features)

## Installation and Run

### Option A — Windows quick start

1. Download or clone this repository.
2. Extract the project ZIP if you downloaded a ZIP.
3. Open the project folder in File Explorer.
4. Run `START_AI_SAFE.bat`.
5. Follow the prompts to configure email settings if requested.
6. Open this address in your browser:

   `http://127.0.0.1:5000`

### Option B — Run from a terminal

Open a terminal in the folder containing `app.py`, then run:

```bash
python -m venv .venv
```

Activate the environment on Windows:

```powershell
.venv\Scripts\Activate.ps1
```

Install the dependencies:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Start the app:

```bash
python app.py
```

Then visit `http://127.0.0.1:5000`.

If PowerShell blocks environment activation, use Command Prompt and run:

```bat
.venv\Scripts\activate.bat
```

## Email OTP Configuration

Email verification and password-reset features require valid SMTP/Gmail configuration.

1. Enable 2-Step Verification on the Gmail account used to send messages.
2. Create a Google App Password.
3. Use the project's setup script or the environment configuration expected by the app.
4. Never put your normal Gmail password, App Password, or other private credentials in source control.

The `.env.example` file documents configuration variables. Create a local `.env` file only if needed by the application, and **never commit the real `.env` file**.

## Arduino Setup

The repository includes sketches in `hardware/`. For the documented live integration, use:

`hardware/AI_SAFE_Arduino_MQ135.ino`

The documented wiring for this sketch is:

- MQ-135 analog output → Arduino A0
- Green LED → D6
- Red LED → D7
- Buzzer → D8
- I2C LCD → SDA/SCL, if using the LCD version
- USB serial baud rate → 9600

Steps:

1. Open the sketch in Arduino IDE.
2. Select the correct Arduino board and port.
3. Upload the sketch.
4. Connect the Arduino to the computer with USB.
5. Configure the serial port if automatic detection does not work.
6. Close Arduino Serial Monitor while the web app is reading the same serial port.
7. Start AI-SAFE and check the dashboard for live readings.

Example environment settings:

```text
ARDUINO_PORT=AUTO
ARDUINO_BAUD=9600
ARDUINO_STATION_ID=1
```

If the dashboard shows that it is waiting for Arduino, check the USB cable, selected COM port, baud rate, sketch, and whether another program is using the serial port.

### Important sensor note

The MQ-135 raw ADC thresholds used in this project are demonstration thresholds. They are **not certified ppm values or an official AQI calculation**. Sensor warm-up, calibration, environment, and hardware quality affect readings.

## Data and Database

- The `data/` directory contains project CSV data files.
- The application uses SQLite for local data storage.
- Some seeded stations, incidents, readings, and notifications may be demonstration data.
- Treat demo values separately from readings collected from real hardware.
- Back up any local data you need before deleting or resetting the local database.

## Security Before Publishing

Before uploading the project to a public GitHub repository:

- Do not upload `.env` or real email credentials.
- Do not upload Firebase service-account JSON files or other private keys.
- Do not publish personal information stored in the local database.
- Do not publish generated local databases from `instance/` unless they have been reviewed and sanitized.
- Do not upload `__pycache__/`, `.pyc` files, or your virtual environment.
- Review all configuration files for secrets before committing.

The included `.gitignore` excludes `.env`, `instance/`, Python cache files, and Firebase service-account files. Keep those protections in place.

## Troubleshooting

**The website does not open**
- Confirm the terminal shows that Flask started successfully.
- Check whether port `5000` is already in use.
- Install the dependencies from `requirements.txt`.

**Email OTP does not arrive**
- Verify the sender email and App Password.
- Confirm Gmail 2-Step Verification is enabled.
- Check spam/junk mail and the terminal output.

**Arduino readings are missing**
- Check the COM port and USB cable.
- Confirm the sketch is uploaded and uses 9600 baud.
- Close Arduino Serial Monitor or other serial tools.
- Confirm the sensor wiring and allow the sensor to warm up.

**Camera feature does not work**
- Check camera permission and whether another application is using the webcam.
- Review the terminal output for OpenCV or camera errors.

## Limitations and Future Improvements

- Validate the sensor against suitable calibrated equipment before drawing real-world conclusions.
- Improve sensor calibration and environmental compensation.
- Test alerts under controlled conditions and document false positives/negatives.
- Add automated tests, stronger production deployment settings, and role-based access review.
- Deploy only with secure HTTPS, strong credentials, and properly managed secrets.

## Academic Use

This project is intended for learning and demonstration. Explain which readings are live hardware measurements and which records are generated/demo data during presentations and evaluation.

## License

No license is specified in this repository yet. Add a `LICENSE` file if you intend to permit others to reuse or modify the project.
