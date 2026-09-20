# Legacy
Modulo6_Eejercicio_1
## 1. Orden y Secuencia de Transformaciones Realizadas
1. **Extracción y Carga Inicial:** Conexión desde la fuente legacy e ingreso directo a Power Query.
2. **Depuración de Duplicados:** Eliminación de registros repetidos utilizando la clave primaria de transacción.
3. **Tratamiento de Nulos:** Filtrado de registros incompletos en importes de ventas y reemplazo de valores faltantes en campos descriptivos.
4. **Estandarización de Encabezados:** Renombrado de nombres técnicos a estándar `snake_case`.
5. **Tipificación de Variables:** Asignación explícita de tipos de datos por columna.
6. **Normalización (Modelo Estrella):** Separación del tablón único en una tabla de hechos (`FactVentas`) y una tabla de dimensión (`DimCliente`).

## 2. Justificación Técnica de Tipos de Datos
* **`id_transaccion` / `id_cliente` (Texto):** Aunque contengan números, los identificadores no representan cantidades operables matemáticamente. Definirlos como texto previene agregaciones accidentales (sumas, promedios) y optimiza el filtrado.
* **`fecha_venta` (Fecha):** Esencial para habilitar inteligencia de tiempo (Time Intelligence en DAX) y la conexión futura con una tabla calendario dedicada.
* **`La columna TOT_VENT original presentaba inconsistencias debido a la presencia de valores nulos provenientes del sistema legacy. Para resolverlo, se generó una nueva columna calculada multiplicando la cantidad vendida por el precio unitario (aplicando los descuentos correspondientes), garantizando que el 100% de los registros cuente con un importe correcto y sin vacíos. Se le asignó el tipo de dato Número decimal fijo (Moneda) para evitar errores de redondeo por coma flotante y asegurar precisión matemática exacta en la consolidación financiera del reporte.
---

## 3. Estrategia de Gestión de Nulos y Duplicados
* **Duplicados:** Se aplicó eliminación de duplicados en la clave única de transacción. Un registro duplicado altera métricas de volumen transaccional e ingresos reales.
* **Nulos en Montos:** Se duplico la columna TOT_VENT, se cambiò el nombre por TOTAL_VENTA, se genero una columna nueva calculada, cantidad_vendida * precio_unitario - descuento_porcentual. 
* **Nulos en Atributos Secundarios:** En campos como la email_cliente y telefono_cleinte, los valores nulos se imputaron con el texto `"Sin telefono"`"Sin eamil", preservando la integridad conceptual sin perder el registro de la transacción.

---

## 4. Criterio de Normalización
Se aplicaron los principios de normalización de modelos relacionales (1NF / 2NF):
* **`DimCliente`:** Contiene la información propia de la entidad cliente.
* **`FactVentas`:** Registra las métricas numéricas y claves de relación (`codigo_operacion`, `fecha_venta`, `codigo_cliente`, `total_venta`).
* **Justificación:** Mantiene una única fuente de verdad para los datos del cliente, reduce la redundancia de almacenamiento y facilita el modelado dimensional bajo esquema en estrella en Power BI.
Adjunto capturas de pantalla
<img width="1344" height="718" alt="image" src="https://github.com/user-attachments/assets/f82c0cd9-42c1-4342-87ef-c10a4adee534" />
<img width="1359" height="676" alt="image" src="https://github.com/user-attachments/assets/6ed14390-25dd-4d34-96db-e551be4da8e9" />
