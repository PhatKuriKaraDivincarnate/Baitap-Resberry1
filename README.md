# Baitap-Resberry1

!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mô phỏng cảm biến và joystick</title>
    <style>
        /* CSS */
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f9;
            color: #333;
        }

        header {
            background-color: #4caf50;
            color: white;
            padding: 10px 20px;
            text-align: center;
        }

        main {
            padding: 20px;
        }

        .card {
            background: white;
            margin: 10px auto;
            padding: 15px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
            border-radius: 8px;
            max-width: 400px;
        }

        .card h2 {
            margin-top: 0;
            color: #4caf50;
        }

        .led-display {
            font-family: monospace;
            text-align: center;
            margin-top: 10px;
            font-size: 24px;
            background: #222;
            color: #0f0;
            padding: 10px;
            border-radius: 5px;
        }

        footer {
            text-align: center;
            margin-top: 20px;
            font-size: 14px;
        }
    </style>
</head>
<body>
    <header>
        <h1>Trình mô phỏng cảm biến và joystick</h1>
    </header>
    <main>
        <div class="card">
            <h2>Nhiệt độ và độ ẩm</h2>
            <p id="temp">Nhiệt độ: Đang tải...</p>
            <p id="humidity">Độ ẩm: Đang tải...</p>
        </div>

        <div class="card">
            <h2>Trạng thái Joystick</h2>
            <p id="joystick">Đang tải...</p>
        </div>

        <div class="card">
            <h2>Hiển thị tên</h2>
            <div class="led-display" id="led-display">_</div>
        </div>
    </main>
    <footer>
        <p>NGUYEN CHI PHAT| Mô phỏng cảm biến và joystick</p>
    </footer>
    
    <script>
        // Mô phỏng nhiệt độ và độ ẩm
        function getTemperatureAndHumidity() {
            return fetch('/api/temperature')
                .then(response => response.json())
                .then(data => {
                    document.getElementById("temp").textContent = Nhiệt độ: ${data.temperature.toFixed(1)}°C;
                });

            return fetch('/api/humidity')
                .then(response => response.json())
                .then(data => {
                    document.getElementById("humidity").textContent = Độ ẩm: ${data.humidity.toFixed(1)}%;
                });
        }

        // Mô phỏng trạng thái joystick
        function getJoystickState() {
            const states = ["up", "down", "left", "right", "center", "neutral"];
            return states[Math.floor(Math.random() * states.length)];
        }

        // Mô phỏng hiển thị tên trên LED
        const nameToDisplay = "NGUYEN CHI PHAT"; 
        let currentCharIndex = 0;

        function displayNameOnLED() {
            const ledDisplay = document.getElementById("led-display");
            ledDisplay.textContent = nameToDisplay[currentCharIndex];
            currentCharIndex = (currentCharIndex + 1) % nameToDisplay.length;
        }

        // Cập nhật nhiệt độ, độ ẩm và trạng thái joystick mỗi giây
        setInterval(() => {
            getTemperatureAndHumidity();
            document.getElementById("joystick").textContent = Joystick: ${getJoystickState()};
        }, 1000);

        // Hiển thị tên trên LED mỗi 300ms
        setInterval(displayNameOnLED, 300);
    </script>
</body>
</html>







from flask import Flask, jsonify
from sense_emu import SenseHat

app = Flask(_name_)
sense = SenseHat()


@app.route('/api/temperature')
def get_temperature():
    temp = sense.get_temperature()  
    return jsonify({'temperature': temp})


@app.route('/api/humidity')
def get_humidity():
    humidity = sense.get_humidity()  
    return jsonify({'humidity': humidity})

@app.route('/')
def index():
    return open('index.html').read()

if _name_ == '_main_':
    
    app.run(host='0.0.0.0', port=5000)
