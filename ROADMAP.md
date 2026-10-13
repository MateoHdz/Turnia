# Roadmap: Turnia, planificador de turnos con costo laboral (Colombia)

> Documento de contexto para trabajar con Claude Code en VS Code.
> Versión 2 · 13 de octubre de 2026 · Autor: Quantum Suscripciones, con Claude.
> Cambios frente a la versión 1: normativa verificada (sección 4), decisiones de producto tomadas en la Fase 0, costos reales de infraestructura, cumplimiento legal del SaaS y orden de fases ajustado.

---

## 0. Instrucciones para Claude Code (leer primero)

1. **Estado actual: Fase 0 entregada, pendiente de aprobación.** El diseño funcional está en el lienzo [Turnia — Diseño funcional](https://claude.ai/artifact/XkUzxcR3ui5szsdsdtwutp) y su copia en [diseno/prototipo/](diseno/prototipo/). No escribas código de producto hasta que el dueño escriba "aprobado".
2. Trabaja por fases, en el orden de la sección 7. Cada fase termina cuando cumple sus criterios y el dueño la aprueba.
3. **El motor de reglas es el corazón del producto.** Usa solo las reglas de la sección 4. Las marcadas como *decisión provisional* se implementan como parámetros, nunca fijas en el código. Si aparece una duda legal nueva, detente y pregunta.
4. Prioriza velocidad y bajo costo: servicios administrados y pocas dependencias.
5. Todo en español de Colombia: interfaz, textos y moneda (`$ 1.750.905`, COP sin decimales).
6. Commits pequeños con mensajes claros y pruebas automáticas para todo cálculo de dinero.

---

## 1. Contexto: qué construimos y por qué

**El problema.** La reforma laboral (Ley 2466 de 2025) y la reducción de jornada (Ley 2101 de 2021) cambiaron el costo de cada hora trabajada:

- El recargo dominical y festivo sube por etapas: 80% desde el 1-jul-2025, **90% desde el 1-jul-2026** y 100% desde el 1-jul-2027.
- La jornada nocturna empieza a las **7:00 p. m.** desde el 25-dic-2025 (antes a las 9:00 p. m.).
- La jornada máxima es de **42 horas semanales** desde el 15-jul-2026, con un máximo ordinario de 8 horas al día.
- Las horas extra tienen tope de 2 diarias y 12 semanales. **Ya no requieren permiso del Ministerio del Trabajo, pero el empleador debe llevar un registro** (art. 12, Ley 2466).

Los dueños de restaurantes, panaderías, droguerías y otros negocios con turnos arman los horarios en papel, en WhatsApp o en Excel. **No saben cuánto les cuesta un turno hasta que llega la nómina.**

**Por qué es un hueco (verificado el 13-oct-2026).** El software de nómina (Siigo, Alegra, Aleluya) liquida *después* de trabajar. Los planificadores extranjeros (Deputy, Homebase, When I Work) calculan con reglas de EE. UU. GeoVictoria, Buk y Humand atienden empresas medianas y grandes, a precio por cotización. No encontramos una herramienta que muestre el costo con recargos colombianos **mientras se arma el horario**, desde el celular y a precio de microempresa. El competidor más cercano es Bitákora: recalcula recargos para la nómina y cobra $13.800 por colaborador al mes.

**La propuesta.** Una app web donde el dueño arma el horario semanal y ve en tiempo real:
- el costo de cada turno, de cada persona y de la semana, separando el salario fijo de lo que se paga aparte (recargos y extras);
- quién puede cubrir un turno al menor costo;
- alertas legales: más de 2 extras al día, más de 12 a la semana y semanas sin día de descanso;
- la comparación de escenarios ("¿turnos de 8 h o de 7 h?").

Después comparte el horario con cada empleado por un enlace de WhatsApp.

**Modelo de negocio.**
- **Calculadora gratuita** de recargos: gancho de tráfico y captura de correos.
- **Planificador por suscripción mensual por sede:**

| Plan | Personas | Precio al mes |
| --- | --- | --- |
| Sede pequeña | hasta 10 | $ 49.000 |
| Sede mediana | 11 a 25 | $ 79.000 |
| Sede grande | 26 a 50 | $ 119.000 |

  Prueba gratis de 30 días, sin tarjeta. Los precios son una hipótesis: el piloto prueba dos niveles.

**Prioridades, en orden:**
1. Salir rápido. La calculadora debe estar en línea **antes del 1-dic-2026**, para captar el pico de búsquedas por el salario mínimo 2027; el MVP del planificador, en 3 o 4 semanas.
2. Costo de operación bajo hasta tener clientes de pago.
3. Cálculos correctos: un error en dinero destruye la confianza.
4. Simple de usar desde el celular.

---

## 2. Usuarios

| Usuario | Qué necesita | Dispositivo |
| --- | --- | --- |
| Dueño o administrador | Armar el horario, ver el costo y evitar multas | Celular (principal) y computador |
| Empleado | Ver su horario de la semana | Celular, por enlace, sin cuenta |
| Contador | Recibir novedades y el registro de horas extra | Computador (archivo exportado) |

---

## 3. Alcance

### Dentro del MVP
- Calculadora pública de recargos, sin registro.
- Registro e inicio de sesión del dueño (Google o enlace por correo), con consentimiento explícito de datos.
- Un negocio con una o más sedes. La interfaz del MVP muestra una sede a la vez.
- **Empleados:**
  - nombre, cargo y celular;
  - forma de pago (salario mensual o por horas);
  - salario o valor de la hora;
  - jornada pactada (horas por semana y días por semana);
  - **día de descanso** (domingo por defecto).
- **Configuración de la sede:**
  - horario de atención;
  - forma de repartir la jornada: 6 días de 7 h, 5 días de 8 h 24 min o jornada flexible pactada por escrito.
- **Plantillas de turno** (por ejemplo "Mañana 7 h", "Cierre 7 h", "Partido").
- **Tablero semanal** con dos vistas, *por día* y *por persona*:
  - crear, editar y eliminar turnos, y copiar la semana anterior;
  - turno partido: dos turnos el mismo día con descanso intermedio.
- **"¿Quién cubre este turno?":** ordena al equipo por el costo de asignarle un turno.
- **Costo en vivo:** por turno, por persona, por día y por semana, separando el salario fijo de los recargos y extras.
- **Alertas legales:** más de 2 h extra al día, más de 12 a la semana y semana sin día de descanso. **Avisos de costo:** horas extra y horas pagadas sin programar.
- Escenarios A y B: duplicar, editar, comparar costo y horas cubiertas, y usar B.
- Publicar y compartir: enlace público de solo lectura por empleado, sin salarios, y botón "Enviar por WhatsApp" con un enlace `wa.me` y el mensaje armado. **Sin** WhatsApp Business API.
- **Exportar** a Excel las novedades de la semana y el **registro de horas extra** (art. 12, Ley 2466).
- Cobro manual de la suscripción (Fase 4).

### Fuera del MVP (no construir todavía)
- Marcación de entrada y salida.
- Nómina completa, prestaciones sociales y seguridad social.
- Integración directa con software de nómina o con la DIAN.
- Apps nativas: el MVP es una web adaptable (PWA).
- Control mensual de habitualidad dominical: 3 o más domingos o festivos trabajados en el mes obligan a dar un descanso compensatorio. En el MVP se muestra solo un aviso.
- Regímenes especiales: dirección y confianza, vigilancia privada, salud, menores de edad y aprendices. Se muestra un aviso y no se calculan.

---

## 4. Reglas laborales (fuente única de verdad)

> Verificadas el 13-oct-2026 con fuentes primarias y especializadas (sección 9 y [docs/investigacion-normativa.md](docs/investigacion-normativa.md)). Es un informe de investigación, no asesoría legal: **un contador laboral debe validar las decisiones provisionales antes del lanzamiento de la calculadora.**

**Principios del motor:**
- Todas las reglas van **versionadas por fecha de vigencia**: el motor recibe la fecha de cada minuto trabajado y aplica la regla vigente ese día.
- **Fecha mínima soportada: 1-ene-2026.** Fechas anteriores se rechazan con un mensaje.

### 4.1 Valor de la hora ordinaria
`valor_hora = salario_mensual / (horas_semanales_pactadas × 5)`

| Vigencia | Jornada máxima semanal | Divisor (tiempo completo) |
| --- | --- | --- |
| 15-jul-2025 a 14-jul-2026 | 44 h | 220 |
| Desde 15-jul-2026 | 42 h | 210 |

- **Tiempo parcial:** se usan sus horas pactadas. Por ejemplo, 24 h × 5 = 120. *Decisión provisional, pregunta C7.*
- **Pago por horas:** se usa el valor pactado por hora.
- **Salario mínimo 2026:** $1.750.905, equivalente a $8.338 por hora con divisor 210.
  - Estado jurídico: el Decreto 1469 de 2025 fue suspendido en febrero de 2026, el Decreto 159 de 2026 mantuvo la misma cifra y en julio de 2026 se revocó la suspensión.
  - El proceso sigue y el valor nunca cambió.
  - **El salario mínimo 2027 se fija entre el 15 y el 31 de diciembre de 2026.** Debe poder configurarse por año.
- **Auxilio de transporte 2026:** $249.095 (Decreto 1470 de 2025). No entra en el valor de la hora.

### 4.2 Franjas
| Vigencia | Diurna | Nocturna |
| --- | --- | --- |
| Hasta 24-dic-2025 | 06:00–21:00 | 21:00–06:00 |
| Desde 25-dic-2025 | 06:00–19:00 | 19:00–06:00 |

Recargo nocturno: 35% (art. 168 CST, sin cambios).

### 4.3 Recargo dominical y festivo (art. 14, Ley 2466)
| Vigencia | Recargo `D` |
| --- | --- |
| 1-jul-2025 a 30-jun-2026 | 80% |
| 1-jul-2026 a 30-jun-2027 | 90% |
| Desde 1-jul-2027 | 100% |

- Si un domingo coincide con un festivo, se paga **un solo** recargo.
- El empleador puede anticipar el 100%: debe ser un parámetro de la sede.

### 4.4 Factores sobre el valor de la hora ordinaria (confirmados)
| Tipo de hora | Factor total | Con D = 90% | Lo que se paga **aparte** del salario mensual |
| --- | --- | --- | --- |
| Ordinaria diurna | 1,00 | 1,00 | 0 (ya va en el salario) |
| Ordinaria nocturna | 1,35 | 1,35 | 0,35 |
| Dominical o festiva diurna | 1 + D | 1,90 | **1 + D** (ver 4.5) |
| Dominical o festiva nocturna | 1 + D + 0,35 | 2,25 | **1 + D + 0,35** |
| Extra diurna | 1,25 | 1,25 | 1,25 |
| Extra nocturna | 1,75 | 1,75 | 1,75 |
| Extra dominical o festiva diurna | 1,25 + D | 2,15 | 2,15 |
| Extra dominical o festiva nocturna | 1,75 + D | 2,65 | 2,65 |

- Coincide con las tablas de Actualícese y de Noticias RCN con divisor 210.
- El recargo nocturno no se suma a la hora extra, porque la extra nocturna ya lo incluye; sí se suma al recargo dominical.

### 4.5 Trabajo en el día de descanso (domingo o festivo)
- **Decisión provisional: el empleado con salario mensual recibe la hora completa más el recargo (1 + D), aparte del salario.** El salario ya paga el día de descanso.
  - Es la posición dominante (MinTrabajo 7105/2018 y 58916/2023; Tribunal Superior de Medellín, S-202 de 2024).
  - La alternativa, pagar solo D, tiene riesgo judicial.
  - El motor la deja como parámetro `descansoCompleto`, activado por defecto. *Pregunta C1.*
- **Día de descanso pactado por escrito** (Ley 2466): el recargo aplica a ese día y a los festivos, y el domingo pasa a ser un día ordinario. Va como campo por empleado, con el aviso de que requiere pacto escrito. *Pregunta C2.*
- **Habitualidad:** se cuentan domingos y festivos trabajados en el mes calendario. Hasta 2 son ocasionales (el trabajador elige compensatorio o dinero). 3 o más son habituales (compensatorio remunerado además del pago).

### 4.6 Horas extra: cómo se determinan
**Decisión provisional, preguntas C4, C5 y C6.**
- **Tope diario de horas ordinarias:**
  - jornada fija: horas pactadas ÷ días pactados (42 ÷ 6 = 7 h), sin pasar de 8 h;
  - 5 días de 8 h 24 min: 8,4 h por el art. 164;
  - jornada flexible pactada por escrito: 9 h.
- **Tope semanal:** las horas pactadas.
- **Orden:** primero se aplica el exceso diario y luego el semanal. El exceso semanal se imputa a las últimas horas de la semana (lunes a domingo, en orden cronológico).
- **Alerta de 2 h extra por día:** cuenta solo el exceso diario. El exceso semanal se controla con el tope de 12 h por semana y la alerta de día de descanso.
- **Turnos que cruzan la medianoche:** cada minuto se clasifica por la hora y la fecha calendario. Las horas después de las 00:00 del domingo son dominicales (Concepto MinProtección 266516 de 2008). Para los topes, el turno cuenta en el día en que **empieza**.
- **Límites:** 2 extras al día y 12 a la semana; es decir, 54 h semanales en total.
  - Excepciones: vigilancia (Ley 1920 de 2018) y salud. En esos casos se avisa en vez de bloquear.
- **Validaciones:**
  - no se permiten dos turnos el mismo día, salvo un turno partido con descanso intermedio, que no cuenta como jornada (art. 167);
  - una jornada flexible debe tener mínimo 4 h continuas.

### 4.7 Festivos de Colombia (19 desde 2026)
- **Fijos:** 1-ene, 1-may, 20-jul, 7-ago, 8-dic y 25-dic.
- **Trasladables al lunes (Ley 51 de 1983):** 6-ene, 19-mar, 29-jun, 15-ago, 12-oct, 1-nov, 11-nov y, **desde 2026, 9-jul (Virgen de Chiquinquirá, Ley 2578 de 2026)**.
- **Según la Pascua:**
  - Jueves y Viernes Santo;
  - Ascensión: Pascua + 43, en lunes;
  - Corpus Christi: Pascua + 64, en lunes;
  - Sagrado Corazón: Pascua + 71, en lunes.
- **Probar contra las listas oficiales:**
  - 2026: 1-ene, 12-ene, 23-mar, 2-abr, 3-abr, 1-may, 18-may, 8-jun, 15-jun, 29-jun, 13-jul, 20-jul, 7-ago, 17-ago, 12-oct, 2-nov, 16-nov, 8-dic, 25-dic.
  - 2027: 1-ene, 11-ene, 22-mar, 25-mar, 26-mar, 1-may, 10-may, 31-may, 7-jun, 5-jul, 12-jul, 20-jul, 7-ago, 16-ago, 18-oct, 1-nov, 15-nov, 8-dic, 25-dic.

### 4.8 Costo semanal que muestra la app
- **Empleado con salario mensual:**
  - salario fijo prorrateado (salario × 7 ÷ 30), que se paga igual sin importar el horario;
  - más los recargos y extras de la semana.
- **Empleado por horas:**
  - horas ordinarias × valor de la hora;
  - más los recargos y extras.
- **"Horas pagadas sin programar":** horas pactadas menos horas ordinarias programadas.
- **No incluye** prestaciones sociales, seguridad social ni auxilio de transporte. La interfaz lo dice.

### 4.9 Dinero, tiempo y redondeo
- El tiempo se maneja en **minutos enteros**.
- El dinero se calcula sin redondear valores intermedios (el valor de la hora no se redondea). **Solo se redondea al peso cada concepto final**, y el total es la suma de los conceptos ya redondeados, para que la pantalla siempre cuadre.
- Los turnos se guardan como **fecha local + minuto de inicio y de fin** en hora de Colombia (UTC−5, sin horario de verano). El motor nunca usa la zona horaria del servidor.

### 4.10 Aviso legal en la interfaz
"Los cálculos son una estimación basada en la normativa vigente y no reemplazan la asesoría de un contador."

### 4.11 Preguntas para el contador (antes de lanzar la calculadora)
1. **C1.** Domingo o festivo trabajado dentro de la jornada pactada, con salario mensual: ¿se paga 1 + D o solo D?
2. **C2.** ¿Es válido el pacto escrito de otro día de descanso? ¿Conviene ofrecerlo?
3. **C3.** Trabajo ocasional en el que el trabajador elige compensatorio: ¿se debe además el recargo D?
4. **C4.** Turnos que cruzan la medianoche: ¿a qué día se imputan las extras para el tope diario? ¿Pocas horas en domingo cuentan para la habitualidad?
5. **C5.** Exceso semanal: ¿a qué horas y días se imputan las extras? ¿Cuentan para el tope de 2 al día?
6. **C6.** Cinco días de 8 h 24 min: ¿basta el art. 164 o hace falta pacto flexible? En jornada flexible, ¿los días de 9 h deben caer entre 6:00 y 19:00?
7. **C7.** Divisor para tiempo parcial y para quien adoptó las 42 h antes del 15-jul-2026.
8. **C8.** Recargos para cargos de dirección o confianza y para aprendices.
9. **C9.** Efecto de los recargos y extras en prestaciones y aportes, para una futura función de costo total.

---

## 5. Tecnología

### Recomendación
| Pieza | Elección | Por qué |
| --- | --- | --- |
| Lenguaje | TypeScript en todo | Un solo lenguaje para interfaz, servidor y motor |
| Framework | Next.js (App Router) | Calculadora estática con buen SEO y app con servidor en el mismo proyecto |
| Interfaz | Tailwind CSS + shadcn/ui | Componentes listos; se ajustan al diseño aprobado |
| Base de datos y usuarios | Supabase (Postgres + Auth + RLS), **región EE. UU.** | EE. UU. tiene nivel adecuado de protección según la SIC; la región São Paulo (Brasil) no lo tiene |
| Hosting | Vercel | Despliegue automático desde GitHub |
| Pruebas | Vitest | Rápidas para el motor |
| Correos | Resend | 3.000 correos al mes gratis |
| Analítica | Plausible o PostHog | Medir la calculadora y el embudo |
| Validación | Zod | Formularios y datos |
| Exportar | SheetJS (xlsx) | Novedades y registro de horas extra |

**Cambio frente a la versión 1:** se quita dnd-kit. En el celular se asignan turnos con plantillas y toques, no arrastrando. El arrastre en computador queda para después del MVP.

### Costos reales
- **Prototipo y desarrollo:** US$0.
- **Desde que la landing anuncia un servicio de pago:**
  - el plan Hobby de Vercel no permite uso comercial, así que se necesita **Vercel Pro (US$20/mes)** o un hosting gratuito que permita uso comercial;
  - **Supabase Pro (US$25/mes)** desde el primer cliente de pago, porque el plan gratuito se pausa tras 7 días sin actividad y no tiene copias de seguridad.
- **Total realista: unos US$45/mes**, más el dominio. Equivale a 3 o 4 suscripciones.

### Pagos y facturación
1. **Primeros 10–20 clientes:** transferencia o llave Bre-B propia con una referencia por sede, conciliadas en el panel de administración y activadas a mano. Cero comisiones.
2. **Después: Wompi**, la opción más barata:
   - 2,65% + $700 por cobro, unos $3.356 en un cobro de $80.000;
   - paga al día hábil siguiente;
   - guarda tarjeta, Nequi y Bancolombia, y los cobros se disparan con un cron propio.
   Mercado Pago Suscripciones queda como plan B. Hay que seguir de cerca el débito recurrente de Bre-B, que está en piloto desde julio de 2026.
3. **IVA:** el software en la nube está **excluido de IVA** (art. 476, num. 21, ET; Oficio DIAN 13566 de 2026). Lo confirma el contador.
4. **Facturación:**
   - empezar como persona natural con el **facturador gratuito de la DIAN**;
   - con servicios excluidos de IVA y menos de 3.500 UVT al año no es obligatorio facturar, pero conviene hacerlo para que los clientes puedan deducir el gasto;
   - decidir con el contador entre el Régimen SIMPLE y una SAS.

### Descartado y por qué
- **App nativa:** duplica el trabajo; la PWA basta.
- **Backend separado:** más piezas sin beneficio en el MVP.
- **No-code:** el motor necesita pruebas y precisión.
- **WhatsApp Business API:** cuesta y exige aprobación; `wa.me` basta para el MVP.

---

## 6. Arquitectura

```
/src
  /lib/rules          <- motor: TypeScript puro, sin interfaz ni base de datos
    ruleset.ts        <- reglas versionadas por fecha (sección 4), parámetros configurables
    holidays.ts       <- festivos de Colombia (incluye Ley 2578 de 2026)
    calculate.ts      <- clasifica cada minuto y calcula el costo por turno, persona, día y semana
    coverage.ts       <- "¿quién cubre este turno?"
    limits.ts         <- alertas legales y avisos de costo
    *.test.ts
  /app
    /(public)/calculadora   <- calculadora gratuita (estática, SEO)
    /(public)/              <- landing
    /(app)/horario          <- tablero semanal (vistas por día y por persona)
    /(app)/equipo
    /(app)/escenarios
    /(app)/publicar
    /(app)/cuenta
    /h/[token]              <- horario público del empleado (solo lectura, noindex, sin salarios)
    /admin                  <- activar y desactivar sedes (cobro manual)
```

**Motor de reglas.**
- **Entrada:** lunes de la semana, empleados (forma de pago, salario o valor de la hora, jornada pactada, días y día de descanso), turnos (día, minuto de inicio y de fin) y configuración de la sede.
- **Proceso:** clasifica cada minuto como diurno o nocturno, ordinario o en día de descanso o festivo, y ordinario o extra; luego aplica los factores vigentes ese día.
- **Salida:**
  - minutos y dinero por tipo, por turno, por persona y por día;
  - salario fijo, recargos y extras;
  - alertas.
- Es una función pura, totalmente probada y compartida por la calculadora y el planificador.
- **Implementación de referencia:** el prototipo de la Fase 0 ([diseno/prototipo/Main.dc.html](diseno/prototipo/Main.dc.html), función `motor()`). Se reescribe en TypeScript con pruebas; no se copia tal cual.

**Modelo de datos (Postgres).**
- `organizations` → `locations` (horario de atención y forma de repartir la jornada) → `employees`.
  - `employees`: nombre, cargo, celular, forma de pago, salario, valor de la hora, horas por semana, días por semana, día de descanso y si hay pacto escrito.
- `shift_templates` (sede, nombre y bloques).
- `schedules`: semana de una sede, con estado `borrador` o `publicado` y nombre del escenario.
- `shifts`: horario, empleado, fecha local, minuto de inicio y de fin, y nota.
- `share_links`: empleado, semana y token aleatorio largo, revocable.
- `subscriptions`: sede, plan, estado y fecha de vencimiento.
- `rule_params`: salario mínimo por año y parámetros del motor, como `descansoCompleto` y anticipar el 100%.
- **Seguridad por filas (RLS) por organización.** La vista pública `/h/[token]` se sirve desde el servidor y nunca expone salarios ni costos.

---

## 7. Fases

### Fase 0: Diseño funcional ✅ entregado el 13-oct-2026, pendiente de aprobación
- **Entregable:** el lienzo [Turnia — Diseño funcional](https://claude.ai/artifact/XkUzxcR3ui5szsdsdtwutp), con 13 pantallas y un motor real que calcula en vivo. Copia en [diseno/prototipo/](diseno/prototipo/).
  - **App del dueño:**
    1. Onboarding.
    2. Equipo.
    3. Horario por día.
    4. Horario por persona.
    5. ¿Quién cubre?
    6. Costo de un turno.
    7. Escenarios A y B.
    8. Publicar y enviar.
    9. Cuenta y suscripción.
  - **Además:**
    10. Vista del empleado.
    11. Horario en computador.
    12. Calculadora gratuita.
    13. Landing.
- **Criterio de salida:** el dueño escribe "aprobado" o pide cambios.

### Fase 0.5: Validación (en paralelo, 1–2 semanas, la hace el dueño)
- 10 a 15 conversaciones con dueños o administradores de restaurantes y panaderías en Cali (guía en el anexo A).
- **Reunión con un contador laboral** para resolver las preguntas C1 a C9 de la sección 4.11. Sus respuestas se convierten en 15–20 casos de prueba resueltos a mano.
- Comprar el dominio y crear la llave Bre-B del negocio.

### Fase 1: Motor de reglas (2–4 días)
- Proyecto base: Next.js, TypeScript, Tailwind, Vitest y repositorio en GitHub.
- Implementar `/lib/rules` según la sección 4.
- **Criterios:**
  - Pruebas para cada tipo de hora y cada cambio de vigencia: 30-jun/1-jul-2026, 14/15-jul-2026 y 30-jun/1-jul-2027.
  - Pruebas para turnos que cruzan la medianoche, día de descanso pactado, festivos 2026–2027 (19 por año), salario mensual y pago por horas.
  - Reproducir la tabla de Noticias RCN y Actualícese con el salario mínimo de 2026 ($8.338 la hora; $15.842 la dominical diurna).
  - Pasar los casos del contador.

### Fase 2: Calculadora gratuita y landing (3–4 días) · **en línea antes del 1-dic-2026**
- Calculadora y landing según el diseño aprobado.
- Lista del piloto (correo, negocio y número de empleados) guardada en Supabase, con consentimiento.
- **Páginas SEO:**
  - "salario mínimo 2027 valor hora" (se publica el día del decreto);
  - "recargo dominical 2026";
  - "recargo nocturno desde las 7 p. m.";
  - "cómo calcular horas extra con 42 horas";
  - "festivos 2027 Colombia".
- Desplegar con dominio propio y analítica.
- **Criterio:** publicada, Lighthouse ≥ 90 en celular y capturando correos.

### Fase 2.5: Base legal del SaaS (1–2 días, antes de la Fase 3)
- Política de tratamiento de datos y aviso de privacidad de Turnia, que es **responsable** de los datos de sus clientes.
- Términos de servicio con anexo de transmisión de datos, porque Turnia es **encargado** de los datos de los empleados de sus clientes.
- Plantilla de autorización para que el cliente la firme con sus empleados.
- Casillas de consentimiento separadas: una para el servicio y otra para recibir información.
- Lista pública de sub-encargados (Vercel, Supabase, Resend y su región).
- DPAs firmados con Supabase y Vercel.
- Procedimientos de consultas (10 días hábiles), reclamos (15 días hábiles) e incidentes (reporte a la SIC en 15 días hábiles).
- El Registro Nacional de Bases de Datos **no aplica** mientras los activos sean menores de 100.000 UVT.

### Fase 3: Planificador MVP (2–3 semanas)
- Autenticación y onboarding.
- Equipo, sede y plantillas.
- Tablero con vistas por día y por persona.
- "¿Quién cubre?" y costo en vivo con el motor.
- Alertas.
- Escenarios A y B.
- Publicar con enlace por empleado y WhatsApp.
- Exportar novedades y registro de horas extra.
- **Criterio:** un dueño arma su semana completa (8 personas) en menos de 10 minutos desde el celular.

### Fase 4: Cobro (2–4 días)
- Panel interno para activar y desactivar sedes y conciliar pagos Bre-B.
- Prueba gratis de 30 días; al vencer, la sede queda en solo lectura.
- Integración con Wompi cuando haya 10 o más clientes de pago.

### Fase 5: Piloto en Cali (2–4 semanas, en paralelo con la Fase 4)
- 10 negocios gratis durante un mes, a cambio de su opinión.
- **Medir:**
  - semanas publicadas por negocio;
  - retención semanal;
  - uso de "¿quién cubre?";
  - errores reportados en los cálculos.
- Probar dos precios ($49.000 y $79.000 para la misma sede) y convertir a pago.

### Fase 6: Después del MVP (según demanda)
- Control mensual de habitualidad dominical y de compensatorios.
- Marcación de asistencia: horas reales frente a programadas.
- Exportar en el formato de Siigo, Alegra y Aleluya; tratarlos como aliados, no como rivales.
- Varias sedes en un solo tablero y arrastrar turnos en computador.
- Costo total para la empresa (prestaciones y seguridad social).
- Plan para contadores que manejan varios clientes.

---

## 8. Riesgos y cómo mitigarlos
| Riesgo | Mitigación |
| --- | --- |
| Error en un cálculo | Motor puro con pruebas, casos del contador, aviso legal y decisiones provisionales como parámetros |
| La norma cambia o se interpreta distinto (C1–C9) | Reglas versionadas por fecha y parámetros por sede; actualizar en diciembre (salario mínimo) y en julio de 2027 (100%) |
| El decreto del salario mínimo 2026 aún está en litigio | El valor es configurable; vigilar la sentencia del Consejo de Estado |
| La mayoría de microempresas son informales (84%) y no pagan recargos | Enfocar en negocios formales con nómina; validar en la Fase 0.5 |
| Bitákora o Aleluya agregan esta función | Velocidad, foco en el celular, "¿quién cubre?" y precio por sede; cercanía con contadores |
| Los dueños no pagan | Validar con la calculadora y el piloto antes de la Fase 6 |
| Datos personales (salarios) | Rol de encargado, contrato de transmisión, RLS, enlace público sin salarios y datos en EE. UU. |

---

## 9. Fuentes principales
- [Ley 2466 de 2025](https://www.cancilleria.gov.co/normograma/compilacion/docs/ley_2466_2025.htm) · [Ley 2101 de 2021](https://www.cancilleria.gov.co/normograma/compilacion/docs/ley_2101_2021.htm) · [Ley 2578 de 2026 (festivo 9-jul)](https://www.alcaldiabogota.gov.co/sisjur/normas/Norma1.jsp?i=193309)
- [Decreto 159 de 2026 (salario mínimo)](https://www.cancilleria.gov.co/normograma/compilacion/docs/decreto_0159_2026.htm) · [Revocatoria de la suspensión, jul-2026](https://www.infobae.com/colombia/2026/07/17/consejo-de-estado-revoco-la-suspension-del-decreto-del-gobierno-petro-que-subio-23-el-salario-minimo-de-2026/)
- [Actualícese: horas extra y recargos 2026](https://actualicese.com/horas-extra-y-recargos-2026-en-colombia/) · [Gerencie: trabajo dominical y festivo](https://www.gerencie.com/trabajo-dominical-y-festivo.html) · [Externado: el recargo dominical en la reforma](https://derlaboral.uexternado.edu.co/analisis-y-opinion/el-desvanecimiento-del-recargo-dominical-en-la-reforma-laboral/)
- [Concepto MinProtección 266516 de 2008 (turno que cruza la medianoche)](https://www.cijuf.org.co/conceptosminproteccion/diciembre/c357565.html)
- [Oficio DIAN 13566 de 2026 (nube excluida de IVA)](https://normograma.dian.gov.co/dian/compilacion/docs/oficio_dian_13566_2026.htm) · [Circular SIC 005 de 2017 (países adecuados)](https://normograma.dian.gov.co/dian/compilacion/docs/circular_superindustria_0005_2017.htm)
- [Wompi: tarifas](https://wompi.com/es/co/planes-tarifas/) · [Vercel: uso justo](https://vercel.com/docs/limits/fair-use-guidelines) · [Supabase: precios](https://supabase.com/pricing)
- Detalle completo: [docs/investigacion-normativa.md](docs/investigacion-normativa.md) y [docs/investigacion-mercado.md](docs/investigacion-mercado.md).

---

## Anexo A. Guía de entrevistas (Fase 0.5)
Unos 20 minutos, en el negocio y en un momento tranquilo. No vender: escuchar.

1. ¿Cuántas personas trabajan por turnos y quién arma el horario?
2. ¿Cómo lo arman hoy (papel, Excel, WhatsApp)? ¿Cuánto tiempo les toma a la semana?
3. ¿Cómo se lo comunican al equipo? ¿Qué pasa cuando alguien falta?
4. ¿Pagan recargo dominical, nocturno y horas extra? ¿Quién los calcula y cuándo se enteran del valor?
5. ¿Les ha llegado una nómina más alta de lo esperado? ¿Por qué fue?
6. ¿Notaron el cambio del recargo dominical al 90% o de la jornada nocturna desde las 7 p. m.?
7. Mostrar el prototipo en el celular: ¿qué entienden? ¿Qué les sobra? ¿Qué les falta?
8. ¿Pagarían $49.000 al mes por sede? ¿Y $79.000? ¿Quién toma esa decisión?
9. ¿Tienen contador? ¿Nos lo presentarían?
