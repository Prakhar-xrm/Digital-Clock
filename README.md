<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Clock Application</title>
    <style>
        :root {
            --primary-color: #2563eb;
            --secondary-color: #1e40af;
            --text-color: #1f2937;
            --light-color: #f9fafb;
            --shadow-color: rgba(0, 0, 0, 0.1);
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: var(--light-color);
            color: var(--text-color);
            margin: 0;
            padding: 20px;
            transition: background-color 0.3s;
        }

        .clock-container {
            width: 100%;
            max-width: 600px;
            background: white;
            border-radius: 16px;
            box-shadow: 0 10px 30px var(--shadow-color);
            overflow: hidden;
            padding: 20px;
        }

        .header {
            text-align: center;
            margin-bottom: 20px;
        }

        .header h1 {
            color: var(--primary-color);
            font-size: 2rem;
            margin: 0;
        }

        .tabs {
            display: flex;
            justify-content: center;
            margin-bottom: 30px;
            border-bottom: 1px solid #e5e7eb;
        }

        .tab {
            padding: 10px 20px;
            cursor: pointer;
            font-weight: 600;
            color: #6b7280;
            transition: all 0.3s;
            position: relative;
        }

        .tab.active {
            color: var(--primary-color);
        }

        .tab.active::after {
            content: '';
            position: absolute;
            bottom: -1px;
            left: 0;
            right: 0;
            height: 2px;
            background-color: var(--primary-color);
        }

        .tab:hover:not(.active) {
            color: var(--secondary-color);
        }

        .tab-content {
            display: none;
            padding: 20px;
        }

        .tab-content.active {
            display: block;
        }

        /* Analog Clock */
        .analog-clock {
            position: relative;
            width: 300px;
            height: 300px;
            margin: 0 auto;
            border: 10px solid var(--primary-color);
            border-radius: 50%;
            box-shadow: 0 0 20px var(--shadow-color);
        }

        .clock-face {
            position: relative;
            width: 100%;
            height: 100%;
            transform: rotate(90deg);
        }

        .hand {
            position: absolute;
            top: 50%;
            left: 50%;
            background-color: var(--text-color);
            transform-origin: left center;
            border-radius: 10px;
            box-shadow: 0 0 5px var(--shadow-color);
        }

        .hour-hand {
            width: 70px;
            height: 8px;
            z-index: 3;
        }

        .minute-hand {
            width: 100px;
            height: 6px;
            z-index: 2;
        }

        .second-hand {
            width: 110px;
            height: 3px;
            background-color: var(--primary-color);
            z-index: 1;
        }

        .center-dot {
            position: absolute;
            top: 50%;
            left: 50%;
            width: 15px;
            height: 15px;
            background-color: var(--primary-color);
            border-radius: 50%;
            transform: translate(-50%, -50%);
            z-index: 4;
        }

        /* Digital Clock */
        .digital-clock {
            text-align: center;
            font-size: 4rem;
            font-weight: 700;
            color: var(--primary-color);
            margin: 30px 0;
        }

        .date {
            text-align: center;
            font-size: 1.5rem;
            color: var(--text-color);
            margin-bottom: 20px;
        }

        /* Responsive */
        @media (max-width: 500px) {
            .analog-clock {
                width: 250px;
                height: 250px;
            }

            .digital-clock {
                font-size: 3rem;
            }
        }
    </style>
</head>
<body>
    <div class="clock-container">
        <div class="header">
            <h1>Clock Application</h1>
        </div>

        <div class="tabs">
            <div class="tab active" onclick="switchTab('analog')">Analog</div>
            <div class="tab" onclick="switchTab('digital')">Digital</div>
        </div>

        <div id="analog" class="tab-content active">
            <div class="analog-clock">
                <div class="clock-face">
                    <div class="hand hour-hand"></div>
                    <div class="hand minute-hand"></div>
                    <div class="hand second-hand"></div>
                    <div class="center-dot"></div>
                </div>
            </div>
        </div>

        <div id="digital" class="tab-content">
            <div class="digital-clock">
                <span id="hours">00</span>:<span id="minutes">00</span>:<span id="seconds">00</span>
            </div>
            <div class="date">
                <span id="day">Monday</span>, <span id="month">January</span> <span id="date">1</span>, <span id="year">2023</span>
            </div>
        </div>
    </div>

    <script>
        function updateClock() {
            const now = new Date();
            
            // Analog clock
            const hourHand = document.querySelector('.hour-hand');
            const minuteHand = document.querySelector('.minute-hand');
            const secondHand = document.querySelector('.second-hand');
            
            const hours = now.getHours();
            const minutes = now.getMinutes();
            const seconds = now.getSeconds();
            
            const hourDeg = (hours % 12) * 30 + (minutes / 2);
            const minuteDeg = minutes * 6;
            const secondDeg = seconds * 6;
            
            hourHand.style.transform = `rotate(${hourDeg}deg)`;
            minuteHand.style.transform = `rotate(${minuteDeg}deg)`;
            secondHand.style.transform = `rotate(${secondDeg}deg)`;
            
            // Digital clock
            document.getElementById('hours').textContent = String(hours).padStart(2, '0');
            document.getElementById('minutes').textContent = String(minutes).padStart(2, '0');
            document.getElementById('seconds').textContent = String(seconds).padStart(2, '0');
            
            // Date
            const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
            const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
            
            document.getElementById('day').textContent = days[now.getDay()];
            document.getElementById('month').textContent = months[now.getMonth()];
            document.getElementById('date').textContent = now.getDate();
            document.getElementById('year').textContent = now.getFullYear();
        }
        
        function switchTab(tabName) {
            // Hide all tabs
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            
            // Deactivate all tab buttons
            document.querySelectorAll('.tab').forEach(tab => {
                tab.classList.remove('active');
            });
            
            // Show selected tab
            document.getElementById(tabName).classList.add('active');
            
            // Activate selected tab button
            document.querySelector(`.tab[onclick="switchTab('${tabName}')"]`).classList.add('active');
        }
        
        // Update clock immediately and then every second
        updateClock();
        setInterval(updateClock, 1000);
    </script>
</body>
</html>

