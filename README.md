<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Ramito de Flores Amarillas</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      background: linear-gradient(to bottom, #fff8dc, #ffeb99);
      text-align: center;
      font-family: 'Segoe UI', sans-serif;
      padding: 20px;
      margin: 0;
    }
    h1 {
      color: #f4c430;
      font-size: 2em;
      margin-bottom: 20px;
    }
    canvas {
      width: 100%;
      max-width: 500px;
      height: auto;
      border: 2px solid #f4c430;
      background-color: #fffef0;
      border-radius: 10px;
      display: none;
    }
    button {
      padding: 12px 24px;
      font-size: 1em;
      background-color: #f4c430;
      color: white;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      margin-bottom: 20px;
    }
    button:hover {
      background-color: #e0b000;
    }
    .mensaje-final {
      font-size: 1.2em;
      color: #d2691e;
      font-style: italic;
      min-height: 40px;
      transition: opacity 1s ease;
      padding: 0 10px;
    }
  </style>
</head>
<body>
  <h1>🌼 Ramito de Flores Amarillas 🌼</h1>
  <button onclick="iniciarAnimacion()">Recibir flores</button>
  <canvas id="florCanvas" width="500" height="500"></canvas>
  <div class="mensaje-final" id="mensajeFinal"></div>

  <script>
    const canvas = document.getElementById("florCanvas");
    const ctx = canvas.getContext("2d");
    const flores = [
      { x: 150, y: 200 },
      { x: 250, y: 180 },
      { x: 350, y: 200 }
    ];
    const petalos = 8;
    const radio = 30;
    let florActual = 0;
    let mensajeIndex = 0;

    const mensajesBonitos = [
      "Que esta flor te recuerde lo especial que eres 🌞",
      "Tu luz hace florecer el mundo 💛",
      "Gracias por existir 🌻",
      "Eres primavera en días grises 🌼",
      "Hoy florece tu sonrisa 🌸",
      "Tu presencia es un regalo 🌟"
    ];

    function dibujarTallo(x, y) {
      ctx.beginPath();
      ctx.moveTo(x, y + 25);
      ctx.lineTo(x, 450);
      ctx.lineWidth = 6;
      ctx.strokeStyle = "#228B22";
      ctx.stroke();
    }

    function dibujarHojas(x, y) {
      ctx.fillStyle = "#32CD32";
      ctx.beginPath();
      ctx.ellipse(x - 20, y + 100, 15, 30, Math.PI / 4, 0, 2 * Math.PI);
      ctx.fill();
      ctx.beginPath();
      ctx.ellipse(x + 20, y + 130, 15, 30, -Math.PI / 4, 0, 2 * Math.PI);
      ctx.fill();
    }

    function dibujarPetalo(x, y, angulo) {
      ctx.save();
      ctx.translate(x, y);
      ctx.rotate(angulo);
      ctx.beginPath();
      ctx.moveTo(0, 0);
      ctx.bezierCurveTo(radio, -radio, radio, radio, 0, 2 * radio);
      ctx.bezierCurveTo(-radio, radio, -radio, -radio, 0, 0);
      ctx.fillStyle = "#f4c430";
      ctx.fill();
      ctx.restore();
    }

    function dibujarCentro(x, y) {
      ctx.beginPath();
      ctx.arc(x, y, 20, 0, 2 * Math.PI);
      ctx.fillStyle = "#d2691e";
      ctx.fill();
    }

    function dibujarFlor(x, y, callback) {
      dibujarTallo(x, y);
      setTimeout(() => {
        dibujarHojas(x, y);
        let i = 0;
        const intervalo = setInterval(() => {
          const angulo = (2 * Math.PI / petalos) * i;
          dibujarPetalo(x, y, angulo);
          i++;
          if (i >= petalos) {
            clearInterval(intervalo);
            setTimeout(() => {
              dibujarCentro(x, y);
              if (callback) callback();
            }, 300);
          }
        }, 150);
      }, 300);
    }

    function iniciarDibujo() {
      if (florActual < flores.length) {
        const { x, y } = flores[florActual];
        dibujarFlor(x, y, () => {
          florActual++;
          iniciarDibujo();
        });
      } else {
        iniciarMensajes();
      }
    }

    function iniciarMensajes() {
      const mensajeDiv = document.getElementById("mensajeFinal");
      mensajeDiv.style.opacity = 1;
      mensajeDiv.textContent = mensajesBonitos[mensajeIndex];
      setInterval(() => {
        mensajeDiv.style.opacity = 0;
        setTimeout(() => {
          mensajeIndex = (mensajeIndex + 1) % mensajesBonitos.length;
          mensajeDiv.textContent = mensajesBonitos[mensajeIndex];
          mensajeDiv.style.opacity = 1;
        }, 1000);
      }, 4000);
    }

    function iniciarAnimacion() {
      document.querySelector("button").style.display = "none";
      canvas.style.display = "inline-block";
      iniciarDibujo();
    }
  </script>
</body>
</html>
