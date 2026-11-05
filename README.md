# Ayudas Baleares · versión 1

Buscador de ayudas y trámites de las Illes Balears (Mallorca, Menorca, Eivissa y Formentera).
Primera versión centrada en **vivienda y alquiler**, más los trámites en los que más gente se pierde:
NIE/TIE, DNI, permiso de conducir, ITV por isla, empadronamiento, tarjeta sanitaria, descuento de
residente, Targeta Única de transporte, inscripción en el SOIB, ingresos mínimos y **pago de multas**
(DGT, Ajuntament de Palma y resto de ayuntamientos o Govern).

Disponible en **español, inglés y alemán**.

Datos revisados el **5 de noviembre de 2026**.

## Archivos

| Archivo | Para qué sirve |
|---|---|
| `index.html` | La web: diseño y funcionamiento. No contiene datos de ayudas. |
| `ayudas.json` | **Todas las fichas.** Es el único archivo que hay que tocar para actualizar ayudas. |
| `textos.es.json`, `textos.en.json`, `textos.de.json` | Textos de la interfaz en cada idioma. |
| `fichas.en.json`, `fichas.de.json` | Traducción de los textos de cada ficha (nombre, resumen, requisitos…). Las fechas, enlaces y estados salen siempre de `ayudas.json`. |
| `README.md` | Este documento. |

## Cómo abrir la web en tu ordenador

El navegador no deja leer archivos JSON si abres `index.html` con doble clic, así que hace falta
un pequeño servidor local. Con Python (viene instalado en Mac y Linux):

```bash
cd ayudas-baleares
python3 -m http.server 8000
```

Luego abre <http://localhost:8000> en el navegador. Para pararlo, pulsa `Ctrl + C`.

Para publicarla en internet gratis puedes subir la carpeta tal cual a GitHub Pages, Netlify o
Cloudflare Pages: no necesita base de datos ni servidor propio.

## Cómo actualizar o añadir una ayuda

Abre `ayudas.json` con cualquier editor de texto (por ejemplo VS Code) y edita la ficha. Cada ficha
tiene estos campos:

| Campo | Qué poner |
|---|---|
| `id` | Identificador único, sin espacios ni tildes. |
| `tipo` | `ayuda` o `tramite`. |
| `nombre`, `organismo` | Nombre y quién lo gestiona. |
| `categoria` | Una de las claves de `categorias` (alquiler, vivienda, documentacion, vehiculo, transporte, salud, empleo, ingresos). Puedes añadir más. |
| `interesaA` | Lista de perfiles a los que puede interesar. |
| `islas` | Islas donde se aplica. |
| `municipios` | Vacío `[]` si vale para toda la isla; si no, la lista de municipios. |
| `resumen` | Explicación en lenguaje sencillo. |
| `requisitos` | **Solo los que publica la fuente oficial.** Vacío `[]` si no se han comprobado: la web mostrará «Consulta la convocatoria oficial». |
| `importe` | Texto, o `null` si no está confirmado. |
| `plazo` | `{ "inicio": "AAAA-MM-DD", "fin": "AAAA-MM-DD" }` o `null`. |
| `estado` | `abierta`, `proxima`, `cerrada`, `por-confirmar` o `permanente` (trámites sin plazo). Si hay `plazo`, la web calcula el estado sola según la fecha de hoy. |
| `comprobado` | Fecha en que revisaste la ficha en la fuente oficial. |
| `verificacion` | `completa` o `parcial`. Con `parcial`, la ficha muestra «Ficha con datos sin confirmar». |
| `enlace`, `enlaceTexto` | Página oficial principal y, opcionalmente, el texto del botón. |
| `enlacesExtra` | Otras páginas oficiales útiles. |
| `pendientes` | Lista de datos que no has podido confirmar. Se muestran en amarillo. |
| `notas`, `codigoSIA` | Opcionales. |
| `criterios` | Para el cuestionario (ver abajo). |

Antes de guardar, comprueba que el JSON es válido (por ejemplo en <https://jsonlint.com>). Una coma
de más impide que se cargue la web.

### Criterios del cuestionario

El cuestionario solo muestra una ficha si alguna respuesta encaja con sus criterios, y la oculta si
una respuesta los contradice. Las preguntas sin responder no excluyen nada.

| Criterio | Ejemplo | Efecto |
|---|---|---|
| `edadMax` | `35` | Excluye si la persona es mayor. |
| `vivienda` | `["alquilo-vivienda","alquilo-habitacion","quiero-comprar","propia","otra"]` | Situación de vivienda. |
| `empleo` | `["trabaja","paro","estudia","jubilado","otro"]` | Situación laboral. |
| `nacionalidad` | `["es","ue","otra"]` | Nacionalidad. |
| `vehiculo` | `true` | Tiene vehículo o carné. |
| `ingresosBajos` | `true` | Hogar con ingresos bajos. |
| `residenciaMin` | `5` | Años mínimos en Baleares. |
| `general` | `true` | Trámite útil para cualquier residente. |
| `avisos` | `["discapacidad","familia"]` | Añade una nota si aplica, sin excluir. |

## Idiomas

La web tiene botones ES, EN y DE. Recuerda el idioma elegido y, la primera vez, usa el del navegador.

- **Al añadir o cambiar una ficha en `ayudas.json`**, añade o actualiza su traducción en `fichas.en.json` y `fichas.de.json`, con la misma clave `id`. Si falta, la ficha se muestra en español.
- En las traducciones, `requisitos`, `interesaA` y `pendientes` deben tener los mismos elementos y en el mismo orden que en español. `extras` traduce, en orden, los textos de `enlacesExtra`.
- Los nombres oficiales (empadronamiento, NIE, Targeta Única…) se mantienen y el buscador encuentra las fichas también por su nombre en español.

**Para añadir otro idioma (por ejemplo, catalán):**

1. Copia `textos.es.json` como `textos.ca.json` y traduce los valores (no las claves).
2. Copia `fichas.en.json` como `fichas.ca.json` y traduce los textos.
3. En `index.html`, añade `"ca"` a la lista `IDIOMAS` y un botón `<button type="button" data-lang="ca" lang="ca">CA</button>` junto a los demás.

## Fuentes oficiales consultadas (5/11/2026)

- Bono Alquiler Joven 2026-2027: [sede electrónica](https://www.caib.es/seucaib/es/200/personas%20/tramites/tramite/6513013) y [web del Bono](https://www.caib.es/sites/bolloguerjove/es/bo_lloguer_jove/)
- Ayudas al alquiler: [BOIB](https://www.caib.es/eboibfront/), [resolución de 2026 sobre la convocatoria 2025](https://intranet.caib.es/eboibfront/pdf/es/2026/67/1220081)
- Ayuda de 10.000 € para comprar vivienda: [DG de Vivienda y Arquitectura](https://www.caib.es/sites/dgha/es/ajudes_adquisicio_habitatge/)
- Nova Hipoteca Jove: [requisitos](https://www.caib.es/sites/nova_hipoteca_jove/es/requisitos/) y [plazo](https://www.caib.es/sites/nova_hipoteca_jove/es/plazo_de_presentacian/)
- Registro de demandantes de vivienda protegida: [sede electrónica](https://www.caib.es/seucaib/es/200/personas/tramites/tramite/658369), [preguntas frecuentes](https://www.caib.es/sites/informaciohabitatge/es/preguntes_frecuents_v2/)
- ITP primera vivienda: [consulta del Consell Fiscal](https://www.caib.es/sites/consellfiscal/es/d/cons_08-25/)
- Cita previa de extranjería: [sede de Administraciones Públicas](https://sede.administracionespublicas.gob.es/pagina/index/directorio/icpplus)
- DNI y pasaporte: [citapreviadnie.es](https://www.citapreviadnie.es/)
- Permiso de conducir: [sede de la DGT](https://sede.dgt.gob.es/es/permisos-de-conducir/obtencion-y-gestion-de-permisos/)
- ITV: [Consell de Mallorca](https://www.conselldemallorca.es/es/itv), [Consell Insular de Menorca](https://www.cime.es/OrgansGovern/Servei.aspx?ID=10020), [Consell d'Eivissa](https://www.conselldeivissa.es/es/departamento-territorio-mobilidad-infraestructuras-intrusismo-oturistica/servicio-itv), [ITV de Formentera](https://itv.conselldeformentera.cat/)
- Empadronamiento (ejemplo de Palma): [hoja informativa](https://seuelectronica.palma.es/portal/PALMA/sede/RecursosWeb/DOCUMENTOS/1/58_2820_2.pdf)
- Descuento de residente: [Servicio de Transporte Aéreo del Govern](https://www.caib.es/sites/transportaeri/es/descuentos_de_residente-3903/)
- Tarjeta sanitaria: [sede electrónica](https://www.caib.es/seucaib/es/200/personas/tramites/tramite/5339364)
- Targeta Única: [TIB](https://www.tib.org/es/billetes-y-tarifas/tarjeta-unica), [preguntas frecuentes del Govern](https://www.caib.es/webgoib/es/w/targeta-unica-de-transport-public-de-mallorca), [gratuidad 2026](https://www.tib.org/es/w/el-transport-p%C3%BAblic-interurb%C3%A0-mant%C3%A9-la-gratu%C3%AFtat-l-any-2026)
- Multas de la DGT: [pago de multas](https://sede.dgt.gob.es/es/multas/pago-de-multas/) y [qué hacer si has recibido una multa](https://www.dgt.es/nuestros-servicios/multas-y-sanciones/que-hacer-si-has-recibido-una-multa/)
- Multas de Palma: [información del Ajuntament](https://mobipalma.mobi/ca/mobilitat/tramits-i-gestions/multes/), [pago por internet](https://seuelectronica.palma.cat/acceso-pagar-por-internet)
- Multas de otros ayuntamientos y del Govern: [Cartero Virtual de la ATIB](https://www.atib.es/TL/contenidoCartero.aspx?Id=12200&lang=es)
- Inscripción en el SOIB: [sede electrónica](https://caib.es/seucaib/es/200/personas%20/tramites/tramite/6204695)
- Renta Social Garantizada: [web de la RESOGA](https://www.caib.es/sites/rendasocialgarantida/es/requisitos_para_acceder_a_la_prestacion/) y [sede electrónica](https://www.caib.es/seucaib/es/tramites/tramite/2615446/)

## Datos pendientes de verificar

1. **Ayudas al alquiler 2026:** no hay convocatoria oficial publicada. La prensa apunta al primer trimestre de 2027.
2. **ITP:** precio máximo, límites de renta, residencia y préstamo mínimo. Confirmar con la ATIB.
3. **Ayuda de 10.000 €:** si se cierra antes por agotarse el presupuesto.
4. **Renta Social Garantizada:** la web dice 3 años de residencia y la sede electrónica 12 meses. Importe sin confirmar.
5. **Ingreso Mínimo Vital:** requisitos e importes no revisados en la web de la Seguridad Social.
6. **DNI:** tasa y documentación. **Permiso de conducir:** tasa, exención para mayores de 70 y antelación.
7. **NIE/TIE:** documentación y tasas por trámite.
8. **ITV:** tarifas de Mallorca, Menorca y Formentera; forma actual de pedir cita en Menorca; teléfono de Formentera.
9. **Empadronamiento:** la hoja de Palma es de marzo de 2023; el resto de municipios no se han revisado.
10. **Targeta Única:** condiciones exactas de gratuidad y si incluye el bus urbano de la EMT.
11. **Descuento de residente:** porcentaje en transporte marítimo.
12. **Vivienda protegida:** límites de ingresos.
13. **Multas de Palma:** plazo exacto para pagar con el 50% de reducción.
14. **Multas de otros ayuntamientos:** qué ayuntamientos cobran a través de la ATIB y si el Cartero Virtual permite pagar multas o solo consultarlas.

## Siguientes pasos sugeridos

- Revisar las fichas una vez al mes (los plazos cambian) y actualizar `comprobado`.
- Añadir categorías: familia (cheque canguro, ayudas por hijos), estudios y becas, discapacidad, energía (bono social).
- Añadir ayudas de consells insulares y ayuntamientos grandes (Palma, Eivissa, Maó, Ciutadella, Calvià, Manacor).
- Traducir al catalán.
- Añadir las multas de otros ayuntamientos grandes (Eivissa, Maó, Ciutadella, Calvià, Manacor).
