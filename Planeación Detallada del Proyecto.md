# Planeación Detallada del Proyecto

## Plataforma de Certificación de Arte con NFT

### 1.Problemática Específica y Alcance
Problema Central: La falta de un mecanismo técnico unificado, confiable y accesible que permita a artistas digitales demostrar de manera irrefutable la autoría y originalidad de sus obras, facilitando al mismo tiempo su comercialización en un mercado digital saturado y propenso a la infracción de derechos de autor.

Alcance Técnico del MVP (Producto Mínimo Viable):

Frontend: Una aplicación web responsive desarrollada con React.js y Tailwind CSS.
Backend: Una API RESTful construida con Node.js (Express) o Python (Django REST Framework).
Base de Datos: PostgreSQL para datos transaccionales (usuarios, obras, metadatos).
IA: Implementación inicial usando la API de Google Vision o un modelo pre- entrenado de TensorFlow.js para la detección de similitudes, alojado en un servicio como Google Cloud AI Platform o una instancia de AWS SageMaker.
Blockchain: Implementación en la testnet de Polygon (Mumbai) o Ethereum (Sepolia) para reducir costos durante el desarrollo. Uso de la biblioteca Web3.js o Ethers.js para la interacción con un Smart Contract estándar ERC-721.
Almacenamiento: Almacenamiento de imágenes en un bucket de Amazon S3 o Google Cloud Storage, con los metadatos cruciales (hash de la imagen, enlace al NFT) guardados en la base de datos relacional.


### 2.Historias de Usuario Desglosadas en Tareas Técnicas Épica 1: Gestión de Usuarios y Autenticación

HU01 - Registro de Usuario:
Tarea 1.1: Diseñar esquema de BD para la tabla users (id, email_hashed, password_hashed, created_at).
Tarea 1.2: Implementar endpoint POST /api/auth/register.
Tarea 1.3: Implementar formulario de registro en React con validaciones (email válido, contraseña segura).
Tarea 1.4: Implementar hash de contraseñas con bcrypt.

HU02 - Perfil de Artista:
Tarea	2.1:	Extender	esquema	de users con campos bio, avatar_url, social_links (JSON).
Tarea 2.2: Crear endpoints GET y PATCH /api/users/me.
Tarea 2.3: Crear componente de edición de perfil en el frontend. Épica 2: Flujo Principal de Certificación

HU03 - Subir Obra:
Tarea 3.1: Diseñar esquema de BD para la tabla artworks (id, user_id, title, description, technique, image_url, status, etc.).
Tarea 3.2: Crear endpoint POST /api/artworks con middleware de upload (usando Multer).
Tarea 3.3: Integrar cliente SDK (AWS S3 o GCS) para subir la imagen al cloud storage.
Tarea 3.4: Crear componente de drag-and-drop en el frontend con preview de la imagen.
HU04 - Verificación de Formato:
Tarea	4.1:	Implementar	middleware	que verifique req.file.mimetype (image/jpeg, image/png).
Tarea 4.2: Implementar verificación de tamaño de archivo (ej: max 10MB).

HU05 - Análisis de Originalidad:
Tarea 5.1: Investigar y seleccionar API de IA (Google Vision SafeSearch o Custom Vision).
Tarea 5.2: Implementar un servicio aiService.js que llame a la API seleccionada.
Tarea 5.3: Crear un job en cola (con Bull.js o similar) para procesar el análisis de forma asíncrona tras la subida.
Tarea 5.4: Diseñar un esquema para guardar el resultado del análisis (score de confianza, etiquetas) en la BD.
(Las Épicas 3, 4 y 5 seguirían el mismo nivel de desglose detallado)...

### 3.Diseño Técnico de Interfaces y Arquitectura

Arquitectura Propuesta: Microservicios incipientes. Un backend monolítico bien estructurado con separación de concerns, preparado para una posible escisión futura en microservicios (Servicio de Usuarios, Servicio de Obras, Servicio de IA, Servicio de Blockchain).

Especificación de APIs:
Auth API: /api/auth/login, /api/auth/register. Devuelve JWTs.
Users API: /api/users/me (GET, PATCH).
Artworks API: /api/artworks (GET, POST), /api/artworks/:id (GET).

Admin
API: /api/admin/artworks/pending (GET), /api/admin/artworks/:id/approve (POST) - Protegida con rol de admin.
Wireframes Detallados (Descripción Funcional):
Vista de Detalle de Obra: La página tendrá un layout de dos columnas. La columna izquierda contendrá la imagen con un zoom suave al pasar el cursor. La columna derecha tendrá: Título (h1), Nombre del Artista (enlace a su perfil), Descripción (párrafo), Técnica (badge). Debajo, una sección acordeón desplegable titled "Certificado Digital" mostrará: Dirección del Contrato, Token ID, un botón "Ver en Polygon Scan" (enlace externo) y un QR code que codifique la dirección del contrato y el token ID para una verificación fácil.

### 4.Cronograma de Sprints con Entregables Específicos 


## Sprint 1 (Semana 1-2): Núcleo de la Aplicación
Objetivo: tener una API funcional y un frontend básico conectado.
Tareas Críticas Backend: Configurar proyecto Node.js/Express, conectar a PostgreSQL, implementar autenticación JWT, configurar upload a Cloud Storage.

Tareas Críticas Frontend: Configurar React con Vite, crear componentes de Router, Login, Registro, Formulario de Subida básico.
Entregable Específico: Un usuario puede registrarse, loguearse y subir una imagen que se guarda en la nube y su referencia en la BD. No hay UI bonita.

## Sprint 2 (Semana 3-4): Integración de IA y Panel de Admin
Objetivo: Automatizar el análisis de imágenes y permitir la moderación.
Tareas Críticas Backend: Implementar el servicio de IA, diseñar e implementar el sistema de colas (Bull.js), crear endpoints de admin.
Tareas Críticas Frontend: Crear vista de panel de admin (lista de obras con estado pending_review), implementar lógica para aprobar/rechazar.

Entregable Específico: Tras subir una obra, el admin recibe una notificación (en la UI) y puede revisarla y cambiar su estado manualmente. El resultado del análisis de IA se guarda en la BD.


## Sprint 3 (Semana 5-6): Integración con Blockchain
Objetivo: Emitir un NFT real upon approval.
Tareas Críticas Backend: Configurar Web3.js con provider de Mumbai, desplegar Smart	Contract	ERC-721	simple,	crear servicio blockchainService.mintNFT(artworkId, artistWalletAddress).
Tareas Críticas Frontend: Modificar el flujo de aprobación para que, al hacer click "Aprobar", el backend ejecute mintNFT y actualice la obra con la dirección del contrato y el token ID.
Entregable Específico: El admin puede aprobar una obra y se genera un NFT real en Mumbai. La página de detalle de la obra muestra la dirección del contrato y el token ID.
Sprint 4 (Semana 7-8): Refinamiento, Testing y Despliegue
Objetivo: Tener un MVP desplegado y usable.
Tareas Críticas: Implementar la Galería Pública con paginación, mejorar masivamente el UI/UX con Tailwind CSS, escribir pruebas unitarias para servicios críticos (auth, upload), configurar despliegue en Vercel (Frontend) y Railway/Render (Backend y BD).
Entregable Específico: La aplicación está en producción. Un usuario anónimo puede ver la galería. Un artista puede registrarse, subir una obra y verla certificada con NFT tras ser aprobada.


### 5.Plan de Pruebas Específico
Pruebas Unitarias (Jest):
Servicio de utilidad que calcula el hash de una imagen.
Servicio que formatea los metadatos para la blockchain.
Lógica de validación de contraseñas.
Pruebas de Integración (Supertest):
Flujo completo de registro -> login -> obtención de token.
Flujo de subida de obra (mockeando el upload a S3 y la llamada a la IA).
Endpoint de aprobación de admin (mockeando la transacción a blockchain).
Pruebas E2E (Cypress o Playwright):
Un usuario completa el registro.
Un usuario sube una obra.
Un admin se loguea, revisa la obra y la aprueba.
Un usuario verifica que su obra ahora muestra el badge de "Verificado".


### 6.Asignación de Responsabilidades Técnicas Específicas

arquitecto Backend & DevOps: [Nombre, e.g., Brayan]. Responsable de la configuración inicial del proyecto backend, la estructura de la BD, la configuración de los servicios en la nube (S3, IA), el diseño de la API, el script de despliegue y la configuración del entorno de producción. Revisará todos los Pull Requests relacionados con la lógica del servidor.

Especialista en Smart Contracts & Blockchain: [Nombre, e.g., Laura]. Responsable de investigar las testnets, escribir y desplegar el contrato ERC-721 (usando Hardhat o Truffle), desarrollar el servicio blockchainService.js, gestionar las wallets del proyecto y la seguridad de las claves privadas. Será el único que maneje las claves de despliegue.

Líder de Frontend & UX: [Nombre, e.g., Brayan]. Responsable de la arquitectura de la aplicación React, la elección de librerías de UI (TanStack Table para la lista de admin), la implementación de la lógica de estados globales (Context API o Zustand), la garantía de que la aplicación sea totalmente responsive, y la implementación final de los diseños. Coordinará las tareas de los otros desarrolladores frontend.

Desarrollador Frontend: [Nombre, e.g., Laura]. Responsable de implementar componentes específicos bajo la dirección del Líder de Frontend (ej: el componente de la galería, el formulario de perfil), escribir pruebas unitarias para los componentes y ayudar a resolver bugs de la interfaz.

Coordinador de QA & Testing: [Nombre, e.g., Laura]. Responsable de definir el plan de pruebas, escribir los casos de prueba E2E en Cypress, ejecutar pruebas de usabilidad con usuarios de prueba, llevar un registro de bugs en un board (GitHub Projects) y verificar que las correcciones sean efectivas antes de marcar una tarea como completada.

