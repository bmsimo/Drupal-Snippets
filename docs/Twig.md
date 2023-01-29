## Twig

Twig is a template engine for PHP. It is used by Drupal to render HTML pages. Twig is a very powerful tool that allows you to create complex templates with very little code. It is also very easy to learn. Here is a list of the most common Twig functions and filters that you will use in your Drupal projects.

### Accessing variables

- It is recommended to use the dot (.) syntax to access variables.

```php
{{ variable['key'] }} {# This is allowed, but use {{ variable.key }} instead! #}
{{ variable[0] }} {# This is allowed, but use {{ variable.0 instead! }} #}
```

### Loops

- Using if conditions in loops:

You can limit the items returned in an iteration by adding an if clause. The example below will loop over all rows of the items variable, but only print out those where item.status evaluates to TRUE.

```php
{% for item in items if item.status %}
  {{ item.title }}
{% endfor %}
```

### Adding classes

```php
<ul class='blog-post__tags field__items'>
    {% for item in items %}
      <li{{ item.attributes.addClass('blog-post__tag') }}>{{ item.content }}</li>
    {% endfor %}
  </ul>
```

### Filters and Functions

Drupal has some Drupal-specific Twig functions that are meant to be called within a Twig file.

- url($name, $parameters, $options): Generates an absolute URL, given a route name and optional parameters.
- path($name, $parameters, $options): Generates a relative URL path given a route name and parameters.
- link($text, $url, $attributes): Create a link in HTML.
- file_url($uri): Accepts a relative path from the root and creates an absolute path to the file.

```php
<img
  src="{{ file_url(node.field_image.entity.uri.value) }}"
  alt="{{ node.field_image.alt }}"
  height="{{ node.field_image.height }}"
  width="{{ node.field_image.width }}"
/>

<!-- Output -->
<img src="https://example.com/sites/default/files/path/to/file.jpg" alt="My image alt" height="640" width="480" />
```

#### Using named arguments with filters

Arguments to filters can be named, so that a filter can be used without specifying a value for all arguments. This is useful when using filters like round, which takes two arguments: round(precision, method). Perhaps you only want to specify a value for the second argument and want to leave the first one as default. Writing your function like the example can make your templates more readable.

```php
 <div>{{ 3.14159|round(method="ceil") }}</div>
 ```



### Check if variable is not null

```php
{% if foo is not null and foo is not empty %}
{% if foo|length %}
```

### Render Node elements and fields

```php

{# Node Title #}
{{node.getTitle()}}

{# Image Field #}
{{ file_url(node.field_name.entity.fileuri) }} // Url
{{node.field_name.entity.alt.value}} // Alt

{# Normal field Value #}
{{node.field_name.value}}
{{node.field_name.value|raw}} // Raw Value

{# Date Field #}
{{ node.field_name.value | date("d/m/Y") }}


{{ file_url(node.field_name[key].entity.uri.value) }}
{{ node.field_name[key].alt }}
```

### Render Referenced Media Elements from a referenced Item

```php
{# Media element = Image #}
{% set IMAGEURL = node.FIELD_NAME.entity.MEDIA_FIELD_NAME|file_uri %}
{% set IMAGEALT = node.FIELD_NAME.entity.MEDIA_FIELD_NAME.entity.field_media_image.alt %}
{{ file_url(IMAGEURL)}}
{{ IMAGEALT }}

{# Using Twig tweak #}
{{ drupal_entity('media', node.FIELD_NAME.entity.MEDIA_FIELD_NAME.entity.id) }}
```

### Render multiple field elements

```php
// Loop through multiple items in a field, for example tags
{% for item in content.field_tags|children %}
  <p>{{ item }}</p>
{% endfor %}

{% for element in testArray %}
  {{ element.someString }},
{% endfor %}

{% for role in user.roles %}
  {{ role.name }}
  {% if not loop.last %},{% endif %}
{% endfor %}
```

### Render multiple taxonomy terms field

```php
// Core Method
{% if content.field_type_of_action %}
  {% for item in content.field_type_of_action['#items'] %}
    {{item.entity.tid.value}}
    {{item.entity.name.value}}
  {% endfor %}
{% endif %}

// Using Twig Tweak
{% set tid = node.field_type_of_action.target_id %}
{% if tid %} {{ drupal_field('name', 'taxonomy_term', tid) }} {% endif %}
```

### Render/Access List (Text) Field type

```php
{{node.field_name[0].value}}
```

### Render Translated content types/Taxonomies

```php
// THEMENAME.theme or MODULENAME.module
THEME_NAME_PREPROCESS_NODE(&variables){
$language = \Drupal::languageManager()->getCurrentLanguage()->getId();
variables['language'] = $language;
}

// Twig File
{% for item in content.FIELD_NAME['#items'] %}
  {{item.entity.TRANSLATION(LANGUAGE).name.value}}
{% endfor %}

// This Works for field values
{{node.translation(LANGUAGE).body.value}}
```

### Render the label of a select field type

```php
{% for key, item in NODE.CONTENT_TYPE.entity.FIELD_NAME if key|first != '#' %}
    {% set VALUE = item.value %}
    {{ NODE.CONTENT_TYPE.entity.FIELD_NAME.getSetting('allowed_values')[VALUE] }}
{% endfor %}
```
### Reference

For more information about Twig, see the [Twig documentation](https://twig.symfony.com/doc/2.x/).
Also here are some useful links that can help you to understand how to use Twig in Drupal 8/9:
- [Rendereable Elements Attributes](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Render%21Element%21RenderElement.php/class/RenderElement/9.1.x)
- [Render API](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Render%21theme.api.php/group/theme_render/9.1.x)

