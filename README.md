<!DOCTYPE html>
<html lang="tg">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lucky Demo - Омузиши</title>
    <style>
        body { background-color: #0b0e11; color: white; font-family: sans-serif; text-align: center; }
        .game-box { border: 2px solid #2b3139; padding: 20px; border-radius: 15px; width: 300px; margin: 50px auto; background: #181a20; }
        #multiplier { font-size: 48px; color: #f0b90b; margin: 20px 0; }
        .balance { font-size: 18px; color: #00ff00; }
        input { width: 80%; padding: 10px; margin: 10px 0; border-radius: 5px; border: none; }
        button { width: 85%; padding: 15px; font-weight: bold; cursor: pointer; border-radius: 5px; border: none; }
        #betBtn { background-color: #f0b90b; color: black; }
        #cashOutBtn { background-color: #2ebd85; color: white; display: none; }
        .log { margin-top: 20px; font-size: 14px; color: #848e9c; }
    </style>
</head>
<body>

    <div class="game-box">
        <div class="balance">Баланс: <span id="balance">1000</span> TJS</div>
        <div id="multiplier">x1.00</div>
        
        <input type="number" id="betAmount" value="10" min="1">
        <button id="betBtn" onclick="startGame()">СТАВКА</button>
        <button id="cashOutBtn" onclick="cashOut()">ЗАБРАТЬ</button>

        <div class="log" id="status">Барои оғоз тугмаро пахш кунед</div>
    </div>

    <script>
        let balance = 1000;
        let currentMultiplier = 1.00;
        let isPlaying = false;
        let gameInterval;
        let crashPoint;

        function startGame() {
            let bet = parseFloat(document.getElementById('betAmount').value);
            if (bet > balance || bet <= 0) {
                alert("Маблағ нодуруст аст!");
                return;
            }

            balance -= bet;
            updateUI();
            
            isPlaying = true;
            currentMultiplier = 1.00;
            crashPoint = (Math.random() * 5 + 1).toFixed(2); // Нуқтаи сухтан аз 1 то 6
            
            document.getElementById('betBtn').style.display = 'none';
            document.getElementById('cashOutBtn').style.display = 'block';
            document.getElementById('status').innerText = "Парвоз давом дорад...";

            gameInterval = setInterval(() => {
                currentMultiplier += 0.01;
                document.getElementById('multiplier').innerText = 'x' + currentMultiplier.toFixed(2);

                if (currentMultiplier >= crashPoint) {
                    gameOver(false);
                }
            }, 100);
        }

        function cashOut() {
            if (!isPlaying) return;
            let bet = parseFloat(document.getElementById('betAmount').value);
            let winAmount = bet * currentMultiplier;
            balance += winAmount;
            gameOver(true, winAmount);
        }

        function gameOver(isWin, amount = 0) {
            clearInterval(gameInterval);
            isPlaying = false;
            
            if (isWin) {
                document.getElementById('status').innerText = "Шумо бурд кардед: " + amount.toFixed(2) + " TJS";
                document.getElementById('status').style.color = "#2ebd85";
            } else {
                document.getElementById('status').innerText = "Сӯхт! Нуқтаи сухтан: x" + crashPoint;
                document.getElementById('status').style.color = "#f6465d";
                document.getElementById('multiplier').innerText = "CRASHED!";
            }

            document.getElementById('betBtn').style.display = 'block';
            document.getElementById('cashOutBtn').style.display = 'none';
            updateUI();
        }

        function updateUI() {
            document.getElementById('balance').innerText = balance.toFixed(2);
        }
    </script>
</body>
</html>
