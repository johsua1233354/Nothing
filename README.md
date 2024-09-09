<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Practica de apunteria</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            font-family: Arial, sans-serif;
            background-color: #222;
            color: white;
        }

        #game-area {
            position: relative;
            width: 800px;
            height: 600px;
            background-color: #333;
            border: 2px solid white;
            overflow: hidden;
        }

        .target {
            position: absolute;
            width: 50px;
            height: 50px;
            background-color: red;
            border-radius: 50%;
            cursor: pointer;
        }

        #score {
            position: absolute;
            top: 10px;
            left: 10px;
            font-size: 20px;
        }

        #timer {
            position: absolute;
            top: 10px;
            right: 10px;
            font-size: 20px;
        }
    </style>
</head>
<body>
    <div id="game-area">
        <div id="score">Puntuación: 0</div>
        <div id="timer">Tiempo: 30</div>
    </div>

    <script>
        const gameArea = document.getElementById('game-area');
        const scoreElement = document.getElementById('score');
        const timerElement = document.getElementById('timer');

        let score = 0;
        let gameDuration = 30; // Duración del juego en segundos
        let gameInterval;
        let countdownInterval;

        // Función para generar una posición aleatoria dentro del área de juego
        function getRandomPosition() {
            const x = Math.floor(Math.random() * (gameArea.clientWidth - 50)); // 50 es el tamaño del target
            const y = Math.floor(Math.random() * (gameArea.clientHeight - 50));
            return { x, y };
        }

        // Función para crear y mostrar un nuevo objetivo (círculo)
        function createTarget() {
            const target = document.createElement('div');
            target.classList.add('target');
            const { x, y } = getRandomPosition();
            target.style.left = `${x}px`;
            target.style.top = `${y}px`;

            // Cuando se haga clic en el objetivo
            target.addEventListener('click', () => {
                score++;
                scoreElement.textContent = `Puntuación: ${score}`;
                target.remove(); // Eliminar el objetivo actual
                createTarget(); // Crear uno nuevo
            });

            gameArea.appendChild(target);
        }

        // Función para iniciar el juego
        function startGame() {
            score = 0;
            scoreElement.textContent = `Puntuación: ${score}`;
            timerElement.textContent = `Tiempo: ${gameDuration}`;
            
            // Iniciar el temporizador del juego
            countdownInterval = setInterval(() => {
                gameDuration--;
                timerElement.textContent = `Tiempo: ${gameDuration}`;
                if (gameDuration <= 0) {
                    endGame();
                }
            }, 1000);

            // Iniciar el juego creando el primer objetivo
            createTarget();
        }

        // Función para finalizar el juego
        function endGame() {
            clearInterval(countdownInterval);
            alert(`¡Juego terminado! Tu puntuación es: ${score}`);
            document.querySelectorAll('.target').forEach(target => target.remove()); // Eliminar todos los objetivos
            gameDuration = 60; // Reiniciar la duración del juego
        }

        // Iniciar el juego cuando la página se cargue
        startGame();
    </script>
</body>
</html>
