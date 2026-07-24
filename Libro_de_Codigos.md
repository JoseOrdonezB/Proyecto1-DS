# Libro de Códigos
### Proyecto No. 1 – Obtención y Limpieza de Datos
**Data Science, Sección 10 — Universidad del Valle de Guatemala**
Jose Ordoñez (231329) · Adrián González (23152) · José Antón (221041) · Gadiel Ocaña (231270)

---

## Información general

| Campo | Valor |
|---|---|
| Fuente de los datos | Ministerio de Educación de Guatemala – Sistema de e-Servicios (http://www.mineduc.gob.gt/BUSCAESTABLECIMIENTO_GE/), 22 archivos CSV por departamento (Petén no disponible en el portal) |
| Fecha de extracción | 24 de julio de 2026 |
| Versión del conjunto limpio | v1.1 – `datos_establecimientos_limpio.csv` (incluye corrección de CIUDAD CAPITAL/ZONA N y de errores ortográficos de municipio) |
| Nivel escolar filtrado | Diversificado |
| Registros | 11,347 |
| Variables | 17 originales + 2 auxiliares de trazabilidad (`ZONA_CIUDAD`, `TELEFONO_original`) |

---

## CODIGO
- **Descripción:** Identificador único del establecimiento educativo asignado por el MINEDUC.
- **Tipo de dato:** Texto (character)
- **Dominio permitido:** Cadena alfanumérica con formato `NN-NN-NNNN-NN`.
- **Valores posibles:** Un código distinto por establecimiento (0 códigos repetidos con datos contradictorios).
- **Tratamiento aplicado:** `str_squish()`; se descartaron registros sin código o que no cumplían el patrón `NN-NN` al inicio; se verificó unicidad (0 duplicados exactos).

## DISTRITO
- **Descripción:** Código del distrito educativo al que pertenece el establecimiento.
- **Tipo de dato:** Texto (character)
- **Dominio permitido:** Código alfanumérico tipo `NN-NNN`.
- **Valores posibles:** 508 valores faltantes (4.48 %); resto son códigos de distrito.
- **Tratamiento aplicado:** Conservado como texto para no perder ceros iniciales ni guiones. Valores faltantes/marcadores especiales estandarizados a NA.

## DEPARTAMENTO
- **Descripción:** Departamento de Guatemala donde se ubica el establecimiento.
- **Tipo de dato:** Factor
- **Dominio permitido:** Catálogo oficial de 22 departamentos de Guatemala.
- **Valores posibles:** 21 departamentos presentes (Petén no disponible en el portal de origen).
- **Tratamiento aplicado:** Mayúsculas y espacios normalizados. Los 2,159 registros con el valor ficticio "CIUDAD CAPITAL" (código "00") se reasignaron a "GUATEMALA", preservando la zona original en `ZONA_CIUDAD`.

## MUNICIPIO
- **Descripción:** Municipio donde se ubica el establecimiento.
- **Tipo de dato:** Factor
- **Dominio permitido:** Catálogo oficial de municipios asociados a su departamento (340 a nivel nacional).
- **Valores posibles:** 318 municipios presentes en el conjunto de datos tras la corrección.
- **Tratamiento aplicado:** Mayúsculas y espacios normalizados. Los valores "ZONA 1" a "ZONA 25" se reasignaron a "GUATEMALA". Se corrigieron 3 errores ortográficos (QUEZALTEPEQUE, PACHALUN, SAN MIGUEL PANAM). 8 variantes que el MINEDUC nombra distinto al catálogo (p. ej. "SAN MIGUEL TUCURU") se documentan como alias válidos, no como error.

## ESTABLECIMIENTO
- **Descripción:** Nombre oficial del centro educativo.
- **Tipo de dato:** Texto (character)
- **Dominio permitido:** Texto libre, sin dominio cerrado.
- **Valores posibles:** Nombre tal como lo registra el MINEDUC; no se corrige ortografía por lineamiento del proyecto.
- **Tratamiento aplicado:** Eliminación de espacios/caracteres invisibles y puntuación repetida. Duplicados parciales por similitud Jaro-Winkler (≥0.93) dentro del mismo municipio; casos con similitud 1 documentados para revisión manual, sin eliminar registros.

## DIRECCION
- **Descripción:** Dirección física del establecimiento.
- **Tipo de dato:** Texto (character)
- **Dominio permitido:** Texto libre.
- **Valores posibles:** 65 valores faltantes (0.57 %).
- **Tratamiento aplicado:** Normalización de espacios, caracteres invisibles y puntuación repetida. Valores faltantes conservados como NA, sin imputar.

## TELEFONO
- **Descripción:** Número de contacto del establecimiento.
- **Tipo de dato:** Texto (character)
- **Dominio permitido:** 8 dígitos numéricos con formato `NNNN-NNNN`.
- **Valores posibles:** 1,129 valores faltantes (9.95 %); el resto sigue el formato `NNNN-NNNN`.
- **Tratamiento aplicado:** Se removió cualquier carácter no numérico (guiones, "FAX", comas). Los que quedaron con exactamente 8 dígitos se reformatearon; el resto se marcó como NA. El valor original se conserva en `TELEFONO_original`.

## SUPERVISOR
- **Descripción:** Nombre del supervisor educativo asignado al establecimiento.
- **Tipo de dato:** Texto (character)
- **Dominio permitido:** Texto libre.
- **Valores posibles:** 511 valores faltantes (4.50 %).
- **Tratamiento aplicado:** Normalización de espacios y caracteres invisibles. Valores faltantes conservados como NA, sin imputar.

## DIRECTOR
- **Descripción:** Nombre del director del establecimiento.
- **Tipo de dato:** Texto (character)
- **Dominio permitido:** Texto libre.
- **Valores posibles:** 1,724 valores faltantes (15.20 %); es la variable con más datos faltantes del conjunto.
- **Tratamiento aplicado:** El marcador "--" (y equivalentes) se reemplazó por NA. Normalización de espacios y caracteres invisibles. No se imputan nombres de directores.

## NIVEL
- **Descripción:** Nivel educativo que ofrece el establecimiento (variable cualitativa ordinal).
- **Tipo de dato:** Factor
- **Dominio permitido:** Niveles del sistema educativo guatemalteco, filtrados hasta Diversificado.
- **Valores posibles:** DIVERSIFICADO (único valor, por el filtro aplicado en la obtención de datos).
- **Tratamiento aplicado:** Mayúsculas y espacios normalizados; convertida a factor.

## SECTOR
- **Descripción:** Sector administrativo al que pertenece el establecimiento.
- **Tipo de dato:** Factor
- **Dominio permitido:** {OFICIAL, PRIVADO, MUNICIPAL, COOPERATIVA}
- **Valores posibles:** COOPERATIVA, MUNICIPAL, OFICIAL, PRIVADO.
- **Tratamiento aplicado:** Mayúsculas y espacios normalizados; convertida a factor.

## AREA
- **Descripción:** Área geográfica donde se ubica el establecimiento.
- **Tipo de dato:** Factor
- **Dominio permitido:** {URBANA, RURAL, SIN ESPECIFICAR}
- **Valores posibles:** RURAL, SIN ESPECIFICAR, URBANA.
- **Tratamiento aplicado:** Mayúsculas y espacios normalizados; convertida a factor. "SIN ESPECIFICAR" se conserva como categoría propia del MINEDUC, no como valor faltante.

## STATUS
- **Descripción:** Estado operativo del establecimiento.
- **Tipo de dato:** Factor
- **Dominio permitido:** {ABIERTA, CERRADA DEFINITIVAMENTE, CERRADA TEMPORALMENTE, TEMPORAL NOMBRAMIENTO, TEMPORAL TITULOS}
- **Valores posibles:** Los 5 valores del dominio están presentes.
- **Tratamiento aplicado:** Mayúsculas y espacios normalizados; convertida a factor.

## MODALIDAD
- **Descripción:** Modalidad lingüística de enseñanza.
- **Tipo de dato:** Factor
- **Dominio permitido:** {MONOLINGUE, BILINGUE}
- **Valores posibles:** BILINGUE, MONOLINGUE.
- **Tratamiento aplicado:** Mayúsculas y espacios normalizados; convertida a factor.

## JORNADA
- **Descripción:** Jornada horaria en la que opera el establecimiento.
- **Tipo de dato:** Factor
- **Dominio permitido:** {MATUTINA, VESPERTINA, NOCTURNA, DOBLE, INTERMEDIA, SIN JORNADA}
- **Valores posibles:** Los 6 valores del dominio están presentes.
- **Tratamiento aplicado:** Mayúsculas y espacios normalizados; convertida a factor.

## PLAN
- **Descripción:** Plan de estudios bajo el cual opera el establecimiento.
- **Tipo de dato:** Factor
- **Dominio permitido:** 13 categorías administrativas (DIARIO(REGULAR), FIN DE SEMANA, SABATINO, DOMINICAL, A DISTANCIA, VIRTUAL A DISTANCIA, entre otras).
- **Valores posibles:** 13 valores distintos presentes en el conjunto de datos.
- **Tratamiento aplicado:** Mayúsculas y espacios normalizados; convertida a factor.

## DEPARTAMENTAL
- **Descripción:** Dirección Departamental de Educación responsable administrativamente del establecimiento.
- **Tipo de dato:** Factor
- **Dominio permitido:** 25 direcciones departamentales (algunos departamentos, como Guatemala y Quiché, se dividen en varias direcciones departamentales).
- **Valores posibles:** 25 valores distintos presentes (p. ej. GUATEMALA NORTE, GUATEMALA SUR, GUATEMALA ORIENTE, GUATEMALA OCCIDENTE, QUICHÉ, QUICHÉ NORTE).
- **Tratamiento aplicado:** Mayúsculas y espacios normalizados; convertida a factor. Se verificó consistencia con DEPARTAMENTO.

## ZONA_CIUDAD *(variable derivada)*
- **Descripción:** Número de zona de la Ciudad de Guatemala, extraído del valor original de MUNICIPIO cuando este era "ZONA N".
- **Tipo de dato:** Texto (character)
- **Dominio permitido:** Números del 1 al 25, o NA para el resto del país.
- **Valores posibles:** 9,188 NA (establecimientos fuera de la ciudad capital); 2,159 registros con número de zona.
- **Por qué se creó:** para no perder el detalle de la zona al reasignar "CIUDAD CAPITAL"/"ZONA N" a GUATEMALA/GUATEMALA.
- **Cómo se calculó:** se extrajo el número contenido en el MUNICIPIO original con una expresión regular.
- **Utilidad:** permite análisis a nivel de zona de la ciudad capital sin alterar la estructura estándar de departamento/municipio.

## TELEFONO_original *(variable derivada)*
- **Descripción:** Valor de TELEFONO tal como venía en el archivo crudo, antes de normalizar el formato.
- **Tipo de dato:** Texto (character)
- **Dominio permitido:** Texto libre (formatos variados: con/sin guion, con "FAX", separación por coma, etc.).
- **Valores posibles:** Igual completitud que TELEFONO antes de la limpieza.
- **Por qué se creó:** para dejar trazabilidad de la limpieza de TELEFONO y poder auditar cada reformateo.
- **Cómo se calculó:** copia directa de TELEFONO antes de aplicarle la limpieza de formato.
- **Utilidad:** permite verificar manualmente cualquier número que haya quedado como NA tras la limpieza.
