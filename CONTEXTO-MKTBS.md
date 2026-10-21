# Contexto del proyecto: CRM de MKT/BS Ferias

Este archivo lo carga Claude Code en cada sesión (vía `CLAUDE.md`). Resume el encargo de Aldo del 2026-10-21. Lo que está aquí manda sobre las instrucciones genéricas de Atomic CRM cuando haya conflicto.

## Con quién trabajas

- Aldo es el dueño y supervisor. **No es programador**: entiende conceptos, pero hay que tratarlo como principiante.
- Háblale **siempre en español**.
- Antes de cada paso, explica en palabras simples qué vas a hacer y por qué. Antes de ejecutar un comando, di qué hace.
- Si Aldo tiene que hacer algo (crear una cuenta, copiar una clave), dale instrucciones paso a paso: qué botón pulsar y qué debería ver.
- Trabaja por fases pequeñas y detente al final de cada una para que él verifique.
- Si algo es ambiguo o hay que elegir entre opciones, pregúntale antes de decidir.

## Entorno

- **Mac Mini M4 (32 GB)** en la oficina: aquí vive el proyecto (`~/crm-ferias`) y aquí trabaja Claude Code. Es para desarrollo y automatizaciones, **no** para producción.
- Aldo se conecta desde su MacBook Pro (casa) o su iPhone. Muchas veces **no está frente a la Mac Mini**: avísale con anticipación si un paso requiere estar físicamente ahí (por ejemplo, aprobar un permiso del sistema).
- Aldo debe poder ver el CRM desde cualquier lugar: **Supabase en la nube** (un proyecto de desarrollo, separado del de producción) y **vistas previas de Vercel por rama**, con enlaces que abran en el iPhone. `localhost` sirve solo para pruebas internas de Claude.
- Para que revise algo: dale el enlace de la vista previa y dile qué probar.
- La Mac Mini no debe suspenderse y Docker debe arrancar solo.

### Herramientas instaladas en la Mac Mini (2026-10-21)

- Node: el proyecto usa **Node 24** (`.nvmrc`). Está instalado con Homebrew como `node@24`, sin reemplazar el Node 26 del sistema. Antes de usar `npm`/`npx` en este proyecto: `export PATH="/opt/homebrew/opt/node@24/bin:$PATH"`.
- Docker: motor **Colima** (gratuito, sin ventana), instalado con Homebrew y registrado como servicio (`brew services start colima`), así arranca solo al iniciar sesión. 4 CPU, 8 GB de RAM, 60 GB de disco.
- GitHub CLI (`gh`) con sesión iniciada como `AldoNieto`.

## La empresa

MKT/BS Ferias (Perú) organiza unas 12 ferias al año y vende stands a empresas expositoras. Ferias principales: **FIMM, EPI (Expo Perú Industrial), EEE&L y H&C (Hoteles y Cubiertos)**.

- Equipo: hasta unos 10 vendedores y un supervisor (Aldo).
- Base maestra: ~2000 empresas, que crecerá con leads del formulario web y de campañas en Meta (Facebook/Instagram).

## Objetivo

Un CRM que los vendedores abran todos los días para trabajar su pipeline (computadora y celular), y que permita a Aldo monitorear su actividad y resultados.

## Base técnica

- Fork de Atomic CRM: https://github.com/AldoNieto/atomic-crm (React + shadcn/ui + Supabase, MIT). `upstream` apunta a marmelab/atomic-crm.
- Base de datos y login: Supabase. Publicación y vistas previas: Vercel.
- **El fork es público**: nunca subir claves, contraseñas ni datos de clientes. Usar archivos `.env*.local` (ignorados por git) y verificar antes de cada commit. Recordarle a Aldo en la Fase 1 que quiere pasarlo a un repositorio privado.

## El ERP existente (no tocar)

ERP propio a medio terminar: **app.mktbs.pe** ("MKT/BS Ferias"), en Heroku con base de datos en Hostinger. No tenemos su código. **Nunca tocar el ERP en producción ni su base de datos.**

Diseño completo en Figma: https://www.figma.com/design/LHSdwh6jQtMjNtX2lEkJej/ERP (conectar Figma a Claude Code cuando lleguemos a lo visual).

Lo que ya tiene el ERP (no duplicar; el CRM debe alimentarlo):
- Login por rol: Ventas, Administración, Clientes (expositores), Logística, Marketing.
- Ferias con pestañas: FIMM, EPI, EEE&L, H&C.
- Vista del vendedor: cuota mensual, venta efectiva, proyección, necesidad por semana.
- Registro de venta: empresa, RUC, fecha, contacto, teléfono, área/stand, precio lista, precio final, % descuento, inicial, número de cuotas, beneficios.
- Cronograma de pagos por cuota: fecha, monto, estado (Pagado / Vencido / Por vencer).
- Portal del expositor, Administración (por facturar, por cobrar, flujo de caja) y Logística.

**Rol del CRM:** todo lo que pasa ANTES de la venta (prospectos, pipeline, actividades, seguimiento de propuestas). Una oportunidad ganada debe quedar con los mismos datos que una "Nueva venta" del ERP. **El RUC es la llave única** de cada empresa y une ambos sistemas.

**Visión:** si no se recupera el código del ERP, este CRM podría ser el primer módulo de un ERP nuevo sobre la misma base (Supabase), usando el Figma como plano. Diseñar el modelo de datos pensando en eso, sin construir todavía lo que no sea CRM.

**Estilo visual:** el del Figma (logo MKT/BS, naranja como color principal, barra lateral oscura). Debe funcionar bien en celular.

## Fases

0. Preparación: Mac Mini, clonar el fork, correrlo en local, cuentas de Supabase y Vercel, primera vista previa en internet.
1. Modelo de datos: Ferias, RUC en empresas, campos de venta del ERP, cuotas mensuales por vendedor.
2. Pipeline de venta de stands: Nuevo lead → Contactado → Reunión/Interesado → Propuesta enviada → Negociación → Ganado / Perdido; cada oportunidad vinculada a una feria.
3. Importación de las 2000 empresas desde Excel, sin duplicados por RUC, y asignación a vendedores.
4. Entrada automática de leads (formulario web y Meta Lead Ads), con detección de duplicados y asignación automática.
5. Panel de supervisor: actividad por vendedor y día, leads sin atender, oportunidades estancadas, pipeline por feria, avance vs. cuota. Cómodo desde el iPhone.
6. Diseño visual según el Figma, publicación en producción y prueba con 1–2 vendedores antes de abrirlo a todos.

## Reglas de trabajo

- Nunca tocar app.mktbs.pe ni su base de datos.
- Nunca escribir contraseñas ni claves en el código; usar `.env*.local` y verificar que no se suban.
- **Una rama de git por fase** (`fase-0-preparacion`, `fase-1-modelo-datos`, …).
- Commit y push a GitHub al terminar cada paso que funcione, con mensajes claros en español.
- Mantener `PROGRESO.md` al día (hecho, pendiente, decisiones) en lenguaje simple.
