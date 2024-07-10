// Variables globales
let playerName = '';
let score = 0;
let highScore = 0;
let currentLevel = 1;

const startButton = document.getElementById('startButton');
const gameArea = document.getElementById('gameArea');
const playerNameSpan = document.getElementById('playerName');
const scoreSpan = document.getElementById('score');
const highScoreSpan = document.getElementById('highScore');
const questionSpan = document.getElementById('question');
const answerInput = document.getElementById('answer');
const submitAnswerButton = document.getElementById('submitAnswerButton');
const feedbackMessage = document.getElementById('feedback');
const fruitImage = document.getElementById('fruitImage');

// Evento para comenzar el juego
startButton.addEventListener('click', () => {
    playerName = document.getElementById('username').value;
    if (playerName.trim() === '') {
        alert('Por favor, ingrese un nombre de usuario válido.');
        return;
    }
    gameArea.style.display = 'block';
    playerNameSpan.textContent = playerName;
    highScoreSpan.textContent = highScore;
    startGame();
});

// Función para iniciar el juego
function startGame() {
    loadQuestion();
}

// Función para cargar una pregunta
function loadQuestion() {
    let number1, number2, operation, correctAnswer;

    // Lógica para generar preguntas según el nivel actual
    switch (currentLevel) {
        case 1:
            number1 = Math.floor(Math.random() * 10) + 1;
            number2 = Math.floor(Math.random() * 10) + 1;
            operation = '+';
            correctAnswer = number1 + number2;
            break;
        case 2:
            number1 = Math.floor(Math.random() * 9) + 1;
            number2 = Math.floor(Math.random() * 9) + 1;
            operation = '-';
            correctAnswer = number1 - number2;
            break;
        case 3:
            number1 = Math.floor(Math.random() * 5) + 1;
            number2 = Math.floor(Math.random() * 5) + 1;
            operation = '*';
            correctAnswer = number1 * number2;
            break;
        // Implementar más niveles según tus requisitos
        default:
            number1 = Math.floor(Math.random() * 10) + 1;
            number2 = Math.floor(Math.random() * 10) + 1;
            operation = '+';
            correctAnswer = number1 + number2;
            break;
    }

    questionSpan.textContent = `${number1} ${operation} ${number2} = `;
}

// Evento para enviar respuesta
submitAnswerButton.addEventListener('click', () => {
    const userAnswer = parseInt(answerInput.value);
    if (isNaN(userAnswer)) {
        alert('Por favor, ingrese una respuesta válida.');
        return;
    }

    // Lógica para verificar la respuesta
    const correctAnswer = getCorrectAnswer();
    if (userAnswer === correctAnswer) {
        score++;
        feedbackMessage.textContent = '¡Respuesta correcta!';
        showFruitImage(); // Mostrar imagen de fruta correspondiente
    } else {
        feedbackMessage.textContent = 'Respuesta incorrecta. Intenta de nuevo.';
    }

    // Actualizar puntajes y preparar la siguiente pregunta
    scoreSpan.textContent = score;
    answerInput.value = '';
    setTimeout(() => {
        fruitImage.style.display = 'none'; // Ocultar la imagen después de un tiempo
        loadQuestion();
    }, 1500); // Tiempo en milisegundos antes de cargar la siguiente pregunta
});

// Función para mostrar la imagen de la fruta correspondiente
function showFruitImage() {
    const fruitOptions = ['apple', 'pear', 'orange', 'mango', 'peach', 'lemon'];
    const randomFruit = fruitOptions[Math.floor(Math.random() * fruitOptions.length)];
    const imageUrl = `${randomFruit}.png`;
    
    fruitImage.src = imageUrl;
    fruitImage.style.display = 'block';
}

// Función para obtener la respuesta correcta según la operación actual
function getCorrectAnswer() {
    const questionText = questionSpan.textContent.trim();
    const parts = questionText.split(' ');
    const number1 = parseInt(parts[0]);
    const operation = parts[1];
    const number2 = parseInt(parts[2]);

    switch (operation) {
        case '+':
            return number1 + number2;
        case '-':
            return number1 - number2;
        case '*':
            return number1 * number2;
        // Implementar más operaciones según tus requisitos
        default:
            return 0;
    }
}
