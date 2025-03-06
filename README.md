<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Game Platform</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { text-align: center; background: #222; color: white; }
        canvas { background: #111; display: block; margin: 20px auto; }
        .info { font-size: 1.2rem; margin: 10px; }
    </style>
</head>
<body>
    <h1>Game Platform - Vượt Chướng Ngại Vật</h1>
    <p class="info">Điểm số: <span id="score">0</span> | Cấp độ: <span id="level">1</span></p>
    <canvas id="gameCanvas" width="800" height="400"></canvas>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        let player = { x: 50, y: 300, width: 30, height: 30, speed: 5, dy: 0 };
        let goal = { x: 750, y: 300, width: 30, height: 30 };
        let gravity = 0.5;
        let isJumping = false;
        let score = localStorage.getItem("score") || 0;
        let level = localStorage.getItem("level") || 1;

        document.getElementById("score").innerText = score;
        document.getElementById("level").innerText = level;

        function drawPlayer() {
            ctx.fillStyle = "cyan";
            ctx.fillRect(player.x, player.y, player.width, player.height);
        }

        function drawGoal() {
            ctx.fillStyle = "gold";
            ctx.fillRect(goal.x, goal.y, goal.width, goal.height);
        }

        function update() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawPlayer();
            drawGoal();
            player.y += player.dy;
            player.dy += gravity;
            if (player.y + player.height >= canvas.height) {
                player.y = canvas.height - player.height;
                isJumping = false;
            }
            if (player.x + player.width >= goal.x && player.y + player.height >= goal.y) {
                score++;
                level++;
                localStorage.setItem("score", score);
                localStorage.setItem("level", level);
                document.getElementById("score").innerText = score;
                document.getElementById("level").innerText = level;
                player.x = 50;
                goal.x = Math.random() * (canvas.width - 50) + 50;
            }
            requestAnimationFrame(update);
        }

        document.addEventListener("keydown", (e) => {
            if (e.key === "ArrowRight" || e.key === "d") player.x += player.speed;
            if (e.key === "ArrowLeft" || e.key === "a") player.x -= player.speed;
            if ((e.key === "ArrowUp" || e.key === "w") && !isJumping) {
                player.dy = -10;
                isJumping = true;
            }
        });

        update();
    </script>
</body>
</html>
