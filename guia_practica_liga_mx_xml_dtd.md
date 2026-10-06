# Práctica guiada: Diseño y validación de resultados de Liga MX con XML y DTD

## 1. Propósito

Diseñar un formato XML para representar información estructurada de
partidos de fútbol y definir mediante un DTD las reglas que deben
cumplir los documentos creados con dicho formato.

Al finalizar, serás capaz de identificar entidades de
información, diseñar una estructura XML jerárquica, distinguir elementos
y atributos, construir XML bien formado, definir un DTD, validar el XML
y documentar el trabajo mediante Git.

## 2. Problema

Se desea almacenar en XML los resultados y estadísticas de los partidos
correspondientes a una jornada de Liga MX. La fuente indicada en el
ejercicio es la sección de resultados de ESPN México. La solución debe
representar el marcador, la jornada y las estadísticas disponibles de
cada encuentro.

## 3. Actividad 1 - Analizar la información

**Tiempo: 15 minutos**

Antes de escribir XML, consultar la información disponible para un
partido e identifique los datos necesarios.

  | Jornada     | Partido          | Estadísticas     |
  |-------------|------------------|------------------|
  | Fecha       | Equipo local     | Posesión         |
  | Competencia | Equipo visitante | Tiros            |
  | Temporada   | Marcador         | Tiros a puerta   |
  |             | Estadio          | Faltas           |
  |             | Estado           | Tarjetas         |
  |             |                  | Tiros de esquina |

### Preguntas de análisis

1.  ¿Cuál debería ser el elemento raíz?
2.  ¿Una jornada puede contener varios partidos?
3.  ¿Cada partido debe contener exactamente dos equipos?
4.  ¿Cómo distinguirían al equipo local del visitante?
5.  ¿El marcador debe representarse como un solo dato o separar los
    goles?
6.  ¿Las estadísticas pertenecen al partido o a cada equipo?
7.  ¿Qué datos son obligatorios?
8.  ¿Cuáles podrían ser opcionales?

## 4. Actividad 2 - Diseñar el modelo conceptual

**Tiempo: 15 minutos**

Gráficamente podríamos tener la siguiente jerarquía. 
Este esquema no constituye la solución definitiva. 
Cada equipo deberá ampliarlo.

``` text
liga
│
└── jornada
    │
    ├── partido
    │   ├── equipoLocal
    │   ├── equipoVisitante
    │   ├── marcador
    │   └── estadisticas
    ├── partido
    └── partido
```

Determinar si cada dato se representa como elemento o atributo (*Completar la tabla*):

  | Información        | Elemento/Atributo | Justificación |
  |--------------------|-------------------|---------------|
  | Jornada            |                   |               |
  | Fecha              |                   |               |
  | ID del partido     |                   |               |
  | Equipo local       |                   |               |
  | Equipo visitante   |                   |               |
  | Goles              |                   |               |
  | Estadio            |                   |               |
  | Estado del partido |                   |               |
  | Posesión           |                   |               |
  | Tarjetas           |                   |               |

Registrar los cambios en el repositorio.
``` bash
git commit -m "Actividad 2"
```

## 5. Actividad 3 - Construir un XML mínimo

**Tiempo: 15 minutos**

Crear el documento  `xml/resultados.xml` con:

``` xml
<?xml version="1.0" encoding="UTF-8"?>
```

Implemente un solo partido con identificación, fecha, equipo local,
equipo visitante y marcador.

Comprobar que existe un solo elemento raíz, todas las etiquetas están
cerradas, el anidamiento es correcto, los atributos están entre comillas
y no existen etiquetas cruzadas.

Registrar los cambios en el repositorio.
``` bash
git commit -m "Actividad 3"
```

## 6. Actividad 4 - Incorporar estadísticas

**Tiempo: 15 minutos**

Ampliar el primer partido para incluir estadísticas relevantes. Decidir
cómo relacionar cada estadística con el equipo correspondiente.

``` text
Partido
        │
        ├──────────────┐
        ▼              ▼
     Local          Visitante
        │              │
        ▼              ▼
 Estadísticas     Estadísticas
```

Discutir qué diseño es más comprensible, reduce duplicación, facilita el
procesamiento y permite agregar estadísticas. Registre la decisión en
`README.md`.

Registrar los cambios en el repositorio.
``` bash
git commit -m "Actividad 4"
```

## 7. Actividad 5 - Diseñar el DTD

**Tiempo: 25 minutos**

Crear el archivo:

``` text
dtd/resultados.dtd
```

Definir el elemento raíz y sus descendientes mediante `<!ELEMENT ...>`.

| Operador | Significado |
|-|--|
| , | Secuencia |
| "|"  |  Alternativa|
| ? | Cero o una vez |
| * | Cero o más |
| + | Una o más veces |

Determine las cardinalidades y completar la siguiente tabla:

| Regla                                     | Expresión DTD |
|-------------------------------------------|---------------|
| Una liga contiene una o más jornadas      |               |
| Una jornada contiene uno o más partidos   |               |
| Un partido tiene exactamente un local     |               |
| Un partido tiene exactamente un visitante |               |
| Una estadística opcional                  |               |
| Puede haber cero o más tarjetas           |               |


Registrar los cambios en el repositorio.
``` bash
git commit -m "Actividad 5"
```

## 8. Actividad 6 - Definir atributos

**Tiempo: 10 minutos**

Definir los atributos necesarios para cada elemento mediante:

``` dtd
<!ATTLIST ... >
```

Considerar `#REQUIRED`, `#IMPLIED` e `ID`.

**Pregunta:** Si cada partido tiene un identificador `P001`, `P002`,
etc., ¿qué ventaja tendría declararlo como `ID` en lugar de `CDATA`?

Registrar los cambios en el repositorio.
``` bash
git commit -m "Actividad 6"
```

## 9. Actividad 7 - Asociar XML y DTD

**Tiempo: 5 minutos**

El XML deberá hacer referencia al DTD externo mediante:

``` xml
<!DOCTYPE ... SYSTEM "...">
```

Con la estructura:

``` text
liga-mx-xml/
├── xml/
│   └── resultados.xml
└── dtd/
    └── resultados.dtd
```

determinar la ruta relativa correcta desde el XML hasta el DTD.

Registrar los cambios en el repositorio.
``` bash
git commit -m "Actividad 7"
```

## 10. Actividad 8 - Pruebas negativas

**Tiempo: 10 minutos**

Cree `resultados-invalido.xml` e introduzca, uno por uno:

1.  Ausencia del equipo visitante;
2.  Dos equipos locales;
3.  Orden incorrecto de elementos;
4.  Ausencia de un atributo obligatorio;
5.  Un `ID` duplicado;
6.  Un elemento no declarado.

Complear la siguiente tabla:

| Prueba                     | ¿Bien formado? | ¿Válido? | Error detectado |
|----------------------------|----------------|----------|-----------------|
| Falta visitante            |                |          |                 |
| Dos locales                |                |          |                 |
| Orden incorrecto           |                |          |                 |
| Falta atributo obligatorio |                |          |                 |
| ID duplicado               |                |          |                 |
| Elemento desconocido       |                |          |                 |                      

> **XML bien formado ≠ XML válido**


Registrar los cambios en el repositorio.
``` bash
git commit -m "Actividad 8"
```
## 11. Actividad 9 - Construir la jornada solicitada

**Tiempo: 15--20 minutos**

Una vez validado el modelo con un partido, incorpore los resultados
correspondientes al **domingo 27 de septiembre de 2026**, fecha
solicitada en el ejercicio.

Si aparece información no contemplada, determine primero si representa
un nuevo requisito legítimo antes de modificar el DTD.

El XML completo deberá seguir siendo válido respecto a `resultados.dtd`.

Registrar los cambios en el repositorio.
``` bash
git commit -m "Actividad 9"
```


## 12. Entregables

``` text
liga-mx-xml/
├── README.md
├── xml/
│   ├── resultados.xml
│   └── resultados-invalido.xml
└── dtd/
    └── resultados.dtd
```

El `README.md` deberá documentar el propósito, modelo jerárquico,
criterios para elementos y atributos, estadísticas seleccionadas,
instrucciones de validación, resultados de pruebas negativas y
decisiones ante el cambio de requisitos. Puedes utilizar este ducumento
como base para tu `README.md`.

Finalmente, entregue el enlace al repositorio.

