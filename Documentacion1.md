# Documentación del Objeto `ModelBBDD`

## Descripción General

El objeto `ModelBBDD` es una clase PHP encargada de gestionar la conexión a una base de datos MySQL, ejecutar consultas SQL y recuperar los resultados para ser utilizados en la vista de la aplicación. Esta clase facilita la interacción con la base de datos y garantiza que la conexión sea segura y que los datos se gestionen correctamente.

La clase se conecta a una base de datos llamada `enlaces` y ofrece un método específico para obtener los datos de una vista llamada `vista_enlaces`. Estos datos se muestran en una tabla HTML en la interfaz de usuario.

## Código de la Clase `ModelBBDD`

### Atributos

1. **`$host`**: Cadena que contiene el nombre del servidor de la base de datos. En este caso, está configurado como `'localhost'`.
2. **`$user`**: Nombre de usuario para la base de datos. En este caso, es `'root'`.
3. **`$password`**: Contraseña de la base de datos. En este caso, está vacía.
4. **`$database`**: Nombre de la base de datos a la que se conecta. En este caso, se conecta a la base de datos llamada `'enlaces'`.
5. **`$connection`**: Objeto que almacena la conexión a la base de datos.

### Constructor

```php
public function __construct() {
    $this->connection = new mysqli($this->host, $this->user, $this->password, $this->database);
    
    // Verificar la conexión
    if ($this->connection->connect_error) {
        die("Error de conexión: " . $this->connection->connect_error);
    }
}
El constructor de la clase establece la conexión a la base de datos usando las credenciales y parámetros definidos en los atributos. Si la conexión falla, el script termina con un mensaje de error que describe el problema.

Métodos
getVistaEnlaces()
php

public function getVistaEnlaces() {
    $query = "SELECT * FROM vista_enlaces";
    $result = $this->connection->query($query);

    // Verificar si la consulta fue exitosa
    if (!$result) {
        die("Error en la consulta: " . $this->connection->error);
    }

    // Almacenar los resultados en un array
    $data = [];
    while ($row = $result->fetch_assoc()) {
        $data[] = $row;
    }

    return $data;
}


Descripción:

Este método ejecuta una consulta SQL para obtener todos los registros de la vista vista_enlaces. Utiliza el método query() del objeto de conexión para ejecutar la consulta. Si la consulta es exitosa, almacena los resultados en un arreglo asociativo y los devuelve.

Entrada: No recibe parámetros.
Salida: Un arreglo asociativo con los registros obtenidos de la vista vista_enlaces.
Flujo:

Se ejecuta la consulta SELECT * FROM vista_enlaces.
Si la consulta falla, se muestra un mensaje de error.
Si la consulta es exitosa, los resultados se almacenan en un arreglo $data.
El arreglo $data se devuelve con todos los registros obtenidos.

closeConnection()
php

public function closeConnection() {
    $this->connection->close();
}
Descripción:

Este método cierra la conexión a la base de datos al finalizar las operaciones. Es importante llamar a este método para liberar los recursos utilizados por la conexión una vez que se hayan obtenido los datos.

Entrada: No recibe parámetros.
Salida: Ninguna (la conexión a la base de datos se cierra).
Uso del Objeto ModelBBDD en la Vista
Código PHP en la Vista
php

// Uso del objeto ModelBBDD para mostrar los datos
$model = new ModelBBDD();
$vistaEnlaces = $model->getVistaEnlaces();
En el archivo que se encarga de la presentación (vista), se crea una instancia del objeto ModelBBDD y se llama al método getVistaEnlaces() para recuperar los datos de la base de datos. Los resultados se almacenan en la variable $vistaEnlaces.

Código HTML para Mostrar los Datos
html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vista de Enlaces</title>
    <style>
        table {
            border-collapse: collapse;
            width: 100%;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: left;
        }
        th {
            background-color: #f2f2f2;
        }
    </style>
</head>
<body>
    <h1>Enlaces y Categorías</h1>
    <table>
        <thead>
            <tr>
                <th>ID Vínculo</th>
                <th>Enlace</th>
                <th>Título</th>
                <th>Categoría</th>
                <th>Tipo</th>
            </tr>
        </thead>
        <tbody>
            <?php if (!empty($vistaEnlaces)) : ?>
                <?php foreach ($vistaEnlaces as $enlace) : ?>
                    <tr>
                        <td><?= $enlace['pk_vinculo']; ?></td>
                        <td><a href="<?= $enlace['enlace']; ?>" target="_blank"><?= $enlace['enlace']; ?></a></td>
                        <td><?= $enlace['titulo']; ?></td>
                        <td><?= $enlace['categoria']; ?></td>
                        <td><?= $enlace['tipo']; ?></td>
                    </tr>
                <?php endforeach; ?>
            <?php else : ?>
                <tr>
                    <td colspan="5">No hay datos disponibles</td>
                </tr>
            <?php endif; ?>
        </tbody>
    </table>
</body>
</html>

Descripción:

En el archivo HTML, los datos recuperados se muestran en una tabla. La tabla tiene cinco columnas: ID del vínculo, Enlace, Título, Categoría y Tipo. Cada registro obtenido se muestra en una fila de la tabla.

PHP en la vista: Si la variable $vistaEnlaces no está vacía, se recorre el arreglo y se muestra cada enlace con su respectiva información. Si la variable está vacía, se muestra un mensaje indicando que no hay datos disponibles.
HTML: La tabla se estructura con un encabezado que define las columnas y un cuerpo que contiene las filas generadas por el código PHP.
Cierre de la Conexión
php

// Cerrar la conexión a la base de datos
$model->closeConnection();

Al final de la ejecución del script, se cierra la conexión a la base de datos utilizando el método closeConnection(), lo que garantiza que los recursos se liberen adecuadamente.

Flujo Completo de Datos
El controlador o archivo principal crea una instancia de la clase ModelBBDD.
Se llama al método getVistaEnlaces() para obtener los datos de la base de datos.
Los datos se pasan a la vista y se presentan en una tabla HTML.
Al finalizar, se cierra la conexión a la base de datos.
Resumen
La clase ModelBBDD facilita la gestión de la conexión a la base de datos y la obtención de los datos necesarios para ser mostrados en la vista. A través de su método getVistaEnlaces(), se realiza una consulta para obtener todos los registros de la vista vista_enlaces y se retornan en un formato adecuado para ser procesados por la vista. Finalmente, el método closeConnection() garantiza que la conexión a la base de datos se cierre correctamente después de que se han obtenido y mostrado los datos.

Esta documentación cubre todos los aspectos del objeto `ModelBBDD`, desde su conexión a la base de datos hasta el uso en la vista. También se detallan las responsabilidades de cada método y la estructura general del flujo de datos.
