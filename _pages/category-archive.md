---
title: "Categorie"
layout: single
permalink: /categories/
description: "Tutte le categorie di prodotti digitali: template, fogli Excel, planner, guide, prompt e workflow."
author_profile: false
classes: wide
---
<style>
.cx-intro{color:#6c757d;margin:0 0 1.4rem}
.cx-lista{display:flex;flex-direction:column;gap:.8rem}
.cx-riga{position:relative;display:grid;grid-template-columns:5.5rem minmax(0,1fr) auto;gap:1.2rem;align-items:center;padding:.9rem 1.2rem .9rem .9rem;background:var(--global-card-bg-color,#fff);border:1px solid #dee2e6;border-radius:12px;cursor:pointer;transition:box-shadow .15s,border-color .15s,transform .15s}
.cx-riga:hover{border-color:hsl(var(--h) 45% 62%);box-shadow:0 6px 22px rgba(0,0,0,.07);transform:translateY(-1px)}
.cx-ico{width:5.5rem;aspect-ratio:1/1;border-radius:12px;display:flex;align-items:center;justify-content:center;overflow:hidden;background:linear-gradient(135deg,hsl(var(--h) 80% 93%),hsl(calc(var(--h) + 30) 75% 87%));font-size:2.6rem;line-height:1}
.cx-ico img{width:100%;height:100%;object-fit:cover}
.cx-testo{min-width:0}
.cx-meta{margin:0 0 .15rem;font-size:.8rem;color:#6c757d}
.cx-riga h3{margin:0 0 .25rem;font-size:1.2rem;line-height:1.25}
.cx-riga h3 a{color:var(--t-text,#212529);text-decoration:none}
.cx-riga h3 a::after{content:"";position:absolute;inset:0;border-radius:inherit}
.cx-riga:hover h3 a{color:var(--t-accent-dark,#3f6b00)}
.cx-desc{margin:0;color:#6c757d;font-size:.95rem;line-height:1.45}
.cx-cta{display:inline-flex;align-items:center;gap:.4rem;font-weight:500;font-size:.95rem;color:var(--t-accent-dark,#3f6b00);white-space:nowrap}
.cx-cta svg{width:1.1rem;height:1.1rem;transition:transform .15s}
.cx-riga:hover .cx-cta svg{transform:translateX(3px)}
.cx-h2{margin:2.2rem 0 .8rem;font-size:1.1rem;font-weight:700}
.cx-blog{display:flex;flex-wrap:wrap;gap:.5rem}
.cx-pill{display:inline-flex;align-items:center;gap:.5rem;padding:.3rem .85rem;border:1px solid #dee2e6;border-radius:6px;color:var(--t-text,#212529);text-decoration:none;font-size:.9rem;text-transform:capitalize}
.cx-pill span{color:#6c757d;font-size:.8rem}
.cx-pill:hover{border-color:var(--t-accent-dark,#3f6b00);color:var(--t-accent-dark,#3f6b00);text-decoration:none}
@media(max-width:639.98px){.cx-riga{grid-template-columns:4rem minmax(0,1fr) auto;gap:.8rem;padding:.75rem}.cx-ico{width:4rem;font-size:2rem;border-radius:10px}.cx-cta span{display:none}.cx-desc{font-size:.88rem}}
</style>
{%- assign cx_tot = 0 -%}
{%- assign cx_ncat = 0 -%}
{%- for c in site.data.catalogo.categorie -%}
{%- assign cx_p = site.data.catalogo.prodotti | where: 'categoria', c.id | where_exp: 'x', 'x.visibile != false' -%}
{%- if cx_p.size > 0 -%}{%- assign cx_ncat = cx_ncat | plus: 1 -%}{%- assign cx_tot = cx_tot | plus: cx_p.size -%}{%- endif -%}
{%- endfor -%}
<p class="cx-intro">{{ cx_ncat }} categorie e {{ cx_tot }} prodotti digitali. Scegli un argomento: paghi una volta sola e ricevi i file subito.</p>
<div class="cx-lista">
{%- for c in site.data.catalogo.categorie -%}
{%- assign prods = site.data.catalogo.prodotti | where: 'categoria', c.id | where_exp: 'x', 'x.visibile != false' -%}
{%- if prods.size == 0 -%}{%- continue -%}{%- endif -%}
{%- assign pagina = site.negozio | where: 'cid', c.id | first -%}
{%- assign minp = prods | sort: 'prezzo_centesimi' | first -%}
{%- assign eu = minp.prezzo_centesimi | divided_by: 100 -%}
{%- assign ce = minp.prezzo_centesimi | modulo: 100 -%}
<article class="cx-riga" style="--h: {{ forloop.index0 | times: 47 | modulo: 360 }}">
<div class="cx-ico">{% if c.immagine and c.immagine != '' %}<img src="{% if c.immagine contains '://' %}{{ c.immagine }}{% else %}{{ '/' | append: c.immagine | relative_url }}{% endif %}" alt="" loading="lazy">{% else %}{{ c.icona }}{% endif %}</div>
<div class="cx-testo">
<p class="cx-meta">{{ prods.size }} {% if prods.size == 1 %}prodotto{% else %}prodotti{% endif %} &middot; da {{ eu }},{% if ce < 10 %}0{% endif %}{{ ce }} &euro;</p>
<h3><a href="{{ '/negozio/categoria/' | append: c.id | append: '/' | relative_url }}">{{ c.nome }}</a></h3>
<p class="cx-desc">{{ pagina.description }}</p>
</div>
<span class="cx-cta"><span>Vedi prodotti</span><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M5 12h14M13 6l6 6-6 6"/></svg></span>
</article>
{%- endfor -%}
</div>
{%- if site.categories.size > 0 %}
<p class="cx-h2">Dal blog</p>
<div class="cx-blog">
{%- assign cx_blog = site.categories | sort -%}
{%- for cat in cx_blog -%}
<a id="{{ cat[0] | slugify }}" class="cx-pill" href="{{ '/categoria/?cat=' | append: cat[0] | relative_url }}">{{ cat[0] }} <span>{{ cat[1].size }}</span></a>
{%- endfor -%}
</div>
{%- endif %}
