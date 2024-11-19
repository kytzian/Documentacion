# Documentación del Proyecto: Aplicación MVC para Consultas en una Vista

## Descripción General

Esta aplicación utiliza el patrón de diseño **Modelo-Vista-Controlador (MVC)** para consultar y mostrar datos almacenados en una base de datos. La funcionalidad principal incluye:

1. **Búsqueda por Categorías**: Permite al usuario consultar registros según una categoría específica.
2. **Búsqueda por Lenguajes de Programación**: Muestra registros asociados a categorías marcadas como "LENGUAJE".
3. **Búsqueda por Palabras Clave**: Filtra registros cuyos títulos o enlaces contengan palabras específicas.

## Componentes de la Aplicación

### 1. El Modelo

El archivo `ModelBBDD.php` contiene la clase que maneja la conexión a la base de datos y las consultas SQL.

**Responsabilidades:**

- Conectar a la base de datos utilizando MySQLi.
- Ejecutar consultas para buscar datos basados en los parámetros proporcionados.
- Devolver los resultados como un arreglo asociativo.

**Funciones Principales:**

- `buscarPorCategoria($categoria)`: Filtra registros por una categoría específica.
- `buscarPorTipo($tipo)`: Devuelve registros clasificados según un tipo dado (por ejemplo, "LENGUAJE").
- `buscarPorPalabraClave($palabra)`: Busca registros que contengan una palabra clave en el título o enlace.

### 2. El Controlador

El archivo `controller.php` conecta el modelo con la vista y gestiona las solicitudes del usuario.

**Responsabilidades:**

- Determinar la acción solicitada por el usuario (a través de `$_GET`).
- Llamar a las funciones correspondientes del modelo para obtener los datos.
- Pasar los datos recuperados a la vista para su renderizado.

**Acciones Soportadas:**

- `buscarCategoria`: Busca registros según la categoría.
- `buscarLenguaje`: Muestra todos los registros clasificados como "LENGUAJE".
- `buscarPalabra`: Busca registros por palabras clave en el título o enlace.

### 3. La Vista

El archivo `vista.php` presenta los datos recuperados al usuario y contiene formularios para realizar búsquedas.

**Características:**

- Formulario para buscar por categoría.
- Botón para buscar registros clasificados como "LENGUAJE".
- Formulario para buscar por palabras clave.
- Tabla para mostrar los resultados de las búsquedas.

**Diseño de la Interfaz:**

- **Formulario de búsqueda por categoría**: Permite al usuario introducir el nombre de una categoría para filtrar los datos.
- **Botón de lenguajes**: Realiza una búsqueda rápida de registros clasificados como lenguajes.
- **Formulario de búsqueda por palabras clave**: Encuentra registros basándose en palabras clave.

**Flujo de Trabajo:**

1. **Inicio**: El usuario accede a `index.php`.
2. **Interacción del Usuario**: Elige una acción (buscar por categoría, buscar lenguajes o buscar palabras clave).
3. **Lógica del Controlador**:
    - Recoge los datos del formulario o parámetros de la URL.
    - Llama al modelo para realizar la consulta correspondiente.
    - Recibe los resultados y los pasa a la vista.
4. **Presentación de Resultados**: La vista renderiza los datos en una tabla.