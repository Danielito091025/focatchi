# focatchi
Una foca feliz
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FocaTchi - Mi foca virtual</title>

<style>
    * {
        box-sizing: border-box;
    }

    body {
        margin: 0;
        min-height: 100vh;
        font-family: "Trebuchet MS", Arial, sans-serif;
        background: linear-gradient(#b9eaff, #e9ddff);
        color: #244b79;
        display: flex;
        justify-content: center;
        padding: 20px;
    }

    .juego {
        width: 100%;
        max-width: 440px;
        text-align: center;
        background: #ffffff;
        border: 4px solid #648edb;
        border-radius: 25px;
        padding: 20px;
        box-shadow: 0 8px 0 #a4b9ef;
        align-self: flex-start;
    }

    h1 {
        margin: 0;
        color: #526fd0;
        font-size: 34px;
    }

    .subtitulo {
        margin: 8px 0 16px;
    }

    .escenario {
        background: linear-gradient(#9cddff 60%, #65bce9 60%);
        border: 3px solid #6099dc;
        border-radius: 18px;
        padding: 25px 10px;
        margin-bottom: 15px;
    }

    #foca {
        font-size: 100px;
        line-height: 1.5;
        animation: flotar 2s infinite ease-in-out;
    }

    @keyframes flotar {
        0%, 100% { transform: translateY(0); }
        50% { transform: translateY(-8px); }
    }

    #mensaje {
        background: white;
        border-radius: 12px;
        padding: 12px;
        font-weight: bold;
        min-height: 44px;
    }

    .estadisticas {
        text-align: left;
        margin: 15px 0;
    }

    .estadistica {
        margin: 12px 0;
    }

    .barra {
        height: 15px;
        background: #e3e8f4;
        border-radius: 10px;
        overflow: hidden;
        margin-top: 5px;
    }

    .relleno {
        height: 100%;
        width: 100%;
        transition: width .3s;
        border-radius: 10px;
    }

    .hambre { background: #ffb45e; }
    .higiene { background: #57c8e9; }
    .felicidad { background: #f28bc7; }
    .energia { background: #9b8bfa; }

    .botones {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 10px;
        margin-top: 18px;
    }

    button {
        border: none;
        border-radius: 13px;
        padding: 13px 6px;
        background: #8178ed;
        color: white;
        font-size: 15px;
        font-weight: bold;
        cursor: pointer;
        box-shadow: 0 4px 0 #5e55bd;
    }

    button:active {
        transform: translateY(3px);
        box-shadow: 0 1px 0 #5e55bd;
    }

    button:disabled {
        opacity: .55;
        cursor: not-allowed;
    }

    .reiniciar {
        margin-top: 18px;
        background: #5ba9d8;
        box-shadow: 0 4px 0 #397da9;
        width: 100%;
    }

    .nota {
        font-size: 12px;
        color: #66809e;
        margin-top: 18px;
    }
</style>
</head>

<body>
<div class="juego">
    <h1>🦭 FocaTchi</h1>
    <p class="subtitulo">¡Cuida a tu pequeña foca!</p>

    <div class="escenario">
        <div id="foca">🦭</div>
        <div id="mensaje">¡Hola! Soy Copito. ¿Jugamos?</div>
    </div>

    <div class="estadisticas">
        <div class="estadistica">
            🍣 Hambre: <span id="numHambre">80</span>%
            <div class="barra">
                <div id="barraHambre" class="relleno hambre"></div>
            </div>
        </div>

        <div class="estadistica">
            🫧 Higiene: <span id="numHigiene">80</span>%
            <div class="barra">
                <div id="barraHigiene" class="relleno higiene"></div>
            </div>
        </div>

        <div class="estadistica">
            💗 Felicidad: <span id="numFelicidad">80</span>%
            <div class="barra">
                <div id="barraFelicidad" class="relleno felicidad"></div>
            </div>
        </div>

        <div class="estadistica">
            🌙 Energía: <span id="numEnergia">80</span>%
            <div class="barra">
                <div id="barraEnergia" class="relleno energia"></div>
            </div>
        </div>
    </div>

    <div class="botones">
        <button onclick="alimentar()">🐟 Dar pescado</button>
        <button onclick="banar()">🫧 Bañar</button>
        <button onclick="jugar()">⚽ Jugar</button>
        <button onclick="dormir()">💤 Dormir</button>
    </div>

    <button class="reiniciar" onclick="reiniciar()">
        🔄 Volver a empezar
    </button>

    <p class="nota">
        Hecho con ❤️ para los amantes de las focas.
    </p>
</div>

<script>
    const foca = {
        hambre: 80,
        higiene: 80,
        felicidad: 80,
        energia: 80,
        durmiendo: false
    };

    let mensajeAnterior = "";
    let ultimoTiempo = Date.now();

    function limitar(valor) {
        return Math.max(0, Math.min(100, valor));
    }

    function actualizar() {
        const estados = ["hambre", "higiene", "felicidad", "energia"];

        estados.forEach(estado => {
            document.getElementById("num" +
                estado.charAt(0).toUpperCase() + estado.slice(1)
            ).textContent = foca[estado];

            document.getElementById("barra" +
                estado.charAt(0).toUpperCase() + estado.slice(1)
            ).style.width = foca[estado] + "%";
        });

        if (foca.durmiendo) {
            document.getElementById("foca").textContent = "😴";
        } else if (foca.hambre < 25) {
            document.getElementById("foca").textContent = "🥺";
        } else if (foca.felicidad < 25) {
            document.getElementById("foca").textContent = "😢";
        } else {
            document.getElementById("foca").textContent = "🦭";
        }

        if (foca.hambre === 0 || foca.higiene === 0) {
            mostrarMensaje("¡Copito necesita que lo cuides!");
        } else if (foca.felicidad === 0) {
            mostrarMensaje("Copito está triste. ¡Juega con él!");
        }

        document.querySelectorAll(".botones button")
            .forEach(boton => {
                boton.disabled = foca.durmiendo;
            });
    }

    function mostrarMensaje(texto) {
        mensajeAnterior = texto;
        document.getElementById("mensaje").textContent = texto;
    }

    function alimentar() {
        if (foca.durmiendo) return;

        if (foca.hambre === 100) {
            mostrarMensaje("¡Copito ya está llenito! 🐟");
            return;
        }

        foca.hambre = limitar(foca.hambre + 20);
        foca.felicidad = limitar(foca.felicidad + 5);
        mostrarMensaje("¡Qué rico pescado! Gracias 🐟");
        actualizar();
    }

    function banar() {
        if (foca.durmiendo) return;

        if (foca.higiene === 100) {
            mostrarMensaje("¡Copito ya está muy limpio! 🫧");
            return;
        }

        foca.higiene = limitar(foca.higiene + 25);
        foca.felicidad = limitar(foca.felicidad + 5);
        mostrarMensaje("¡Qué baño tan refrescante! 🫧");
        actualizar();
    }

    function jugar() {
        if (foca.durmiendo) return;

        if (foca.energia < 15) {
            mostrarMensaje("Copito tiene sueño. Déjalo descansar 🌙");
            return;
        }

        foca.felicidad = limitar(foca.felicidad + 20);
        foca.energia = limitar(foca.energia - 15);
        foca.hambre = limitar(foca.hambre - 10);
        mostrarMensaje("¡Copito se divirtió muchísimo! ⚽");
        actualizar();
    }

    function dormir() {
        if (foca.durmiendo) return;

        foca.durmiendo = true;
        mostrarMensaje("Zzz... Copito está descansando 🌙");
        actualizar();

        setTimeout(() => {
            foca.energia = limitar(foca.energia + 35);
            foca.durmiendo = false;
            mostrarMensaje("¡Buenos días! Copito está descansado 🦭");
            actualizar();
        }, 5000);
    }

    function reiniciar() {
        foca.hambre = 80;
        foca.higiene = 80;
        foca.felicidad = 80;
        foca.energia = 80;
        foca.durmiendo = false;
        mostrarMensaje("¡Hola de nuevo! Vamos a jugar 💗");
        actualizar();
    }

    // Las necesidades cambian poco a poco con el tiempo.
    setInterval(() => {
        if (!foca.durmiendo) {
            foca.hambre = limitar(foca.hambre - 2);
            foca.higiene = limitar(foca.higiene - 1);
            foca.felicidad = limitar(foca.felicidad - 1);
            foca.energia = limitar(foca.energia - 1);
            actualizar();
        }
    }, 10000);

    actualizar();
</script>
</body>