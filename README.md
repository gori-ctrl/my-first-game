# my-first-game
<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ジャンプゲーム</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>画面クリックかスペースキーでジャンプ！</h1>
    <div id="game-container">
        <div id="player"></div>
        <div id="obstacle"></div>
    </div>
    <script src="script.js"></script>
</body>
</html>
body {
    text-align: center;
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
}

#game-container {
    width: 600px;
    height: 200px;
    border: 2px solid #333;
    margin: 50px auto;
    position: relative;
    background-color: #fff;
    overflow: hidden;
}

#player {
    width: 30px;
    height: 30px;
    background-color: #3498db;
    position: absolute;
    bottom: 0;
    left: 50px;
}

#obstacle {
    width: 20px;
    height: 40px;
    background-color: #e74c3c;
    position: absolute;
    bottom: 0;
    right: -20px;
    animation: moveObstacle 2s infinite linear;
}

/* 障害物が左に流れるアニメーション */
@keyframes moveObstacle {
    0% { right: -20px; }
    100% { right: 620px; }
}

/* ジャンプのアニメーション */
.jump {
    animation: jumpAnim 0.5s linear;
}

@keyframes jumpAnim {
    0% { bottom: 0; }
    50% { bottom: 100px; }
    100% { bottom: 0; }
}
const player = document.getElementById("player");
const obstacle = document.getElementById("obstacle");

// ジャンプ処理
function jump() {
    if (!player.classList.contains("jump")) {
        player.classList.add("jump");
        setTimeout(() => {
            player.classList.remove("jump");
        }, 500);
    }
}

// キーボードとクリックのイベント
document.addEventListener("keydown", (e) => {
    if (e.code === "Space") jump();
});
document.addEventListener("click", jump);

// 当たり判定のチェック（10ミリ秒ごと）
setInterval(() => {
    const playerBottom = parseInt(window.getComputedStyle(player).getPropertyValue("bottom"));
    const obstacleLeft = obstacle.getBoundingClientRect().left - document.getElementById("game-container").getBoundingClientRect().left;

    // プレイヤーと障害物がぶつかったかどうかの判定
    if (obstacleLeft > 50 && obstacleLeft < 80 && playerBottom <= 40) {
        alert("ゲームオーバー！");
        location.reload(); // 画面をリロードしてリスタート
    }
}, 10);
