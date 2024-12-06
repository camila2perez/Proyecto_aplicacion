# Objetivo del prototipo

Permitir a los usuarios:

1. Visualizar obras de arte en RA en su entorno físico.

2. Acceda a la historia detallada de cada pintura y explore sus detalles artísticos.

3. Personalizar elementos de las obras (colores, marcos).

4. Guarde y acuñar la obra personalizada como NFT para compra o venta.

5. Interactuar con una comunidad mediante comentarios y valoraciones.

# Fases del proceso

1. Diseño de la Arquitectura del Prototipo

    La aplicación tiene 4 módulos principales:

Visualización en RA : Permite superponer obras de arte en el entorno físico del usuario.

Información Histórica : Proporciona datos detallados sobre las obras, artistas y estilos artísticos.

Personalización digital : Brinda herramientas para modificar colores, tamaños o marcos.

NFTs y Comunidad : Funciones para guardar la obra personalizada como NFT, gestionar la billetera y un sistema de comentarios.

2. Herramientas y Tecnologías Utilizadas

     Desarrollo de RA :

Unity : Motor de desarrollo para crear experiencias interactivas.
 
AR Foundation : Framework que permite integrar ARKit (iOS) y ARCore (Android).

Desarrollo de IA (si es necesario para personalización avanzada):

TensorFlow Lite : Para implementar modelos de visión por computadora.

 Blockchain y NFT :

Ethereum Blockchain : Para acuñar NFT.

MetaMask : Integración para billeteras de los usuarios.

  Base de datos e historia :

Firebase : Para almacenar la información histórica y gestionar los datos de los usuarios.

Diseño y Modelado 3D :

Blender : Para preparar los modelos 3D de las pinturas.

Figma : Para diseñar la interfaz (UI/UX).

3. Descripción del Flujo del Prototipo

Paso 1: Escaneo del Entorno y Selección de la Obra

El usuario abre la aplicación y permite acceder a la cámara.

La aplicación escanea el entorno para detectar superficies planas (paredes o mesas) utilizando ARKit/ARCore .

El usuario selecciona una obra de arte desde un catálogo interactivo.

Proceso técnico:

El sistema utiliza SLAM (Simultaneous Localization and Mapping) para mapear el entorno.

Una base de datos con las obras de arte está vinculada al backend de Firebase, que permite cargar imágenes y modelos 3D.

Paso 2: Visualización en Realidad Aumentada

La obra seleccionada aparece superpuesta en la superficie detectada.

El usuario puede mover, escalar y rotar la obra para ajustarla en el espacio físico.

Proceso técnico:

Unity renderiza el modelo 3D con textura de alta resolución.
Se utilizan scripts de interacción para ajustar la posición y escalar mediante gestos táctiles.

Paso 3: Información detallada

El usuario puede tocar la obra para desplegar información, como:

Biografía del artista.

Descripción del estilo o época.

Significado detrás de la obra.

La información se presenta en formato interactivo, con texto, imágenes y vídeos.

Proceso técnico:

Los datos se cargan dinámicamente desde Firebase mediante API REST.

Se utiliza un diseño modular para mostrar texto enriquecido, imágenes y vídeos.

Paso 4: Personalización Digital

Opciones de personalización :

Cambiar colores predominantes (usando filtros de color en tiempo real).
Ajustar el tamaño del marco o elegir entre diferentes estilos de marcos.
Añadir elementos decorativos (por ejemplo, luces o efectos de textura).

Interactividad :
Los cambios se reflejan en tiempo real sobre la obra visualizada en RA.
El usuario puede guardar varias configuraciones para comparar estilos.

Confirmación de la personalización :

Una vez satisfecho, el usuario puede guardar su versión personalizada como un archivo único.

Proceso técnico:

Se usan sombreadores y materiales dinámicos en Unity para permitir cambios de color y textura.

Los ajustes se guardan como datos en Firebase, vinculados al perfil del usuario.

Si el modelo personalizado será acuñado como NFT, se genera una versión optimizada (menor tamaño de archivo, formato compatible).

Paso 5: Guardar y acuñar como NFT

El usuario selecciona la opción “Guardar como NFT”.
Se solicita la conexión de la billetera (ejemplo: MetaMask ).
La obra personalizada se acuña como un NFT único en la blockchain

incluyendo: Metadatos (nombre del artista, personalizaciones, fecha de creación).

Propiedades del NFT para el mercado (como rareza o edición limitada).

El NFT se envía directamente a la billetera del usuario.

Opcional: El usuario puede listar el NFT en mercados como OpenSea.

Proceso técnico:

Contratos inteligentes en Ethereum :

Se crea un contrato inteligente para acuñar el NFT usando bibliotecas como web3.js o ethers.js
