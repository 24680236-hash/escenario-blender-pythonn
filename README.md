# escenario-blender-pythonn
Generador de escenario 3D en Blender utilizando Python (bpy). El script crea automáticamente un pasillo con tramo recto y sección curva, aplica materiales con colores RGB, agrega iluminación y anima una cámara que recorre la escena mediante una trayectoria programada.
#  Generador de Escenario en Blender con Python

Este proyecto consiste en la creación automática de un escenario 3D utilizando Python dentro de Blender.  
El script genera un pasillo recto con una sección curva, aplica materiales con colores definidos, agrega iluminación y anima una cámara que recorre el escenario.

---

##  Tecnologías utilizadas

- Blender
- Python (API bpy)

---

##  Características del proyecto

✔ Generación automática de paredes  
✔ Tramo recto y tramo curvo  
✔ Aplicación de materiales con colores RGB  
✔ Iluminación con luz tipo SUN y POINT  
✔ Creación de curva como trayectoria de cámara  
✔ Animación automática de la cámara (200 frames)

---

##  Estructura del Escenario

El script realiza los siguientes pasos:

1. Limpia la escena actual.
2. Crea materiales con nodos (Principled BSDF).
3. Genera un pasillo recto con paredes alternadas.
4. Construye una sección curva utilizando funciones trigonométricas.
5. Agrega un suelo escalado.
6. Inserta luces con diferentes intensidades.
7. Crea una cámara animada que sigue una trayectoria curva.

---

##  Cómo ejecutar el proyecto

1. Abrir Blender.
2. Ir a la pestaña **Scripting**.
3. Crear un nuevo archivo de texto.
4. Copiar y pegar el código del proyecto.
5. Presionar **Run Script**.
6. Reproducir la animación desde el Timeline.

---

## Resultado

La cámara realiza un recorrido dinámico por el pasillo, mostrando la estructura recta y la sección curva, con iluminación aplicada y materiales visibles en modo Render.

---

## Objetivo Académico

Este proyecto demuestra el uso de:

- Programación estructurada en Python
- Uso de ciclos `for`
- Funciones matemáticas (`math.cos`, `math.sin`)
- Creación de materiales con nodos
- Uso de constraints (Follow Path, Track To)
- Animación mediante keyframes
- 
 ## PROGRAMA
import bpy
import math

def crear_material(nombre, color_rgb):
    if nombre in bpy.data.materials:
        return bpy.data.materials[nombre]
    mat = bpy.data.materials.new(name=nombre)
    mat.use_nodes = True
    bsdf = mat.node_tree.nodes["Principled BSDF"]
    bsdf.inputs['Base Color'].default_value = (*color_rgb, 1.0)
    bsdf.inputs['Roughness'].default_value = 0.7
    return mat

def generar_escenario():

    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete(use_global=False)

    mat_pared_a = crear_material("ParedOscura", (0.1, 0.1, 0.1))
    mat_pared_b = crear_material("ParedDetalle", (0.8, 0.2, 0.0))

    largo_pasillo = 10
    ancho_pasillo = 4
    radio_curva = 12

    # TRAMO RECTO
    for i in range(largo_pasillo):

        bpy.ops.mesh.primitive_cube_add(location=(-ancho_pasillo, i * 2, 1))
        pared_izq = bpy.context.active_object

        if i % 2 == 0:
            pared_izq.data.materials.append(mat_pared_a)
        else:
            pared_izq.data.materials.append(mat_pared_b)
            pared_izq.scale.z = 1.5

        bpy.ops.mesh.primitive_cube_add(location=(ancho_pasillo, i * 2, 1))
        pared_der = bpy.context.active_object
        pared_der.data.materials.append(mat_pared_a)

    # TRAMO CURVO
    cy = (largo_pasillo - 1) * 2
    cx = radio_curva

    for j in range(1, largo_pasillo + 1):

        angulo = math.pi - (j * (math.pi / 2) / largo_pasillo)
        rotacion_z = math.pi - angulo

        x_izq = cx + (radio_curva + ancho_pasillo) * math.cos(angulo)
        y_izq = cy + (radio_curva + ancho_pasillo) * math.sin(angulo)

        bpy.ops.mesh.primitive_cube_add(location=(x_izq, y_izq, 1),
                                        rotation=(0, 0, rotacion_z))
        pared_izq_curva = bpy.context.active_object

        if j % 2 == 0:
            pared_izq_curva.data.materials.append(mat_pared_a)
        else:
            pared_izq_curva.data.materials.append(mat_pared_b)
            pared_izq_curva.scale.z = 1.5

        x_der = cx + (radio_curva - ancho_pasillo) * math.cos(angulo)
        y_der = cy + (radio_curva - ancho_pasillo) * math.sin(angulo)

        bpy.ops.mesh.primitive_cube_add(location=(x_der, y_der, 1),
                                        rotation=(0, 0, rotacion_z))
        pared_der_curva = bpy.context.active_object
        pared_der_curva.data.materials.append(mat_pared_a)

    # SUELO
    bpy.ops.mesh.primitive_plane_add(size=1,
                                     location=(radio_curva/2, cy/2 + radio_curva/2, 0))
    suelo = bpy.context.active_object
    suelo.scale.x = (ancho_pasillo * 2) + radio_curva + 10
    suelo.scale.y = (largo_pasillo * 2) + radio_curva + 10

    # LUCES
    bpy.ops.object.light_add(type='SUN',
                             location=(10, 10, 10),
                             rotation=(math.radians(-45), math.radians(30), 0))
    bpy.context.active_object.data.energy = 3

    bpy.ops.object.light_add(type='POINT', location=(0, 5, 4))
    bpy.context.active_object.data.energy = 500

    bpy.ops.object.light_add(type='POINT',
                             location=(radio_curva, cy + radio_curva/2, 4))
    bpy.context.active_object.data.energy = 800

    # CÁMARA
    bpy.ops.object.camera_add(location=(0, -6, 2))
    camera = bpy.context.active_object
    bpy.context.scene.camera = camera

    # CURVA CAMINO
    curve_data = bpy.data.curves.new('CamPathData', type='CURVE')
    curve_data.dimensions = '3D'
    spline = curve_data.splines.new('POLY')

    puntos_camino = [(0, -6, 1.5), (0, cy, 1.5)]

    pasos_curva = 20
    for f in range(1, pasos_curva + 1):
        progreso = f / float(pasos_curva)
        angulo = math.pi - (progreso * (math.pi / 2))
        px = cx + radio_curva * math.cos(angulo)
        py = cy + radio_curva * math.sin(angulo)
        puntos_camino.append((px, py, 1.5))

    spline.points.add(len(puntos_camino) - 1)

    for i, pt in enumerate(puntos_camino):
        spline.points[i].co = (*pt, 1.0)

    cam_path = bpy.data.objects.new('Cam_Path', curve_data)
    bpy.context.scene.collection.objects.link(cam_path)

    # CONSTRAINTS
    follow_path = camera.constraints.new(type='FOLLOW_PATH')
    follow_path.target = cam_path
    follow_path.use_fixed_location = True
    follow_path.forward_axis = 'FORWARD_Y'
    follow_path.up_axis = 'UP_Z'

    bpy.ops.object.empty_add(type='PLAIN_AXES', location=(0, 0, 1.5))
    cam_target = bpy.context.active_object

    fp_target = cam_target.constraints.new(type='FOLLOW_PATH')
    fp_target.target = cam_path
    fp_target.use_fixed_location = True

    track_to = camera.constraints.new(type='TRACK_TO')
    track_to.target = cam_target
    track_to.track_axis = 'TRACK_NEGATIVE_Z'
    track_to.up_axis = 'UP_Y'

    # ANIMACIÓN
    bpy.context.scene.frame_start = 1
    bpy.context.scene.frame_end = 200

    follow_path.offset_factor = 0.0
    fp_target.offset_factor = 0.05
    camera.keyframe_insert(data_path=f'constraints["{follow_path.name}"].offset_factor', frame=1)
    cam_target.keyframe_insert(data_path=f'constraints["{fp_target.name}"].offset_factor', frame=1)

    follow_path.offset_factor = 1.0
    fp_target.offset_factor = 1.0
    camera.keyframe_insert(data_path=f'constraints["{follow_path.name}"].offset_factor', frame=200)
    cam_target.keyframe_insert(data_path=f'constraints["{fp_target.name}"].offset_factor', frame=200)

    bpy.context.view_layer.update()

generar_escenario()
---
![WhatsApp Image 2026-02-25 at 23 10 04](https://github.com/user-attachments/assets/174b1e19-f031-4181-a799-338fe3139a76)
## VIDEO 

