# Setup — Configuración inicial para usar el skill

Esta guía se realiza **una sola vez** por persona antes de poder usar el skill `sesame-blog-publisher`.
Tiempo estimado: 5 minutos.

---

## Requisitos previos

- Tener Claude Code instalado
- Tener acceso a las credenciales de WordPress staging (pedir a m.lorente@sesametime.com)
- Sistema operativo: Windows (las instrucciones usan PowerShell)

---

## Paso 1 — Crear el archivo `.env`

Crea el archivo en tu directorio de trabajo local (la carpeta donde vayas a ejecutar el skill):

```
C:\Users\TU_USUARIO\Downloads\sesame-blog\  (o la ruta que uses)
```

Contenido del `.env`:

```
# WordPress REST API - Sesame HR Staging
WP_URL=https://staging.sesamehr.es
WP_USER=tu-email@sesametime.com
WP_APP_PASSWORD=xxxx xxxx xxxx xxxx xxxx xxxx
```

> ⚠️ La App Password de WordPress tiene espacios — se escribe tal cual, sin comillas ni modificaciones.

**Nunca compartir este archivo ni subirlo a Git.**

---

## Paso 2 — Generar tu App Password en WordPress

1. Ir a `https://staging.sesamehr.es/wp-admin/`
2. Iniciar sesión con tus credenciales habituales
3. Ir a: **Usuarios → Tu perfil → Contraseñas de aplicación**
4. Escribir un nombre descriptivo (ej: `claude-blog-publisher`)
5. Hacer clic en **"Añadir nueva contraseña de aplicación"**
6. Copiar la contraseña generada (formato: `xxxx xxxx xxxx xxxx xxxx xxxx`) y pegarla en el `.env`

> Si no tienes la sección "Contraseñas de aplicación" en tu perfil, el administrador de WP debe habilitar esta funcionalidad para tu rol de usuario (requiere rol Editor o superior).

---

## Paso 3 — Verificar la conexión

Ejecuta este comando en PowerShell para confirmar que las credenciales funcionan:

```powershell
$env = @{}
Get-Content ".env" | Where-Object { $_ -notmatch "^#" -and $_ -match "=" } |
  ForEach-Object { $p = $_ -split "=",2; $env[$p[0].Trim()] = $p[1].Trim() }

$b64 = [Convert]::ToBase64String(
  [System.Text.Encoding]::UTF8.GetBytes("$($env.WP_USER):$($env.WP_APP_PASSWORD)"))

$r = Invoke-RestMethod "$($env.WP_URL)/wp-json/wp/v2/users/me" `
  -Headers @{ Authorization = "Basic $b64" } -TimeoutSec 15

Write-Host "✓ Conectado como:" $r.name "— Rol:" ($r.roles -join ", ")
```

Resultado esperado:
```
✓ Conectado como: Tu Nombre — Rol: editor
```

Si ves error 401 → revisar WP_USER y WP_APP_PASSWORD en el `.env`.
Si ves error de conexión → comprobar que WP_URL es correcto y tienes acceso a la red.

---

## Reglas de publicación — leer antes de usar el skill

### Siempre borrador primero

El skill **nunca publica directamente** en estado `published`. El flujo es siempre:

```
Generación → draft en staging → preview URL → revisión humana → publicar
```

Esto garantiza que ningún artículo llega a staging sin revisión editorial.

### Solo staging (por ahora)

El skill apunta exclusivamente a `staging.sesamehr.es`. La publicación en producción (`www.sesamehr.es`) requiere credenciales distintas y se hará solo cuando el equipo lo indique expresamente.

### La preview requiere estar logueado

Para ver la preview de un borrador en staging necesitas estar logueado en el WP Admin de staging en el mismo navegador. Si la preview aparece en blanco o redirige al login, inicia sesión primero en `https://staging.sesamehr.es/wp-admin/`.

### El post siempre se puede editar antes de publicar

Tras crear el borrador, el skill devuelve la **Edit URL** (`/wp-admin/post.php?post=ID&action=edit`). Desde ahí se pueden hacer ajustes directamente en el editor de WordPress antes de publicar (añadir banners HubSpot, ajustar Yoast, revisar imágenes).

---

## Estructura de archivos del skill

```
~/.claude/skills/sesame-blog-publisher/
├── SKILL.md          ← Flujo principal (Claude lo carga automáticamente)
├── blog-structure.md ← Template HTML del artículo
├── style-guide.md    ← Manual de estilo editorial completo
└── setup.md          ← Este archivo (solo configuración inicial)
```

---

## Soporte

Problemas técnicos con la API o las credenciales → m.lorente@sesametime.com
Dudas editoriales sobre el manual de estilo → responsable del equipo de Content
