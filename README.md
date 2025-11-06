# LT Line Monitoring System MVP

A complete IoT-based monitoring solution for Low Tension (LT) distribution lines with real-time data visualization and automatic trip detection.

## 🚀 Features

- **Real-time Monitoring**: Live voltage, current, and line status monitoring
- **Trip Detection**: Automatic detection of line breaks/snapped wires
- **Professional Dashboard**: Modern DISCOM-style web interface
- **Alert System**: Instant notifications for line trips
- **Historical Data**: Trends charts and historical readings
- **IoT Integration**: ESP8266-based sensor nodes
- **RESTful API**: Complete backend API for data management

## 📁 Project Structure

```
python-ide/
├── backend/           # Flask backend server
│   ├── app.py        # Main Flask application
│   ├── requirements.txt
│   └── .env          # Database configuration
├── frontend/          # Web dashboard
│   ├── index.html    # Main dashboard page
│   ├── styles.css    # Modern CSS styling
│   └── dashboard.js  # JavaScript functionality
├── esp8266/           # Arduino code for IoT devices
│   ├── lt_line_monitor.ino
│   └── libraries_required.txt
└── docs/             # Documentation
```

## 🛠️ System Components

### 1. Backend (Flask + MySQL)
- **Flask**: Web framework for API endpoints
- **MySQL**: Database for storing transformer and reading data
- **SQLAlchemy**: ORM for database operations
- **CORS**: Cross-origin resource sharing support

### 2. Frontend (HTML + CSS + JavaScript)
- **Modern Dashboard**: Professional DISCOM-style interface
- **Chart.js**: Real-time trending graphs
- **Responsive Design**: Works on desktop and mobile
- **Auto-refresh**: Live data updates every 5 seconds

### 3. IoT Hardware (ESP8266)
- **ZMPT101B**: Voltage sensor for AC voltage measurement
- **SCT-013**: Current transformer for current measurement
- **ADS1115**: 16-bit ADC for precise current readings
- **ESP8266**: WiFi microcontroller for data transmission

## 📋 Prerequisites

### Software Requirements
- Python 3.8+ 
- MySQL 8.0+
- Arduino IDE 2.0+
- Modern web browser

### Hardware Requirements
- ESP8266 (NodeMCU/Wemos D1 Mini)
- ZMPT101B Voltage Sensor
- SCT-013 Current Transformer
- ADS1115 16-bit ADC
- Breadboard and jumper wires

## 🚀 Quick Start

### 1. Database Setup

Create MySQL database:
```sql
CREATE DATABASE lt_monitoring;
CREATE USER 'lt_user'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON lt_monitoring.* TO 'lt_user'@'localhost';
FLUSH PRIVILEGES;
```

### 2. Backend Setup

```bash
# Navigate to backend directory
cd backend

# Install Python dependencies
pip install -r requirements.txt

# Configure database in .env file
DB_HOST=localhost
DB_USER=lt_user
DB_PASSWORD=your_password
DB_NAME=lt_monitoring

# Run the Flask application
python app.py
```

The backend will be available at: `http://localhost:5000`

### 3. Frontend Setup

```bash
# Navigate to frontend directory
cd frontend

# Open dashboard in web browser
# Simply open index.html in your browser or use a local web server

# For development server (optional):
python -m http.server 8000
# Then visit: http://localhost:8000
```

### 4. ESP8266 Setup

1. **Install Arduino IDE** and required libraries (see `esp8266/libraries_required.txt`)

2. **Configure the code** in `lt_line_monitor.ino`:
   ```cpp
   const char* WIFI_SSID = "Your_WiFi_Name";
   const char* WIFI_PASSWORD = "Your_WiFi_Password";
   const char* SERVER_URL = "http://192.168.1.100:5000";  // Your Flask server IP
   const char* TRANSFORMER_ID = "TX001";  // Unique transformer ID
   ```

3. **Wire the hardware** according to the pin diagram

4. **Upload the code** to ESP8266

## 📊 API Endpoints

### Transformer Management
- `POST /add_transformer` - Register new transformer
- `GET /get_transformers` - List all transformers

### Reading Management
- `POST /add_reading` - Add new sensor reading
- `GET /get_readings/<transformer_id>` - Get readings for transformer
- `GET /get_latest_reading/<transformer_id>` - Get latest reading

### System Status
- `GET /health` - Health check endpoint
- `GET /` - API information

### Example API Usage

**Add Transformer:**
```bash
curl -X POST http://localhost:5000/add_transformer \
  -H "Content-Type: application/json" \
  -d '{"transformer_id": "TX001", "location": "Feeder Line 1"}'
```

**Add Reading:**
```bash
curl -X POST http://localhost:5000/add_reading \
  -H "Content-Type: application/json" \
  -d '{
    "transformer_id": "TX001",
    "voltage": 230.5,
    "current": 5.2,
    "trip_status": false
  }'
```

## 🔌 Hardware Wiring

### ESP8266 Pin Connections:
```
ZMPT101B Voltage Sensor:
├── VCC → 3.3V
├── GND → GND
└── OUT → A0

ADS1115 ADC:
├── VCC → 3.3V
├── GND → GND
├── SDA → D2 (GPIO4)
└── SCL → D1 (GPIO5)

SCT-013 Current Sensor:
└── Connect to ADS1115 A0 with burden resistor circuit
```

## 🎯 Dashboard Features

### Real-time Monitoring Cards
- **Voltage Card**: Shows current voltage with status indication
- **Current Card**: Displays load current with trend
- **Trip Status**: Visual alert for line break detection

### Charts and Analytics
- **Trend Charts**: Real-time voltage and current graphs
- **Historical Data**: Tabular view of recent readings
- **Status Indicators**: Color-coded system health

### Alert System
- **Trip Alerts**: Instant popup notifications
- **Audio Alerts**: Sound notifications for critical events
- **Status Dashboard**: Connection and system status

## 🛡️ Trip Detection Logic

The system detects line trips using the following algorithm:

1. **Normal Operation**: Both voltage and current are within expected ranges
2. **Trip Detection**: When voltage > 180V but current drops below 0.05A
3. **Confirmation**: Trip status maintained until current returns to normal
4. **Alert Generation**: Immediate notification to dashboard and operators

## 🔧 Configuration Options

### Backend Configuration (.env)
```env
# Database
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=lt_monitoring

# Flask
FLASK_ENV=development
FLASK_DEBUG=True
```

### Frontend Configuration (dashboard.js)
```javascript
const CONFIG = {
    API_BASE_URL: 'http://localhost:5000',
    REFRESH_INTERVAL: 5000,  // 5 seconds
    VOLTAGE_THRESHOLDS: { low: 200, high: 250 },
    CURRENT_THRESHOLDS: { low: 0.1, high: 15 }
};
```

### ESP8266 Configuration
```cpp
// Sensor calibration
#define VOLTAGE_CALIBRATION 250.0
#define CURRENT_CALIBRATION 10.0
#define TRIP_THRESHOLD 0.05
#define READING_INTERVAL 5000  // 5 seconds
```

## 📱 Dashboard Screenshots

The dashboard provides:
- **Professional DISCOM Interface**: Clean, modern design
- **Real-time Data Cards**: Live voltage, current, and status
- **Interactive Charts**: Trend visualization with Chart.js
- **Responsive Layout**: Works on desktop, tablet, and mobile
- **Alert Modals**: Instant trip notifications

## 🚨 Troubleshooting

### Common Issues:

1. **Database Connection Failed**
   - Verify MySQL is running
   - Check database credentials in `.env`
   - Ensure database exists

2. **ESP8266 Not Connecting**
   - Check WiFi credentials
   - Verify server IP address
   - Check network connectivity

3. **No Data in Dashboard**
   - Verify backend is running on port 5000
   - Check browser console for errors
   - Ensure transformers are registered

4. **Sensor Readings Incorrect**
   - Check hardware wiring
   - Adjust calibration values
   - Verify sensor power supply

## 📈 Scaling and Production

For production deployment:

1. **Database**: Use MySQL with proper indexes and replication
2. **Backend**: Deploy with Gunicorn + Nginx
3. **Frontend**: Serve via CDN or web server
4. **Security**: Add authentication and HTTPS
5. **Monitoring**: Add logging and health checks

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request
6. 
## 🌱 Branch Strategy

This repository uses multiple branches for organized development:

### Feature Branches:
- **feature/alerting-sms-whatsapp** - SMS/WhatsApp notification system
- **feature/hardware-v2** - Next-gen hardware with ESP32 and advanced sensors
- **feature/analytics-export** - Data export to CSV, Excel, PDF with BI integration
- **feature/authentication** - User authentication and role-based access control

### Bugfix Branches:
- **bugfix/dashboard-ui** - Dashboard UI improvements and fixes

### Documentation Branches:
- **docs/improved-readme** - Enhanced documentation and guides

### Testing Branches:
- **testing/hardware-sim** - Hardware simulation and automated testing

### Deployment Branches:
- **deployment/dockerize** - Docker containerization and CI/CD pipeline

Each branch contains a `TODO.md` file outlining planned features and implementation steps.

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 👥 Support

For support and questions:
- Create an issue in the repository
- Check troubleshooting section
- Review hardware wiring diagrams

## 🎉 Acknowledgments

- Flask community for excellent web framework
- Chart.js for beautiful data visualization
- ESP8266 community for IoT platform
- Adafruit for sensor libraries

---

**Built for Distribution Companies (DISCOMs) to monitor LT line health and detect faults in real-time.**
