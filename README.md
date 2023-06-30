# Laboratorio_4


En el codigo HTML encontramos

<!DOCTYPE html>
<html>
<head>
 <title>Revista Interactiva</title>
 <link rel="stylesheet" type="text/css" href="estilos.css">
</head>
<body>
 <h1 id="revista-titulo"></h1>
 <div id="articulos-container"></div>
 <div id="modal" class="modal">
 <div class="modal-content">
 <span class="close">&times;</span>
 <h2 id="modal-titulo"></h2>
 <img id="modal-imagen" src="" alt="" class="modal-imagen">
 <p id="modal-contenido"></p>
 </div>
 </div>
 <script src="app.js"></script>
</body>
</html>

en el codigo CSS encontramos

body {
    position: relative;
   }
   body::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 200%;
    height: 200%;
    background-image: url('imagenes/fondo.png');
    background-repeat: repeat-x;
    background-size: 80px;
    opacity: 0.7;
    transform: rotate(10deg);
    /* Ajusta el valor de la opacidad (0.7 en este caso) */
    z-index: -1;
   }
   h1 {
    text-align: center;
    padding: 20px 0;
   }
   #articulos-container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    padding: 20px;
   }
   .articulo-card {
    flex-basis: 300px;
    flex-grow: 1;
    padding: 20px;
    margin: 10px;
    background-color: #f5f5f5;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease;
   }
   .articulo-card:hover {
    transform: translateY(-5px);
    cursor: pointer;
   }
   .articulo-card h3 {
    margin-top: 0;
   }
   .articulo-card img {
    width: 100%;
    height: auto;
    margin-bottom: 10px;
   }
   .articulo-card p {
    margin-bottom: 0;
   }
   /* Estilos para el modal */
   .modal {
    display: none;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgba(0, 0, 0, 0.4);
   }
   .modal-content {
    background-color: #fefefe;
    margin: 0 auto 0 auto;
    /* Ajustamos los márgenes */
    padding: 20px;
    border: 1px solid #888;
    width: auto;
    max-width: 60%;
    box-sizing: border-box;
    text-align: center;
    overflow: auto;
    border-radius: 10px;
    border: 2px dashed black;
   }
   .close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
    cursor: pointer;
   }
   .close:hover,
   .close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
   }
   .modal-imagen {
    display: block;
    width: 300px;
    height: auto;
    max-width: 100%;
    max-height: 100%;
    margin: 20px auto;
    object-fit: contain;
   }
   #modal-contenido {
    line-height: 1.5;
    text-align: justify;
   }
   @media screen and (max-width: 600px) {
    .modal-content {
    margin: 20px;
    padding: 10px;
    width: auto;
    max-width: none;
    border-radius: 10px;
    }
    .modal-imagen {
    max-width: 100%;
    }
   }
   .modal-abierto {
    animation: abrirModal 0.2s ease-out;
   }
   @keyframes abrirModal {
    0% {
    opacity: 0.2;
    transform: scale(2.3);
    }
    100% {
    opacity: 1;
    transform: scale(1.2);
    }
   }
   .modal-cerrado {
    animation: cerrarModal 0.3s ease;
   }
   @keyframes cerrarModal {
    0% {
    opacity: 1;
    transform: scale(1);
    }
    80% {
    opacity: 0;
    transform: scale(3);
    }
    100% {
    opacity: 0;
    transform: scale(2);
    }
   }


   en el codigo Javascript encontramos

   fetch('data.json')
 .then(response => response.json())
 .then(data => {
 const revistaTitulo = document.getElementById('revista-titulo');
 const articulosContainer = document.getElementById('articulos-container');
 revistaTitulo.textContent = data.titulo;
 data.articulos.forEach(articulo => {
 const articuloCard = document.createElement('div');
 articuloCard.classList.add('articulo-card');
 articuloCard.innerHTML = `
 <h3>${articulo.titulo}</h3>
 <img src="${articulo.imagen}" alt="">
 <p>${limitarDescripcion(articulo.contenido, 10)}</p>
 `;
 articuloCard.addEventListener('click', () => {
 mostrarModal(articulo);
 });
 articulosContainer.appendChild(articuloCard);
 });
 const imagenes = document.querySelectorAll('.articulo-card img');
 imagenes.forEach(function (imagen) {
 imagen.style.display = 'none'; // Oculta todas las imágenes al cargar la página
 });
 })
 .catch(error => {
 console.log('Error al cargar el archivo JSON:', error);
 });
function limitarDescripcion(texto, longitud) {
 const palabras = texto.split(' ');
 if (palabras.length > longitud) {
 return palabras.slice(0, longitud).join(' ') + '...' + '<br><br>Seguir leyendo...';
 }
 return texto;
}
function mostrarModal(articulo) {
 const modal = document.getElementById('modal');
 const modalTitulo = document.getElementById('modal-titulo');
 const modalImagen = document.getElementById('modal-imagen');
 const modalContenido = document.getElementById('modal-contenido');
 modalTitulo.textContent = articulo.titulo;
 modalContenido.textContent = articulo.contenido;
 modalContenido.classList.add('modal-abierto');
 modal.style.display = 'block';
 modalImagen.style.display = 'none'; // Oculta la imagen inicialmente
 modalImagen.onload = function () {
 modalImagen.style.display = 'block'; // Muestra la imagen cuando se carga
 };
 modalImagen.src = articulo.imagen;
 const closeBtn = document.getElementsByClassName('close')[0];
 closeBtn.addEventListener('click', () => {
 modalContenido.classList.add('modal-cerrado');
 setTimeout(() => {
 modalContenido.classList.remove('modal-cerrado');
 modal.style.display = 'none';
 }, 300);
 });
 window.addEventListener('click', event => {
 if (event.target === modal) {
 modalContenido.classList.remove('modal-abierto');
 modal.style.display = 'none';
 }
 });
}


en el Data json encontramos

{
    "titulo": "Revista de Partes de Computadoras",
    "articulos": [
    {
    "titulo": "Procesadores",
    "imagen": "imagenes/procesador.jpg",
    "contenido": "Los procesadores son componentes fundamentales en una computadora. Son responsables de realizar las operaciones y cálculos necesarios para el funcionamiento del sistema. Estos chips de silicio contienen millones de transistores y pueden ejecutar instrucciones en nanosegundos. Los procesadores actuales ofrecen un rendimiento excepcional y están diseñados para manejar tareas complejas como juegos, edición de video y renderizado 3D. Con el avance de la tecnología, los procesadores cada vez son más rápidos y eficientes, lo que permite realizar tareas más exigentes y mejorar la experiencia del usuario."
    },
    {
    "titulo": "Tarjetas gráficas",
    "imagen": "imagenes/tarjeta-madre.jpg",
    "contenido": "Las tarjetas gráficas son componentes esenciales para los amantes de los juegos y el diseño gráfico. Estas unidades de procesamiento gráfico (GPU) están diseñadas específicamente para manejar tareas intensivas en gráficos, como renderizado en 3D, efectos visuales y cálculos matemáticos complejos. Las tarjetas gráficas modernas ofrecen un rendimiento excepcional y son capaces de mostrar gráficos de alta calidad y realismo en tiempo real. Además, las últimas tarjetas gráficas cuentan con tecnologías avanzadas como trazado de rayos y inteligencia artificial, lo que permite obtener imágenes más detalladas y efectos visuales impresionantes."
    },
    {
    "titulo": "Memoria RAM",
    "imagen": "imagenes/memoria-ram.jpeg",
    "contenido": "La memoria RAM (Random Access Memory) es un componente vital en cualquier computadora. Actúa como espacio de almacenamiento temporal para los datos y programas que se están utilizando en el momento. Cuanta más memoria RAM tenga un equipo, más rápido y eficiente será. La memoria RAM permite acceder rápidamente a los datos, lo que acelera el rendimiento general del sistema. En la actualidad, existen diferentes tipos de RAM, como DDR3, DDR4 y DDR5, cada una con diferentes velocidades y capacidades. Los usuarios que realizan tareas exigentes, como edición de video o juegos, suelen requerir una mayor cantidad de memoria RAM para garantizar un funcionamiento fluido y sin interrupciones."
    },
    {
    "titulo": "Disco duro",
    "imagen": "imagenes/disco-duro.jpg",
    "contenido": "El disco duro es el dispositivo de almacenamiento principal de la computadora, donde se guardan los archivos y programas. Los discos duros, también conocidos como HDD, son un componente informático que sirve para almacenar de forma permanente tus datos. Esto quiere decir, que los datos no se borran cuando se apaga la unidad como pasa en los almacenados por la memoria RAM. La primera empresa en comercializarlos fue IBM en 1956."
    }
    ]
   }

   
