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
    <div class="info-lateral">
        <h3>Datos del Alojamiento</h3>
        <p><strong>Número de Parte:</strong> ___________</p>
        <p><strong>Alojamiento:</strong> Hospedaje Nuestra Señora de Ujué</p>
        <p><strong>Dirección:</strong> Avenida Nuestra Señora de Ujué N.18</p>
        <p><strong>Código Postal:</strong> 31300</p>
        <p><strong>Localidad:</strong> Tafalla</p>
        <p><strong>Provincia:</strong> Navarra</p>
        <p><strong>NIF/CIF:</strong> B31970734</p>
        <h3>Datos del Viajero</h3>
        <p><strong>Fecha del parte:</strong> <span id="fecha_parte"></span></p>
    </div>

    <div class="formulario">
        <h1>Check-in Automático</h1>
        <form id="checkinForm">
            <label>Subir foto del DNI:</label>
            <input type="file" accept="image/*" capture="camera" id="dniFoto" onchange="procesarDNI()">
            
            <label>Nombre:</label>
            <input type="text" name="nombre" id="nombre" required>
            
            <label>Apellidos:</label>
            <input type="text" name="apellidos" id="apellidos" required>
            
            <label>Sexo (M/F):</label>
            <input type="text" name="sexo" id="sexo" required>
            
            <label>Número de Documento de Identidad:</label>
            <input type="text" name="dni" id="dni" required>
            
            <label>Número de Soporte:</label>
            <input type="text" name="n_soporte" id="n_soporte" required>
            
            <label>Tipo de Documento:</label>
            <input type="text" name="tipo_documento" id="tipo_documento" required>
            
            <label>Nacionalidad:</label>
            <input type="text" name="nacionalidad" id="nacionalidad" required>
            
            <label>Fecha de Nacimiento:</label>
            <input type="date" name="fecha_nacimiento" id="fecha_nacimiento" required>
            
            <label>Fecha de Expedición:</label>
            <input type="date" name="fecha_expedicion" id="fecha_expedicion" required>
            
            <label>Dirección:</label>
            <input type="text" name="direccion" id="direccion" required>
            
            <label>Código Postal:</label>
            <input type="text" name="codigo_postal" id="codigo_postal" required>
            
            <label>Localidad:</label>
            <input type="text" name="localidad" id="localidad" required>
            
            <label>País:</label>
            <input type="text" name="pais" id="pais" required>
            
            <label>Teléfono Móvil:</label>
            <input type="text" name="telefono_movil" id="telefono_movil" required>
            
            <label>Número de Viajeros:</label>
            <input type="number" name="numero_viajeros" id="numero_viajeros" required>
            
            <label>Relación de Parentesco (si es menor de 16 años):</label>
            <input type="text" name="relacion_parentesco" id="relacion_parentesco">
            
            <canvas id="firmaCanvas" width="400" height="150"></canvas>
<button type="button" onclick="limpiarFirma()">Limpiar Firma</button>

<script>
    let canvas = document.getElementById("firmaCanvas");
    let ctx = canvas.getContext("2d");

    let firmando = false;

    // Ajustar el tamaño del canvas en dispositivos móviles
    function ajustarCanvas() {
        let rect = canvas.getBoundingClientRect();
        canvas.width = rect.width;
        canvas.height = rect.height;
        ctx.strokeStyle = "black";
        ctx.lineWidth = 2;
        ctx.lineJoin = "round";
        ctx.lineCap = "round";
    }

    ajustarCanvas(); // Llamamos al inicio

    // Dibujar con el ratón
    canvas.addEventListener("mousedown", (e) => {
        firmando = true;
        ctx.beginPath();
        ctx.moveTo(e.offsetX, e.offsetY);
    });

    canvas.addEventListener("mousemove", (e) => {
        if (!firmando) return;
        ctx.lineTo(e.offsetX, e.offsetY);
        ctx.stroke();
    });

    canvas.addEventListener("mouseup", () => {
        firmando = false;
    });

    // Dibujar con el dedo en móviles
    canvas.addEventListener("touchstart", (e) => {
        e.preventDefault(); // Evitar el desplazamiento de la pantalla
        firmando = true;
        let touch = e.touches[0];
        let rect = canvas.getBoundingClientRect();
        ctx.beginPath();
        ctx.moveTo(touch.clientX - rect.left, touch.clientY - rect.top);
    });

    canvas.addEventListener("touchmove", (e) => {
        if (!firmando) return;
        e.preventDefault();
        let touch = e.touches[0];
        let rect = canvas.getBoundingClientRect();
        ctx.lineTo(touch.clientX - rect.left, touch.clientY - rect.top);
        ctx.stroke();
    });

    canvas.addEventListener("touchend", () => {
        firmando = false;
    });

    // Limpiar firma
    function limpiarFirma() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
    }
</script>

