---
title: Sistema de Gestión de Estrellas en Python
date: 2024-07-12 17:50:00 +0800
categories: [Programación, Desarrollo de Software]
tags: [Python, Tkinter, MySQL, CRUD]
image:
  path: /assets/img/post/ejercicio_crud_estrellas/1.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
---

[Link Repositorio Github](https://github.com/mateosolinho/python/tree/master/projects/star_manager)

## Introducción

En el mundo del desarrollo de Software, el concepto de ```CRUD``` **(Crear, Leer, Actualizar y Eliminar)** es fundamental para la gestión de datos en aplicaciones.

Un sistema ```CRUD``` permite a los usuarios interactuar con una base de datos de manera completa: **añadir nuevos registros, leer o consultar datos existentes, actualizar registros y eliminar aquellos que ya no sean necesarios**.

En esta entrada, exploraremos cómo construir un sistema ```CRUD``` para gestionar un **catálogo de estrellas** utilizando ```Python```, ```Tkinter``` para la **interfaz gráfica** y ```MySQL``` como **sistema de gestión de bases de datos**.

## ¿Qué es un CRUD?

![img](/assets/img/post/ejercicio_crud_estrellas/5.png)

```CRUD``` es un acrónimo de las **cuatro operaciones básicas** que se pueden realizar en una base de datos:

* **Crear**: **Inserción** de nuevos datos en la base de datos.

* **Leer**: **Consulta o lectura** de datos existentes.

* **Actualizar**: **Modificación** de datos existentes en la base de datos.

* **Eliminar**: **Eliminación** de datos en la base de datos.

### Ventajas del Enfoque CRUD

1. **Estructura Clara**: Proporciona una **estructura estandarizada** para interactuar con datos.

2. **Escalabilidad**: Permite la **adición de funcionalidades y datos** sin reestructurar el sistema.

3. **Interfaz de Usuario**: Facilita la cración de **interfaces gráficas de usuario** para gestionar datos de **manera eficiente**.

### Desventajas del Enfoque

1. **Complejidad Inicial**: Requiere una **planificación cuidadosa** para definir las operaciones y su interacción.

2. **Rendimiento**: En aplicaciones con **grandes volúmenes de datos**, las operaciones ```CRUD``` pueden afectar al rendimiento si estas no están optimizadas.

3. **Seguridad**: La implementación incorrecta puede llevar a **vulnerabilidades** en la gestión de datos.

## Tecnologías y Software utilizado

### XAMP

![img](/assets/img/post/ejercicio_crud_estrellas/4.png)

```XAMPP``` es una **solución de software** que proporciona una plataforma de ```servidor web local``` que incluye ```Apache```, ```MySQL``` (o MariaDB), ```PHP``` y ```Perl```. Aquí está un desglose de su uso y ventajas:

1. **¿Qué es XAMPP?**
  
    ```XAMPP``` es un **paquete de software** que facilita la instalación de un ```servidor web local``` en sistemas operativos como ```Windows```, ```Linux``` o ```macOS```. Su nombre es un acrónimo de las tecnologías incluidas:

    * **X**: Multiplataforma *(Cross-Platform)*
    * **A**: Apache *(Servidor web)*
    * **M**: MySQL/MariaDB *(Sistema de gestión de bases de datos)*
    * **P**: PHP *(Lenguaje de programación del lado del servidor)*
    * **P**: Perl *(Lenguaje de programación)*
  
2. **Uso en el Desarrollo de la Aplicación**

    Para la aplicación, ```XAMPP``` se utiliza principalmente para:

    * **Desarrollar y Probar**: Proporciona un entorno de **servidor local** donde se puede **desarrollar y probar** la base de datos y el código de la aplicación antes de desplegarlo en un servidor en vivo.

    * **Gestionar la Base de Datos**: ```XAMPP``` incluye ```MySQL/MariaDB```, que se utiliza para **almacenar y gestionar** los datos de la aplicación.

3. **Ventajas**

    * **Fácil Instalación**: ```XAMPP``` se instala **rápidamente y configura automáticamente** el servidor web y la base de datos.

    * **Multiplataforma**: Compatible con **varios sistemas operativos**, lo que facilita el desarrollo en diferentes entornos.

    * **Todo en Uno**: Incluye todas las **herramientas necesarias** para el desarrollo web en un solo paquete.

4. **Desventajas**

    * **Seguridad**: Al estar configurado para facilitar el desarrollo, ```XAMPP``` **no es seguro para un entorno de producción** sin una configuración adicional.

### HeidiSQL

![img](/assets/img/post/ejercicio_crud_estrellas/3.png)

HeidiSQL es una herramienta de gestión y administración de bases de datos MySQL, MariaDB y PostgreSQL. A continuación se detalla su uso y características:

1. **¿Qué es HeidiSQL?**
  
    ```HeidiSQL``` es una **aplicación gráfica** que proporciona una **interfaz de usuario** para gestionar bases de datos ```SQL```. Permite la administración de **bases de datos, tablas, consultas SQL y la visualización de datos** de manera intuitiva.
  
2. **Uso en el Desarrollo de la Aplicación**

    Para la aplicación, ```HeidiSQL``` se utiliza para:

    * **Administrar la Base de Datos**: **Crear, modificar y eliminar bases de datos y tablas**. En nuestro caso, se usa para gestionar la tabla de ```stars```.
    * **Ejecutar Consultas SQL**: **Probar y ejecutar** consultas ```SQL``` directamente, lo que ayuda en la depuración y desarrollo de la lógica de base de datos.

    * **Visualizar Datos**: **Ver y editar datos** directamente en una interfaz gráfica.

3. **Ventajas**

    * **Interfaz Gráfica Amigable**: Proporciona una **interfaz intuitiva** que facilita la administración de bases de datos **sin necesidad de escribir comandos** ```SQL``` manualmente.

    * **Soporte para Múltiples Bases de Datos**: Compatible con ```MySQL```, ```MariaDB``` y ```PostgreSQL```.

    * **Funcionalidades Avanzadas**: Permite **importar y exportar datos, realizar backups**, y otras tareas administrativas.

4. **Desventajas**

    * **Limitaciones en Funcionalidades Avanzadas**: Aunque potente, puede **no ser tan completo** como algunas herramientas profesionales para bases de datos más complejas.

### SQL (Structured Query Language)

![img](/assets/img/post/ejercicio_crud_estrellas/6.jpg)

`SQL` es un lenguaje de programación estándar utilizado **para gestionar y manipular bases de datos relacionales**. Aquí está cómo se utiliza y sus beneficios:

1. **¿Qué es SQL?**

    `SQL (Structured Query Language)` es el lenguaje utilizado para interactuar con sistemas de gestión de bases de datos relacionales. Permite a los usuarios realizar **diversas operaciones en la base de datos**, incluyendo:

   * **Consulta de Datos**: Recuperar datos de tablas.

   * **Inserción de Datos**: Agregar nuevos registros a tablas.

   * **Actualización de Datos**: Modificar registros existentes.

   * **Eliminación de Datos**: Eliminar registros de tablas.

   * **Estructuración de Bases de Datos**: Crear y modificar tablas y otros objetos de base de datos.

2. **Uso en el Desarrollo de la Aplicación**

    Para la aplicación, `SQL` se utiliza para:

    * **Crear la Estructura de la Base de Datos**: Definir tablas y sus campos utilizando comandos `SQL`.

    * **Manipular Datos**: Insertar, actualizar y eliminar datos en la base de datos.

    * **Realizar Consultas**: Recuperar datos para mostrarlos en la interfaz de usuario.

3. **Ventajas**

    * **Estándar de la Industria**: `SQL` es el lenguaje de consulta estándar para bases de datos relacionales.

    * **Flexible y Poderoso**: Permite realizar operaciones complejas y consultas detalladas.

    * **Ampliamente Soportado**: Compatible con casi todos los sistemas de gestión de bases de datos relacionales.

4. **Desventajas**

    * **Complejidad en Consultas Avanzadas**: Las consultas complejas pueden ser difíciles de escribir y optimizar.

    * **Dependencia del Sistema de Gestión de Bases de Datos**: Aunque `SQL` es un estándar, las implementaciones pueden variar ligeramente entre diferentes sistemas de gestión de bases de datos.

## Estructura del Proyecto

El proyecto está diseñado para **gestionar información sobre estrellas en un catálogo**. La aplicación permite a los usuarios:

* **Añadir nuevos registros de estrellas**.
* **Leer datos existentes y mostrarlos en una tabla**.
* **Actualizar registros seleccionados**.
* **Eliminar registros**.
* **Exportar datos en formato `CSV`**.
* **Generar reportes y gráficos en base a los datos**.

## Funcionamiento y Explicación

A continuación, se detalla el funcionamiento y cómo cada parte contribuye a la funcionalidad general.

### Estructura de la aplicación

La aplicación está diseñada para **gestionar y analizar datos astronómicos**, combinando una interfaz intuitiva con potentes funcionalidades.

A través de la aplicación, los usuarios pueden **añadir**, **consultar**, **actualizar** y **eliminar** registros de estrellas en una base de datos MySQL.

La interfaz presenta un título y un footer, con dos dataframes que muestran la información de manera estructurada.

Los usuarios pueden interactuar con los datos, buscar información específica, y realizar análisis detallados mediante la **generación de gráficos** que visualizan parámetros como **masa**, **radio**, **distancia** y **luminosidad** de las estrellas.

Además, la aplicación permite **exportar datos** y **estadísticas** a archivos **PDF**, proporcionando informes que incluyen tanto la información de la base de datos como análisis estadísticos.

Esto hace que la aplicación sea una herramienta completa para la **gestión**, **visualización** y **reporte** de datos astronómicos.

![img](/assets/img/post/ejercicio_crud_estrellas/2.png)

****

### Función `fetch_data`

La función `fetch_data` se encarga de **recuperar todos los registros** de la tabla `stars` en la base de datos MySQL y mostrarlos en la interfaz gráfica de la aplicación.

```python
def fetch_data(self):
        conn = mysql.connector.connect(host="localhost", username="root", database="Mydata")
        my_cursor = conn.cursor()
        my_cursor.execute("select * from stars")
        rows=my_cursor.fetchall()
        if len(rows)!=0:
            self.stars_table.delete(*self.stars_table.get_children())
            for i in rows:
                self.stars_table.insert("",END,values=i)
            conn.commit()
        conn.close()
```

1. **Conexión a la Base de Datos**: Se establece una conexión con la base de datos MySQL utilizando `mysql.connector.connect`, especificando el host, el usuario y la base de datos.

2. **Creación del Cursor**: Se crea un cursor con `conn.cursor()`, que permite ejecutar comandos SQL.

3. **Ejecución de Consulta**: Se ejecuta una consulta SQL (`"select * from stars"`) para seleccionar todos los registros de la tabla `stars`.

4. **Obtención de Resultados**: Se obtienen todos los registros con `my_cursor.fetchall()`, que devuelve una lista de tuplas.

5. **Actualización de la Interfaz**: Si se obtienen registros, se elimina el contenido actual de `stars_table` (la tabla en la interfaz gráfica) y se insertan los nuevos datos utilizando `self.stars_table.insert()`.

6. **Confirmación y Cierre**: Se confirma la transacción con `conn.commit()` y se cierra la conexión a la base de datos con `conn.close()`.

****

### Función `delete_data`

La función `delete_data` se encarga de **eliminar un registro específico** de la tabla `stars` basado en el ID seleccionado en la interfaz gráfica.

```python
def delete_data(self):
        if self.name.get() == "":
            messagebox.showerror("Error", "Name field is required to delete a record.")
            return

        conn = mysql.connector.connect(host="localhost", username="root", database="Mydata")
        my_cursor = conn.cursor()
    
        try:
            my_cursor.execute("DELETE FROM stars WHERE id=%s", (self.selected_id,))
            conn.commit()

            if my_cursor.rowcount > 0:
                self.fetch_data()
                self.clear()
                messagebox.showinfo("Success", "Record has been deleted successfully.")
            else:
                messagebox.showwarning("Warning", "No record found with the provided ID.")
    
        except mysql.connector.Error as err:
            messagebox.showerror("Error", f"Error: {err}")
    
        finally:
            conn.close()
```

1. **Validación de Entrada**: Se verifica si el campo `name` está vacío. Si lo está, se muestra un mensaje de error y se termina la función.

2. **Conexión a la Base de Datos**: Se establece una conexión con la base de datos MySQL.

3. **Creación del Cursor**: Se crea un cursor para ejecutar comandos SQL.

4. **Ejecución de Eliminación**: Se ejecuta una consulta SQL (`"DELETE FROM stars WHERE id=%s"`) para eliminar el registro con el ID especificado (`self.selected_id`).

5. **Confirmación y Mensajes**: Se confirma la transacción con `conn.commit()`. Si la eliminación es exitosa, se actualizan los datos y se limpian los campos de entrada. Se muestra un mensaje de éxito si el registro se elimina correctamente, o un mensaje de advertencia si no se encuentra el registro.

6. **Manejo de Errores y Cierre**: Se manejan posibles errores con un bloque `try-except`, y finalmente, se cierra la conexión a la base de datos en el bloque `finally`.

****

### Función `get_cursor`

La función `get_cursor` permite seleccionar un registro en la tabla de la interfaz gráfica y cargar sus datos en los campos de entrada para permitir su edición.

```python
def get_cursor(self, event):
        cursor_row = self.stars_table.focus()
        content = self.stars_table.item(cursor_row)
        row = content['values']
        self.selected_id = row[0]
        self.name.set(row[1])
        self.catalog.set(row[2])
        self.constellation.set(row[3])
        self.spectral.set(row[4])
        self.distance.set(row[5])
        self.radius.set(row[6])
        self.mass.set(row[7])
        self.visibility.set(row[8])
```

1. **Obtener Fila Seleccionada**: Se obtiene la fila actualmente seleccionada en `stars_table` con `self.stars_table.focus()`.

2. **Extraer Valores**: Se extraen los valores de la fila seleccionada mediante `self.stars_table.item(cursor_row)['values']`.

3. **Actualizar Campos**: Los valores extraídos se asignan a los campos de entrada correspondientes (como `self.name`, `self.catalog`, etc.), y se establece el ID del registro seleccionado en `self.selected_id`.

****

### Función `add_stars`

La función `add_stars` agrega un nuevo registro a la tabla `stars` en la base de datos, utilizando los valores ingresados en los campos de entrada.

```python
def add_stars(self):
        if self.name.get()=="":
            messagebox.showerror("Error","All fields are required!")
        else:
            conn = mysql.connector.connect(host="localhost", username="root", database="Mydata")
            my_cursor = conn.cursor()
            my_cursor.execute("""CREATE TABLE IF NOT EXISTS Stars ( id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(255) NOT NULL, catalog VARCHAR(255), constellation VARCHAR(255), spectral VARCHAR(255), distance DOUBLE, radius DOUBLE, mass DOUBLE, visibility DOUBLE);""")

            my_cursor.execute("insert into stars (name, catalog, constellation, spectral, distance, radius, mass, visibility) values (%s, %s, %s, %s, %s, %s, %s, %s)", (
                self.name.get(),
                self.catalog.get(),
                self.constellation.get(),
                self.spectral.get(),
                self.distance.get(),
                self.radius.get(),
                self.mass.get(),
                self.visibility.get(),
            ))

            conn.commit()
            self.fetch_data()
            self.clear()
            conn.close()
            messagebox.showinfo("Success","Record has been inserted sucessfully")
```

1. **Validación de Campos**: Se verifica que el campo `name` no esté vacío. Si lo está, se muestra un mensaje de error y se termina la función.

2. **Conexión a la Base de Datos**: Se establece una conexión con la base de datos MySQL.

3. **Creación de Tabla (si no existe)**: Se ejecuta una consulta SQL para crear la tabla `stars` si no existe, especificando las columnas y tipos de datos.

4. **Inserción de Registro**: Se ejecuta una consulta SQL ("`insert into stars (name, catalog, constellation, spectral, distance, radius, mass, visibility) values (%s, %s, %s, %s, %s, %s, %s, %s)`") para insertar un nuevo registro con los valores proporcionados en los campos de entrada.

5. **Confirmación y Actualización**: Se confirma la transacción con `conn.commit()`, se actualizan los datos en la interfaz con `self.fetch_data()`, se limpian los campos de entrada con `self.clear()`, y se cierra la conexión.

****

### Función `update_data`

La función `update_data` actualiza un registro existente en la tabla `stars` con los valores modificados en los campos de entrada, basado en el ID seleccionado.

```python
def update_data(self):
        if self.selected_id is None:
            messagebox.showerror("Error", "Select a record to update.")
            return

        conn = mysql.connector.connect(host="localhost", username="root", database="Mydata")
        my_cursor = conn.cursor()

        try:
            my_cursor.execute("""UPDATE stars SET name = %s, catalog = %s, constellation = %s, spectral = %s, distance = %s, radius = %s, mass = %s, visibility = %s WHERE id = %s """, (
            self.name.get(),
            self.catalog.get(),
            self.constellation.get(),
            self.spectral.get(),
            self.distance.get(),
            self.radius.get(),
            self.mass.get(),
            self.visibility.get(),
            self.selected_id
            ))

            conn.commit()
            if my_cursor.rowcount > 0:
                self.fetch_data()
                self.clear()
                messagebox.showinfo("Success", "Record has been updated successfully.")
            else:
                messagebox.showwarning("Warning", "No record found with the provided ID.")

        except mysql.connector.Error as err:
            messagebox.showerror("Error", f"Error: {err}")

        finally:
            conn.close()
```

1. **Validación de Selección**: Se verifica que `self.selected_id` no sea `None`, lo que indica que se ha seleccionado un registro para actualizar. Si no hay un ID seleccionado, se muestra un mensaje de error y se termina la función.

2. **Conexión a la Base de Datos**: Se establece una conexión con la base de datos MySQL.

3. **Ejecución de Actualización**: Se ejecuta una consulta SQL ("UPDATE stars SET ... WHERE id = %s") para actualizar el registro con el ID especificado (`self.selected_id`) con los nuevos valores proporcionados en los campos de entrada.

4. **Confirmación y Mensajes**: Se confirma la transacción con `conn.commit()`. Si la actualización es exitosa, se actualizan los datos y se limpian los campos de entrada. Se muestra un mensaje de éxito si el registro se actualiza correctamente, o un mensaje de advertencia si no se encuentra el registro.

5. **Manejo de Errores y Cierre**: Se manejan posibles errores con un bloque `try-except`, y finalmente, se cierra la conexión a la base de datos en el bloque `finally`.

****

### Función `search_data`

La función `search_data` busca registros en la tabla `stars` basados en un criterio de búsqueda específico y muestra los resultados en la interfaz gráfica.

```python
def search_data(self):
        conn = mysql.connector.connect(host="localhost", username="root", database="Mydata")
        my_cursor = conn.cursor() 
        my_cursor.execute("select * from stars where "+str(self.search_by.get())+" LIKE '%"+str(self.search_txt.get())+"%'")
        rows = my_cursor.fetchall()
        if len(rows) != 0:
            self.stars_table.delete(*self.stars_table.get_children())
            for row in rows:
                self.stars_table.insert("", END, values=row)
        else:
            messagebox.showinfo("Info", "No records found.")
        conn.close()
```

1. **Conexión a la Base de Datos**: Se establece una conexión con la base de datos MySQL.

2. **Creación del Cursor**: Se crea un cursor para ejecutar comandos SQL.

3. **Ejecución de Búsqueda**: Se ejecuta una consulta SQL con un filtro basado en el criterio de búsqueda especificado por el usuario (`self.search_by.get()`) y el texto de búsqueda (`self.search_txt.get()`).

4. **Actualización de la Interfaz**: Si se encuentran registros, se eliminan todos los elementos existentes en `stars_table` y se insertan los resultados de la búsqueda. Si no se encuentran registros, se muestra un mensaje informativo.

5. **Cierre de Conexión**: Finalmente, se cierra la conexión a la base de datos.

****

### Función `plot_distribution`

La función `plot_distribution` genera un gráfico de dispersión que muestra la distribución de un parámetro específico de los registros en la tabla `stars`.

```python
def plot_distribution(self):
        conn = mysql.connector.connect(host="localhost", username="root", database="Mydata")
        my_cursor = conn.cursor()
        
        parameter = self.search_by.get()
        
        if parameter not in ('Mass', 'Radius', 'Distance', 'Visibility'):
            messagebox.showwarning("Parameter Error", "Invalid parameter selected.")
            return
        
        my_cursor.execute(f"SELECT id, {parameter} FROM stars")
        data = my_cursor.fetchall()
        conn.close()
        
        if not data:
            messagebox.showinfo("Info", "No data to plot.")
            return
        
        ids, values = zip(*data)
        
        plot_window = Toplevel(self.root)
        plot_window.title(f'{parameter} Distribution')
        plot_window.geometry("800x600")
        
        fig, ax = plt.subplots(figsize=(8, 6))
        ax.scatter(ids, values, alpha=0.7, edgecolors='w')
        ax.set_title(f'Distribution of {parameter} Values')
        ax.set_xlabel('Star ID')
        ax.set_ylabel(parameter)
        ax.grid(True)
        
        canvas = FigureCanvasTkAgg(fig, master=plot_window)
        canvas.draw()
        canvas.get_tk_widget().pack(fill=BOTH, expand=1)
        
        close_btn = Button(plot_window, text="Close", command=plot_window.destroy)
        close_btn.pack(pady=10)
```

1. **Conexión a la Base de Datos**: Se establece una conexión con la base de datos MySQL.

2. **Validación de Parámetro**: Se verifica que el parámetro seleccionado para el gráfico sea uno de los valores válidos ('Mass', 'Radius', 'Distance', 'Visibility').

3. **Obtención de Datos**: Se ejecuta una consulta SQL para obtener los valores del parámetro seleccionado.

4. **Generación del Gráfico**: Se utiliza Matplotlib para crear un gráfico de dispersión con los IDs de las estrellas en el eje X y los valores del parámetro en el eje Y.

5. **Interfaz de Gráfico**: Se muestra el gráfico en una nueva ventana (plot_window) utilizando `FigureCanvasTkAgg`.

****

### Función `export_data`

La función `export_data` exporta los registros de la tabla `stars` a un archivo `CSV`.

```python
def export_data(self):
        conn = mysql.connector.connect(host="localhost", username="root", database="Mydata")
        my_cursor = conn.cursor()

        try:
            my_cursor.execute("SELECT * FROM stars")
            data = my_cursor.fetchall()
            
            column_names = [desc[0] for desc in my_cursor.description]
            
            filetypes = (
                ('CSV files', '*.csv'),
                ('All files', '*.*')
            )
            filename = filedialog.asksaveasfilename(
                defaultextension=".csv",
                filetypes=filetypes,
                title="Save data as"
            )

            if filename:
                try:
                    with open(filename, mode='w', newline='') as file:
                        writer = csv.writer(file)
                        writer.writerow(column_names)
                        writer.writerows(data)
                    messagebox.showinfo("Success", f"Data saved as {filename}")
                except IOError as e:
                    messagebox.showerror("Error", f"Error saving file: {e}")
        except mysql.connector.Error as err:
            messagebox.showerror("Database Error", f"Error fetching data: {err}")
        finally:
            conn.close()
```

1. **Conexión a la Base de Datos**: Se establece una conexión con la base de datos MySQL.

2. **Ejecución de Consulta**: Se ejecuta una consulta SQL para obtener todos los registros de la tabla `stars`.

3. **Escritura en CSV**: Se abre un archivo CSV en modo escritura y se escribe la cabecera y los registros obtenidos de la base de datos.

4. **Cierre de Archivo**: Se cierra el archivo CSV después de escribir los datos.

****

### Función `generate_report`

La función `generate_report` crea un informe completo en formato PDF que incluye los datos de la tabla `stars` y estadísticas calculadas para los parámetros específicos.

```python
def generate_report(self):
        conn = mysql.connector.connect(host="localhost", username="root", database="Mydata")
        my_cursor = conn.cursor()
        
        my_cursor.execute("SELECT * FROM stars")
        self.data = my_cursor.fetchall()

        stats = {}
        for param in ('Mass', 'Radius', 'Distance', 'Visibility'):
            my_cursor.execute(f"SELECT {param} FROM stars")
            values = [row[0] for row in my_cursor.fetchall() if row[0] is not None]
            stats[param] = {
                'count': len(values),
                'average': sum(values) / len(values) if values else 0,
                'min': min(values) if values else 0,
                'max': max(values) if values else 0
            }

        conn.close()
        self.create_pdf(self.data, stats)
```

1. **Conexión a la Base de Datos**: Se establece una conexión con la base de datos MySQL.

2. **Ejecución de Consulta**: Se ejecuta una consulta SQL para obtener todos los registros de la tabla `stars`.

3. **Cálculo de Estadísticas**: Para cada parámetro relevante (`Mass`, `Radius`, `Distance`, `Visibility`), se calcula la cantidad de registros (`count`), el promedio (`average`), el valor mínimo (`min`) y el valor máximo (`max`).

4. **Cierre de Conexión**: Se cierra la conexión a la base de datos.

5. **Generación del PDF**: Se llama a la función `create_pdf` para generar un archivo PDF que contenga los datos de los registros y las estadísticas calculadas. Se especifica el nombre del archivo PDF y el formato.

****

### Función `create_pdf`

La función `create_pdf` se encarga de crear un archivo PDF con los datos de la tabla `stars` y las estadísticas calculadas en `generate_report`.

```python
def create_pdf(self, data, stats):
        filetypes = (
            ('PDF files', '*.pdf'),
            ('All files', '*.*')
        )
        
        filename = filedialog.asksaveasfilename(
            defaultextension=".pdf",
            filetypes=filetypes,
            title="Save PDF as",
            initialfile="star_catalog_report.pdf"
        )
        
        if filename:
            try:
                pdf = FPDF()
                pdf.add_page()
                
                # Información general
                pdf.set_font("Arial", size=10)
                pdf.cell(0, 10, f"Report generated on: {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}", ln=True)
                pdf.ln(10)
                
                # Agregar tabla de datos
                pdf.set_font("Arial", 'B', 10)
                column_widths = [25, 40, 25, 25, 20, 25, 15, 20]  # Ancho de las columnas
                
                headers = ['Name', 'Catalog', 'Constellation', 'Spectral', 'Distance', 'Radius', 'Mass', 'Visibility']
                pdf.set_font("Arial", 'B', 10)
                for i, header in enumerate(headers):
                    pdf.cell(column_widths[i], 10, header, border=1, align='C')
                pdf.ln()

                pdf.set_font("Arial", size=9)
                for row in data:
                    for i, item in enumerate(row[1:]):  # Omitir la primera columna (ID)
                        pdf.cell(column_widths[i], 10, str(item), border=1)
                    pdf.ln()
                
                pdf.ln(10)
                pdf.set_font("Arial", 'B', 10)
                pdf.cell(0, 10, 'Statistics', ln=True)
                pdf.set_font("Arial", size=9)
                
                for param, stat in stats.items():
                    pdf.cell(0, 10, f"{param} - Count: {stat['count']}, Average: {stat['average']:.2f}, Min: {stat['min']}, Max: {stat['max']}", ln=True)
                
                pdf.output(filename)
                messagebox.showinfo("Success", f"Report saved as {filename}")
                
            except Exception as e:
                messagebox.showerror("Error", f"Error saving PDF: {e}")
```

1. **Selección de Archivo PDF**: Se abre un diálogo para que el usuario elija el nombre y la ubicación del archivo PDF a guardar.

2. **Creación del Documento PDF**: Se utiliza la librería FPDF para crear un nuevo documento PDF.

3. **Agregar Contenido al PDF**:

    * **Información General**: Se añade la fecha y hora de generación del informe.

    * **Tabla de Datos**: Se agrega una tabla con los datos de la tabla `stars`. Los encabezados y el contenido se formatean y se insertan en el PDF.

    * **Estadísticas**: Se añaden las estadísticas calculadas (cantidad de registros, promedio, mínimo y máximo) para cada parámetro relevante.

4. **Guardar el PDF**: Se guarda el archivo PDF en la ubicación especificada por el usuario.

5. **Manejo de Errores**: Se manejan posibles errores durante la creación o guardado del PDF, mostrando un mensaje de error si ocurre algún problema.

****

## Conclusión General

En este proyecto, hemos desarrollado una aplicación para gestionar y analizar datos astronómicos mediante la implementación de un CRUD (Crear, Leer, Actualizar y Eliminar) básico. Este sistema no solo facilita la administración de una base de datos de estrellas, sino que también proporciona herramientas para generar informes detallados y realizar análisis gráficos. A continuación, se presentan algunas reflexiones clave sobre lo aprendido y la importancia de los CRUDs en el contexto de desarrollo de software

## Reflexión Final

El desarrollo de esta aplicación no solo ha mostrado la importancia de los CRUDs en la administración de bases de datos, sino que también ha subrayado cómo la combinación de herramientas tecnológicas y técnicas de programación puede facilitar la gestión, análisis y presentación de datos. La habilidad para crear interfaces intuitivas y funcionales, junto con la capacidad de generar informes y visualizaciones, proporciona a los usuarios un control más completo sobre la información y mejora la toma de decisiones basada en datos.

En resumen, el aprendizaje y aplicación de CRUDs en el desarrollo de software es una habilidad esencial que forma la columna vertebral de la mayoría de las aplicaciones modernas.

La experiencia adquirida en este proyecto refuerza la importancia de comprender y aplicar estos conceptos para desarrollar aplicaciones efectivas y útiles.

*Espero que os haya gustado y servido, cualquier comentario es de mucha ayuda. Adios!*
