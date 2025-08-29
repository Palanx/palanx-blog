---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true
showHero: true
backgroundImage: "img/recipes/{{ .Name | urlize }}.jpg"
tags: ["cooking"]
categories: ["Cooking"]
showSummary: true
summary: ""
authors:
  - "palanx"
showAuthorsBadges: true
---

{{< lead >}}
Short intro: context of the recipe, pairings or personal notes.
{{< /lead >}}

# Ingredients
{{< keyword icon="user" >}} X servings {{< /keyword >}}
* **…** ingredient 1
* **…** ingredient 2
* **…**

# Step by Step
{{< keyword icon="clock" >}} XX min {{< /keyword >}}
1. Step 1…
2. Step 2…
3. Step 3…

# Notes / Tips
- Variations, substitutions or extra recommendations.
- Internal links to other recipes: `[Another recipe](/en/cook/another-recipe)`.