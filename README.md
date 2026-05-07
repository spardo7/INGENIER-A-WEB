# 🧺 La Canasta Familiar — Tienda Online

> Sitio web completo para un fruver bogotano con tienda online, carrito, sistema de repartidores con mapa en tiempo real, panel de administración y minijuego interactivo.

---

## 📋 Tabla de Contenidos

- [Demo y Tecnologías](#-tecnologías-utilizadas)
- [Estructura del Proyecto](#-estructura-del-proyecto)
- [Funcionalidades](#-funcionalidades)
- [Panel de Administración](#️-panel-de-administración)
- [Sistema de Repartidores y Mapa](#-sistema-de-repartidores-y-mapa)
- [Firebase](#-configuración-de-firebase)
- [Google Maps API](#-google-maps-api)
- [Agente Secreto](#-misión-agente-dom-modo-secreto)
- [Cómo ejecutar](#-cómo-ejecutar)

---

## 🛠 Tecnologías Utilizadas

| Tecnología | Uso |
|---|---|
| HTML5 + CSS3 | Estructura y estilos del sitio |
| JavaScript (Vanilla) | Lógica del frontend |
| Firebase Auth | Registro e inicio de sesión de usuarios |
| Cloud Firestore | Base de datos de productos y carritos |
| Google Maps JS API | Mapa de seguimiento del domiciliario |
| Google Geocoding API | Convertir dirección de texto a coordenadas |
| WhatsApp API | Envío de pedidos por mensaje |
| Canvas API | Minijuego de arrastrar frutas |

---

## 📁 Estructura del Proyecto

```
la-canasta-familiar/
│
├── index.html              # Archivo principal (todo el sitio)
├── css/
│   └── visual.css          # Estilos generales del sitio
├── images/
│   ├── Favicon Canasta.jpg
│   ├── Fondo de pantalla.png
│   ├── Maps Canasta.jpg
│   ├── Tomates.jpg
│   ├── Brocoli_Fresco.jpg
│   ├── Platano_Maduro.jpg
│   ├── Zanahoria.jpg
│   ├── Naranja_Valencia.jpg
│   ├── Lechuca_Crespa.jpg
│   ├── Fresas.jpg
│   ├── Canasta_familiar.jpg
│   ├── Pina.jpg
│   ├── Papa_Pastusa.jpg
│   └── default.jpg
├── video/
│   └── videoplayback (1).mp4
└── audio/
    └── videoplayback.m4a
```

---

## ✨ Funcionalidades

### 🛒 Tienda Online
El catálogo de productos se compone de dos fuentes:

1. **Productos base** — definidos directamente en el código (`productosBase[]`), siempre visibles sin necesidad de conexión a Firestore.
2. **Productos de Firestore** — cargados dinámicamente desde la base de datos al iniciar la página mediante `cargarProductosFirestore()`.

Los productos se pueden **filtrar por búsqueda** (nombre o categoría) y **ordenar** por precio ascendente/descendente, nombre o popularidad.

### 🔐 Autenticación de Usuarios
Usa **Firebase Authentication** con correo y contraseña:

- **Registro** — crea cuenta y guarda el perfil en Firestore (`/usuarios/{uid}`)
- **Login** — inicia sesión y carga el carrito guardado del usuario
- **Logout** — cierra sesión y limpia el estado local
- El estado del usuario se escucha en tiempo real con `onAuthStateChanged`

### 🛒 Carrito de Compras
- Flota sobre la página (carrito flotante)
- Persiste en Firestore para usuarios autenticados
- Muestra cantidad total e items
- Permite eliminar productos individuales o vaciar todo
- Incluye campo para ingresar la **dirección de domicilio**
- Botón de pedido por WhatsApp

---

## ⚙️ Panel de Administración

Accesible desde el enlace **⚙️ Admin** en la barra de navegación.

### 🔑 Credenciales de Acceso

| Usuario | Contraseña | Rol |
|---|---|---|
| `admin` | `canasta2026` | Administrador |
| `empleado` | `fruver123` | Empleado |

> ⚠️ Estas credenciales están definidas en el array `EMPLEADOS[]` dentro del código. No usan Firebase Auth — son solo para el panel interno.

### Funciones del Panel

#### ➕ Registrar Producto
Formulario completo con los siguientes campos:

| Campo | Descripción |
|---|---|
| Nombre | Nombre del producto (obligatorio) |
| Emoji / Ícono | Emoji representativo (ej: 🥭) |
| Categoría | Frutas, Verduras, Canastas u Otros |
| Precio (COP) | Precio en pesos colombianos |
| Popularidad | Valor de 0 a 100 |
| Ruta de imagen | Ruta relativa a la carpeta `images/` |
| Descripción | Texto descriptivo del producto |
| Stock disponible | Cantidad en inventario |
| Unidad de venta | kg, unidad, 500g, docena, libra, caja, canasta |
| Peso por unidad | En kg |
| Dimensiones | Largo × Ancho × Alto en cm |
| En Oferta | Checkbox para marcar como oferta |

Al registrar, el producto se guarda en Firestore (`/productos`) y se agrega automáticamente al catálogo visible en la tienda sin recargar la página.

#### 📋 Inventario
Tabla con todos los productos registrados en Firestore, mostrando emoji, nombre, categoría, precio, stock, unidad, peso, dimensiones y estado (oferta/normal). Cada fila tiene un botón **🗑️ Eliminar** que borra el producto de Firestore y lo quita del catálogo en tiempo real.

#### 📊 Estadísticas
Tarjetas en la parte superior del panel que muestran:
- Total de productos registrados
- Cantidad de frutas
- Cantidad de verduras
- Productos en oferta

---

## 🚚 Sistema de Repartidores y Mapa

Al hacer clic en **"📱 Pedir por WhatsApp"** desde el carrito, el sistema:

1. Lee la **dirección de domicilio** ingresada por el cliente
2. Abre WhatsApp con el resumen del pedido
3. Navega automáticamente a la sección **🚚 Repartidores**
4. Muestra el **progreso del pedido** en 5 pasos:
   - 🛒 Pedido Recibido
   - 🔍 Buscando Repartidor
   - ✅ Repartidor Asignado
   - 🚀 En Camino
   - ✅ Entregado

5. Despliega el **mapa de seguimiento** con:
   - 🧺 Marcador verde en la tienda (Cl. 8 #79c-16)
   - 📍 Marcador rojo en la dirección del cliente (geocodificada con Google)
   - 🛵 Marcador naranja animado que se mueve desde la tienda hasta el cliente
   - Línea de ruta curva (Bezier cuadrática) entre los dos puntos
   - ETA en tiempo real (minutos restantes)
   - Distancia aproximada en km

### Lógica de asignación de repartidores

El sistema elige automáticamente el **mejor repartidor disponible**, priorizando los de Zona 1-2. Los 4 repartidores del sistema son:

| Nombre | Zona | Teléfono |
|---|---|---|
| Juan Pérez 🛵 | Zona 1-2 | 320-123-4567 |
| María Gómez 🚲 | Zona 3-5 | 320-234-5678 |
| Carlos López 🛴 | Zona 1-2 | 320-345-6789 |
| Ana Rodríguez 🛵 | Zona 3-5 | 320-456-7890 |

### Velocidad de animación del mapa
La duración del recorrido animado es **proporcional a la distancia** real entre la tienda y el cliente, simulando ~30 km/h en ciudad. A mayor distancia, más tiempo tarda la moto en llegar en pantalla.

---

## 🔥 Configuración de Firebase

El proyecto usa Firebase con el siguiente setup en `firebaseConfig`:

```javascript
const firebaseConfig = {
  apiKey: "...",
  authDomain: "la-canasta-familiar.firebaseapp.com",
  projectId: "la-canasta-familiar",
  storageBucket: "la-canasta-familiar.firebasestorage.app",
  messagingSenderId: "...",
  appId: "..."
};
```

### Colecciones en Firestore

| Colección | Descripción |
|---|---|
| `/usuarios/{uid}` | Perfil del usuario: nombre, email, teléfono, carrito, lista |
| `/productos/{id}` | Productos registrados desde el panel admin |

---

## 🗺 Google Maps API

Se usan dos APIs de Google Cloud:

| API | Para qué se usa |
|---|---|
| **Maps JavaScript API** | Renderizar el mapa interactivo en la sección de repartidores |
| **Geocoding API** | Convertir la dirección escrita por el cliente en coordenadas lat/lng |

La API Key se define en el script del mapa:

```javascript
const MAPS_KEY = 'TU_API_KEY_AQUI';
```

> 💡 Ambas APIs se habilitan en [console.cloud.google.com](https://console.cloud.google.com) → APIs y servicios → Biblioteca, buscando sus nombres en inglés.

---

## 🎮 Minijuego — Llena la Canasta

En la sección **🎮 Juegos & Más** hay un canvas interactivo donde el usuario arrastra 8 frutas (Tomate, Manzana, Naranja, Limón, Uvas, Banano, Fresa, Kiwi) hacia una canasta. Lleva puntaje y muestra un mensaje al completar el juego.

Este juego hace parte de la **Misión Agente Dom** (ver abajo).

---

## 🕵️ Misión Agente Dom (Modo Secreto)

El sitio incluye un sistema de gamificación oculto que otorga un **30% de descuento** al cliente que complete 3 condiciones:

| # | Condición | Cómo cumplirla |
|---|---|---|
| 1 | ✅ Iniciar sesión | Crear cuenta o hacer login |
| 2 | ✅ Completar el minijuego | Llevar todas las frutas a la canasta |
| 3 | ✅ Hacer un pedido por WhatsApp | Con al menos 2 productos en el carrito |

Al completar las 3 condiciones:
- El título del sitio se resalta en dorado ✨
- Aparece un banner azul con el descuento calculado
- El botón de WhatsApp aplica automáticamente el 30% OFF al total del pedido

---

## 🚀 Cómo Ejecutar

1. Clona o descarga el repositorio
2. Asegúrate de tener todos los archivos de `images/`, `audio/` y `video/`
3. Reemplaza `MAPS_KEY` con tu API Key de Google Maps
4. Abre `index.html` directamente en el navegador

> ⚠️ Para que Firebase funcione correctamente el sitio debe servirse desde un servidor HTTP (no desde `file://`). Puedes usar la extensión **Live Server** de VS Code o cualquier servidor local.

---

## 📞 Contacto

**La Canasta Familiar**  
📍 Cl. 8 #79c-16, Bogotá, Colombia  
📞 320 8968237  
🌐 [fruverhome.co](https://fruverhome.co/)
