# be-my-valentine-
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SURE KA NA BA? 😈💖</title>
    <style>
        :root {
            --hot-pink: #ff006e;
            --pastel-pink: #ffafbd;
            --white: #ffffff;
        }

        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            font-family: 'Comic Sans MS', 'Chalkboard SE', cursive;
            background: linear-gradient(135deg, var(--pastel-pink), #ffc3a0);
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* Floating Hearts */
        .hearts-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 1;
        }

        .heart {
            position: absolute;
            color: var(--hot-pink);
            opacity: 0.6;
            animation: float 5s linear infinite;
        }

        @keyframes float {
            0% { transform: translateY(110vh) scale(0.5); opacity: 1; }
            100% { transform: translateY(-10vh) scale(1.5); opacity: 0; }
        }

        /* Main Card */
        .card {
            background: rgba(255, 255, 255, 0.95);
            padding: 40px;
            border-radius: 40px;
            box-shadow: 0 20px 50px rgba(255, 0, 110, 0.2);
            text-align: center;
            z-index: 10;
            max-width: 85%;
            border: 5px solid var(--hot-pink);
        }

        h1 {
            color: var(--hot-pink);
            font-size: 2.2rem;
            margin-bottom: 30px;
        }

        .btn-wrap {
            display: flex;
            flex-direction: column;
            gap: 20px;
            align-items: center;
            min-height: 200px;
        }

        button {
            padding: 18px 40px;
            font-size: 1.3rem;
            border: none;
            border-radius: 100px;
            cursor: pointer;
            font-weight: 900;
            transition: transform 0.1s ease-out;
            box-shadow: 0 8px 15px rgba(0,0,0,0.1);
        }

        #yesBtn {
            background: var(--hot-pink);
            color: white;
            animation: pulse 1s infinite alternate;
            z-index: 100;
        }

        @keyframes pulse {
            from { transform: scale(1); box-shadow: 0 0 10px var(--hot-pink); }
            to { transform: scale(1.1); box-shadow: 0 0 30px var(--hot-pink); }
        }

        #noBtn {
            background: #e0e0e0;
            color: #666;
            position: relative;
            z-index: 50;
        }

        /* The Loading Overlay */
        #loadingOverlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: white;
            z-index: 2000;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

        .bar-bg {
            width: 250px;
            height: 20px;
            background: #eee;
            border-radius: 10px;
            margin-top: 20px;
            overflow: hidden;
        }

        .bar-fill {
            width: 0%;
            height: 100%;
            background: var(--hot-pink);
            transition: width 0.1s;
        }

        #loadText {
            font-size: 1.5rem;
            color: var(--hot-pink);
            font-weight: bold;
        }

        /* Success screen */
        .success-content {
            animation: bounceIn 0.8s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }

        @keyframes bounceIn {
            0% { transform: scale(0); }
            100% { transform: scale(1); }
        }

        .shake-intense {
            animation: shake 0.1s infinite;
        }

        @keyframes shake {
            0% { transform: translate(2px, 2px) rotate(0deg); }
            50% { transform: translate(-2px, -2px) rotate(1deg); }
            100% { transform: translate(2px, 2px) rotate(-1deg); }
        }
    </style>
</head>
<body>

    <div class="hearts-container" id="hearts"></div>

    <div class="card" id="mainCard">
        <h1>Can you be my Valentine? 💖</h1>
        <div class="btn-wrap">
            <button id="yesBtn">YES 💖</button>
            <button id="noBtn">NAHH 🙈</button>
        </div>
    </div>

    <div id="loadingOverlay">
        <div id="loadText">Checking feelings...</div>
        <div class="bar-bg">
            <div class="bar-fill" id="bar"></div>
        </div>
    </div>

    <audio id="sfx-dodge" src="https://assets.mixkit.co/active_storage/sfx/2571/2571-preview.mp3"></audio>
    <audio id="sfx-success" src="https://assets.mixkit.co/active_storage/sfx/2019/2019-preview.mp3"></audio>

    <script>
        const yesBtn = document.getElementById('yesBtn');
        const noBtn = document.getElementById('noBtn');
        const overlay = document.getElementById('loadingOverlay');
        const bar = document.getElementById('bar');
        const loadText = document.getElementById('loadText');
        const sfxDodge = document.getElementById('sfx-dodge');
        const sfxSuccess = document.getElementById('sfx-success');

        let clickCount = 0;
        let yesSize = 1;
        const noTexts = [
            "NAHH 🙈",
            "Sure ka na ba? 🤔",
            "SUREE?! 🤨",
            "Sure na ba talaga? 🥺",
            "SURE 100%? 😤"
        ];

        // DODGE LOGIC
        const dodge = () => {
            sfxDodge.currentTime = 0;
            sfxDodge.play().catch(() => {});

            // Update text
            clickCount++;
            if (clickCount < noTexts.length) {
                noBtn.innerText = noTexts[clickCount];
            } else {
                noBtn.innerText = noTexts[noTexts.length - 1];
                noBtn.classList.add('shake-intense');
                noBtn.style.fontSize = "2rem";
            }

            // Grow YES button
            yesSize += 0.4;
            yesBtn.style.transform = `scale(${yesSize})`;

            // Random position
            const x = Math.random() * (window.innerWidth - noBtn.offsetWidth);
            const y = Math.random() * (window.innerHeight - noBtn.offsetHeight);
            
            noBtn.style.position = 'fixed';
            noBtn.style.left = `${x}px`;
            noBtn.style.top = `${y}px`;
            noBtn.style.transition = 'all 0.15s ease-out';
        };

        noBtn.addEventListener('mouseover', dodge);
        noBtn.addEventListener('touchstart', (e) => {
            e.preventDefault();
            dodge();
        });

        // LOADING LOGIC
        yesBtn.addEventListener('click', () => {
            overlay.style.display = 'flex';
            let progress = 0;
            const status = ["Checking feelings...", "Asking the universe...", "Reading your mind...", "Finalizing destiny..."];
            
            const interval = setInterval(() => {
                progress += 1.5;
                bar.style.width = `${progress}%`;

                if (progress > 25) loadText.innerText = status[1];
                if (progress > 50) loadText.innerText = status[2];
                if (progress > 75) loadText.innerText = status[3];

                if (progress >= 100) {
                    clearInterval(interval);
                    showWin();
                }
            }, 50);
        });

        function showWin() {
            sfxSuccess.play().catch(() => {});
            overlay.innerHTML = `
                <div class="success-content">
                    <h1 style="font-size: 4rem;">YAY! 💖</h1>
                    <p style="font-size: 1.5rem; color: #ff006e;">You just made my heart the happiest!</p>
                    <p style="font-size: 1.2rem;">Happy Valentine's Day! ✨</p>
                </div>
            `;
        }

        // HEARTS
        setInterval(() => {
            const h = document.createElement('div');
            h.classList.add('heart');
            h.innerHTML = ['❤️','💖','💗','💕'][Math.floor(Math.random()*4)];
            h.style.left = Math.random() * 100 + 'vw';
            h.style.fontSize = (Math.random() * 20 + 10) + 'px';
            document.getElementById('hearts').appendChild(h);
            setTimeout(() => h.remove(), 5000);
        }, 300);
    </script>
</body>
</html>
