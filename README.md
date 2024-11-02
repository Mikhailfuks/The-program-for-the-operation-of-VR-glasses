// HTML для сцены
<!DOCTYPE html>
<html>
<head>
  <title>3D Игра в очках</title>
  <style>
    body {
      margin: 0;
      overflow: hidden;
    }
    canvas {
      display: block;
    }
  </style>
</head>
<body>
  <canvas id="myCanvas"></canvas>
  <script src="script.js"></script>
</body>
</html>

// JavaScript код игры
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d');

// Настройка размера холста
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

// Переменные для 3D эффекта
const eyeSeparation = 0.1; // Расстояние между глазами
const focalLength = 1; // Фокусное расстояние

// Объекты в 3D пространстве
const cube = {
  x: 0,
  y: 0,
  z: -2,
  size: 0.5,
};

// Функция для отрисовки кубика
function drawCube(x, y, z, size) {
  // Вычисляем координаты вершин кубика в 3D пространстве
  const vertices = [
    { x: x - size / 2, y: y - size / 2, z: z - size / 2 },
    { x: x + size / 2, y: y - size / 2, z: z - size / 2 },
    { x: x + size / 2, y: y + size / 2, z: z - size / 2 },
    { x: x - size / 2, y: y + size / 2, z: z - size / 2 },
    { x: x - size / 2, y: y - size / 2, z: z + size / 2 },
    { x: x + size / 2, y: y - size / 2, z: z + size / 2 },
    { x: x + size / 2, y: y + size / 2, z: z + size / 2 },
    { x: x - size / 2, y: y + size / 2, z: z + size / 2 },
  ];

  // Отрисовка линий для каждой грани
  ctx.strokeStyle = 'white';
  ctx.beginPath();
  ctx.moveTo(project(vertices[0]).x, project(vertices[0]).y);
  ctx.lineTo(project(vertices[1]).x, project(vertices[1]).y);
  ctx.lineTo(project(vertices[2]).x, project(vertices[2]).y);
  ctx.lineTo(project(vertices[3]).x, project(vertices[3]).y);
  ctx.lineTo(project(vertices[0]).x, project(vertices[0]).y);
  ctx.stroke();

  ctx.beginPath();
  ctx.moveTo(project(vertices[4]).x, project(vertices[4]).y);
  ctx.lineTo(project(vertices[5]).x, project(vertices[5]).y);
  ctx.lineTo(project(vertices[6]).x, project(vertices[6]).y);
  ctx.lineTo(project(vertices[7]).x, project(vertices[7]).y);
  ctx.lineTo(project(vertices[4]).x, project(vertices[4]).y);
  ctx.stroke();

  ctx.beginPath();
  ctx.moveTo(project(vertices[0]).x, project(vertices[0]).y);
  ctx.lineTo(project(vertices[4]).x, project(vertices[4]).y);
  ctx.stroke();

  ctx.beginPath();
  ctx.moveTo(project(vertices[1]).x, project(vertices[1]).y);
  ctx.lineTo(project(vertices[5]).x, project(vertices[5]).y);
  ctx.stroke();

  ctx.beginPath();
  ctx.moveTo(project(vertices[2]).x, project(vertices[2]).y);
  ctx.lineTo(project(vertices[6]).x, project(vertices[6]).y);
  ctx.stroke();

  ctx.beginPath();
  ctx.moveTo(project(vertices[3]).x, project(vertices[3]).y);
  ctx.lineTo(project(vertices[7]).x, project(vertices[7]).y);
  ctx.stroke();
}

// Функция для проекции 3D точки на 2D экран
function project(point) {
  return {
    x: (point.x * focalLength) / (focalLength + point.z) + canvas.width / 2,
    y: (point.y * focalLength) / (focalLength + point.z) + canvas.height / 2,
  };
}

// Функция для отрисовки левого глаза
function drawLeftEye() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  drawCube(cube.x - eyeSeparation / 2, cube.y, cube.z, cube.size);
}

// Функция для отрисовки правого глаза
function drawRightEye() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  drawCube(cube.x + eyeSeparation / 2, cube.y, cube.z, cube.size);
}

// Основной цикл игры
function gameLoop() {
  // Отрисовка левого и правого глаза
  drawLeftEye();
  drawRightEye();

  // Обновление позиции кубика
  cube.z += 0.01;

  // Задержка для плавного движения
  requestAnimationFrame(gameLoop);
}

// Запуск игры
gameLoop();
