# Diseño funcional (Fase 0)

- **Versión en vivo:** [Turnia — Diseño funcional](https://claude.ai/artifact/XkUzxcR3ui5szsdsdtwutp). Pulsa Play en «1 · Inicio» para recorrer la app completa.
- **Copia local:** [prototipo/](prototipo/). Cada `.dc.html` es una pantalla y [prototipo/canvas.json](prototipo/canvas.json) indica cómo se ubican en el lienzo.

## Pantallas
| # | Archivo | Qué muestra |
| --- | --- | --- |
| 1 | `Main.dc.html` | App completa, empezando por el onboarding: cuenta, negocio y sede con la forma de repartir la jornada |
| 2 | `Equipo.dc.html` | Lista de personas, valor de su hora y formulario: forma de pago, jornada y día de descanso |
| 3 | `Horario.dc.html` | Tablero por día: costo de la semana, alertas y costo de cada turno |
| 4 | `HorarioPersona.dc.html` | Tablero por persona: horas frente a la jornada pactada |
| 5 | `Cubrir.dc.html` | "¿Quién cubre este turno?" ordenado por costo |
| 6 | `Turno.dc.html` | Desglose del costo de un turno con la regla aplicada |
| 7 | `Escenarios.dc.html` | Comparación A/B de costo, horas extra, horas cubiertas y alertas |
| 8 | `Publicar.dc.html` | Envío por WhatsApp y exportación para el contador |
| 9 | `Cuenta.dc.html` | Prueba gratis, planes, pago por Bre-B y ajustes |
| 10 | `VistaEmpleado.dc.html` | Enlace público del empleado (sin salarios) |
| 11 | `Escritorio.dc.html` | Tablero semanal en computador |
| 12 | `Calculadora.dc.html` | Calculadora gratuita de recargos |
| 13 | `Landing.dc.html` | Página de inicio pública |

Las pantallas 2 a 9 abren la misma app (`Main.dc.html`) en distintos puntos. La función `motor()` de `Main.dc.html` es la implementación de referencia de las reglas de la sección 4 del [ROADMAP](../ROADMAP.md). Es un prototipo: en la Fase 1 se reescribe en TypeScript con pruebas.

## Sistema visual
- **Tipografías:** Bricolage Grotesque para títulos y Manrope para la interfaz, con cifras tabulares.
- **Colores:**
  - tinta `#16231F`;
  - fondo `#F3F4EF`;
  - verde Turnia `#0E6B57`.
- **Colores por tipo de hora:**
  - nocturna, índigo `#3D47A8`;
  - dominical o festiva, ámbar `#E39A2B`;
  - extra, bermellón `#D9592B`.
- **Celular:** botones de mínimo 44 px de alto, pensados para usarse con una mano.
