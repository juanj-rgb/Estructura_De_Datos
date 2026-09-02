# Laboratorio 1: Estructuras de Datos Masivas en Dispositivo de Acceso Directo (DASD)

**Estudiante:** [Juan José Durango Carmona ]  
**Asignatura:** Estructuras de Datos  
**Profesor:** [EDISON ALEJANDRO MONTOYA GOMEZ]  
**Universidad:**  [Universidad de Antioquia]  

---

##  Descripción del Proyecto

Este proyecto implementa y manipula una **matriz bidimensional gigante de 100,000 x 100,000 elementos** (10,000,000,000 de celdas) almacenada directamente en almacenamiento secundario (disco duro). 

Para evitar saturar la memoria RAM (*Memory Overflow*), la matriz no se carga completa en memoria. En su lugar, se gestiona mediante un archivo binario plano con **Registros de Longitud Fija (LRECL)** codificados a nivel de **bits** (1 bit por celda), reduciendo el peso de la matriz de **9.31 GB a solo 1.16 GB**.

---

##  Contenido del Repositorio

* `laboratorio1_final.py`: Script principal que contiene la lógica de generación rápida en disco, consultas DASD O(1) y el módulo de inspección física.
* `README.md`: Documentación completa del proyecto y guía de verificación.
* `.gitignore`: Configuración para excluir el archivo binario pesado (`.dat`) del control de versiones.

---

##  Conceptos de Unidad 1 Aplicados

1. **Almacenamiento Binario Masivo:** Uso de registros de 1 bit por celda optimizados mediante máscaras de bits (`bit shifting`).
2. **Acceso Directo DASD ($O(1)$):** Navegación por el archivo usando `f.seek(offset)` para leer coordenadas $(i, j)$ sin recorrer el archivo.
3. **Mapeo Bidimensional a Lineal (Row-Major Order):**
   $$\text{Índice Global} = (i \times 100,000) + j$$
   $$\text{Byte Offset} = \frac{\text{Índice Global}}{8}$$
4. **Muestreo por Lotes / Paginación de Aplicación:** Lectura parcial y vista por submatrices sin volcar la matriz a RAM.

---

##  Método de Verificación del Archivo Generado

El programa incluye un módulo de **Inspección Física** para probar la existencia geométrica de la matriz directamente desde las propiedades del archivo `.dat`:

1. **Verificación de Dimensiones:** 
   El archivo mide exactamente **1,250,000,000 Bytes**. Dado que cada fila es un registro fijo de $12,500 \text{ Bytes}$ ($100,000 \text{ bits}$):
   $$\text{Filas} = \frac{1,250,000,000 \text{ Bytes}}{12,500 \text{ Bytes/Fila}} = 100,000 \text{ Filas}$$

2. **Verificación por Patrón Periódico:** 
   El archivo escribe alternadamente el patrón `0xAA` (`10101010`) en filas pares y `0x55` (`01010101`) en filas impares.
   * **Offset 0 Bytes (Inicio Fila 0):** Lee `0xAA`
   * **Offset 12,500 Bytes (Inicio Fila 1):** Lee `0x55`
   Esto demuestra que la estructura matricial existe físicamente en el archivo con un periodo exacto de 12,500 bytes.

---

##  Instrucciones de Ejecución

1. Ejecutar el script principal con Python 3:
   ```bash
   python laboratorio1_final.py
