---
layout: page
title: Menus
permalink: "menu.html"
---

Menus
=====

Menus are generated from the MPTT tree, using the MPTT template tag.


How to use it
-------------

In your template, import the mptt tags:

{% raw %}
	{% load mptt_tags %}
{% endraw %}

To create a navigation list, use the following code passing on a MPTT tree node (nodes):

{% raw %}
	<ul>
	    {% recursetree nodes %}
	        {% if node.get_published %}<li>
	            <a href="{{node.get_absolute_url}}">{{node.get_published.translated.title}}</a>
	            {% if not node.is_leaf_node %}
	                <ul class="children">
	                    {{ children }}
	                </ul>
	            {% endif %}
	        </li>{% endif %}
	    {% endrecursetree %}
	</ul>
{% endraw %}	

Getting the node in Python:

	nodes = page_original.get_root().get_descendants(include_self=True)
	
It first get the root of the current page instance, then look up it root node and get all descendants. This will generate a full tree.

!!! Please note that because the tree can ony be generated by "original" pages, the template output should lookup the "published" version to write the approved published content. Hence:
	
	{{node.get_published.translated.title}}
	
The "translated" method automatically lookup the current language set in your session to render the right translation.


Get the full tree
-----------------

You can use the template tage to get the full tree for a specific model:
	
{% raw %}
	{% full_tree_for_model cmsbase.Page as pages %}
{% endraw %}