# PROGRESO del CRM MKT/BS Ferias

Última actualización: 2026-10-21

## Dónde estamos

**Fase 0 — Preparación.** La Mac Mini está lista, Atomic CRM funciona en local y la base de datos de desarrollo ya está en la nube (Supabase). El CRM ya está en internet (https://crm-ferias.vercel.app) y Aldo creó su usuario administrador. Cada rama de GitHub genera su propia vista previa en Vercel. El plan detallado está en [PLAN-FASE-0-1.md](PLAN-FASE-0-1.md) y **espera la aprobación de Aldo**.

## Hecho

### 2026-10-21 — Preparación de la Mac Mini
- Confirmado el equipo: Mac Mini M4, 32 GB de RAM, macOS 27, usuario `aldo_nieto`.
- Ya estaban instalados: git, Homebrew, GitHub CLI (con sesión de AldoNieto) y Node 26.
- Instalado **Node 24**, que es la versión que pide el proyecto (archivo `.nvmrc`). Aldo había pedido Node 22, pero Atomic CRM ya exige 24. Convive con el Node 26 sin reemplazarlo.
- Instalado **Docker** usando **Colima**: un motor de Docker gratuito y sin ventanas, que no pide aceptar licencias ni contraseña de administrador. Quedó configurado para **arrancar solo** al iniciar sesión.
- Revisado que la Mac Mini **no se suspende** (ya estaba así) y que se **reinicia sola** tras un corte de luz.
- Clonado el fork en `~/crm-ferias`.
- Atomic CRM funcionando en local: base de datos Supabase local en Docker + la aplicación web. Se creó un usuario de prueba **solo local** (no existe en internet).
- Creados `CONTEXTO-MKTBS.md` (todo el encargo de Aldo, que Claude lee en cada sesión) y este `PROGRESO.md`.

### 2026-10-21 — Supabase en la nube
- Aldo creó su cuenta de Supabase (entrando con GitHub), la organización **MKT BS Ferias** (plan Free) y el proyecto **crm-ferias-dev** (región EE. UU. Este). Guardó la contraseña de la base en su gestor; Claude no la tiene ni la necesita.
- Aldo conectó la Mac Mini a su cuenta de Supabase (`supabase login`).
- Claude cargó las tablas de Atomic CRM en `crm-ferias-dev` y desplegó sus funciones en la nube (invitar usuarios, cambiar contraseña, etc.).
- Los datos de conexión quedaron en `.env.production.local`, que git ignora: no se suben a GitHub.
- También: la cuenta de Vercel ya existe (plan Hobby, con verificación en dos pasos).
- Incidente reparado: al instalar Node 24 se actualizó una librería compartida y el Node 26 dejó de abrir. Se arregló actualizando Node 26 a 26.12.

### 2026-10-21 — Vercel y primera publicación
- La Mac Mini quedó conectada a Vercel (`vercel login`) y se creó el proyecto **crm-ferias** (plan Hobby).
- En Vercel se cargaron la dirección de Supabase y su clave pública (no son secretas, pero no van en el repositorio).
- Primera publicación: https://crm-ferias.vercel.app (rama `main` = esta dirección).
- En Supabase se configuró la dirección del sitio y las direcciones permitidas para las vistas previas.
- Aldo creó su usuario administrador desde el navegador: funciona.
- Aldo instaló la app de Vercel en GitHub y Claude conectó el repositorio: cada rama nueva tendrá su enlace de vista previa.

## Pendiente

- Aprobación de Aldo del plan de Fases 0 y 1.
- Activar el **inicio de sesión automático** en la Mac Mini (requiere a Aldo frente a ella o por Compartir Pantalla). Sin esto, después de un corte de luz la Mac enciende pero Docker no arranca hasta que alguien inicie sesión.
- Pasar Vercel a Pro antes de la Fase 6 (producción con vendedores).
- Aldo: abrir el CRM en el iPhone e iniciar sesión.
- Apagar dos automatismos de GitHub del proyecto original (`deploy.yml` y `claude.yml`) antes de unir nada a `main`: necesita el visto bueno de Aldo.
- Fase 1: Aldo explicará cómo funciona la cuota del vendedor antes de modelarla.
- Fase 1: decidir si se pasa el repositorio a privado (explicado a Aldo, espera respuesta).

## Decisiones tomadas

| Fecha | Decisión | Por qué |
|---|---|---|
| 2026-10-21 | Node 24 en vez de 22 | Es la versión que exige el proyecto. |
| 2026-10-21 | Docker con Colima en vez de Docker Desktop | Gratis para empresas, sin ventanas, arranca solo y no requiere estar frente a la Mac. |
| 2026-10-21 | El contexto del proyecto va en `CONTEXTO-MKTBS.md`, y `CLAUDE.md` solo lo enlaza | `CLAUDE.md` ya existía (lo trae Atomic CRM). Así no chocamos con sus actualizaciones futuras. |
| 2026-10-21 | Vercel y Supabase: se entra con el GitHub personal de Aldo, pero lo de adentro va a nombre de MKT/BS (organización/equipo) | Los datos y la facturación son de la empresa y se pueden traspasar. |
| 2026-10-21 | Supabase de desarrollo en EE. UU. Este (Americas) | Opción por defecto; buena conexión desde Lima. |
| 2026-10-21 | Vercel en plan Hobby (gratis) por ahora | Decisión de Aldo; se pasa a Pro antes de abrirlo a los vendedores. |
| 2026-10-21 | Ferias: feria (marca) + edición (EPI → EPI 2027) | Sirve también para el futuro ERP. |
| 2026-10-21 | RUC único pero opcional | Hay empresas sin RUC (por ejemplo, extranjeras). |
| 2026-10-21 | Moneda de los stands: dólares | Decisión de Aldo. |
| 2026-10-21 | Cada vendedor ve solo sus empresas y oportunidades; el supervisor ve todo | Decisión de Aldo. |
| 2026-10-21 | Una rama por fase; esta es `fase-0-preparacion` | Regla de Aldo. |

## Cómo retomar (para Claude)

```bash
cd ~/crm-ferias
export PATH="/opt/homebrew/opt/node@24/bin:$PATH"
npx supabase start   # base de datos local (Docker/Colima ya arrancó solo)
npm run dev          # aplicación en http://localhost:5173
```
