# github.io
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Schluckauf Stop</title>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            border-radius: 24px;
            padding: 40px 30px;
            max-width: 420px;
            width: 100%;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            text-align: center;
        }

        h1 {
            color: #333;
            font-size: 2rem;
            margin-bottom: 10px;
        }

        .subtitle {
            color: #667eea;
            font-size: 1.1rem;
            margin-bottom: 25px;
            font-weight: 500;
        }

        .emoji {
            font-size: 4rem;
            margin-bottom: 20px;
        }

        .message {
            font-size: 1.5rem;
            font-weight: 600;
            color: #667eea;
            margin-bottom: 30px;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .timer-display {
            font-size: 4rem;
            font-weight: 700;
            color: #333;
            font-family: 'SF Mono', 'Consolas', monospace;
            margin-bottom: 30px;
            letter-spacing: 2px;
        }

        .lap-times {
            background: #f8f9fa;
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 30px;
            max-height: 200px;
            overflow-y: auto;
            display: none;
        }

        .lap-times h3 {
            color: #666;
            font-size: 0.9rem;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 15px;
        }

        .lap-item {
            display: flex;
            justify-content: space-between;
            padding: 10px 15px;
            background: white;
            border-radius: 10px;
            margin-bottom: 8px;
            font-weight: 500;
        }

        .lap-item:last-child {
            margin-bottom: 0;
        }

        .lap-number {
            color: #667eea;
        }

        .lap-time {
            color: #333;
            font-family: 'SF Mono', 'Consolas', monospace;
        }

        .buttons {
            display: flex;
            flex-direction: column;
            gap: 14px;
        }

        button {
            padding: 18px 30px;
            font-size: 1.1rem;
            font-weight: 600;
            border: none;
            border-radius: 14px;
            cursor: pointer;
            transition: all 0.2s ease;
            width: 100%;
        }

        button:active {
            transform: scale(0.98);
        }

        .btn-primary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .btn-primary:hover {
            box-shadow: 0 8px 25px rgba(102, 126, 234, 0.4);
        }

        .btn-secondary {
            background: #f0f0f0;
            color: #333;
        }

        .btn-secondary:hover {
            background: #e0e0e0;
        }

        /* Größere rote Taste */
        .btn-danger {
            background: #ff6b6b;
            color: white;
            padding: 24px 30px;
            font-size: 1.4rem;
            border-radius: 18px;
        }

        .btn-danger:hover {
            background: #ee5a5a;
            transform: scale(1.03);
        }

        .btn-success {
            background: #51cf66;
            color: white;
        }

        .btn-success:hover {
            background: #40c057;
        }

        .hidden {
            display: none !important;
        }

        .breathing-animation {
            animation: breathe 4s ease-in-out infinite;
        }

        @keyframes breathe {
            0%, 100% {
                transform: scale(1);
                opacity: 0.8;
            }

            50% {
                transform: scale(1.1);
                opacity: 1;
            }
        }

        .success-message {
            color: #51cf66;
            font-size: 1.3rem;
            margin-bottom: 20px;
        }

        .tip {
            background: #fff3cd;
            border-radius: 12px;
            padding: 15px;
            margin-bottom: 25px;
            font-size: 0.95rem;
            color: #856404;
        }

        @media (max-width: 380px) {
            .container {
                padding: 30px 20px;
            }

            h1 {
                font-size: 1.6rem;
            }

            .timer-display {
                font-size: 3rem;
            }

            .emoji {
                font-size: 3rem;
            }

            .btn-danger {
                font-size: 1.2rem;
                padding: 20px 24px;
            }
        }
    </style>
</head>

<body>

    <div class="container">

        <!-- Start Screen -->
        <div id="startScreen">

            <div class="emoji">😮‍💨</div>

            <h1>Schluckauf Stop</h1>

            <div class="subtitle">
                Tief Luftholen und Start drücken.
            </div>

            <div class="tip">
                💡 <strong>Tipp:</strong>
                Halte die Luft so lange wie möglich an.
                Drücke bei jedem Schluckauf die Rundentaste.
            </div>

            <div class="buttons">
                <button class="btn-primary" onclick="startExercise()">
                    🚀 Starten
                </button>
            </div>

        </div>

        <!-- Exercise Screen -->
        <div id="exerciseScreen" class="hidden">

            <div class="emoji breathing-animation">🫁</div>

            <div class="message" id="message">
                Atem anhalten!
            </div>

            <div class="timer-display" id="timer">
                00:00.0
            </div>

            <div class="lap-times" id="lapTimesContainer">
                <h3>🔔 Schluckauf-Zeiten</h3>
                <div id="lapTimesList"></div>
            </div>

            <div class="buttons">

                <button class="btn-danger" id="lapButton" onclick="recordLap()">
                    🔔 Schluckauf!
                </button>

                <button class="btn-success hidden" id="successButton" onclick="showSuccess()">
                    ✅ Geschafft! Kein Schluckauf mehr
                </button>

                <button class="btn-secondary" onclick="resetExercise()">
                    🔄 Neu starten
                </button>

            </div>

        </div>

        <!-- Success Screen -->
        <div id="successScreen" class="hidden">

            <div class="emoji">🎉</div>

            <h1>Gratulation!</h1>

            <div class="success-message" id="successStats"></div>

            <div class="buttons">
                <button class="btn-primary" onclick="resetExercise()">
                    🔄 Nochmal versuchen
                </button>
            </div>

        </div>

    </div>

    <script>

        let timerInterval;
        let startTime;
        let elapsedTime = 0;
        let lapTimes = [];
        let isRunning = false;

        function startExercise() {

            document.getElementById('startScreen').classList.add('hidden');
            document.getElementById('exerciseScreen').classList.remove('hidden');
            document.getElementById('successScreen').classList.add('hidden');

            startTimer();
        }

        function startTimer() {

            startTime = Date.now() - elapsedTime;
            isRunning = true;

            timerInterval = setInterval(() => {

                elapsedTime = Date.now() - startTime;
                updateTimerDisplay(elapsedTime);

            }, 100);
        }

        function updateTimerDisplay(time) {

            const minutes = Math.floor(time / 60000);
            const seconds = Math.floor((time % 60000) / 1000);
            const tenths = Math.floor((time % 1000) / 100);

            document.getElementById('timer').textContent =
                `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}.${tenths}`;
        }

        function formatTime(time) {

            const minutes = Math.floor(time / 60000);
            const seconds = Math.floor((time % 60000) / 1000);
            const tenths = Math.floor((time % 1000) / 100);

            return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}.${tenths}`;
        }

        function recordLap() {

            if (!isRunning) return;

            const lapTime = elapsedTime;
            lapTimes.push(lapTime);

            document.getElementById('lapTimesContainer').style.display = 'block';
            document.getElementById('successButton').classList.remove('hidden');

            const lapsList = document.getElementById('lapTimesList');

            const lapItem = document.createElement('div');

            lapItem.className = 'lap-item';

            lapItem.innerHTML = `
                <span class="lap-number">Schluckauf ${lapTimes.length}</span>
                <span class="lap-time">${formatTime(lapTime)}</span>
            `;

            lapsList.insertBefore(lapItem, lapsList.firstChild);

            updateMessage();

            const button = document.getElementById('lapButton');

            button.style.transform = 'scale(0.95)';

            setTimeout(() => {
                button.style.transform = '';
            }, 150);
        }

        function updateMessage() {

            const messages = [
                "Weiter anhalten! 💪",
                "Du schaffst das! 🌟",
                "Halte durch! 🫁",
                "Fast geschafft! ✨",
                "Konzentriere dich! 🎯"
            ];

            const randomMessage =
                messages[Math.floor(Math.random() * messages.length)];

            document.getElementById('message').textContent = randomMessage;
        }

        function showSuccess() {

            clearInterval(timerInterval);
            isRunning = false;

            document.getElementById('exerciseScreen').classList.add('hidden');
            document.getElementById('successScreen').classList.remove('hidden');

            const totalTime = formatTime(elapsedTime);
            const lapCount = lapTimes.length;

            let statsText = `Gesamtzeit: ${totalTime}`;

            if (lapCount > 0) {

                statsText += `<br>${lapCount} Schluckauf${lapCount > 1 ? 's' : ''} überwunden!`;

            } else {

                statsText = `Kein einziger Schluckauf!<br>Zeit: ${totalTime}`;
            }

            document.getElementById('successStats').innerHTML = statsText;
        }

        function resetExercise() {

            clearInterval(timerInterval);

            isRunning = false;
            elapsedTime = 0;
            lapTimes = [];

            document.getElementById('timer').textContent = '00:00.0';
            document.getElementById('lapTimesList').innerHTML = '';
            document.getElementById('lapTimesContainer').style.display = 'none';
            document.getElementById('successButton').classList.add('hidden');

            document.getElementById('message').textContent =
                'Atem anhalten!';

            document.getElementById('startScreen').classList.remove('hidden');
            document.getElementById('exerciseScreen').classList.add('hidden');
            document.getElementById('successScreen').classList.add('hidden');
        }

        window.onbeforeunload = function () {

            if (isRunning) {
                return "Die Übung läuft noch. Wirklich verlassen?";
            }
        };

    </script>

</body>
</html>
