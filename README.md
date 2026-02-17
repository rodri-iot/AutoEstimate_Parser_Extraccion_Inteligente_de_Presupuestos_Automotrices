# AutoEstimate Parser: Extracción Inteligente de Presupuestos Automotrices
Microservicio que automatice la extracción de datos desde presupuestos en PDF.

## 1. Introducción
Este proyecto es un microservicio diseñado para resolver el problema de la doble carga de datos en la industria de reparaciones vehiculares. Muchos talleres y concesionarias generan sus presupuestos en sistemas propietarios que exportan a PDF, obligando a los peritos o administradores a transcribir manualmente cada pieza, código y costo a sus plataformas de gestión.

**AutoEstimate Parser** utiliza técnicas de procesamiento de documentos y lógica de ciencia de datos para convertir estos PDFs heterogéneos en datos estructurados (JSON) listos para ser consumidos por cualquier API.

## 2. Definición del Problema
- Inconsistencia: Cada proveedor tiene un formato de presupuesto distinto.
- Opacidad de Datos: La información relevante (códigos OEM, horas de mano de obra, precios) está "atrapada" en capas de texto de archivos PDF.
- Ineficiencia: El proceso de transcripción manual toma entre 10 y 20 minutos por siniestro, con un alto riesgo de error humano en valores numéricos.

## 3. Alcance del MVP
El sistema permite cargar un archivo PDF y devuelve automáticamente:

- Identificación del Proveedor: Clasificación automática basada en el layout del documento.
- Desglose de Ítems:
    - Repuestos: Extracción de Descripción, Código de Parte (OEM) y Precio.
    - Mano de Obra: Identificación de horas de Chapa y paños de Pintura.
- Lógica Predictiva: Asignación de "Costo 0" en ítems de sustitución para fases de licitación.
- Normalización: Limpieza de formatos de moneda y unidades (ej: conversión de strings "1.250,50 $" a floats "1250.50").

## 4. Stack Tecnológico
- Lenguaje: Python 3.x
- Parsing de PDF: `pdfplumber` (análisis de tablas y coordenadas de texto).
- Procesamiento de Datos: `Pandas` para la estructuración y limpieza.
- Interfaz de Usuario: `Streamlit` para una demo rápida y funcional.
- Algoritmos de Matching: `RapidFuzz` para relacionar descripciones de talleres con catálogos estándar.

## 5. Arquitectura de la Solución
``` bash
graph LR
A[Presupuesto PDF] --> B(Identificador de Formato)
B --> C{Motor de Extracción}
C --> D[Parser Regla A]
C --> E[Parser Regla B]
D & E --> F(Limpieza y Normalización)
F --> G(Fuzzy Matching con Diccionario)
G --> H[JSON Estructurado]
```
## 6. Funcionalidades Destacadas para el Negocio
- Escalabilidad: Diseñado para añadir nuevos formatos de proveedores mediante "Templates" de extracción sin modificar el núcleo del sistema.
- Bajo Costo Operativo: A diferencia de soluciones basadas en LLMs comerciales, este sistema procesa los documentos localmente en milisegundos, sin costos por token o página.
- Validación Humana: El output está diseñado para pre-llenar formularios, manteniendo siempre una instancia de validación final por el usuario.

## 7. Instalación y Uso (Local)
- Clonar el repositorio.
- Instalar dependencias: pip `install -r requirements.txt`
- Ejecutar la app: streamlit `run app.py`