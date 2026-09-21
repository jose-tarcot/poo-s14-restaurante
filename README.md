# restaurante_app — Semana 14

**Estudiante:** José Alberto Tarco Tipán  
**Materia:** Programación Orientada a Objetos  
**Institución:** Universidad Estatal Amazónica  
**Entrega:** Semana 14 — Componentes y contenedores

---

## 1. Descripción general

Esta entrega evoluciona `restaurante_app` (Parrilla del Valle) a partir de la interfaz gráfica construida en la Semana 13. El objetivo central es aplicar correctamente **componentes y contenedores de Tkinter/ttk** para transformar la sección del menú en una gestión completa: registro, consulta, actualización y eliminación de platos, manteniendo la arquitectura modular y la persistencia en JSON ya existentes.

---

## 2. Estructura del proyecto

```
restaurante_app/
├── datos/
│   ├── productos.json
│   └── usuarios.json
├── modelos/
│   ├── __init__.py
│   ├── producto.py
│   └── usuario.py
├── servicios/
│   ├── __init__.py
│   ├── archivo_servicio.py
│   └── restaurante_servicio.py
├── ui/
│   ├── __init__.py
│   ├── login_view.py
│   └── main_view.py
└── main.py
```

---

## 3. Responsabilidad de cada capa

| Capa | Responsabilidad |
|---|---|
| `modelos/` | `Producto` (con tiempo de preparación) y `Usuario` (con mesa asignada), validados con `property`. |
| `servicios/archivo_servicio.py` | Lee y escribe los archivos JSON de `datos/`. |
| `servicios/restaurante_servicio.py` | Convierte los datos en objetos, valida el acceso y expone el CRUD completo de platos (`registrar_producto`, `buscar_producto_por_codigo`, `actualizar_producto`, `eliminar_producto`, `guardar_productos`). |
| `ui/` | `LoginView` y `MainView`, construidas con Tkinter; solicitan las operaciones a `RestauranteServicio` sin tocar el JSON directamente. |
| `main.py` | Crea la única ventana principal y controla el cambio entre vistas. |

---

## 4. Componentes y contenedores utilizados

- `Frame`: separa encabezado, barra de navegación, contenido y barra de estado.
- `LabelFrame`: agrupa el formulario ("Datos del plato") y el listado ("Platos del menu" / "Consulta de clientes").
- `Entry`: código, nombre y precio del plato, y credenciales de acceso.
- `ttk.Combobox` (solo lectura): selector de categoría restringido a `PARRILLA`, `MARISCOS`, `VEGETARIANO`, `PASTA`, `BEBIDA`.
- `ttk.Spinbox`: selectores numéricos para el tiempo de preparación (minutos) y el stock del plato.
- `ttk.Treeview` + `ttk.Scrollbar`: tablas de platos y clientes, con desplazamiento vertical.
- `ttk.Button` / `ttk.Style`: botones de acción (`Registrar`, `Cargar / Consultar`, `Actualizar`, `Eliminar`, `Limpiar`) conectados mediante `command=`.
- Gestores de geometría: `pack()` para la estructura general y `grid()` dentro del formulario y del bloque formulario/listado.

---

## 5. Operaciones implementadas sobre el menú

| Operación | Acción en la interfaz | Método en `RestauranteServicio` |
|---|---|---|
| Registrar | Botón **Registrar** | `registrar_producto(codigo, nombre, precio, categoria, tiempo_preparacion, stock)` |
| Consultar | Botón **Cargar / Consultar** | `buscar_producto_por_codigo(codigo)` |
| Actualizar | Botón **Actualizar** | `actualizar_producto(codigo, nombre, precio, categoria, tiempo_preparacion, stock)` |
| Eliminar | Botón **Eliminar** | `eliminar_producto(codigo)` |

Todas las validaciones (campos vacíos, precio o tiempo no numérico, categoría inválida, código duplicado) se resuelven en `Producto` y `RestauranteServicio`; la interfaz solo captura los datos y muestra el resultado con `messagebox`.

---

## 6. Persistencia

Los cambios sobre el menú se guardan de inmediato en `datos/productos.json` mediante `RestauranteServicio.guardar_productos()`, que delega en `ArchivoServicio`. Al reabrir la aplicación, los platos modificados se conservan.

---

## 7. Flujo de la aplicación

```
Inicio de la aplicacion
        ↓
main.py prepara Tkinter y los servicios
        ↓
LoginView (usuario y clave del cliente)
        ↓
RestauranteServicio valida el acceso
        ↓
MainView
        ↓
Inicio (resumen) | Usuarios (clientes) | Productos (formulario + tabla del menu)
        ↓
Registrar | Cargar/Consultar | Actualizar | Eliminar
        ↓
RestauranteServicio procesa la operacion y persiste en productos.json
        ↓
Actualizacion de la tabla y de la barra de estado
```

---

## 8. Credenciales de acceso (demostración)

| Usuario | Contraseña |
|---|---|
| `jtarco` | `grill2026` |
| `admin` | `admin456` |

La contraseña debe incluir al menos un número, según la validación del modelo `Usuario`.

---

## 9. Ejecución

```bash
cd restaurante_app
python main.py
```

Requiere **Python 3.10 o superior** y Tkinter disponible en la instalación.

---

## 10. Pruebas realizadas

1. Se ejecuta `main.py` y la aplicación inicia sin errores.
2. El acceso mediante usuario y contraseña continúa funcionando.
3. La interfaz principal se muestra correctamente después del acceso.
4. La opción **Usuarios** permite consultar los clientes en una tabla, incluyendo su mesa.
5. La opción **Productos** presenta un formulario organizado con componentes y contenedores.
6. Se registra un nuevo plato y aparece de inmediato en la tabla del menú.
7. Se carga/consulta un plato existente mediante su código.
8. Se actualiza la información de un plato y los cambios se conservan.
9. Se elimina un plato y deja de aparecer en la tabla.
10. Las modificaciones se mantienen después de cerrar y volver a ejecutar la aplicación.
11. La interfaz solicita las operaciones a `RestauranteServicio` y no manipula directamente `productos.json`.
12. La navegación y los controles resultan claros: barra superior, formulario, tabla y barra de estado.

---

## 11. Nota educativa sobre autenticación

El acceso de esta etapa es una simulación pedagógica. Las contraseñas se guardan en JSON en texto plano solo con fines didácticos; no representa una práctica segura para un sistema real.
