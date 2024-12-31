<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>烟花效果</title>
  <style>
    body {
      margin: 0;
      overflow: hidden;
      background: black;
    }
    canvas {
      display: block;
    }
  </style>
</head>
<body>
  <canvas id="fireworks"></canvas>
  <script>
    const canvas = document.getElementById('fireworks');
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    const particles = [];
    const colors = ['#ff4b5c', '#ffeb3b', '#4caf50', '#2196f3', '#ff9800'];

    function createParticle(x, y) {
      for (let i = 0; i < 100; i++) {
        particles.push({
          x: x,
          y: y,
          dx: (Math.random() - 0.5) * 4,
          dy: (Math.random() - 0.5) * 4,
          size: Math.random() * 3 + 1,
          color: colors[Math.floor(Math.random() * colors.length)],
          alpha: 1,
          life: Math.random() * 30 + 30
        });
      }
    }

    function drawParticles() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      particles.forEach((particle, index) => {
        ctx.beginPath();
        ctx.arc(particle.x, particle.y, particle.size, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(${hexToRgb(particle.color)},${particle.alpha})`;
        ctx.fill();
        particle.x += particle.dx;
        particle.y += particle.dy;
        particle.alpha -= 0.01;
        particle.life--;
        if (particle.life <= 0) particles.splice(index, 1);
      });
    }

    function hexToRgb(hex) {
      const bigint = parseInt(hex.slice(1), 16);
      const r = (bigint >> 16) & 255;
      const g = (bigint >> 8) & 255;
      const b = bigint & 255;
      return `${r},${g},${b}`;
    }

    function animate() {
      drawParticles();
      requestAnimationFrame(animate);
    }

    canvas.addEventListener('click', (e) => {
      createParticle(e.clientX, e.clientY);
    });

    animate();
  </script>
</body>
</html>
