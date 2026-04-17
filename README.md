# 🕵️‍♂️ Agente Secreto DOM - Sistema de Recompensas Interactivo

Este proyecto implementa un **agente dinámico en el DOM** que monitorea el comportamiento del usuario en tiempo real y desbloquea una recompensa especial cuando se cumplen ciertas condiciones.

---

## 🎯 Objetivo

Activar automáticamente un **descuento del 30%** cuando:

- El usuario interactúa con la plataforma
- Completa acciones específicas
- Alcanza un monto mínimo de compra

---

## ⚙️ ¿Cómo funciona?

El script utiliza:

- `MutationObserver` para detectar cambios en el DOM
- Sobrescritura de funciones (`override`)
- Monitoreo continuo con `setInterval`
- Manipulación dinámica del DOM

---

## ✅ Condiciones del Sistema

Para activar el **Agente DOM**, se deben cumplir las siguientes 3 condiciones:

### 1️⃣ Autenticación del usuario
El usuario debe:
- Iniciar sesión o
- Registrarse en la plataforma

📌 Se detecta cuando:
- Aparece el botón `#botonLogout`
- O el saludo `#saludoUsuario`

---

### 2️⃣ Completar el mini-juego 🎮
El usuario debe completar el juego de frutas.

📌 Se valida cuando:
```js
frutas.every(f => f.enCanasta)
```

---

### 3️⃣ Carrito con mínimo 3 productos 🛒
El usuario debe agregar al menos 3 productos.

📌 Se detecta mediante:
```js
window._carrito.length >= 3
```

---

## 💰 Condición adicional para descuento

Además de completar las 3 condiciones:

👉 El total de la compra debe ser mayor o igual a **$150.000**

---

## 🎁 Recompensa

Si todas las condiciones se cumplen:

- 🎉 Se activa el **Agente DOM**
- 🎨 Se modifica el diseño de la página
- 💎 Se muestra un mensaje especial
- 💰 Se aplica:

```
30% DE DESCUENTO
```

---

## 🧠 Lógica del Descuento

El sistema:

1. Calcula el total del carrito
2. Asigna precios estimados por producto
3. Aplica:

```js
descuento = total * 0.30
```

4. Muestra:
- Total original
- Ahorro
- Total final

---

## 🚀 Activación automática

El sistema se inicializa con:

```js
document.addEventListener('DOMContentLoaded', ...)
```

Y monitorea constantemente:

```js
setInterval(verificarMisionCompletada, 500);
```

---

## 🧪 Debug en consola

Se muestra información en tiempo real:

- Estado de cada condición
- Progreso general
- Total del carrito
- Estado del descuento

---

## 📦 Requisitos

Este script depende de:

### HTML
- `#botonLogout`
- `#saludoUsuario`
- `#botonesAuth`
- `#miCanvas`

### Variables globales
- `window._carrito`
- `window.frutas`

### Funciones existentes
- `dibujarCanvas()`
- `renderCarrito()`

---

## ⚠️ Notas importantes

- El script sobrescribe funciones existentes
- Depende de la estructura del DOM
- Diseñado para gamificación en frontend

---

## 💡 Posibles mejoras

- Integrar backend real para precios
- Guardar progreso del usuario
- Mejorar animaciones
- Sistema de recompensas escalables

---

## 👨‍💻 Autor

Proyecto enfocado en:
- Manipulación del DOM
- Experiencia de usuario (UX)
- Gamificación en e-commerce

---

✨ Convierte la interacción del usuario en una misión.
