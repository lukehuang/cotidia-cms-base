Internationalisation
====================

For a thorough doc, look on the django-localeurl repo here:

https://bitbucket.org/carljm/django-localeurl/src/b8ada3a2b6788927ba403fa75b2db4f3775460ec/docs/usage.rst?at=default

Language switcher
-----------------

In your template use the following code as a Bootstrap dropdown:

	<div class="btn-group pull-right">
	  <span class="btn"><img src="/static/admin/img/flags/{{LANGUAGE_CODE}}.png" alt=""></span>
	  <button class="btn dropdown-toggle" data-toggle="dropdown">
	    {% trans "Language" %} &nbsp; <span class="caret"></span>
	  </button>
	  <ul class="dropdown-menu">
	    {% for lang in LANGUAGES %}
		<li class="{% ifequal lang.0 LANGUAGE_CODE %}active{% endifequal %}">
	    	<form class="locale_switcher" method="POST" action="{% url 'localeurl_change_locale' %}">{% csrf_token %}
				<input type="hidden" name="locale" value="{{ lang.0 }}" />
				<a href="#" onclick="$(this).parent().submit()">{{ lang.1 }}</a>
			</form>
		</li>
		{% endfor %} 
	  </ul>
	</div>

Or as a simple select box:

	<form id="locale_switcher" method="POST" action="{% url 'localeurl_change_locale' %}">{% csrf_token %}
	    <select name="locale" onchange="$('#locale_switcher').submit()">
	        {% for lang in LANGUAGES %}
	            <option value="{{ lang.0 }}" {% ifequal lang.0 LANGUAGE_CODE %}selected="selected"{% endifequal %}>{{ lang.1 }}</option>
	        {% endfor %}
	    </select>
	    <noscript>
	        <input type="submit" value="Set" />
	    </noscript>
	</form>
	
Settings
--------

Here's the default settings:

	LANGUAGES = (
	    ('en', 'English'),
	    # ('nl', 'Dutch'),
	    # ('es', 'Spanish'),
	    # ('pt', 'Portuguese'),
	    # ('de', 'German'),
	)
	DEFAULT_LANGUAGE = LANGUAGES[0][0]

	LOCALE_INDEPENDENT_PATHS = (
	    r'^/admin/',
	    r'^/uploads/',
	)
	PREFIX_DEFAULT_LOCALE = False if len(LANGUAGES) == 1 else True


The LANGUAGES define all the supported languages for the application.

If the list equals 1, there's only one language and therefore the site is not multilingual, so we set the following value automatically:

	# Define wether or not to display the url prefix
	# False if we have only one language supported
	PREFIX_DEFAULT_LOCALE = False if len(LANGUAGES) == 1 else True
	
The LOCALE_INDEPENDENT_PATHS specify the urls that doesn't require localeurl support. Useful for the admin or api call such as the ajax file upload the Redactor text editor.