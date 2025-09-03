# Autonomous-Racing-Car-using-TORCS-and-Neural-Networks

Project Architecture & Workflow
This project implements a complete end-to-end AI racing system using TORCS (The Open Racing Car Simulator) that learns from human driving data and can autonomously control a racing car. The system follows a data-driven approach with three main phases: data collection, model training, and autonomous driving.

<img width="480" height="362" alt="4" src="https://github.com/user-attachments/assets/f40baee8-e89c-417e-912a-0e5be704b212" />


<img width="486" height="384" alt="2" src="https://github.com/user-attachments/assets/732df88a-ab4a-4661-870d-405461800288" />



Phase 1: Data Collection System
The data collection phase utilizes a sophisticated driver interface that supports multiple control modes:
Multi-Modal Control Interface: The system supports three distinct control modes - AI-driven autonomous control, keyboard-based human control, and Xbox 360 controller input, allowing for flexible data collection from different input methods.
Comprehensive Sensor Logging: The driver.py module implements extensive telemetry logging that captures all available sensor data including track distance sensors (19 rangefinders), car state information (speed, RPM, gear, position), and control inputs (acceleration, braking, steering, gear changes).
Real-Time Data Processing: The system processes incoming sensor messages from TORCS in real-time, parsing complex sensor arrays and converting them into structured CSV format with timestamps for analysis.
Structured Data Storage: All collected data is automatically saved to timestamped CSV files in the logs/ directory, with separate files for human vs AI driving sessions, enabling comparative analysis of driving styles.

Phase 2: Data Preprocessing & Feature Engineering
The data preprocessing pipeline transforms raw sensor data into machine learning-ready features:
Data Cleaning Pipeline: The cleaner.py module handles missing value removal, data type conversion, and normalization using MinMaxScaler to ensure consistent data quality across different driving sessions.
Feature Engineering: The system creates derived features including speed differentials, steering changes, and acceleration patterns to capture temporal relationships in driving behavior.
Multi-Track Dataset: The project includes extensive datasets from the BEST/ directory containing driving data across multiple tracks (dirt, oval, road) and car models (Corolla, Lancer, P406), providing diverse training scenarios.
Sensor Integration: The system processes 19 track distance sensors, car state variables (position, angle, speed components), and mechanical state (RPM, gear) to create a comprehensive feature set for the neural network.

Phase 3: Neural Network Training System
The training system implements a sophisticated multi-output neural network architecture:
Multi-Output Regression Model: Uses scikit-learn's MLPRegressor with a deep architecture (256-128-64 hidden layers) to simultaneously predict four control outputs: acceleration, braking, steering, and gear selection.
Advanced Training Configuration: The model employs adaptive learning rates, early stopping with validation, L2 regularization (alpha=0.0001), and batch processing (batch_size=32) to prevent overfitting and ensure stable training.
Feature Scaling & Normalization: Implements StandardScaler for feature normalization, ensuring all input features are on the same scale for optimal neural network performance.
Model Persistence: Trained models and scalers are saved using joblib for efficient loading during inference, enabling quick deployment of trained models.

Phase 4: Autonomous Driving System
The autonomous driving system implements real-time inference and control:
Real-Time Sensor Processing: The NNDriver class extends the base Driver class to parse incoming TORCS sensor messages and extract relevant features in real-time.
State Preparation Pipeline: Implements the same feature extraction and scaling process used during training to ensure consistency between training and inference phases.
Multi-Output Control Generation: The neural network simultaneously generates four control values (acceleration, braking, steering, gear) based on current sensor readings, enabling smooth and coordinated vehicle control.
Safety Constraints: Implements gear range validation (1-6) and ensures all control outputs are within valid ranges to prevent unsafe driving commands.
