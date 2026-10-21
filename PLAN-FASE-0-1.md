# Plan: cerrar la Fase 0 y hacer la Fase 1

Propuesta del 2026-10-21. **Espera la aprobación de Aldo.** Las preguntas que necesito que respondas están marcadas con ❓.

---

## Primero: cómo está hecho Atomic CRM (en simple)

Piensa en el CRM como un restaurante:

- **La sala (el "frontend")**: lo que ves en la pantalla. Está hecho con **React**, una herramienta para construir pantallas que se actualizan solas. Los botones, tablas y formularios vienen de **shadcn/ui**, una caja de piezas de diseño. Todo el código de pantallas está en `src/components/atomic-crm/`, una carpeta por tema: `companies` (empresas), `contacts` (contactos), `deals` (oportunidades, con el tablero tipo Kanban), `tasks` (tareas), `notes` (notas), `dashboard` (inicio), `sales` (usuarios/vendedores), `settings` (configuración).
- **La cocina (el "backend")**: **Supabase**. Es un servicio que nos da, ya armados: la **base de datos** (PostgreSQL, donde se guardan empresas, contactos, oportunidades), el **login** de usuarios, el **almacenamiento** de archivos y unas pequeñas **funciones** en la nube (por ejemplo, invitar a un vendedor por email).
- **El mesero**: un "proveedor de datos" que lleva los pedidos de la pantalla a Supabase y trae las respuestas. Por eso la pantalla no habla directo con la base de datos.
- **La receta de la base de datos**: está en `supabase/schemas/` (cómo son las tablas). Cada cambio se convierte en una **migración**: un archivo con el paso exacto para pasar de la versión anterior a la nueva. Así el mismo cambio se aplica igual en local, en desarrollo y en producción.
- **Personalización**: el archivo `src/App.tsx` permite cambiar desde un solo lugar las etapas del pipeline, los sectores, el logo, los colores y el título.

Lo que trae de fábrica: empresas, contactos, oportunidades en tablero Kanban, tareas, notas, importación desde CSV, usuarios con rol administrador, versión para celular y modo oscuro.

Lo que **no** trae y tendremos que agregar: español (solo trae inglés y francés), RUC, ferias, cuotas, permisos por vendedor (hoy todos los usuarios ven todo) y el panel de supervisor.

**Un concepto que usaremos mucho: la rama.** Una rama de git es una copia paralela del proyecto donde trabajamos sin tocar la versión principal (`main`). Si la rama funciona, se une a la principal; si no, se descarta sin daño. Cada rama tendrá su propia vista previa en Vercel, con su enlace para el iPhone.

---

## Fase 0 — lo que falta

### Paso 0.1 · Inicio de sesión automático en la Mac Mini ⚠️ requiere estar frente a ella
**Por qué:** si se corta la luz, la Mac se reinicia sola, pero Docker solo arranca cuando alguien inicia sesión.
**Tú haces** (frente a la Mac Mini o por Compartir Pantalla desde la MacBook): Ajustes del Sistema → Usuarios y grupos → "Iniciar sesión automáticamente como" → elige `aldo_nieto` → escribe tu contraseña. Debe quedar tu usuario seleccionado.
**Riesgo a saber:** cualquiera con acceso físico a la Mac la encontrará con sesión abierta. Si la oficina no es segura, lo dejamos sin esto y lo aceptamos.

### Paso 0.2 · Apagar automatismos de Atomic CRM que no nos sirven
**Por qué:** tu fork trae flujos de GitHub de los autores originales. Uno de ellos, al subir a `main`, intenta publicar su documentación en tu GitHub Pages; otro espera claves que no tenemos.
**Yo hago:** desactivar `deploy.yml` y `claude.yml` y dejar `check.yml`, que revisa el código en cada cambio y nos sirve.

### Paso 0.3 · Cuenta de Supabase y proyecto de desarrollo
**Tú haces** (desde cualquier lugar, también el iPhone):
1. Entra a supabase.com → botón **Start your project** → **Continue with GitHub** → autoriza con tu cuenta AldoNieto.
2. Si te pide crear una organización: nombre **MKT BS Ferias**, tipo **Company**, plan **Free**.
3. No crees el proyecto a mano: lo creo yo con un script, para que no tengas que hacer clics de más.

**Tú haces** (⚠️ frente a la Mac Mini o por Compartir Pantalla, una sola vez): yo abro el comando `npx supabase login`, se abre el navegador de la Mac Mini y tú pulsas **Authorize**. Con eso la Mac Mini queda autorizada para manejar Supabase sin que me pases ninguna clave.

**Yo hago:** crear el proyecto **crm-ferias-dev** en la región **São Paulo** (la más cercana a Lima), cargarle las tablas de Atomic CRM y guardar sus datos de conexión en un archivo local que **nunca** se sube a GitHub. La contraseña de la base de datos la genera el script; te la dejo en un archivo en la Mac Mini para que la guardes en tu gestor de contraseñas.

**A saber:** el plan gratis permite 2 proyectos y **pausa un proyecto tras 7 días sin uso** (se reactiva con un clic). Para desarrollo está bien; producción irá en el plan Pro (25 USD/mes) en la Fase 6.

### Paso 0.4 · Cuenta de Vercel y primera vista previa
❓ **Decisión antes de crear la cuenta.** El plan gratis de Vercel (Hobby) es, según sus condiciones, solo para uso personal no comercial. Un CRM de empresa es uso comercial. Opciones:
- **Vercel Pro** (20 USD/mes por usuario; solo necesitas 1): lo que pediste, sin dudas legales. **Mi recomendación.**
- **Vercel Hobby** por ahora y Pro al pasar a producción: gratis, pero técnicamente fuera de sus condiciones.
- **Cloudflare Pages**: gratis y permite uso comercial, también da vistas previas por rama. Cambia la herramienta que elegiste.

**Tú haces:** vercel.com → **Sign Up** → **Continue with GitHub** → autoriza. Luego **Add New → Project** → elige **atomic-crm** → **Import**. Yo te dejo escritos los valores exactos que debes pegar en esa pantalla (la dirección de Supabase y su clave pública, que no son secretas pero tampoco van al repositorio).

**Yo hago:** subir la rama `fase-0-preparacion`, ajustar Supabase para aceptar los enlaces de Vercel (para el login y las invitaciones) y comprobar la vista previa.

**Tú pruebas en el iPhone:** abres el enlace que te paso, creas tu usuario administrador (con tu email real) y ves el tablero vacío. Si ves el tablero, la Fase 0 está terminada.

**A saber:** las vistas previas de Vercel piden iniciar sesión en Vercel antes de mostrarse. Es una protección útil: solo tú las ves. Para los vendedores habrá un enlace de producción en la Fase 6.

### Paso 0.5 · Cierre
Actualizo `PROGRESO.md`, subo todo y te pido revisar. Al aprobar, uno la rama a `main`.

---

## Fase 1 — Modelo de datos (rama `fase-1-modelo-datos`)

### Paso 1.0 · Repositorio privado (tu recordatorio)
GitHub no deja volver privado un fork público. La solución es crear un repositorio **nuevo y privado** (por ejemplo `AldoNieto/crm-ferias`) y subir allí todo el historial; el fork público queda como estaba o lo archivas. Conservamos la conexión con Atomic CRM original para traer sus mejoras. Lo hago yo con GitHub CLI; tú solo confirmas. Recomiendo hacerlo **antes** de empezar la Fase 1, porque desde aquí el código ya es propio de MKT/BS. Vercel se reconecta al repositorio nuevo en un par de clics.

### Paso 1.1 · Español
Traducir la interfaz al español (Atomic CRM solo trae inglés y francés). Es base para todo lo demás y para los vendedores.

### Paso 1.2 · Ferias
Tabla nueva de ferias. Mi propuesta (para que sirva también al futuro ERP):
- **Feria** (la marca): FIMM, EPI, EEE&L, H&C… con nombre, sigla y color.
- **Edición** (cada vez que se realiza): por ejemplo "EPI 2027", con fechas, sede, moneda y precio de lista por m².
Cada oportunidad se vincula a una **edición**.

❓ ¿Te sirve separar feria y edición, o prefieres una sola lista ("EPI 2027", "FIMM 2027"…)?

### Paso 1.3 · Empresas con RUC
- Campo **RUC** obligatorio y único (11 dígitos, con la validación oficial de SUNAT: prefijos 10/15/17/20 y dígito verificador). Si dos registros tienen el mismo RUC, el sistema lo impide.
- Campo **razón social** (el nombre legal) además del nombre comercial que ya existe.

❓ ¿Hay empresas en tu base de 2000 **sin** RUC (por ejemplo extranjeras)? Si las hay, el RUC no puede ser obligatorio; lo haríamos único pero opcional, y las marcamos como "sin RUC".

### Paso 1.4 · Oportunidad con los campos de "Nueva venta" del ERP
A cada oportunidad le agrego: edición de feria, área (m²), stand, precio de lista, precio final, % de descuento (se calcula solo), inicial, número de cuotas, beneficios y fecha de venta. Empresa, RUC, contacto y teléfono ya salen de la empresa y el contacto vinculados.
El cronograma de pagos **no** entra: eso es del ERP.

❓ ¿En qué moneda se venden los stands: soles, dólares o depende de la feria?

### Paso 1.5 · Cuotas mensuales por vendedor
Tabla de cuotas: vendedor + mes + monto meta (y opcionalmente la edición de feria). Tú la llenas desde una pantalla sencilla de administración.

❓ ¿La cuota del vendedor es un monto total por mes, o un monto por mes **y por feria** (como sugieren las pestañas del ERP)?

### Paso 1.6 · Quién ve qué
Hoy, en Atomic CRM, todos los usuarios ven todas las empresas y oportunidades.

❓ ¿Cada vendedor debe ver **solo** sus empresas y oportunidades (y tú todo), o todos ven todo pero cada uno edita solo lo suyo?

### Cómo se trabaja cada paso de la Fase 1
1. Cambio la receta de la base de datos y genero la migración.
2. Actualizo pantallas, importación/exportación CSV y pruebas automáticas.
3. Lo pruebo en local, lo aplico al Supabase de desarrollo y subo la rama.
4. Te paso el enlace de la vista previa y qué probar desde el iPhone.
5. Actualizo `PROGRESO.md`.

### Lo que no hacemos todavía
Pipeline de ferias (Fase 2), importar las 2000 empresas (Fase 3), leads automáticos (Fase 4), panel de supervisor (Fase 5), diseño del Figma (Fase 6).
