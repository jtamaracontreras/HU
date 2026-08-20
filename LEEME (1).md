# Pedido de playeras — versión para Vercel + Supabase

Herramienta web para contar plantilla por región/área y capturar la talla de cada
persona, con **base compartida en tiempo real** (varias personas capturan a la vez)
y **contraseña de acceso**.

Son solo 2 archivos importantes:
- `index.html` — la aplicación (esto es lo que se publica en Vercel).
- `supabase_setup.sql` — se ejecuta una vez en Supabase para crear la base.

---

## Paso 1 — Crear la base en Supabase (gratis)

1. Entra a https://supabase.com y crea una cuenta. Crea un **New project**
   (elige un nombre y una contraseña de base de datos; guárdala).
2. Espera 1–2 min a que el proyecto quede listo.
3. En el menú izquierdo abre **SQL Editor → New query**.
4. Abre el archivo `supabase_setup.sql`, copia todo su contenido, pégalo y pulsa **Run**.
   Debe decir "Success".
5. En el menú abre **Settings → API Keys** y copia estos dos valores:
   - **Project URL**  (algo como `https://xxxxx.supabase.co`)
   - La **llave pública**:
     - En proyectos nuevos se llama **Publishable key** y empieza con `sb_publishable_...`
       → esa es la que va en el código.
     - Si tu proyecto tuviera las llaves viejas, entra a la pestaña **Legacy API Keys**
       y copia la **anon public** (empieza con `eyJ...`).
   - ⚠️ NO copies la llave **secret** ni **service_role**: esa es de servidor y no debe
     ir en el `index.html`.

---

## Paso 2 — Poner tus datos en index.html

Abre `index.html` con cualquier editor de texto y busca, cerca del inicio del
`<script>`, este bloque:

```
const SUPABASE_URL      = "TU_SUPABASE_URL";
const SUPABASE_ANON_KEY = "TU_SUPABASE_ANON_KEY";
const ACCESS_CODE       = "playeras2026";
```

Reemplaza:
- `TU_SUPABASE_URL`      → tu **Project URL**.
- `TU_SUPABASE_ANON_KEY` → tu llave **Publishable** (o **anon public** si es proyecto viejo).
- `playeras2026`         → la **contraseña** que quieras para entrar a la app.

Guarda el archivo. (Puedes abrirlo en tu navegador para probar que carga y que la
contraseña funciona; ya debería guardar en Supabase.)

---

## Paso 3 — Subir a GitHub

1. Entra a https://github.com y crea un repositorio nuevo (puede ser **privado**).
2. Sube los archivos de esta carpeta (`index.html`, `supabase_setup.sql`, `LEEME.md`).
   Si no usas la línea de comandos, GitHub tiene un botón **"Add file → Upload files"**.

---

## Paso 4 — Publicar en Vercel

1. Entra a https://vercel.com y crea cuenta (puedes usar tu GitHub).
2. **Add New… → Project** e importa el repositorio que acabas de crear.
3. No hace falta configurar nada de "build": es un sitio estático.
   Framework Preset: **Other**. Deja lo demás por defecto y pulsa **Deploy**.
4. Al terminar, Vercel te da una URL (algo como `https://pedido-playeras.vercel.app`).
   Esa es la liga que compartes con tu equipo.

---

## Cómo se usa

- Comparte la URL de Vercel **y la contraseña** solo con quienes van a capturar.
- Cada quien entra, filtra por región/departamento o busca por nombre, y toca la talla
  de cada persona. Todo se guarda solo y aparece al instante para los demás.
- Para el archivo final: toca **Actualizar** y luego **Exportar a Excel**. La hoja
  "Detalle por persona" es la base completa con todo lo capturado.

## Notas de seguridad (importante)

- La contraseña de la app es una barrera sencilla, suficiente para uso interno; no es
  seguridad de nivel bancario. No publiques la URL ni la contraseña fuera de tu empresa.
- La llave `anon public` de Supabase va dentro del `index.html` (es normal y así se usa),
  por eso la contraseña de la app es la capa que limita quién entra.
- Si quieres algo más estricto (usuarios con login real), se puede, pero es más trabajo.

## Cambiar la contraseña después

Edita `ACCESS_CODE` en `index.html`, guarda y vuelve a subir a GitHub. Vercel
republica solo. (Quien ya haya entrado seguirá dentro hasta que cierre la pestaña.)
