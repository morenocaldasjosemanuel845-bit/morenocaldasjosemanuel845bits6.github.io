<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Problema de ejemplo - Vaciado de tanque</title>
    <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
    <script id="MathJax-script" async
        src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
    </script>
    <style>
        body { font-family: 'Helvetica Neue', Arial, sans-serif; margin: 20px; line-height: 1.6; color: #333; }
        .line { margin: 15px 0; }
        .eq { color: #004d99; font-size: 1.1em; padding: 5px; border-left: 3px solid #007BFF; background-color: #f9f9f9; }
        .resultado { font-weight: bold; color: #28a745; margin-top: 20px; font-size: 1.2em; text-align: center; }
        #nextBtn {
            margin-top: 20px;
            padding: 12px 25px;
            background-color: #007BFF;
            border: none;
            color: white;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.3s ease;
        }
        #nextBtn:hover { background-color: #0056b3; }
        .problema {
            background: #e9ecef;
            border: 1px solid #ced4da;
            padding: 20px;
            margin-bottom: 25px;
            border-radius: 8px;
        }
        h2 { border-bottom: 2px solid #007BFF; padding-bottom: 10px; color: #007BFF; }
    </style>
</head>
<body>
    <h2>Problema de ejemplo - Vaciado de un tanque</h2>
    <div class="problema">
        <p>
            Para el tanque mostrado en la figura 6.14, encuentre el tiempo necesario para vaciarlo desde un nivel de 
            \[3.0 \ \text{m}\] hasta \[0.50 \ \text{m}\].  
            El tanque tiene un diámetro de \[1.50 \ \text{m}\] y la boquilla tiene un diámetro de \[50 \ \text{mm} = 0.050 \ \text{m}\].
        </p>
    </div>

    <div id="solution"></div>
    <button id="nextBtn">Mostrar siguiente paso</button>

    <script>
        const pasos = [
            {tipo:"intro", texto:"1) Para resolver este problema, se utiliza la ecuación para el tiempo de vaciado de un tanque, conocida como la Ecuación (6-26):"},
            {tipo:"eq", linea:0, partes:["t_2 - t_1 = \\frac{2(A_T/A_j)}{\\sqrt{2g}} \\left( h_1^{1/2} - h_2^{1/2} \\right)"]},
            
            {tipo:"intro", texto:"2) Se requieren las áreas del tanque (\\(A_T\\)) y de la boquilla (\\(A_j\\))."},
            {tipo:"intro", texto:"Área del tanque (\\(D_T = 1.50 \\, m\\)): "},
            {tipo:"eq", linea:1, partes:["A_T = \\frac{\\pi (1.50 \\, m)^2}{4} = 1.767 \\, m^2"]},

            {tipo:"intro", texto:"Área de la boquilla (\\(D_j = 50 \\, mm = 0.050 \\, m\\)): "},
            {tipo:"eq", linea:2, partes:["A_j = \\frac{\\pi (0.050 \\, m)^2}{4} = 0.001963 \\, m^2"]},

            {tipo:"intro", texto:"3) Se calcula la relación entre las áreas, que es un término clave en la ecuación:"},
            {tipo:"eq", linea:3, partes:["\\frac{A_T}{A_j} = \\frac{1.767 \\, m^2}{0.001963 \\, m^2} = 900"]},

            {tipo:"intro", texto:"4) Ahora, se sustituyen los valores conocidos en la ecuación (6-26) y se resuelve:"},
            {tipo:"eq", linea:4, partes:["t_2 - t_1 = \\frac{2(900)}{\\sqrt{2(9.81 \\, m/s^2)}} \\Big[ (3.0 \\, m)^{1/2} - (0.50 \\, m)^{1/2} \\Big]"]},
            
            {tipo:"eq", linea:5, partes:["t_2 - t_1 = \\frac{1800}{4.429 \\, m^{1/2}/s} (1.732 - 0.707) \\, m^{1/2}"]},

            {tipo:"eq", linea:6, partes:["t_2 - t_1 = 406.41 \\, s (1.025)"]},

            {tipo:"add", linea:6, texto:"\\approx 417 \\, s"},
            
            {tipo:"intro", texto:"5) Para una mejor comprensión, se convierte el tiempo de segundos a minutos y segundos:"},
            {tipo:"eq", linea:7, partes:["417 \\, s = 6 \\, min \\, 57 \\, s"]},

            {tipo:"final", texto:"Resultado: <br><strong>El tiempo de vaciado es de 417 segundos, o 6 minutos y 57 segundos.</strong>"}
        ];

        const contenedor = document.getElementById('solution');
        const lineas = [];
        let pasoActual = 0;

        document.getElementById('nextBtn').addEventListener('click', () => {
            if(pasoActual >= pasos.length){
                alert("Ya se mostró toda la solución.");
                document.getElementById('nextBtn').style.display = 'none';
                return;
            }
            const paso = pasos[pasoActual];

            if(paso.tipo === "intro"){
                const p = document.createElement('p');
                p.className = 'line';
                p.innerHTML = paso.texto;
                contenedor.appendChild(p);
            } else if(paso.tipo === "eq"){
                const p = document.createElement('p');
                p.className = 'line eq';
                p.dataset.linea = paso.linea;
                p.innerHTML = `\\[ ${paso.partes.join(' ')} \\]`;
                contenedor.appendChild(p);
                lineas[paso.linea] = {elemento: p, partes:[...paso.partes]};
            } else if(paso.tipo === "add"){
                const linea = lineas[paso.linea];
                if (linea) {
                    linea.partes.push(paso.texto);
                    linea.elemento.innerHTML = `\\[ ${linea.partes.join(' ')} \\]`;
                }
            } else if (paso.tipo === "final") {
                const p = document.createElement('p');
                p.className = 'resultado';
                p.innerHTML = paso.texto;
                contenedor.appendChild(p);
            }
            MathJax.typesetPromise();
            pasoActual++;
        });
    </script>
</body>
</html>
