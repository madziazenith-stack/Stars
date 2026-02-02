<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>A Letter For You</title>
    <style>
        :root {
            --paper: #f9f4e8;
            --ink: #2c3e50;
            --accent: #e74c3c;
        }

        body {
            margin: 0;
            height: 100vh;
            background-color: #dcd0c0; /* Desk color */
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
            font-family: 'Courier New', Courier, monospace;
        }

        /* The Paper */
        #paper {
            width: 80%;
            max-width: 600px;
            height: 70vh;
            background: var(--paper);
            padding: 50px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2), inset 0 0 100px rgba(0,0,0,0.05);
            position: relative;
            border-radius: 2px;
            overflow-y: auto;
        }

        /* Vintage Texture Overlay */
        #paper::before {
            content: "";
            position: absolute;
            top: 0; left: 0; right: 0; bottom: 0;
            background: url('https://www.transparenttextures.com/patterns/paper-fibers.png');
            opacity: 0.4;
            pointer-events: none;
        }

        #content {
            font-size: 1.2rem;
            line-height: 1.6;
            color: var(--ink);
            white-space: pre-wrap;
            border-right: 3px solid var(--accent); /* Cursor */
            animation: blink 0.8s infinite;
        }

        @keyframes blink {
            50% { border-color: transparent; }
        }

        .instruction {
            position: absolute;
            bottom: 20px;
            width: 100%;
            text-align: center;
            color: #7f8c8d;
            font-style: italic;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 0.5; }
            50% { opacity: 1; }
        }

        /* Floating Heart Particles */
        .heart {
            position: absolute;
            color: var(--accent);
            pointer-events: none;
            z-index: 100;
            animation: flyUp 2s ease-out forwards;
        }

        @keyframes flyUp {
            0% { transform: translateY(0) scale(1); opacity: 1; }
            100% { transform: translateY(-200px) translateX(var(--x)) rotate(var(--r)); opacity: 0; }
        }
    </style>
</head>
<body>

    <div id="paper">
        <div id="content"></div>
        <div class="instruction">Press any key to start typing...</div>
    </div>

    <script>
        // EDIT YOUR MESSAGE HERE
        const message = `My Dearest,

I wanted to write you something special, something that shows how much you mean to me. 

Every time I think of you, my world gets a little brighter. You are the person who makes the ordinary feel extraordinary. 

Thank you for being you. 

Forever and always,
Me ❤️`;

        let index = 0;
        const contentDiv = document.getElementById('content');
        const paper = document.getElementById('paper');

        document.addEventListener('keydown', (e) => {
            if (index < message.length) {
                // Remove instruction on first keypress
                if (index === 0) document.querySelector('.instruction').style.display = 'none';

                // Add next character
                contentDiv.textContent += message[index];
                index++;

                // Auto-scroll paper
                paper.scrollTop = paper.scrollHeight;

                // Create floating heart
                createHeart();
            }
        });

        function createHeart() {
            const heart = document.createElement('div');
            heart.className = 'heart';
            heart.innerHTML = '❤️';
            
            // Randomize heart float direction
            const randomX = (Math.random() * 100 - 50) + 'px';
            const randomRotate = (Math.random() * 360) + 'deg';
            
            heart.style.setProperty('--x', randomX);
            heart.style.setProperty('--r', randomRotate);
            
            // Spawn near the bottom of the current text
            heart.style.left = (Math.random() * 80 + 10) + '%';
            heart.style.top = '80%';
            
            document.body.appendChild(heart);
            setTimeout(() => heart.remove(), 2000);
        }
    </script>
</body>
</html>
