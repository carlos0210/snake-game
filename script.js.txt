const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

const gridSize = 20;
const canvasSize = 400;
const snake = [{ x: gridSize * 5, y: gridSize * 5 }];
let direction = { x: 0, y: 0 };
let food = { x: gridSize * 10, y: gridSize * 10 };
let score = 0;

function gameLoop() {
    if (updateGame()) {
        drawGame();
        setTimeout(gameLoop, 100);
    }
}

function updateGame() {
    const head = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };

    if (head.x < 0 || head.x >= canvasSize || head.y < 0 || head.y >= canvasSize || snake.some(segment => segment.x === head.x && segment.y === head.y)) {
        alert('Game Over! Score: ' + score);
        return false;
    }

    snake.unshift(head);

    if (head.x === food.x && head.y === food.y) {
        score++;
        food = {
            x: Math.floor(Math.random() * (canvasSize / gridSize)) * gridSize,
            y: Math.floor(Math.random() * (canvasSize / gridSize)) * gridSize
        };
    } else {
        snake.pop();
    }

    return true;
}

function drawGame() {
    ctx.clearRect(0, 0, canvasSize, canvasSize);

    ctx.fillStyle = 'red';
    ctx.fillRect(food.x, food.y, gridSize, gridSize);

    ctx.fillStyle = 'green';
    snake.forEach(segment => ctx.fillRect(segment.x, segment.y, gridSize, gridSize));
}

document.addEventListener('keydown', (event) => {
    switch (event.key) {
        case 'ArrowUp':
            if (direction.y === 0) direction = { x: 0, y: -gridSize };
            break;
        case 'ArrowDown':
            if (direction.y === 0) direction = { x: 0, y: gridSize };
            break;
        case 'ArrowLeft':
            if (direction.x === 0) direction = { x: -gridSize, y: 0 };
            break;
        case 'ArrowRight':
            if (direction.x === 0) direction = { x: gridSize, y: 0 };
            break;
    }
});

gameLoop();
