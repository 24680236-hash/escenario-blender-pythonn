# escenario-blender-pythonn
Generador de escenario 3D en Blender utilizando Python (bpy). El script crea automáticamente un pasillo con tramo recto y sección curva, aplica materiales con colores RGB, agrega iluminación y anima una cámara que recorre la escena mediante una trayectoria programada.
# 🎬 Generador de Escenario en Blender con Python

Este proyecto consiste en la creación automática de un escenario 3D utilizando Python dentro de Blender.  
El script genera un pasillo recto con una sección curva, aplica materiales con colores definidos, agrega iluminación y anima una cámara que recorre el escenario.

---

## 🛠 Tecnologías utilizadas

- Blender
- Python (API bpy)

---

## 🎨 Características del proyecto

✔ Generación automática de paredes  
✔ Tramo recto y tramo curvo  
✔ Aplicación de materiales con colores RGB  
✔ Iluminación con luz tipo SUN y POINT  
✔ Creación de curva como trayectoria de cámara  
✔ Animación automática de la cámara (200 frames)

---

## 📌 Estructura del Escenario

El script realiza los siguientes pasos:

1. Limpia la escena actual.
2. Crea materiales con nodos (Principled BSDF).
3. Genera un pasillo recto con paredes alternadas.
4. Construye una sección curva utilizando funciones trigonométricas.
5. Agrega un suelo escalado.
6. Inserta luces con diferentes intensidades.
7. Crea una cámara animada que sigue una trayectoria curva.

---

## 🚀 Cómo ejecutar el proyecto

1. Abrir Blender.
2. Ir a la pestaña **Scripting**.
3. Crear un nuevo archivo de texto.
4. Copiar y pegar el código del proyecto.
5. Presionar **Run Script**.
6. Reproducir la animación desde el Timeline.

---

## 🎥 Resultado

La cámara realiza un recorrido dinámico por el pasillo, mostrando la estructura recta y la sección curva, con iluminación aplicada y materiales visibles en modo Render.

---

## 📚 Objetivo Académico

Este proyecto demuestra el uso de:

- Programación estructurada en Python
- Uso de ciclos `for`
- Funciones matemáticas (`math.cos`, `math.sin`)
- Creación de materiales con nodos
- Uso de constraints (Follow Path, Track To)
- Animación mediante keyframes

---

## 👩‍💻 Autor

Wendy Sánchez
