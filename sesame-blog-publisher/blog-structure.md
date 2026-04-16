# Estructura HTML tipo — Blog Sesame HR ES

Referencia para generar el HTML de cada artículo. Basada en análisis de artículos reales de sesamehr.es/blog/.

---

## Métricas de referencia

| Métrica | Artículo corto | Artículo largo |
|---|---|---|
| Palabras | ~850 | ~1.400 |
| H2s | 4–5 | 6–8 |
| H3s | 3–6 | 6–10 |
| Imágenes | 1 | 2 |
| CTAs HubSpot | 2 | 3–4 |
| Links internos en texto | 5–8 | 12–15 |

---

## Template HTML

```html
<!-- IMAGEN DESTACADA (featured_media via API, no inline) -->
<!-- Se asigna como featured_media en el payload del post -->

<!-- INTRO: 2 párrafos narrativos. Sin listas. Sin KW en el 1er párrafo. -->
<p>{Párrafo 1: describe el problema/contexto de forma conversacional.
   Tono: "Si diriges un equipo, sabes que..."}</p>

<p>{Párrafo 2: introduce la solución y el valor. Incluir KW principal aquí.
   Ej: "Un software de RRHH como Sesame HR te permite..."}</p>

<!-- H2 #1: Definición ("¿Qué es...?") -->
<h2>¿Qué es {tema} y para qué sirve?</h2>

<p>{Definición clara en 2-3 frases. Incluir KW principal.
   Formato ideal para AI Overviews: definición directa en primera frase.}</p>

<ul>
  <li><strong>{Beneficio 1}</strong>: {explicación breve}</li>
  <li><strong>{Beneficio 2}</strong>: {explicación breve}</li>
  <li><strong>{Beneficio 3}</strong>: {explicación breve}</li>
</ul>

<!-- CTA HUBSPOT #1 (guía descargable o recurso) -->
[hubspot type="cta" portal="PORTAL_ID" id="CTA_ID_1"]

<!-- H2 #2: Por qué / importancia -->
<h2>¿Por qué es importante {tema} en tu empresa?</h2>

<p>{Párrafo contextual. Dato o estadística con fuente.
   Ej: "Según un estudio de McKinsey (2024), las empresas que..."}</p>

<p>{Párrafo de consecuencias de no hacerlo.
   Link interno: <a href="{url_interna_relacionada}">{texto ancla}</a>}</p>

<!-- IMAGEN DE CUERPO (solo si extensión > 1000 palabras) -->
<figure class="wp-block-image size-large">
  <img src="{url_imagen_interna_wp}" alt="{kw_secundaria}" />
</figure>

<!-- H2 #3: Cómo / funcionalidades / pasos -->
<h2>{Cómo / Las X formas de / Funcionalidades de} {tema}</h2>

<p>{Párrafo introductorio de la sección.}</p>

<!-- H3s para sub-secciones -->
<h3>{Subtema 1}</h3>
<p>{Explicación. Link interno si aplica.}</p>

<h3>{Subtema 2}</h3>
<p>{Explicación.}</p>

<h3>{Subtema 3}</h3>
<p>{Explicación. Puede incluir lista numerada si son pasos.}</p>

<!-- CAJA DE DATO DESTACADO (opcional, para stats relevantes) -->
<blockquote>
  <p><strong>{Estadística o dato clave}</strong> — {Fuente, Año}</p>
</blockquote>

<!-- H2 #4: Sesame HR como solución (sección de valor) -->
<h2>{Gestiona / Mejora / Automatiza} {tema} con Sesame HR</h2>

<p>{Párrafo de propuesta de valor de Sesame HR aplicado al tema.
   Tono: soft-sell, no agresivo. Mencionar 2-3 funcionalidades específicas.
   Link a página de producto: <a href="https://staging.sesamehr.es/{funcionalidad}/">{texto}</a>}</p>

<!-- CTA HUBSPOT #2 (prueba gratuita o demo) -->
[hubspot type="cta" portal="PORTAL_ID" id="CTA_ID_2"]

<!-- H2 CONCLUSIÓN: "En definitiva" o "{Verbo} mejor con Sesame HR" -->
<h2>En definitiva: {síntesis en 4-6 palabras}</h2>

<p>{Párrafo de síntesis: recapitula el problema + solución en 2-3 frases.
   Incluir KW principal una vez más de forma natural.}</p>

<p>{Llamada a acción final: "Si quieres [beneficio], prueba Sesame HR gratis
   y descubre cómo [promesa]."
   Link: <a href="https://staging.sesamehr.es/solicitar-demo/">solicita una demo</a>}</p>
```

---

## Checklist antes de pasar a revisión

- [ ] H1 contiene KW principal en las primeras 5 palabras
- [ ] Intro: 2 párrafos, sin listas, sin KW en párrafo 1
- [ ] KW principal en: H1, primer H2, 1 alt text, meta description
- [ ] KW secundarias distribuidas en H2/H3 y cuerpo (densidad ~1-2%)
- [ ] Mínimo 5 links internos en texto (no en listas de "ver también")
- [ ] Mínimo 1 dato/estadística con fuente externa citada
- [ ] 2 CTAs HubSpot en posiciones estándar (o marcador si falta ID)
- [ ] Cierre H2 con CTA a demo/prueba gratuita
- [ ] Alt text de imágenes: descriptivo + KW secundaria
- [ ] Tono conversacional, segunda persona (tú / tu empresa)
- [ ] Sección de valor Sesame HR antes del cierre (soft-sell)
- [ ] Para artículos >1000 palabras: 2 imágenes

---

## Patrones de interlinking por categoría

| Categoría | Páginas de producto a enlazar prioritariamente |
|---|---|
| Control horario | `/control-horario/`, `/fichaje-digital/` |
| Vacaciones y ausencias | `/vacaciones/`, `/gestion-ausencias/` |
| Software de RRHH | `/software-recursos-humanos/`, `/solicitar-demo/` |
| Evaluación desempeño | `/evaluacion-desempeno/`, `/objetivos-okr/` |
| Reclutamiento | `/reclutamiento/`, `/software-seleccion/` |
| Nóminas | `/nominas/`, `/gestion-nominas/` |
| Onboarding | `/onboarding/`, `/portal-empleado/` |
| Comunicación interna | `/comunicacion-interna/`, `/portal-empleado/` |
| Encuestas | `/encuestas-laborales/`, `/clima-laboral/` |

> Siempre enlazar a `https://staging.sesamehr.es/{ruta}/` en staging.
