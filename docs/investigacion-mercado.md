# Investigación: cumplimiento, pagos, competencia y mercado

> Corte: 13 de octubre de 2026. Informe de investigación, no asesoría legal ni tributaria.
> Confianza: **alta**, **media** o **baja**.

## A. Protección de datos (Ley 1581 de 2012)
- **Rol de Turnia:**
  - Es **encargado** de los datos de los empleados de sus clientes. El negocio es el responsable (Concepto SIC 23-394496). Confianza alta.
  - Es **responsable** de los datos de sus propios clientes y de quienes se inscriben en la lista del piloto.
  - **No usar** los datos de los empleados para fines propios (comparativos de salarios, marketing), porque eso convertiría a Turnia en responsable de esos datos.
- **Autorización de los empleados:** la obtiene el empleador. Si existe un contrato de transmisión, pasar los datos a Turnia no exige un nuevo consentimiento (art. 2.2.2.25.5.2, Decreto 1074 de 2015).
- **Registro Nacional de Bases de Datos: no aplica.** Solo obliga a sociedades con activos de más de 100.000 UVT (unos $5.237 millones en 2026) y a las personas naturales nunca (Decreto 090 de 2018).
- **Alojamiento en el exterior:**
  - EE. UU. tiene nivel adecuado según la SIC (Circular 005 de 2017), así que basta con firmar los DPAs.
  - Brasil no está en esa lista; hay que evitar la región São Paulo de Supabase.
  - La SIC publicó en mayo de 2026 un proyecto de resolución sobre este tema; hay que vigilarlo.
- **Incidentes y sanciones:**
  - los incidentes se reportan a la SIC en 15 días hábiles;
  - las multas llegan a 2.000 salarios mínimos.
- **Fuentes:** [Concepto SIC 23-394496](https://normograma.com/keralty/compilacion/docs/concepto_superindustria_0394496_2023.htm) · [Decreto 090 de 2018](https://www.saludcapital.gov.co/Normo/tic/decreto_0090_de_2018.pdf) · [Circular SIC 005 de 2017](https://normograma.dian.gov.co/dian/compilacion/docs/circular_superindustria_0005_2017.htm) · [DPA de Supabase](https://supabase.com/legal/dpa)

## B. Facturación e IVA
- **IVA: el software en la nube está excluido** (art. 476, num. 21, ET). El Oficio DIAN 13566 del 3-ago-2026 pide verificar que el servicio cumpla las 5 características esenciales de la nube. Turnia encaja razonablemente (confianza media-alta), pero lo confirma un contador.
- **Persona natural que solo presta servicios excluidos:** no está obligada a facturar mientras sus ingresos sean menores de 3.500 UVT al año, unos $183 millones. Si no factura, el cliente que quiera deducir el gasto debe emitir un documento soporte.
- **Opciones para facturar:**
  - facturador gratuito de la DIAN;
  - Siigo: 5 documentos al año gratis.
- **Fuentes:** [Oficio DIAN 13566 de 2026](https://normograma.dian.gov.co/dian/compilacion/docs/oficio_dian_13566_2026.htm) · [Facturador gratuito de la DIAN](https://incp.org.co/publicaciones/infoincp-publicaciones/impuestos/2025/07/dian-recuerda-que-dispone-de-un-sistema-gratuito-de-facturacion-electronica/)

## C. Pagos
**Bre-B:**
- Al 5-oct-2026 tenía 36,1 millones de clientes, 115 millones de llaves y unos 10 millones de transacciones al día.
- Los comercios tenían 161.468 llaves activas al 1-sep-2026.
- El débito recurrente está en piloto desde julio de 2026, con DRUO.

Costos de las pasarelas, calculados sobre un cobro de $80.000:

| Opción | ¿Cobro recurrente? | Comisión | Costo en $80.000 | Desembolso |
| --- | --- | --- | --- | --- |
| Bre-B | Manual | No publicada | ~0 | Inmediato |
| [Wompi](https://wompi.com/es/co/planes-tarifas/) | Sí (guarda tarjeta, Nequi y Bancolombia; el cobro lo dispara un cron propio) | 2,65% + $700 | ≈ $3.356 | Día hábil siguiente |
| [Mercado Pago](https://www.mercadopago.com.co/developers/es/docs/subscriptions/overview) | Sí (Suscripciones) | 3,29% + $800 | ≈ $4.084 | Inmediato o a 14 días |
| [ePayco](https://epayco.com/tarifas/) | Sí | 3,29% + $700 | ≈ $3.965 | 24 a 72 h, y cada retiro cuesta |
| PayU | Sí, por API | 3,29% + $300 | ≈ $3.489 | No publicado |

## D. Competencia
| Herramienta | ¿Arma turnos? | ¿Costo con recargos colombianos antes de publicar? | Precio |
| --- | --- | --- | --- |
| GeoVictoria, Buk, Humand | Sí | No hay evidencia; calculan para la nómina, después | Cotización (empresa mediana o grande) |
| Deputy, Homebase, When I Work | Sí | Con reglas de EE. UU. | US$2,5–9 por usuario |
| Connecteam | Sí | No | Gratis hasta 10 usuarios |
| Siigo, Alegra, Aleluya | No | Liquidan después | $10.000–117.000 al mes (Siigo); desde $80.000 (Aleluya) |
| Bitákora (Colombia) | Sí | Recalcula recargos por período para la nómina | $13.800 por colaborador al mes |

**Conclusión (confianza media):**
- No encontramos ninguna herramienta que muestre el costo con recargos colombianos mientras se arma el horario, para microempresas, desde el celular y a este precio.
- El competidor real es Excel, el papel o WhatsApp.
- Bitákora y Aleluya son los más cercanos.

## E. Mercado
- **Empleo en el sector:** 1,845 millones de personas trabajan en alojamiento y servicios de comida, el 7,8% del empleo, en agosto de 2026 ([DANE](https://www.dane.gov.co/files/operaciones/GEIH/bol-GEIH-ago2026.pdf)).
- **Informalidad:** 55,3% a nivel nacional y **84,3% en microempresas**.
- **Restaurantes:** unos 110.000, de los cuales unos 32.000 son formales (Acodres, 2024; confianza baja).
- **El precio de $40.000 a $120.000 por sede es coherente (confianza media):**
  - para 8 empleados, $80.000 es el 0,34% de la nómina;
  - evitar unas 11 horas dominicales al mes paga la suscripción;
  - está en línea con Aleluya y Siigo.

## F. Planes gratuitos de infraestructura
- **[Vercel Hobby](https://vercel.com/docs/limits/fair-use-guidelines): no permite uso comercial.** El plan Pro cuesta US$20 por desarrollador al mes.
- **[Supabase Free](https://supabase.com/pricing):** se pausa tras 1 semana sin actividad y no tiene copias de seguridad. El plan Pro cuesta US$25 al mes.
- **[Resend Free](https://resend.com/pricing):** 3.000 correos al mes, con un máximo de 100 al día.
