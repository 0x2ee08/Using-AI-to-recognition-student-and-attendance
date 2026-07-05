# SmartAttend - Student Attendance & Health Monitoring

An end-to-end student recognition, attendance, and basic health monitoring system. The system automatically recognizes faces, measures body temperature, checks for masks, and updates data in real-time to a Web Dashboard.

## Skills & Technologies Used

| Domain | Applied Technologies / Skills |
| :--- | :--- |
| **AI & Computer Vision** | OpenCV (LBPH Face Recognition), Keras/TensorFlow (Mask Detection), MediaPipe |
| **Backend & Web** | Python, Flask, RESTful API, JWT Authentication |
| **Database** | MongoDB, SQLite |
| **Hardware & IoT** | Raspberry Pi, Arduino, MLX90640 Thermal Sensor, Serial/I2C Communication |
| **Networking & Others** | Socket Communication (File transfer between Pi and Server), Automated Email Notifications |

## How It Works

1. **Collection & Training (Raspberry Pi):** The camera captures student faces to create a dataset, registers the information via API, and trains the recognition model (LBPH) directly on the Pi.
2. **Scanning & Data Transfer (Raspberry Pi):** The Pi continuously reads the video stream and thermal sensor data. When a face is detected, the system crops the image, assigns an estimated temperature, and sends it to the Server via a Socket connection.
3. **Central Processing (Server):** The server receives the image files, runs the recognition model to identify the student ID, and processes the image through a Keras model to verify mask-wearing status.
4. **Storage & Alerts (Web/Database):** Attendance status, temperature, and mask status are stored in MongoDB. In case of anomalies (e.g., high fever, missing mask), the system automatically triggers an email alert and updates the metrics on the Web Dashboard.

## System Architecture Diagram

```text
+-------------------------+            +------------------------------------+
|      CLIENT NODE        |            |            CENTRAL SERVER          |
|                         |            |                                    |
|  [Arduino] (Peripherals)|            |  1. AI Processing:                 |
|           <->           |  Sockets   |     - Face Recognition (LBPH)      |
|  [Raspberry Pi]         |===========>|     - Mask Detection (Keras)       |
|     - Camera (Frames)   | (Images +  |                                    |
|     - MLX90640 (Temp)   |  Temp)     |  2. Database: MongoDB & SQLite     |
|     - Crop & Send Image |            |                                    |
|                         |            |  3. Alerts: Automated Email        |
+-------------------------+            +------------------------------------+
                                                             | (API / HTTP)
                                                             v
                                               +---------------------------+
                                               |       WEB DASHBOARD       |
                                               | - Flask Interface (UI)    |
                                               | - Admin/Student Mgmt      |
                                               | - Charts & Reports        |
                                               +---------------------------+
