---
layout: page
title: Recipes
---

A personal archive of ketogenic recipes, straight to the point, using metric measurements with slight tweaks

# Meals:
<ul>
	{% for recipe in site.pages %}
	{% if recipe.type == 'recipe_meal' %}
	<li>
		<a href="{{ recipe.url }}">{{recipe.title}}</a>
	</li>
	{% endif %}
	{% endfor %}
</ul>

# Snacks:
<ul>
	{% for recipe in site.pages %}
	{% if recipe.type == 'recipe_snack' %}
	<li>
		<a href="{{ recipe.url }}">{{recipe.title}}</a>
	</li>
	{% endif %}
	{% endfor %}
</ul>

# Breads
<ul>
	{% for recipe in site.pages %}
	{% if recipe.type == 'recipe_bread' %}
	<li>
		<a href="{{ recipe.url }}">{{recipe.title}}</a>
	</li>
	{% endif %}
	{% endfor %}
</ul>

# Desserts
<ul>
	{% for recipe in site.pages %}
	{% if recipe.type == 'recipe_dessert' %}
	<li>
		<a href="{{ recipe.url }}">{{recipe.title}}</a>
	</li>
	{% endif %}
	{% endfor %}
</ul>