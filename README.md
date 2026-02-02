<!DOCTYPE html>
<html>
<head>
    <title>Our Universe</title>
    <style>
        body { margin: 0; background: #050505; overflow: hidden; font-family: 'Arial'; }
        #canvas { display: block; }
        .ui {
            position: absolute; top: 50%; left: 50%;
            transform: translate(-50%, -50%);
            color: white; text-align: center;
            pointer-events: none; text-shadow: 0 0 20px rgba(255,255,255,0.5);
        }
    </style>
</head>
<body>
    <div class="ui">
        <h1 style="font-weight: 100; letter-spacing: 5px;">YOU ARE MY UNIVERSE</h1>
        <p style="opacity: 0.6;">Move your cursor to connect the stars</p>
    </div>
    <canvas id="canvas"></canvas>

    <script>
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');
        let particles = [];
        const mouse = { x: null, y: null };

        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;

        window.addEventListener('mousemove', (e) => { mouse.x = e.x; mouse.y = e.y; });

        class Particle {
            constructor() {
                this.x = Math.random() * canvas.width;
                this.y = Math.random() * canvas.height;
                this.size = Math.random() * 2 + 1;
                this.speedX = Math.random() * 1 - 0.5;
                this.speedY = Math.random() * 1 - 0.5;
            }
            update() {
                this.x += this.speedX;
                this.y += this.speedY;
                if (this.x > canvas.width) this.x = 0;
                if (this.x < 0) this.x = canvas.width;
                if (this.y > canvas.height) this.y = 0;
                if (this.y < 0) this.y = canvas.height;
            }
            draw() {
                ctx.fillStyle = 'rgba(255, 255, 255, 0.8)';
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                ctx.fill();
            }
        }

        function init() {
            for (let i = 0; i < 100; i++) particles.push(new Particle());
        }

        function handleParticles() {
            for (let i = 0; i < particles.length; i++) {
                particles[i].update();
                particles[i].draw();
                
                // Connect to mouse
                let dx = mouse.x - particles[i].x;
                let dy = mouse.y - particles[i].y;
                let distance = Math.sqrt(dx*dx + dy*dy);
                
                if (distance < 150) {
                    ctx.strokeStyle = `rgba(255, 182, 193, ${1 - distance/150})`;
                    ctx.lineWidth = 1;
                    ctx.beginPath();
                    ctx.moveTo(particles[i].x, particles[i].y);
                    ctx.lineTo(mouse.x, mouse.y);
                    ctx.stroke();
                }
            }
        }

        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            handleParticles();
            requestAnimationFrame(animate);
        }

        init();
        animate();
    </script>
</body>
</html>
