```markdown
# Plan Detallado para el Sistema de Calculadora y Gestión de Productos

Este plan abarca la implementación de un sistema que permita calcular el precio de un producto según parámetros modificables y gestionar el registro/editado de productos (código y nombre) en dos listas (minorista y mayorista), con capacidad de descargar los datos.

---

## 1. Archivo: src/lib/utils.ts

- **Cambios**:
  - Agregar una función `calculateFinalPrice` que reciba los siguientes parámetros:
    - costoInicial (número)
    - ganancia (número)
    - gastos (porcentaje, número)
    - publicidad (porcentaje, número)
    - ingresosBrutos (porcentaje, número)
    - costoFijoML (número)
    - mercadolibre (porcentaje, número)
    - costoFinanciero (porcentaje, número)
    - pagoNube (porcentaje, número)
    - iva (porcentaje, número) — (para calcular "5%+IVA")
    - comisionTiendaNube (porcentaje, número)
  - Calcular la suma de porcentajes:
    - `sumaPorcentajes = (gastos + publicidad + ingresosBrutos + mercadolibre + costoFinanciero + (pagoNube * (1 + iva/100)) + comisionTiendaNube) / 100`
  - Resolver la fórmula:
    - Se requiere que:  
      `P - (sumaPorcentajes * P + costoFijoML) = costoInicial + ganancia`
      por lo que:  
      `P = (costoInicial + ganancia + costoFijoML) / (1 - sumaPorcentajes)`
  - Incluir manejo de error si el denominador es ≤ 0.
  - Exportar la función para su uso en otros componentes.

---

## 2. Archivo: src/components/CalculatorForm.tsx

- **Funcionalidad**:  
  Creación de un formulario para ingresar los parámetros de cálculo con campos numéricos controlados (usando useState) y valores por defecto:
  - Costo Inicial (defecto: 4800)
  - Gastos (10%)
  - Ganancia (3500)
  - Publicidad (10%)
  - Ingresos Brutos (5%)
  - Costo Fijo ML (1800)
  - MercadoLibre (16%)
  - Costo Financiero (5%)
  - Pago Nube (5%)
  - IVA (configurable, por defecto 21)
  - Comisión Tienda Nube (5%)
- **Cambios**:
  - OnClick del botón "Calcular Precio", invocar `calculateFinalPrice` de utils.
  - Mostrar el precio final calculado en un campo de solo lectura o en un mensaje destacado.
  - Validar que los inputs sean numéricos; en caso de error, desplegar mensajes claros.

---

## 3. Archivo: src/components/ProductForm.tsx

- **Funcionalidad**:  
  Formulario para registrar un artículo con campos:
  - Código (texto)
  - Nombre (texto)
- **Cambios**:
  - Implementar validación de campos obligatorios.
  - Enviar los datos al componente padre mediante una función callback (prop) para agregarlos a la lista.
  - Mostrar mensajes de error o de éxito según la respuesta del usuario (ejemplo: “Producto registrado”).

---

## 4. Archivo: src/components/ProductList.tsx

- **Funcionalidad**:  
  Mostrar dos listas de productos: una para “Minorista” y otra para “Mayorista”.
- **Cambios**:
  - Usar un sistema de pestañas (tabs) para alternar entre las dos listas sin usar iconos externos.
  - Incluir una barra de búsqueda para filtrar rápidamente los productos por código o nombre.
  - Cada producto se mostrará en una tarjeta o fila con sus datos básicos y un botón de “Editar”.
  - Permitir edición en línea o mediante un modal (usar un modal simple creado con divs y estilos de Tailwind) para actualizar el precio y otros valores si es necesario.
  - Implementar un botón “Exportar Listado” que genere un archivo CSV/JSON de forma sencilla.

---

## 5. Archivo: src/app/page.tsx

- **Funcionalidad**:  
  Página principal que integra la calculadora y la gestión de productos.
- **Cambios**:
  - Importar y renderizar los componentes `CalculatorForm`, `ProductForm` y `ProductList` en secciones diferenciadas.
  - Diseñar la interfaz con una jerarquía visual clara:  
    - Encabezado con el título "Sistema de Calculadora y Gestión de Productos".  
    - Sección de "Calculadora de Precios" seguida de la herramienta de registro y listado de productos.
  - Asegurar que el diseño sea moderno y responsivo utilizando Tailwind CSS (espaciados, tipografía, colores y márgenes).

---

## Consideraciones de Manejo de Errores y Buenas Prácticas

- Se validarán todos los campos antes de realizar cálculos o registros.
- Uso de componentes controlados y estados de error para entradas no válidas.
- Código modular y reutilizable, separando la lógica de negocio (utils) de la presentación.
- Se documentarán funciones y componentes con comentarios breves para facilitar mantenimiento.

---

## Pruebas y Validaciones

- Realizar pruebas manuales en el navegador para comprobar que:
  - La calculadora muestra el precio correcto y maneja errores si la suma de porcentajes es excesiva.
  - La creación, edición y búsqueda de productos funcionan sin problemas.
  - El botón de exportación genera un archivo descargable con los datos correctos.
- Verificar que la UI mantenga su estilo limpio y moderno sin dependencias de iconos o imágenes externas no permitidas.

---

## Resumen

- Se crea la función `calculateFinalPrice` en `src/lib/utils.ts` para los cálculos con manejo de errores.
- Se implementa el formulario de calculadora en `CalculatorForm.tsx` con parámetros modificables y validación.
- Se diseña el `ProductForm.tsx` para registrar productos (código y nombre) y se integra en el sistema.
- Se desarrolla `ProductList.tsx` con pestañas para Minorista y Mayorista, búsqueda y edición en línea.
- La página principal en `src/app/page.tsx` orquesta el sistema con un diseño moderno y responsivo.
- Se considera la exportación de datos y se mantienen buenas prácticas y control de errores en toda la aplicación.
