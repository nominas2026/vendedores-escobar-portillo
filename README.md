<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ejecutivo de Ventas</title>

<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: #f2f5f9;
    color: #1d2733;
}

.card {
    max-width: 430px;
    margin: 30px auto;
    background: white;
    border-radius: 18px;
    overflow: hidden;
    box-shadow: 0 5px 20px rgba(0,0,0,.12);
}

.header {
    background: #07549b;
    color: white;
    text-align: center;
    padding: 25px 20px;
}

.header h1 {
    margin: 0;
    font-size: 22px;
}

.header p {
    margin: 8px 0 0;
    font-size: 14px;
}

.content {
    padding: 25px;
}

.foto {
    width: 120px;
    height: 120px;
    border-radius: 50%;
    object-fit: cover;
    display: block;
    margin: 0 auto 20px;
    background: #e8edf3;
}

.nombre {
    text-align: center;
    font-size: 21px;
    font-weight: bold;
    margin-bottom: 5px;
}

.puesto {
    text-align: center;
    color: #07549b;
    font-weight: bold;
    margin-bottom: 25px;
}

.dato {
    padding: 12px 0;
    border-bottom: 1px solid #e5e7eb;
}

.etiqueta {
    font-size: 12px;
    color: #777;
}

.valor {
    font-size: 16px;
    margin-top: 3px;
}

.botones {
    margin-top: 25px;
    display: grid;
    gap: 10px;
}

.boton {
    display: block;
    text-align: center;
    padding: 13px;
    border-radius: 10px;
    text-decoration: none;
    font-weight: bold;
    background: #07549b;
    color: white;
}

.boton.secundario {
    background: #e9f1f8;
    color: #07549b;
}

.footer {
    text-align: center;
    font-size: 11px;
    color: #888;
    padding: 15px;
}
</style>
</head>

<body>

<div class="card">

    <div class="header">
        <h1>COMERCIAL ESCOBAR PORTILLO, S.A.</h1>
        <p>Ficha digital de vendedor</p>
    </div>

    <div class="content">

        <img id="foto" class="foto" style="display:none;">

        <div id="nombre" class="nombre"></div>
        <div class="puesto">EJECUTIVO DE VENTAS</div>

        <div class="dato">
            <div class="etiqueta">DPI</div>
            <div id="dpi" class="valor"></div>
        </div>

        <div class="dato">
            <div class="etiqueta">TELÉFONO</div>
            <div id="telefono" class="valor"></div>
        </div>

        <div class="dato">
            <div class="etiqueta">CORREO</div>
            <div id="correo" class="valor"></div>
        </div>

        <div class="dato">
            <div class="etiqueta">RUTA</div>
            <div id="ruta" class="valor"></div>
        </div>

        <div class="botones">
            <a id="llamar" class="boton">📞 Llamar</a>
            <a id="correoBtn" class="boton secundario">✉️ Enviar correo</a>
            <a id="guardar" class="boton secundario">💾 Guardar contacto</a>
        </div>

    </div>

    <div class="footer">
        Comercial Escobar Portillo, S.A.
    </div>

</div>

<script src="vendedores.js"></script>

<script>

const parametros = new URLSearchParams(window.location.search);
const id = parametros.get("vendedor");

const vendedor = vendedores[id];

if (!vendedor) {

    document.querySelector(".content").innerHTML =
        "<h2 style='text-align:center'>Vendedor no encontrado</h2>";

} else {

    document.getElementById("nombre").textContent = vendedor.nombre;
    document.getElementById("dpi").textContent = vendedor.dpi;
    document.getElementById("telefono").textContent = vendedor.telefono;
    document.getElementById("correo").textContent =
        vendedor.correo || "Pendiente";
    document.getElementById("ruta").textContent =
        vendedor.ruta || "Pendiente";

    document.getElementById("llamar").href =
        "tel:" + vendedor.telefono;

    if (vendedor.correo) {
        document.getElementById("correoBtn").href =
            "mailto:" + vendedor.correo;
    } else {
        document.getElementById("correoBtn").style.display = "none";
    }

    if (vendedor.foto) {
        const foto = document.getElementById("foto");
        foto.src = vendedor.foto;
        foto.style.display = "block";
    }

    document.getElementById("guardar").onclick = function() {

        const vcard =
`BEGIN:VCARD
VERSION:3.0
FN:${vendedor.nombre}
ORG:Comercial Escobar Portillo, S.A.
TITLE:EJECUTIVO DE VENTAS
TEL:${vendedor.telefono}
EMAIL:${vendedor.correo}
NOTE:DPI: ${vendedor.dpi}
END:VCARD`;

        const archivo = new Blob([vcard], {
            type: "text/vcard"
        });

        const enlace = document.createElement("a");
        enlace.href = URL.createObjectURL(archivo);
        enlace.download = vendedor.nombre + ".vcf";
        enlace.click();
    };
}

</script>

</body>
</html>
