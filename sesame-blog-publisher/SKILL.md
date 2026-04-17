---
name: sesame-blog-publisher
description: Use when a Sesame HR ES content team member wants to research, write, and publish a blog post to staging.sesamehr.es. Triggers include "publica un artículo en el blog", "escribe un post para sesamehr.es", "nuevo artículo blog", "quiero subir contenido al blog", or any request to create SEO blog content for Sesame HR España.
---

# Sesame HR Blog Publisher — ES

Workflow de 5 fases para investigar, redactar, revisar y publicar artículos en `staging.sesamehr.es`. Siempre staging primero. Solo mercado ES.

**Credenciales WP:** `C:\Users\m.lorente\Downloads\Nueva carpeta\.env`
**Referencia estructura HTML:** `@blog-structure.md`

---

## Fase 0 — ACTUALIZACIÓN: sincronizar skill desde GitHub

**Ejecutar siempre al inicio, antes de cualquier otra acción.**

```bash
cd C:/Users/m.lorente/skill-redaccion && git -C sesame-skills pull origin main
```

- Si el pull trae cambios → recargar los archivos de la skill (`SKILL.md`, `blog-structure.md`, `style-guide.md`, `setup.md`) y continuar con la versión actualizada.
- Si ya está al día → continuar sin interrumpir al usuario.
- Si falla (sin red, credenciales, etc.) → avisar: *"No pude sincronizar la skill desde GitHub. Continúo con la versión local. Verifica tu conexión si quieres asegurarte de usar la versión más reciente."*

---

## Fase 1 — INPUT: recoger datos del content

Pide estos campos en orden. Usa listas numeradas para los drilldowns.

### 1a. Título y keywords
```
- Título tentativo del artículo
- KW principal (obligatoria)
- KWs secundarias (opcional, separadas por comas)
```

### 1b. Drilldown Categoría
Muestra la lista y pide número:
```
1. Actualidad y Tendencias RRHH  (ID: 47)
2. Base de datos de empleados    (ID: 63)
3. Comunicación interna          (ID: 29)
4. Control de gastos             (ID: 42)
5. Control horario               (ID: 36)
6. Desarrollo profesional        (ID: 28)
7. Employee experience           (ID: 64)
8. Encuestas laborales           (ID: 30)
9. Evaluación de desempeño       (ID: 65)
10. Firma digital                (ID: 44)
11. Formación                    (ID: 68)
12. Gestión de equipos           (ID: 32)
13. Gestión de tareas y proyectos(ID: 38)
14. Gestión de turnos            (ID: 37)
15. Gestión documental           (ID: 39)
16. Horas extras                 (ID: 35)
17. Informes y estadísticas      (ID: 43)
18. Nóminas                      (ID: 40)
19. Noticias                     (ID: 97)
20. Onboarding                   (ID: 25)
21. Productividad laboral        (ID: 27)
22. Reclutamiento y selección    (ID: 26)
23. Salas de reuniones           (ID: 41)
24. Software de RRHH             (ID: 61)
25. Vacaciones y ausencias       (ID: 34)
```

### 1c. Drilldown Autor
Muestra la lista y pide número:
```
1. Adrián Basols     (ID: 28)
2. Alex              (ID: 21)
3. Cristina Martin   (ID: 30)
4. Emilio Heras      (ID: 69)
5. Fran Perez        (ID: 35)
6. Inma Aparici      (ID: 33)
7. Iris Serrador     (ID: 62)
8. Isabel Veillard   (ID: 47)
9. Laura Blasco      (ID: 27)
10. Leticia Gonzalbez(ID: 4)
11. Maria Jara       (ID: 41)
12. Paula Lluesa     (ID: 19)
13. Paula Rubio      (ID: 24)
14. Ricardo López    (ID: 64)
15. Sofia            (ID: 57)
16. Tiago Santos     (ID: 61)
```

---

## Fase 2 — RESEARCH: brief SEO

Ejecuta en este orden:

**2a. Google Search Console** *(requiere MCP de GSC activo)*
- Si GSC MCP no está disponible → avisar al usuario: *"GSC MCP no detectado. Para obtener datos de posicionamiento actuales necesitas conectar Google Search Console en tu configuración de Claude. Continuamos con WebSearch."*
- Si disponible: consulta posición actual de la KW en `https://www.sesamehr.es/` y páginas que ya rankean para ella.

**2b. SERP research** via WebSearch
- Busca: `{kw_principal} site:es` y sin site filter
- Analiza: intención de búsqueda, extensión media del top 3, estructura H2 dominante, preguntas frecuentes (PAA)

**2c. Interlinking interno** via WP API
```powershell
$pair = '{WP_USER}:{WP_APP_PASSWORD}'
$b64 = [Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($pair))
$h = @{ Authorization = "Basic $b64" }
Invoke-RestMethod "https://staging.sesamehr.es/wp-json/wp/v2/posts?search={keyword}&per_page=8&status=publish" -Headers $h
```
→ Selecciona 5-8 posts relacionados como candidatos de interlink.

**2d. Activar skills SEO**
- Usa `anthropic-skills:seo-content` para recomendaciones E-E-A-T, extensión, densidad KW
- Usa `anthropic-skills:seo-geo` para optimizar para AI Overviews y LLMs

**Output del brief** (mostrar al content antes de redactar):
```
KW principal:      [kw]
KW secundarias:    [kws]
Posición actual:   [X en GSC / no posiciona]
Intención:         [informacional / comercial / mixta]
Extensión óptima:  [~XXX palabras]
Estructura H2:     [lista de H2 propuestos]
Interlinks:        [lista de URLs internas]
Optimización LLM:  [recomendaciones: FAQ, definiciones, datos con fuente]
```

---

## Fase 3 — GENERACIÓN DE CONTENIDO

Genera el contenido HTML siguiendo la estructura en `@blog-structure.md`.

**Consultar `@style-guide.md` para todas las reglas de redacción.** Resumen de las más críticas:

- H1: KW principal en las primeras palabras, 50–70 caracteres, promesa concreta
- Intro: 3 movimientos (problema → stakes → promesa editorial). Sin listas. Sin KW forzada en párrafo 1.
- **Tercera persona siempre.** Sin "tú", sin "nosotros", sin preguntas retóricas.
- Todo H2/H3 debe abrirse con párrafo de introducción (nunca heading seguido de lista directamente)
- KW principal en: H1, párrafo 1, al menos un H2, meta description, alt text imagen destacada
- Densidad KW: 1%–1,5%. Usar sinónimos y variaciones en el resto.
- Mínimo 5 links internos integrados en texto (no en listas de "ver también")
- Al menos 1 dato externo con fuente (McKinsey, Deloitte, Gartner, organismo oficial)
- Al menos 1 cita interna de Tiago Santos o Cristina Martín en blockquote de Gutenberg
- 2 CTAs HubSpot: `[hubspot type="cta" portal="PORTAL_ID" id="CTA_ID"]` (si no hay ID → marcador)
- Cierre: transición, no resumen. Máximo 2 párrafos.
- 3 artículos relacionados al final (solo posts publicados)
- Nombre de marca: siempre **Sesame HR** (nunca solo "Sesame")

**Revisar con:** `marketing:brand-review` para validar tono antes de entregar el .docx.

**Guardar .docx** en local:
- Usar skill `anthropic-skills:docx`
- Ruta sugerida: `C:\Users\m.lorente\Downloads\Nueva carpeta\blog-drafts\{slug}-draft.docx`
- Incluir en el .docx: brief SEO + HTML del contenido + notas para el editor

---

## Fase 4 — IMÁGENES

**4a. Buscar en media library interna — SIEMPRE automático, sin esperar indicación del content**

Ejecutar búsqueda con múltiples términos derivados de la KW principal, categoría y tema del artículo:
```powershell
# Guardar como .ps1 y ejecutar con: powershell.exe -ExecutionPolicy Bypass -File search-media.ps1
$envVars = @{}
Get-Content "C:\Users\m.lorente\Downloads\Nueva carpeta\.env" | Where-Object { $_ -notmatch "^#" -and $_ -match "=" } | ForEach-Object {
    $p = $_ -split "=",2
    $envVars[$p[0].Trim()] = $p[1].Trim()
}
$b64 = [Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($envVars["WP_USER"] + ":" + $envVars["WP_APP_PASSWORD"]))
$h = @{ Authorization = "Basic " + $b64 }

foreach ($term in @("{kw_slug}", "{categoria_slug}", "{termino_1}", "{termino_2}", "{termino_3}")) {
    $r = Invoke-RestMethod ("https://staging.sesamehr.es/wp-json/wp/v2/media?search=" + $term + "&media_type=image&per_page=5") -Headers $h
    if ($r.Count -gt 0) {
        Write-Host ("=== " + $term + " ===")
        $r | ForEach-Object { Write-Host ($_.id.ToString() + " | " + $_.slug + " | " + $_.source_url) }
    }
}
```
→ Seleccionar automáticamente sin preguntar al content:
- **Featured image:** la más relevante semánticamente con la KW principal. Preferir .webp y fecha reciente.
- **Imagen de cuerpo:** segunda opción relevante, embebida en el H2 más visual del artículo (solo si extensión > 1.000 palabras).
→ Criterio: relevancia semántica > formato .webp > fecha reciente.

**4b. Si no hay imágenes relevantes:**
- Avisar: *"No encontré imágenes internas relevantes. Pendiente conexión con Canva Team (próximamente). Por ahora, ¿puedes proporcionar una imagen o indicar una búsqueda específica?"*

**Asignación:**
- `featured_media`: ID de WP de la imagen principal
- Imagen de cuerpo: `<figure class="wp-block-image size-large"><img src="{url}" alt="{kw_secundaria}" /></figure>`

---

## Fase 5 — REVISIÓN Y PUBLICACIÓN

### 5a. Entregar para revisión
Confirmar al content:
- Ruta del .docx guardado
- Preview del brief SEO
- URLs de imágenes seleccionadas
- *"Revisa el .docx, haz tus cambios y dime 'aprobado' cuando esté listo."*

Si hay cambios → actualizar HTML y regenerar .docx. Repetir hasta aprobación.

### 5b. Publicar en borrador en staging (tras aprobación)

```powershell
# ⚠️ CRÍTICO: NO usar ConvertTo-Json para el contenido HTML.
# ConvertTo-Json escapa mal tildes, ñ y comillas tipográficas → el content llega vacío a WP.
# SIEMPRE serializar el JSON manualmente como se muestra aquí.
# Guardar como .ps1 y ejecutar con: powershell.exe -ExecutionPolicy Bypass -File publish-post.ps1

$envVars = @{}
Get-Content "C:\Users\m.lorente\Downloads\Nueva carpeta\.env" | Where-Object { $_ -notmatch "^#" -and $_ -match "=" } | ForEach-Object {
    $p = $_ -split "=",2
    $envVars[$p[0].Trim()] = $p[1].Trim()
}
$AUTH = "Basic " + [Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($envVars["WP_USER"] + ":" + $envVars["WP_APP_PASSWORD"]))
$H = @{ Authorization = $AUTH; "Content-Type" = "application/json; charset=utf-8" }
$WP_URL = "https://staging.sesamehr.es"

# Leer HTML y eliminar comentarios del brief
$html = Get-Content "{ruta_draft.html}" -Raw -Encoding UTF8
$html = $html -replace '(?s)<!--.*?-->\s*', ''

# Escapar manualmente para JSON
$htmlEscaped = $html -replace '\\', '\\\\'
$htmlEscaped = $htmlEscaped -replace '"', '\"'
$htmlEscaped = $htmlEscaped -replace "`r`n", '\n'
$htmlEscaped = $htmlEscaped -replace "`n", '\n'
$htmlEscaped = $htmlEscaped -replace "`r", '\n'
$htmlEscaped = $htmlEscaped -replace "`t", '\t'

# Construir JSON manualmente
$jsonBody = '{"title":"{titulo}","content":"' + $htmlEscaped + '","status":"draft","slug":"{slug}","excerpt":"{meta_description}","categories":[{categoria_id}],"author":{autor_id},"featured_media":{media_id}}'

$post = Invoke-RestMethod "$WP_URL/wp-json/wp/v2/posts" -Method Post -Headers $H `
  -Body ([System.Text.Encoding]::UTF8.GetBytes($jsonBody)) -TimeoutSec 30
```

### 5c. Output final

```
✅ POST CREADO EN STAGING
────────────────────────────────────────
ID:          {post.id}
Título:      {post.title.rendered}
Categoría:   {nombre_categoria}
Autor:       {nombre_autor}
Status:      draft
Preview URL: https://staging.sesamehr.es/?p={post.id}&preview=true
Edit URL:    https://staging.sesamehr.es/wp-admin/post.php?post={post.id}&action=edit
────────────────────────────────────────
⚠️  Revisa la preview antes de publicar.
    Para publicar: "publica el post {ID}"
```

### 5d. Confirmación de publicación

Solo tras confirmación explícita → cambiar status a `publish`:
```powershell
Invoke-RestMethod "$WP_URL/wp-json/wp/v2/posts/{id}" -Method Post -Headers $H `
  -Body '{"status":"publish"}' -TimeoutSec 30
```

---

## Errores comunes

| Error | Causa | Solución |
|---|---|---|
| 401 Unauthorized | Credenciales expiradas | Regenerar App Password en WP Admin |
| 403 Forbidden | Rol de usuario sin permisos | Verificar que el usuario tiene rol Editor o Admin |
| GSC MCP no disponible | No configurado | Usar WebSearch como fallback, avisar al usuario |
| No hay imágenes internas | Media library sin assets del tema | Avisar al content, usar placeholder hasta conectar Canva |
| CTAs HubSpot sin ID | Content no especificó IDs | Dejar marcador `[CTA-HUBSPOT-PENDIENTE]` en el HTML |
