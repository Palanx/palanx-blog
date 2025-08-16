---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true
showHero: true
backgroundImage: "img/cook/{{ .Name | urlize }}.jpg"
tags: ["cocina"]
categories: ["Cocina"]
summary: ""
authors:
  - "palanx"
showAuthorsBadges: true
---

{{< lead >}}
Breve intro: contexto de la receta, acompañamientos o notas personales.
{{< /lead >}}

# Ingredientes
{{< keyword icon="user" >}} X raciones {{< /keyword >}}
* **…** ingrediente 1
* **…** ingrediente 2
* **…**

# Paso a paso
{{< keyword icon="clock" >}} XX min {{< /keyword >}}
1. Paso 1…
2. Paso 2…
3. Paso 3…

# Notas / Tips
- Variantes, sustituciones o recomendaciones extra.
- Enlaces internos a otras recetas: `[Otra receta](/es/cook/otra-receta)`.
