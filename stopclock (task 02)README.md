# PRODIGY_WD_02
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Stopwatch Web Application</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
        }
        .stopwatch {
            background-color: white;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        .display {
            font-size: 48px;
            text-align: center;
            margin-bottom: 20px;
        }
        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }
        button {
            font-size: 16px;
            padding: 10px 20px;
            cursor: pointer;
        }
        .laps {
            margin-top: 20px;
            max-height: 150px;
            overflow-y: auto;
        }
    </style>
</head>
<body>
    <div class="stopwatch">
        <div class="display">00:00:00</div>
        <div class="controls">
            <button id="startPause">Start</button>
            <button id="reset">Reset</button>
            <button id="lap">Lap</button>
        </div>
        <div class="laps"></div>
    </div>

    <script>
        let timer;
        let isRunning = false;
        let startTime;
        let elapsedTime = 0;
        let laps = [];

        const display = document.querySelector('.display');
        const startPauseButton = document.getElementById('startPause');
        const resetButton = document.getElementById('reset');
        const lapButton = document.getElementById('lap');
        const lapsContainer = document.querySelector('.laps');

        function formatTime(ms) {
            const date = new Date(ms);
            return date.toISOString().substr(11, 8);
        }

        function updateDisplay() {
            display.textContent = formatTime(elapsedTime);
        }

        function startPause() {
            if (isRunning) {
                clearInterval(timer);
                startPauseButton.textContent = 'Start';
            } else {
                startTime = Date.now() - elapsedTime;
                timer = setInterval(() => {
                    elapsedTime = Date.now() - startTime;
                    updateDisplay();
                }, 10);
                startPauseButton.textContent = 'Pause';
            }
            isRunning = !isRunning;
        }

        function reset() {
            clearInterval(timer);
            isRunning = false;
            elapsedTime = 0;
            laps = [];
            updateDisplay();
            startPauseButton.textContent = 'Start';
            lapsContainer.innerHTML = '';
        }

        function lap() {
            if (isRunning) {
                laps.push(elapsedTime);
                const lapItem = document.createElement('div');
                lapItem.textContent = `Lap ${laps.length}: ${formatTime(elapsedTime)}`;
                lapsContainer.appendChild(lapItem);
            }
        }

        startPauseButton.addEventListener('click', startPause);
        resetButton.addEventListener('click', reset);
        lapButton.addEventListener('click', lap);
    </script>
</body>
</html>
