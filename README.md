# Cold-Chain Monitoring with Machine Learning (Flask)

A Flask web app that monitors pharmaceutical cold-chain temperature sensors. It sends email and SMS alerts when readings cross a threshold, or when an LSTM model **predicts** they're about to.

This is the web application part of my undergraduate dissertation: *Incorporating Machine Learning in a Microcontroller-Driven Sensor System for Monitoring Cold Chain Pharmaceutical Products* (2024).

## Why

Vaccines and many medicines must stay within tight temperature ranges, usually 2–8 °C, and a fridge failure noticed too late means stock gets thrown away. The system pairs cheap Bluetooth sensors with a microcontroller, stores readings as time series, and uses a forecasting model to warn staff before a breach instead of after it.

## How it works

```
Ruuvi BLE sensor ─► ESP32 ─► InfluxDB (time series) ─► Flask app ─► dashboard
                                                          │
                                        LSTM forecast ────┤
                                                          └─► SendGrid email / Twilio SMS alerts
```

- **Dashboard:** live and recent sensor readings.
- **Thresholds:** configurable warning (1–6 °C) and critical (−1–8 °C) limits in `configs/Config.json`, editable from the settings page.
- **Prediction:** a saved Keras LSTM model (`models/LSTMModel/`) forecasts upcoming temperatures and triggers early warnings.
- **Alerts:** email through SendGrid and SMS through Twilio.
- **Login:** a simple user login protects the dashboard and settings.

## Tech stack

Python · Flask 3 · TensorFlow/Keras (LSTM) · InfluxDB · SendGrid · Twilio · pandas · scikit-learn
Hardware: ESP32 microcontroller, Ruuvi Tag BLE sensor

## Getting started

```bash
git clone https://github.com/Aaron-Darcy/cold-chain-monitoring-flask.git
cd cold-chain-monitoring-flask
python -m venv venv
venv\Scripts\activate          # macOS/Linux: source venv/bin/activate
pip install -r requirements.txt
```

Copy `.env.example` to `.env` and fill in your Flask secret key, the SendGrid and Twilio credentials, and the InfluxDB connection details. Then run:

```bash
flask run
```

## Project structure

```
app.py                  # Flask routes (dashboard, login, settings, test alerts)
configs/Config.json     # alert thresholds + contact details
models/LSTMModel/       # saved LSTM model
templates/              # dashboard + settings pages
venv/components/        # alerting, prediction and user/login helpers
Project Documents/      # final dissertation (PDF) + presentation
```

## Related

- [cold-chain-sensor-preprocessing](https://github.com/Aaron-Darcy/cold-chain-sensor-preprocessing): the data preprocessing proof of concept behind the model.
- The full write-up is in `Project Documents/`.
