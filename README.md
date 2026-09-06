[README.md](https://github.com/user-attachments/files/31875351/README.md)
# Tarea Base de Datos - Semana 4

Actividad formativa **"Modelando y Normalizando Datos"** — Analista Programador, DUOC UC.

## Descripción del caso

El **Ministerio del Trabajo de Chile** requiere una base de datos normalizada para almacenar la información de profesionales TIC que postulan, a través de un concurso público, a empresas nacionales y extranjeras (Microsoft, Cisco Systems, Apple, Amazon, Oracle, Google, entre otras) que buscan contratar talento en Chile.

El objetivo del ejercicio es diseñar un **Modelo Entidad-Relación-Extendido (MER-E)** aplicando las tres primeras formas normales (1FN, 2FN y 3FN), identificando entidades, relaciones, identificadores únicos, atributos obligatorios/opcionales, y supertipos/subtipos cuando corresponda.

## Modelo de datos

Diseñado en **Oracle SQL Developer Data Modeler**. Entidades principales:

- **EMPRESA**: datos de la empresa contratante (código, nombre, dirección, ciudad, país, remuneración máxima ofrecida). Se especializa en dos subtipos según su ubicación:
  - `EMPRESA_NACIONAL`
  - `EMPRESA_EXTRANJERA`

- **DATOS_DEL_POSTULANTE**: datos del postulante (folio, nombre, apellidos, género, fecha de nacimiento, grado académico, país de origen, dirección en Chile, estado civil, sitio web, etc.), relacionado 1 a N con EMPRESA (cada postulante aplica a una sola empresa). Se especializa según nacionalidad en:
  - `POSTULANTE_CHILENO` (incluye el atributo `rut`, con validación de dígito verificador)
  - `POSTULANTE_EXTRANJERO`

- **TITULO_PROFESIONAL**: entidad dependiente que resuelve la Primera Forma Normal, registrando entre 1 y 2 títulos profesionales por postulante (nombre de la profesión y fecha de obtención del título), con llave primaria compuesta (`folio_postulante` + `numero_titulo`).

### Decisiones de normalización

- **1FN**: se eliminó el grupo repetitivo de profesiones/títulos duplicados, trasladándolos a la entidad `TITULO_PROFESIONAL`.
- **2FN**: no existen dependencias parciales, ya que las llaves primarias son de un solo atributo (o, en el caso de `TITULO_PROFESIONAL`, todos los atributos no clave dependen de la llave compuesta completa).
- **3FN**: se separaron correctamente los datos de la empresa en su propia entidad, evitando dependencias transitivas a través del folio del postulante.

### Reglas de negocio relevantes

- Atributos condicionales según la ubicación de la empresa: `pretension_renta` (empresas en Chile) vs. `dominio_idiomas`, `fecha_visa`, `identificador_visa` (empresas extranjeras).
- `sitio_web` es opcional pero único (identificador único secundario).
- `rut` solo aplica a postulantes chilenos, y solo existe en el subtipo `POSTULANTE_CHILENO`.

## Archivos del repositorio

- `PRY2204_Exp2_S4_Formato respuesta.docx`: documento de respuesta de la actividad.

## Herramienta utilizada

Oracle SQL Developer Data Modeler.
