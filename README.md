<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Check-in Online</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tesseract.js/4.0.2/tesseract.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: space-between;
            padding: 20px;
        }
        .info-lateral {
            width: 30%;
            padding: 20px;
            border-right: 2px solid #000;
        }
        .formulario {
            width: 65%;
            padding: 20px;
        }
        label {
            display: block;
            margin-top: 10px;
        }
        input {
            width: 100%;
            padding: 5px;
            margin-top: 5px;
        }
        button {
            margin-top: 15px;
            padding: 10px;
            background-color: #28a745;
            color: white;
            border: none;
            cursor: pointer;
        }
        canvas {
            border: 1px solid #000;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <div class="formulario">
        <h1>Check-in Automático</h1>
        <form id="checkinForm">
            <label>Subir foto del DNI:</label>
            <input type="file" accept="image/*" capture="camera" id="dniFoto" onchange="procesarDNI()">
            
            <label>Nombre:</label>
            <input type="text" name="nombre" id="nombre" required>

            <label>Apellidos:</label>
            <input type="text" name="apellidos" id="apellidos" required>

            <label>Número de Documento de Identidad:</label>
            <input type="text" name="dni" id="dni" required>
            
            <h3>Firma:</h3>
            <canvas id="firmaCanvas" width="400" height="150"></canvas>
            <button type="button" onclick="limpiarFirma()">Limpiar Firma</button>
        </form>
    </div>

    <script>
        function procesarDNI() {
            const input = document.getElementById('dniFoto');
            if (input.files.length === 0) return;

            const file = input.files[0];
            const reader = new FileReader();
            reader.onload = function () {
                Tesseract.recognize(reader.result, 'spa', {
                    logger: m => console.log(m)
                }).then(({ data: { text } }) => {
                    console.log('Texto extraído:', text);
                    document.getElementById('dni').value = extraerDNI(text);
                    document.getElementById('nombre').value = extraerNombre(text);
                    document.getElementById('apellidos').value = extraerApellidos(text);
                });
            };
            reader.readAsDataURL(file);
        }

        function extraerDNI(texto) {
            const match = texto.match(/\b\d{8}[A-Z]\b/);
            return match ? match[0] : '';
        }
        
        function extraerNombre(texto) {
            const lineas = texto.split('\n');
            return lineas.length > 2 ? lineas[1] : '';
        }
        
        function extraerApellidos(texto) {
            const lineas = texto.split('\n');
            return lineas.length > 3 ? lineas[2] : '';
        }
        
        function limpiarFirma() {
            const canvas = document.getElementById('firmaCanvas');
            const ctx = canvas.getContext('2d');
            ctx.clearRect(0, 0, canvas.width, canvas.height);
        }
    </script>
</body>
</html>
