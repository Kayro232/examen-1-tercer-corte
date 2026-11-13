# Control de Equipos de Laboratorio

Aplicación de escritorio hecha con JavaFX que se conecta a PostgreSQL mediante Hibernate ORM (Jakarta Persistence). Permite administrar el inventario de equipos de un laboratorio y clasificarlos por tipo.

## Tecnologías utilizadas

- Java 21
- JavaFX 21.0.2
- Hibernate ORM 6.4.4.Final
- PostgreSQL 18 (driver JDBC 42.7.3)
- Maven

## Estructura del proyecto

| Paquete / archivo | Contenido |
| :--- | :--- |
| `model` | Entidades JPA `TipoEquipo` y `Equipo`, relacionadas con `@OneToMany` y `@ManyToOne`. |
| `persistence` | `JpaUtil` administra el `EntityManagerFactory`. `EquipoDAO` realiza el CRUD con transacciones y consultas JPQL. |
| `controller` | `EquipoController` maneja la interfaz gráfica, las validaciones de entrada y las alertas. |
| `resources` | Vista `hello-view.fxml` y configuración en `META-INF/persistence.xml`. |
| `script_bd.sql` | Script reproducible: crea la base de datos, el esquema `laboratorio`, las tablas, las restricciones y los datos iniciales. |

## Cómo ejecutar el proyecto

1. Ejecuta `script_bd.sql` en PostgreSQL. Con eso se crean la base `bd_laboratorio_orm` y el esquema `laboratorio`.
2. Revisa tus credenciales locales en `src/main/resources/META-INF/persistence.xml`.
3. Abre el proyecto en IntelliJ IDEA, deja que Maven descargue las dependencias y ejecuta la clase `me.edu.uam.controlequiposlaboratorio.Launcher`.

## Matriz de pruebas (P1 a P8)

| ID | Prueba | Datos de entrada | Resultado obtenido | Estado |
| :--- | :--- | :--- | :--- | :--- |
| P1 | Registrar un equipo válido | Código: EQ-009, Nombre: Monitor UltraSharp 24, Marca: Dell, Cantidad: 6, Tipo: Cómputo, Estado: Disponible | El equipo se guarda en PostgreSQL, aparece una alerta de éxito y el TableView se actualiza. | Aprobado |
| P2 | Intentar un código duplicado | Código: EQ-001 | El sistema valida la unicidad del código y muestra una alerta que rechaza el registro. | Aprobado |
| P3 | Editar nombre, estado o tipo | Se selecciona EQ-009 y se cambia el estado a En mantenimiento | `merge()` se ejecuta dentro de una transacción y la fila se actualiza en el TableView. | Aprobado |
| P4 | Eliminar un equipo con confirmación | Se selecciona EQ-009 y se confirma en el diálogo | Aparece un `Alert` de confirmación, se ejecuta `remove()` y la tabla se actualiza. | Aprobado |
| P5 | Buscar por nombre parcial | Texto: Proyector | La consulta JPQL con `LIKE` devuelve las coincidencias parciales, sin distinguir mayúsculas. | Aprobado |
| P6 | Filtrar por tipo y combinar con la búsqueda | Texto: Dell, Tipo: Cómputo | La consulta JPQL aplica ambos parámetros y solo muestra los registros que cumplen las dos condiciones. | Aprobado |
| P7 | Ingresar una cantidad negativa | Cantidad: -5 | La validación previa muestra una alerta de advertencia y no permite guardar el valor. | Aprobado |
| P8 | Reiniciar la aplicación | Se cierra la aplicación y se vuelve a ejecutar `Launcher` | Los cambios confirmados antes del cierre siguen en PostgreSQL y se muestran al iniciar de nuevo. | Aprobado |alizados persisten en PostgreSQL y se cargan al iniciar | Aprobado |
