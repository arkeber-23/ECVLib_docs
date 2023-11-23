
## Documentación ECVLib
### Inicio rápido
Para comenzar a utilizar esta librería, descarga el código fuente y, si cuentas con Git, puedes clonarlo mediante el siguiente comando en tu terminal.
```bash
git clone https://github.com/arkeber-23/ECVLib.git
cd ECVLib
php thor serve
```
La instrucción `php thor serve` inicia un servidor.

### Configuración Inicial

####  Configuración de variables de entorno en ECVLib: 

- Ajusta la configuración de tu aplicación según el entorno local o de producción mediante el archivo .env en la raíz del proyecto ECVLib. Este archivo almacena configuraciones clave, incluyendo detalles de conexión a la base de datos, como credenciales y motor (actualmente, solo se admite MySQL)."

```code
DB_DRIVER=
HOST=
PORT=
DB_NAME= 
DB_USER= 
DB_PASSWORD=
DB_CHARSET=
```

- La información de configuración para la librería se almacena en el directorio `ECVLib/Kernel/config/Config.php`. Aunque ECVLib generalmente requiere muy poca configuración adicional, en caso de que necesites cambiar la zona horaria (`timezone`), puedes hacerlo en el archivo **`index.php`** que se encuentra en la raíz del proyecto.

 ```php
new Config('America/Guayaquil', __DIR__ . '/.env');
```
## Lo Básico

### Thor - Generador de Archivos en ECVLib

Thor es una herramienta de línea de comandos que simplifica la generación de archivos en ECVLib.

#### Comandos Thor

- **Crear Modelo, Controlador y Migración:**
    ```bash
    php thor create::all <name>
    ```
    Crea un archivo de modelo, controlador y migración en ECVLib. Este comando agiliza la creación de componentes esenciales para tu aplicación.

- **Crear Modelo:**
    ```bash
    php thor create::model <name>
    ```
    Crea un archivo de modelo en ECVLib. Los modelos son componentes fundamentales para interactuar con la base de datos y representan la estructura de datos de tu aplicación.

- **Crear Controlador:**
    ```bash
    php thor create::controller <name>
    ```
    Crea un archivo de controlador en ECVLib. Los  controladores gestionan la lógica de la aplicación y manejan las interacciones entre el modelo y la vista.

- **Ayuda:**
    ```bash
    php thor -h
    ```
    El comando php thor -h se utiliza para mostrar la ayuda relacionada con el Command-Line Interface (CLI) de la herramienta Thor en ECVLib.

## Rutas
Para gestionar las rutas en tu aplicación, utiliza el archivo **`web.php`** ubicado en la carpeta `src/Routes`. Siéntete libre de crear otros archivos según tus necesidades para mantener una organización clara y estructurada.
  - Metodos soportados
    - *Get*, *Post*, *Delete*, *Patch*,* Options*   

  - ### Ejemplo:
    - **Ruta Básica**
    [![ruta-basica.png](https://i.postimg.cc/kMznB5n5/ruta-basica.png)](https://postimg.cc/XXKMP4BM)

    - **Llamando a un controlador**
    [![ruta-controller.png](https://i.postimg.cc/P5h0xCRY/ruta-controller.png)](https://postimg.cc/YhndDCkC)

    - **Rutas con Parámetros**
    Para obtener o establecer parámetros en tu aplicación, sigue la siguiente convención de URL. Consideremos la ruta base:
    ```php
    https://localhost:8080/home
    ```
    Esto corresponde a la ruta que has creado. Para asignar parámetros, agrega el signo de interrogación `?` seguido del nombre del parámetro y su valor. Por ejemplo, para asignar el parámetro ***name*** con el valor ***arkeber***, la URL se vería así:
    ```php
    https://localhost:8080/home?name=arkeber
    ```
    Si deseas añadir más parámetros, utiliza el símbolo `&` para separarlos. Por ejemplo, para agregar el parámetro ***lastname*** con el valor ***Cabezas***:
    ```php
    https://localhost:8080/home?name=arkeber&lastname=Cabezas
    ```
    - **Obtener Parámetros:**
    Para recuperar parámetros en tu aplicación, siguiendo los   ejemplos proporcionados previamente, es esencial invocar la clase **Request**. Suponiendo que ***$req*** es una instancia de esta clase, el proceso es el siguiente:

    [![ruta-obtener-parametros.png](https://i.postimg.cc/j2J8jTTD/ruta-obtener-parametros.png)](https://postimg.cc/T5fjkZpx)

Estos ejemplos permiten recuperar los valores de los parámetros *name* y *lastname* de manera uniforme, ya sea que los datos se envíen a través del método ***GET o POST***. Importante destacar que, cuando trabajas con el método POST, no es necesario construir la URL de la misma manera que se explicó anteriormente para el método **GET**. Los parámetros en una solicitud **POST** se envían de manera diferente y se accede a ellos de manera de la misma manera.

- **Grupo de Rutas:**

  Para organizar y agrupar rutas en tu aplicación, se utiliza el método prefix del router. Dentro de esta función, puedes crear rutas adicionales que solo funcionarán con ese prefijo específico.
  - **Ejemplo**
    [![ruta-grupo.png](https://i.postimg.cc/WtLrjFTH/ruta-grupo.png)](https://postimg.cc/PvyNQqLW)

- **Rutas con Middleware:**
  En el contexto de una aplicación web, las "rutas con middleware" se refieren a la capacidad de aplicar funciones intermedias específicas a una ruta particular. Los middlewares son capas intermedias de lógica que se ejecutan antes o después del controlador principal de una ruta. Estos pueden realizar tareas como la autenticación, autorización, manipulación de solicitudes o respuestas, entre otros.
    - Antes de agregar un middleware a tu ruta, primero debes generarlo. Para crear un middleware, sigue estos pasos:
    ```php
    php thor create::middleware name_middle
    ```
    Este comando generará un middleware con el nombre especificado, proporcionando una estructura base para que puedas agregar la lógica necesaria en el manejo de las solicitudes.

   - Al generar tu middleware, tendrás acceso a tres funciones fundamentales: `handleInput, process, y handleOutput`.

      - La función **handleInput**: Se ejecuta antes de la función process. Aquí puedes realizar operaciones previas al procesamiento principal del middleware, como la validación de datos de entrada.

      - La función **process**: Es donde implementas la lógica central de tu middleware. Esta función se ejecuta durante el flujo normal del middleware y realiza las acciones deseadas, como autenticación, autorización o manipulación de la solicitud.

      - La función **handleOutput**: Se ejecuta después de que la solicitud ha sido procesada. Aquí puedes realizar operaciones finales, manipular la respuesta o realizar acciones posteriores a la ejecución principal del middleware.
  - **Agregar Middleware a la Ruta**
    [![ruta-middleware.png](https://i.postimg.cc/NjsgP6HP/ruta-middleware.png)](https://postimg.cc/kBH32twx)

## Clase Request

La clase Request se encarga de manejar las solicitudes HTTP entrantes en ECVLib. Proporciona métodos para acceder a información relevante sobre la solicitud, como el método HTTP, la URL, los parámetros y los encabezados. La clase Request es esencial para interactuar con los datos de la solicitud y realizar acciones específicas en respuesta a las solicitudes del cliente en tu aplicación ECVLib.

  - **Metodos**
    - **`getHeaders()`**: Este método devuelve un array que contiene todos los encabezados de la solicitud.

    - **`inputHeader($name)`**: Permite obtener el valor de un encabezado específico proporcionando su nombre como parámetro. Retorna el valor del encabezado o un mensaje indicando que el encabezado no se encontró.

    - **`getContentType()`**: Retorna el tipo de contenido de la solicitud, proporcionando información sobre el formato en que se envían los datos (por ejemplo, JSON, formulario, etc.).

    - **`getMethod()`**: Retorna el método HTTP utilizado en la solicitud, como GET, POST, etc.

    - **`getUrl()`**: Retorna la URL de la solicitud, permitiendo acceder a la dirección completa que el cliente está solicitando.

    - **`getParameters()`**: Retorna un array con los parámetros de la solicitud, facilitando el acceso a datos enviados en la URL o en el cuerpo de la solicitud.

    - **`input($name)`**: Permite obtener el valor de un parámetro específico proporcionando su nombre como parámetro. Retorna el valor del parámetro o una cadena vacía si el parámetro no se encuentra.

    - **`getFiles()`**: Retorna un array con la información de los archivos adjuntos a la solicitud.

    - **`inputFile($files)`**: Permite establecer los archivos adjuntos a la solicitud, facilitando el acceso y manipulación de datos enviados mediante formularios que incluyen archivos.

     Estos métodos proporcionan una interfaz poderosa para interactuar con los detalles de las solicitudes HTTP entrantes en tu aplicación ECVLib. Utiliza estos métodos según tus necesidades específicas para procesar de manera efectiva las solicitudes y realizar acciones correspondientes en tu aplicación.

## Clase Response

La clase `Response` en ECVLib simplifica la manipulación y el envío de respuestas HTTP desde tu aplicación. A continuación, se detallan los métodos disponibles:

- **`httpCode($code)`:** Establece el código de respuesta HTTP y devuelve una nueva instancia de la clase `Response`. Útil para configurar el código de estado de la respuesta.

    ```php
    Response::httpCode(404)->view('error', ['message' => 'Página no encontrada']);
    ```

- **`addHeader($name, $value)`:** Agrega un encabezado a la respuesta HTTP. Puedes proporcionar el nombre y el valor del encabezado, y el método se encargará de añadirlo correctamente.

    ```php
    Response::addHeader('Content-Type', 'application/json')->json(['status' => 'success']);
    ```

- **`json($data = [], $statusCode = 200, $headers = [])`:** Facilita la creación de respuestas JSON. Permite especificar datos, código de estado y encabezados adicionales.

    ```php
    Response::json(['user' => 'John Doe'], 200, ['X-Version: 1.0']);
    ```

- **`view($viewName, $data = [])`:** Muestra una vista en la respuesta. Toma el nombre de la vista y los datos a pasar a la vista. La clase `Response` busca y renderiza la vista automáticamente.

  ```php
    Response::view('home', ['username' => 'john']);
    ```

  - ### **Importante:** 
    Las vistas deben estar ubicadas en la carpeta `resources/views` y deben tener la extensión **`.phtml`**.

    Además, si tienes carpetas dentro de `resources/views` y deseas llamar a una vista dentro de esa carpeta, simplemente utiliza el nombre de la carpeta seguido de un punto y el nombre del archivo. Por ejemplo, si tienes una carpeta `shared` y un archivo `header.phtml`, puedes acceder a la vista con:
    ```php
    Response::view('shared.header', ['data'=>"Holis"]);
    ```

- **`redirect($path)`:** Redirige la solicitud a otra ubicación especificada por la ruta proporcionada. Configura automáticamente la cabecera de ubicación y realiza la redirección. Ten en cuenta que este método está diseñado para redirigir a las rutas dentro de tu aplicación.

    ```php
    Response::redirect('/home');
    ```
- **`redirectTo($url)`:** Este método redirige la solicitud a una URL específica. Si necesitas redirigir a cualquier página externa a tu aplicación, puedes utilizar este método de la siguiente manera:
  ```php
   Response::redirectTo('https://www.ejemplo.com');
  ```
  Ten en cuenta que este método redirige a cualquier URL proporcionada, no solo a las rutas dentro de tu aplicación.


  Estos métodos ofrecen una interfaz para gestionar las respuestas HTTP de tu aplicación ECVLib. Úsalos según tus necesidades específicas para manejar de manera eficiente una variedad de escenarios de respuesta.

## Gestión de Base de Datos
###  TitanOrm

La clase TitanOrm es una herramienta de manipulación de datos diseñada para interactuar con bases de datos mediante la librería EcvLib. Ofrece métodos simples y eficientes para realizar operaciones comunes en tablas de bases de datos, como SELECT, INSERT, UPDATE, DELETE y muchos más.
  
  - **Métodos Principales**
    - `select($column = ['*']):`
        Selecciona datos de la tabla según las      columnas especificadas.
        - Ejemplo de Uso:
          ```php
          $titanOrm = new TitanOrm();
          $results = $titanOrm->select(['id', 'name'])->get();
          ```
    - `insert($data = []):` Inserta datos en la tabla.
        - Ejemplo de Uso:
        ```php
          $titanOrm = new TitanOrm();
          $data = ['name' => 'John Doe', 'email' => 'john@example.com'];
          $success = $titanOrm->insert($data);
        ```
    - `update($data = [], $column = 'id', $id = null):` Actualiza un registro en la tabla.
        - Ejemplo de Uso:
        ```php
          $titanOrm = new TitanOrm();
          $data = ['name' => 'Updated Name'];
          $success = $titanOrm->update($data, 'id', 1);

        ```
    - `delete($column, $id):` Elimina un registro de la tabla.
        - Ejemplo de Uso:
        ```php
          $titanOrm = new TitanOrm();
          $success = $titanOrm->delete('id', 1);
        ```
     - `where($column, $value, $operator = '='):` Establece la cláusula WHERE para la consulta SQL.
        - Ejemplo de Uso:
        ```php
          $titanOrm = new TitanOrm();
          $titanOrm->where('name', 'John')->get();
        ```
      - `join($table, $firstColumn, $operator = '=', $secondColumn = null, $type = 'inner', $where = false):` Realiza una unión con otra tabla.
        - Ejemplo de Uso:
        ```php
          $titanOrm = new TitanOrm();
          $titanOrm->join('orders', 'users.id', '=', 'orders.user_id', 'left')->get();

        ```
      - `orderBy($column, $direction = 'asc'):` Establece la columna y dirección para ordenar los resultados.
        - Ejemplo de Uso:
        ```php
          $titanOrm = new TitanOrm();
          $titanOrm->orderBy('created_at', 'desc')->get();
        ```
      - `groupBy($column):` Agrupa los resultados por una columna específica.
        - Ejemplo de Uso:
        ```php
          $titanOrm = new TitanOrm();
          $titanOrm->groupBy('category')->get();
        ```
      - `get():` Ejecuta la consulta SQL y devuelve todos los registros como un array de objetos.
        - Ejemplo de Uso:
        ```php
          $titanOrm = new TitanOrm();
          $results = $titanOrm->select()->where('status', 'active')->get();
        ```    
      - `first():` Ejecuta la consulta SQL y devuelve el primer registro como un objeto.
        - Ejemplo de Uso:
        ```php
          $titanOrm = new TitanOrm();
          $firstRecord = $titanOrm->select()->where('category', 'tech')->first();
        ```  
      - `lastInsertId():` Recupera el último ID insertado en la base de datos.
        - Ejemplo de Uso:
        ```php
          $titanOrm = new TitanOrm();
          $lastId = $titanOrm->lastInsertId();
        ```  
    ### Información
    Al crear un modelo que hereda de EcvLibModel, este ya cuenta por defecto con métodos como SELECT, INSERT, UPDATE, DELETE y otros necesarios para manipular datos en la base de datos. La única configuración adicional requerida es establecer el nombre de la tabla a la que hace referencia el modelo.
    [![ruta-cambiar-nombre.png](https://i.postimg.cc/sg5SCQ3h/ruta-cambiar-nombre.png)](https://postimg.cc/v4HDWmgY)

    Para configurar el modelo con el nombre de la tabla correspondiente en su base de datos, simplemente cambie el valor de la propiedad  **`$table`** dentro del modelo por el nombre deseado. Esto permitirá que el modelo realice operaciones en la tabla específica cuando se utilicen los métodos proporcionados por defecto.

### Migraciones
Las migraciones en ECVLib te permiten gestionar la estructura de la base de datos de manera eficiente. Para crear una nueva migración, utiliza el siguiente comando:
```bash
  php thor create::migration name_migration
```

- **Ejecutar Todas las Migraciones:**

  ```bash
    php thor migrate
  ```
    Este comando ejecuta todas las migraciones pendientes en tu base de datos. Las migraciones son esenciales para mantener la consistencia entre la estructura de la base de datos y la definición actual de tus modelos. Asegúrate de haber configurado correctamente las credenciales de tu base de datos en el archivo .env antes de ejecutar este comando.
- **Revierte Todas las Migraciones:**

  ```bash
    php thor rollback
  ```
    Este comando revierte todas las migraciones realizadas previamente. Es útil en situaciones donde necesitas deshacer los cambios en tu base de datos. Antes de ejecutar php thor rollback, ten en cuenta que esto eliminará los cambios más recientes en la base de datos. Como precaución, asegúrate de hacer respaldos antes de realizar operaciones de reversión, especialmente en entornos de producción, para evitar pérdida de datos accidental.
- ### **Ejemplo de Migración**
  Aquí tienes un ejemplo de una migración llamada holis. Esta migración crea una tabla llamada holis con columnas para el identificador, el nombre, la descripción y marcas de tiempo.
  ```php
    public function up()
    {
        AdapterSqlGenerator::create('holis', function (Table $table) {
            $table->id('id_holi');
            $table->string('name');
            $table->text('description');
            $table->timeStamps();
        });
    }

    public function down()
    {
        AdapterSqlGenerator::dropTable('holis'); 
    }
  ```
  - **Metodos Soportados**
  Estos son los métodos proporcionados para definir columnas y realizar operaciones en migraciones:
- ### Métodos para Definir Columnas

  1. **`string($name, $length = 50, $parameters = [])`:**
    - Tipo de Dato: String.
    - Parámetros: Nombre, Longitud (predeterminado: 50), Parámetros adicionales.

  2. **`char($name, $parameters = [])`:**
    - Tipo de Dato: Char.
    - Parámetros: Nombre, Parámetros adicionales.

  3. **`text($name)`:**
    - Tipo de Dato: Texto.
    - Parámetros: Nombre.
  4. **`tinyText($name, $parameters = [])`:**
   - Tipo de Dato: TinyText.
   - Parámetros: Nombre, Parámetros adicionales.
  5. **`mediumText($name)`:**
   - Tipo de Dato: MediumText.
   - Parámetros: Nombre.

  6. **`longText($name)`:**
   - Tipo de Dato: LongText.
   - Parámetros: Nombre.

  7. **`blob($name)`:**
   - Tipo de Dato: BLOB.
   - Parámetros: Nombre.

  8. **`tinyBlob($name, $parameters = [])`:**
   - Tipo de Dato: TinyBLOB.
   - Parámetros: Nombre, Parámetros adicionales.

  9. **`mediumBlob($name)`:**
   - Tipo de Dato: MediumBLOB.
   - Parámetros: Nombre.

  10. **`longBlob($name)`:**
    - Tipo de Dato: LongBLOB.
    - Parámetros: Nombre.

  11. **`enum($name, $parameters = [])`:**
    - Tipo de Dato: ENUM.
    - Parámetros: Nombre, Parámetros adicionales.

  12. **`set($name, $parameters = [])`:**
    - Tipo de Dato: SET.
    - Parámetros: Nombre, Parámetros adicionales.

  13. **`integer($name)`:**
    - Tipo de Dato: Entero.
    - Parámetros: Nombre.

  14. **`tinyInteger($name, $parameters = [])`:**
    - Tipo de Dato: TinyInteger.
    - Parámetros: Nombre, Parámetros adicionales.

  15. **`smallInteger($name, $parameters = [])`:**
    - Tipo de Dato: SmallInteger.
    - Parámetros: Nombre, Parámetros adicionales.

  16. **`mediumInteger($name, $parameters = [])`:**
    - Tipo de Dato: MediumInteger.
    - Parámetros: Nombre, Parámetros adicionales.

  17. **`bigInteger($name, $parameters = [])`:**
    - Tipo de Dato: BigInteger.
    - Parámetros: Nombre, Parámetros adicionales.

  18. **`unsignedInteger($name, $parameters = [])`:**
    - Tipo de Dato: Unsigned Integer.
    - Parámetros: Nombre, Parámetros adicionales.

  19. **`float($name, $parameters = [])`:**
    - Tipo de Dato: Punto Flotante.
    - Parámetros: Nombre, Parámetros adicionales (precision y escala).

  20. **`decimal($name, $parameters = [])`:**
    - Tipo de Dato: Decimal.
    - Parámetros: Nombre, Parámetros adicionales (precision y escala).

  21. **`numeric($name, $parameters = [])`:**
    - Tipo de Dato: Número.
    - Parámetros: Nombre, Parámetros adicionales (precision y escala).

  22. **`double($name, $parameters = [])`:**
    - Tipo de Dato: Doble.
    - Parámetros: Nombre, Parámetros adicionales.

  23. **`real($name, $parameters = [])`:**
    - Tipo de Dato: Real.
    - Parámetros: Nombre, Parámetros adicionales.

  24. **`date($name, $parameters = [])`:**
    - Tipo de Dato: Fecha.
    - Parámetros: Nombre, Parámetros adicionales.

  25. **`datetime($name, $parameters = [])`:**
    - Tipo de Dato: Fecha y Hora.
    - Parámetros: Nombre, Parámetros adicionales.

  26. **`time($name, $parameters = [])`:**
    - Tipo de Dato: Hora.
    - Parámetros: Nombre, Parámetros adicionales.

  27. **`timestamp($name, $parameters = [])`:**
    - Tipo de Dato: Marca de Tiempo.
    - Parámetros: Nombre, Parámetros adicionales.

  28. **`year($name, $parameters = [])`:**
    - Tipo de Dato: Año.
    - Parámetros: Nombre, Parámetros adicionales.

  29. **`timeStamps()`:**
    - Tipo de Dato: Dos Marcas de Tiempo (created_at y updated_at).
    - Parámetros: Ninguno.

  30. **`bool($name, $parameters = [])`**
  - Tipo de Dato: Clave Booleano.
  - Parámetros: Nombre, Parámetros adicionales.
  31. **`foreign($name, $parameters = [])`:**
    - Tipo de Dato: Clave Foránea.
    - Parámetros: Nombre, Parámetros adicionales.
  ### Parametros 
  Para los parámetros generales son:

- `unsigned:` Si la columna es sin signo (true o false).
- `auto_increment:` Si la columna es autoincremental (true o false).
- `not_null:` Si la columna no puede ser nula (true o false).
- `precision:` La precisión de la columna (para tipos numéricos).
- `scale:` La escala de la columna (para tipos numéricos).
- `default:` Valor predeterminado de la columna.
- `primary_key:` Si la columna es clave primaria (true o false).
- `unique:` Si la columna debe ser única (true o false).



Para el tipo de dato "foreign," los parámetros adicionales son:

  - `references`: Nombre de la tabla de referencia.
  - `column_name`: Nombre de la columna de referencia.
  - `on_delete`: Acción a realizar cuando se elimina la fila principal.
  - `on_update`: Acción a realizar cuando se actualiza la fila principal.

**Ejemplos de Uso**:

En el siguiente ejemplo, se ilustra una relación entre las entidades de "Alumnos" y "Docentes" en el contexto de una base de datos. Esta relación se establece mediante el uso de claves foráneas y proporciona una conexión lógica entre las dos tablas. La entidad "Alumnos" y "Docentes" están vinculadas por medio de una columna que actúa como una clave foránea en la tabla de "Alumnos", haciendo referencia a la columna principal en la tabla de "Docentes".

[![ruta-docente.png](https://i.postimg.cc/xTcYbF5P/ruta-docente.png)](https://postimg.cc/BX9Rrpd8)

[![alumnos.png](https://i.postimg.cc/nz3bXvQ5/alumnos.png)](https://postimg.cc/5YQkGQgq)

[![Captura-realizada-el-2023-11-23-16-26-56.png](https://i.postimg.cc/C5Rtd90B/Captura-realizada-el-2023-11-23-16-26-56.png)](https://postimg.cc/LgMvbCn2)

## Validación de Datos

El método `validate` de esta de la  clase Request proporciona una robusta funcionalidad para validar datos de entrada según reglas específicas. A continuación se describen las validaciones comunes realizadas por este método:

- **required**: Verifica si un campo está presente y no está vacío.
- **min**: Asegura que un campo tenga al menos una longitud mínima especificada.
- **max**: Garantiza que un campo no exceda una longitud máxima especificada.
- **email**: Valida si un campo sigue el formato de dirección de correo electrónico.
- **numeric**: Confirma si un campo es numérico.
### Uso del Método

En la siguiente ilustración se muestra la creación de un formulario que realiza una petición POST a la ruta /hola. Este formulario puede contener diversos campos, como nombre, correo electrónico, mensaje, entre otros. Al momento de enviar el formulario, los datos ingresados son validados automáticamente mediante el método validate de la clase Request. Este método se encarga de aplicar reglas de validación predefinidas, como la obligatoriedad de ciertos campos, longitud mínima o máxima, formato de correo electrónico, entre otras.

[![fromulario.png](https://i.postimg.cc/QxFMQMmw/fromulario.png)](https://postimg.cc/WhPjjTQw)

[![e.png](https://i.postimg.cc/xTm2jNtf/e.png)](https://postimg.cc/BXJyM6rk)

[![Captura-realizada-el-2023-11-23-16-55-55.png](https://i.postimg.cc/g0zFKSdj/Captura-realizada-el-2023-11-23-16-55-55.png)](https://postimg.cc/MnFF6YnJ)

- Para cambiar el texto del mensaje de respuesta, se puede lograr de la siguiente manera:

 [![code.png](https://i.postimg.cc/zfrW2hPV/code.png)](https://postimg.cc/DSx88ShT)

 [![Captura-realizada-el-2023-11-23-17-12-05.png](https://i.postimg.cc/gjn6LHrB/Captura-realizada-el-2023-11-23-17-12-05.png)](https://postimg.cc/0K1NLmYp)
 
De esta forma, se puede cambiar fácilmente el texto del mensaje a mostrar. Cabe recalcar que los valores de los inputs se encuentran en esta sesión `*$_SESSION['old_values'][nombre_del_input]*`, y los errores en la sesión `*$_SESSION['form_errors'][nombre_del_input]*`.


