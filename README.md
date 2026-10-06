# liga-mx-xml
Nombre: Santiago Beltran Astorga
Expediente: 225203551
## Proposito

El proyecto consiste en representar en XML los resultados y estadisticas de los partidos correspondientes a una jornada de Liga MX, utilizando un DTD para definir y validar la estructura del documento.

## Modelo jerarquico

La estructura utilizada es:

    liga
    └── jornada
        └─── partido
            ├── fecha
            ├── equipo
            │   ├── nombre
            │   ├── goles
            │   └── estadisticas
            │       ├── tiros
            │       ├── faltas
            │       └── tarjetas
            ├── equipo
            ├── estadio
            └── estado
        

Una jornada puede contener uno o mas partidos. Cada partido contiene dos equipos, uno local y uno visitante.

## Criterios para elementos y atributos

Se utilizaron elementos para representar la informacion principal del partido, como la fecha, los equipos, los goles, el estadio, el estado y las estadisticas.

Se utilizaron atributos para informacion que permite identificar o clasificar los datos:

- `id`: identifica de manera unica cada partido.
- `tipo`: distingue si un equipo es local o visitante.

El atributo `id` se utiliza como tipo `ID`, ya que debe ser unico dentro del documento. Esto es mas apropiado que utilizar `CDATA`, porque `CDATA` no impone la restriccion de unicidad.

## Estadisticas seleccionadas

Las estadisticas se relacionan directamente con cada equipo, ya que representan informacion sobre su desempeno durante el partido.

Las estadisticas seleccionadas son:

- Tiros
- Faltas
- Tarjetas

El elemento `estadisticas` es opcional, debido a que puede no existir informacion estadistica disponible para un partido.

## Instrucciones de validacion

El XML utiliza un DTD externo mediante la siguiente declaracion:

    <!DOCTYPE liga SYSTEM "../dtd/resultados.dtd">

La estructura del proyecto es:

    liga-mx-xml/
    ├── README.md
    ├── xml/
    │   ├── resultados.xml
    │   └── resultados-invalido.xml
    └── dtd/
        └── resultados.dtd

Para validar el XML se debe utilizar una herramienta o editor que permita validar un documento XML contra un DTD externo.

El archivo `resultados.xml` debe cumplir tanto las reglas de un XML bien formado como las reglas definidas en `resultados.dtd`.

## Resultados de las pruebas negativas

Se realizaron pruebas para comprobar que el DTD detectara diferentes errores.

| Prueba | ¿Bien formado? | ¿Valido? | Error detectado |
|---|---|---|---|
| Falta visitante | Si | No | No concuerda con la estructura del DTD. |
| Dos locales | Si | Si | El DTD permite que ambos equipos tengan `tipo="local"` porque no establece una condicion para que solo exista un equipo local. |
| Orden incorrecto | Si | No | No concuerda con la estructura del DTD. |
| Falta atributo obligatorio | Si | No | Falta el atributo `id`. |
| ID duplicado | Si | No | El ID esta repetido y debe ser unico. |
| Elemento desconocido | Si | No | El elemento no esta declarado en el DTD. |

## Decisiones ante el cambio de requisitos

Para la Actividad 9 se incorporaron los resultados correspondientes al domingo 27 de septiembre de 2026.

Los partidos utilizados fueron:

- Pumas UNAM 2-3 Atletico de San Luis
- Leon 2-1 FC Juarez
- Necaxa 2-4 America

Como desde el analisis inicial se habia establecido que una jornada puede contener varios partidos, el modelo se adapto para permitir uno o mas elementos `partido` dentro de `jornada`.

No fue necesario agregar nuevos elementos para representar los resultados, ya que la informacion podia representarse utilizando la estructura que ya se habia definido.

Las estadisticas no se agregaron a estos partidos debido a que no se contaba con esa informacion dentro de los datos utilizados para esta actividad.
