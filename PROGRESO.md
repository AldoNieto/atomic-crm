# PROGRESO del CRM MKT/BS Ferias

Última actualización: 2026-10-21

## Dónde estamos

**Fase 0 — Preparación.** La Mac Mini está lista y Atomic CRM funciona en local. Falta: cuentas de Supabase y Vercel, y la primera vista previa en internet. El plan detallado está en [PLAN-FASE-0-1.md](PLAN-FASE-0-1.md) y **espera la aprobación de Aldo**.

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

## Pendiente

- Aprobación de Aldo del plan de Fases 0 y 1.
- Activar el **inicio de sesión automático** en la Mac Mini (requiere a Aldo frente a ella o por Compartir Pantalla). Sin esto, después de un corte de luz la Mac enciende pero Docker no arranca hasta que alguien inicie sesión.
- Cuenta de Supabase y proyecto de desarrollo en la nube.
- Cuenta de Vercel y primera vista previa abierta desde el iPhone.
- Recordatorio para la Fase 1: pasar el repositorio a privado.

## Decisiones tomadas

| Fecha | Decisión | Por qué |
|---|---|---|
| 2026-10-21 | Node 24 en vez de 22 | Es la versión que exige el proyecto. |
| 2026-10-21 | Docker con Colima en vez de Docker Desktop | Gratis para empresas, sin ventanas, arranca solo y no requiere estar frente a la Mac. |
| 2026-10-21 | El contexto del proyecto va en `CONTEXTO-MKTBS.md`, y `CLAUDE.md` solo lo enlaza | `CLAUDE.md` ya existía (lo trae Atomic CRM). Así no chocamos con sus actualizaciones futuras. |
| 2026-10-21 | Una rama por fase; esta es `fase-0-preparacion` | Regla de Aldo. |

## Cómo retomar (para Claude)

```bash
cd ~/crm-ferias
export PATH="/opt/homebrew/opt/node@24/bin:$PATH"
npx supabase start   # base de datos local (Docker/Colima ya arrancó solo)
npm run dev          # aplicación en http://localhost:5173
```
