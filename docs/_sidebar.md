# Drupal Documentation

- [Drupal Documentation](#drupal-documentation)
  - [Learn Drupal](#learn-drupal)
  - [Modules](#modules)
    - [Developmemt](#developmemt)
    - [Productivity](#productivity)
    - [Themes](#themes)
    - [Search](#search)
  - [Configuration](#configuration)
  - [Best Practices](#best-practices)
  - [Nodes](#nodes)
    - [Get Current Node information](#get-current-node-information)
    - [Get current Node information on preview page](#get-current-node-information-on-preview-page)
    - [Get info of a specific Nodes - EntityQuery](#get-info-of-a-specific-nodes---entityquery)
    - [Get info of a specific Nodes - EntityTypeManager](#get-info-of-a-specific-nodes---entitytypemanager)
    - [Links](#links)
    - [Get Taxonomy Term Names](#get-taxonomy-term-names)
      - [Method 1 - entityTypeManager - Gets Taxonomy Terms](#method-1---entitytypemanager---gets-taxonomy-terms)
      - [Method 2 - EntityQuery + load - Get Taxonomy Terms sorted by custom field](#method-2---entityquery--load---get-taxonomy-terms-sorted-by-custom-field)
      - [Method 3 - Same as method 3 but using entityTypeManager](#method-3---same-as-method-3-but-using-entitytypemanager)
  - [Templates](#templates)
    - [Use specific Template](#use-specific-template)
    - [Add current path as css class](#add-current-path-as-css-class)
    - [Get all parameters](#get-all-parameters)
    - [Get current language](#get-current-language)
    - [Get base path](#get-base-path)
  - [Twig](#twig)
    - [Check if variable is not null](#check-if-variable-is-not-null)
    - [Render Node elements and fields](#render-node-elements-and-fields)
    - [Render Referenced Media Elements from a referenced Item](#render-referenced-media-elements-from-a-referenced-item)
    - [Render multiple field elements](#render-multiple-field-elements)
    - [Render multiple taxonomy terms field](#render-multiple-taxonomy-terms-field)
    - [Render/Access List (Text) Field type](#renderaccess-list-text-field-type)
    - [Render Translated content types/Taxonomies](#render-translated-content-typestaxonomies)
    - [Render the label of a select field type](#render-the-label-of-a-select-field-type)
    - [Links](#links-1)
  - [Urls](#urls)
    - [Get current url](#get-current-url)
    - [Get current path](#get-current-path)
    - [Get current path alias](#get-current-path-alias)
    - [Check current path](#check-current-path)
    - [Get current domain](#get-current-domain)
    - [Get BaseUrl](#get-baseurl)
    - [Pass Parameters to another Route](#pass-parameters-to-another-route)
    - [Get query parameters from current URL](#get-query-parameters-from-current-url)
  - [Libraries](#libraries)
    - [Attach library to specific page](#attach-library-to-specific-page)
    - [Attach library to specific form](#attach-library-to-specific-form)
    - [Attach library from twig file](#attach-library-from-twig-file)
  - [Stored Procedures](#stored-procedures)
    - [Execute a Stored Procedure](#execute-a-stored-procedure)
    - [Execute a Stored Procedure and get data as a return](#execute-a-stored-procedure-and-get-data-as-a-return)
  - [SQL](#sql)
    - [Grouping query conditions + Using SQL LIKE operator](#grouping-query-conditions--using-sql-like-operator)
    - [Query Pagination](#query-pagination)
      - [Using PagerSelectExtender](#using-pagerselectextender)
      - [Using Stored Procedure + PHP](#using-stored-procedure--php)
  - [Forms](#forms)
    - [Create a form with Ajax Callback - namespace + buildForm()](#create-a-form-with-ajax-callback---namespace--buildform)
    - [Create a form with Ajax Callback - submitForm()](#create-a-form-with-ajax-callback---submitform)
    - [Create a form with Ajax Callback - validateForm()](#create-a-form-with-ajax-callback---validateform)
    - [Button with other function](#button-with-other-function)
    - [Form elements](#form-elements)
    - [Add classes to form](#add-classes-to-form)
    - [Add class to form element Label](#add-class-to-form-element-label)
    - [Disable advanced search and help links from search form](#disable-advanced-search-and-help-links-from-search-form)
    - [Get the element name instead of value](#get-the-element-name-instead-of-value)
  - [Files & Images](#files--images)
    - [Create CSV](#create-csv)
    - [Create the url of an image](#create-the-url-of-an-image)
  - [Messages](#messages)
  - [Add Metatags](#add-metatags)
  - [Create Next/Previous Post links](#create-nextprevious-post-links)
  - [Other useful functions](#other-useful-functions)
    - [Create Permalink without accents](#create-permalink-without-accents)
    - [Check if string Starts With](#check-if-string-starts-with)
    - [Group Array by Date](#group-array-by-date)
    - [Flatten Array](#flatten-array)
  - [Drush](#drush)
    - [Instalation](#instalation)
    - [Drush Commands i use most](#drush-commands-i-use-most)

## Learn Drupal

- [How to start with Drupal](https://stackoverflow.com/questions/9713806/where-to-learn-drupal-from-scratch/34879758#34879758)
- [Drupalize](https://drupalize.me/)
- [DrupalBook](https://drupalbook.org/about-drupalbook)
- [Heshans Blog](https://www.heididev.com/)
- [Drupal tips](https://codimth.com/)

## Modules ##

### Developmemt ###

- Devel
- Admin Toolbar
- Rabbit Hole => Tested Weekly Passes with D9.3 + PHP 8
- Twig Tweak => Tested Weekly
- Metatag => Tested Weekly
- Vardumper
- Twig Vardumper
- Simple Cron

### Productivity ###

- Easy Breadcrumb => Tested Weekly
- Bulk update fields _(optional)_
- Coffee
- Better Exposed Filters
- Simple XML Sitemap
- Entity Type Clone

### Themes ###

- AT Theme 2.0 + AT Theme generator + AT Tool
- Bootstrap Sass

### Search ###

- Search Api
- Search Api Solr
- Facets

## Configuration

- Config Filter
- Config ignore

## Best Practices



## Nodes

### Get Current Node information

```php
// Using a preprocessing function we can access information on the current node
$node = \Drupal::routeMatch()->getParameter('node'); // get node
if ($node instanceof \Drupal\node\NodeInterface){} // Check if it's node
$CONTENT_TYPE = $node->getType(); // Node type
$nid = $node->id(); // Node id | nid
$title = $node->getTitle();; // Node title
$node->bundle(); // Node Type same as getType()
```

### Get current Node information on preview page

```php
  $routeMatch   = \Drupal::routeMatch();
  $node_preview = $routeMatch->getParameter( 'node_preview' );
```z

### 1.4.3. Get info of a specific Node - NODE::load()

```php
$NODE_TITLE =  Node::load(ID)->getTitle(); // Node title
```

### Get info of a specific Nodes - EntityQuery

```php
$node_ids = Drupal::entityQuery('node')
      ->condition('type', CONTENT_TYPE)
      ->condition('FIELD_NAME', FIELD_ID)
      ->execute(); // Get all the node ids that match these conditions
$nodes = Node::loadMultiple($node_ids); // All the nodes in an assoc array
```

### Get info of a specific Nodes - EntityTypeManager

```php
$query = \Drupal::entityTypeManager()->getStorage('node');
$query_result = $query->getQuery();

$nodo = $query_result->condition('nid', $nid, '=')
        ->condition('type', 'blog')
        ->condition('status', 1)
        ->sort('nid', 'ASC')
        ->range(0, 1)
        ->execute();

$nodo = array_values($nodo);
$nodo = $nodo[0];

$titulo =  Node::load($nodo)->getTitle(); // Titulo

$enlace = \Drupal::service('path_alias.manager')
          ->getAliasByPath('/node/' . $nodo);  // Relative url

```

### Links

[Node Methods](https://api.drupal.org/api/drupal/core%21modules%21node%21src%21Entity%21Node.php/class/Node/9.1.x)

### Get Taxonomy Term Names

#### Method 1 - entityTypeManager - Gets Taxonomy Terms

```php
$tree = \Drupal::entityTypeManager()->getStorage('taxonomy_term')->loadTree(
  'grafica_ambito', // The taxonomy term vocabulary machine name.
  0,                 // The "tid" of parent using "0" to get all.
  1,                 // Get only 1st level.
  TRUE               // Get full load of taxonomy term entity.
);
$results = [];

foreach ($tree as $term) {
  $results[] = $term->getName();
}
```

#### Method 2 - EntityQuery + load - Get Taxonomy Terms sorted by custom field

```php
$taxonomy_terms = \Drupal::entityQuery('taxonomy_term')
    ->condition('vid', "TAXONOMY_TERM")
    ->sort('FIELD');

$results = $taxonomy_terms->execute();

$term_names_array = [];


  $term = \Drupal\taxonomy\Entity\Term::load($result);
  $term_names_array[] = $term->name->value;
}
```

#### Method 3 - Same as method 3 but using entityTypeManager

```php

$taxonomy_terms = \Drupal::entityQuery('taxonomy_term')
    ->condition('vid', "TAXONOMY_TERM")
    ->sort('FIELD');

$results = $taxonomy_terms->execute();

$term_names_array = [];

foreach ($results as $result) {
  $term = \Drupal::entityTypeManager()
    ->getStorage('taxonomy_term')
    ->load($result);
  $term_names_array[] = $term->getName();
}
```

## Templates

### Use specific Template

```php
function THEMENAME_theme_suggestions_page_alter(array &$suggestions, array $variables){
 $suggestions[] = 'page__roads';  //page--roads.html.twig
}
```

### Add current path as css class

```php
$current_path = \Drupal::service('path.current')->getPath();
$path_alias = \Drupal::service('path_alias.manager')->getAliasByPath($current_path);
$path_alias = ltrim($path_alias, '/');
$variables['attributes']['class'][] = 'path-' . \Drupal\Component\Utility\Html::cleanCssIdentifier($path_alias);
```

### Get all parameters

```php
$parameters = \Drupal::routeMatch()->getParameters()->all();
$parameters['taxonomy_term'];
```

### Get current language

```php
$parameters = \Drupal::routeMatch()->getParameters()->all();
$variables['language'] = \Drupal::languageManager()->getCurrentLanguage();
$variables['language'] = \Drupal::languageManager()->getCurrentLanguage()->getId();
```

### Get base path

```php
$variables['base_path'] = base_path();
```

## Twig

### Check if variable is not null

```php
{% if foo is not null and foo is not empty %}
{% if foo|length %}
```

### Render Node elements and fields

```php

{# Node Title #}
{{node.title.value}}

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
// Native
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
// Theme file
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

### Links

- [Rendereable Elements Attributes](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Render%21Element%21RenderElement.php/class/RenderElement/9.1.x)
- [Render API](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Render%21theme.api.php/group/theme_render/9.1.x)

## Urls

### Get current url

```php

// Method 1
$url = Url::fromRoute('<current>');
$url = \Drupal\Core\Url::fromRoute('<current>');
$path = $url->getInternalPath();

// Method 2
Url::fromRoute('<current>',array(),array('absolute'=>'true'))->toString();

// Method 3
use Drupal\Core\Url;
Url::fromRoute('<current>', [], ['query' => \Drupal::request()->query->all(), 'absolute' => 'true'])->toString();

// Method 4
$current_route = \Drupal::routeMatch()->getRouteName();
$current_parameters = \Drupal::routeMatch()->getParameters();

// Method 5
use Drupal\Core\Url;
$url = Url::fromRoute(\Drupal::routeMatch()->getRouteName(), \Drupal::routeMatch()->getRawParameters()->all());
```

### Get current path

```php
$current_path = \Drupal::service('path.current')->getPath();
```

### Get current path alias

```php
$path_alias = \Drupal::service('path_alias.manager')->getAliasByPath($current_path);
```

### Check current path

```php
 \Drupal::service('path.matcher')->isFrontPage();
```

### Get current domain

```php
\Drupal::request()->getSchemeAndHttpHost()
```

### Get BaseUrl

```php
\Drupal::request()->baseUrl()
```

### Pass Parameters to another Route

```php

// On form submit, do this
 $url = Url::fromRoute('ROUTE')->setRouteParameters([
    'name' => $name,
]);

// redirect form to this url with these parameters
$form_state->setRedirectUrl($url);
```

### Get query parameters from current URL

```php

// Recommended
$keyword = \Drupal::request()->query->get('QUERY_PARAMETER');

// Using $_GET[]
$keyword = $_GET["QUERY_PARAMETER"] ?? "";
```

## Libraries

### Attach library to specific page

```php
function THEMENAME_preprocess_page(&$variables){
  $current_path = \Drupal::service('path.current')->getPath();
  if ($current_path === '/mypath') {
    $variables['#attached']['library'][] = 'library/name';
  }
}
```

### Attach library to specific form

```php
function MODULE_NAME_form_alter(&$form, &$form_state, $form_id)
{

  if ($form_id == 'FORM_ID') {
    $form['#attached']['library'][] = 'MODULE_NAME/LIBRARY_NAME';
  }
}
```

### Attach library from twig file

```php
{{ attach_library('library/name') }}
```

## Stored Procedures

### Execute a Stored Procedure

```php

// Executing the query --> RETURNS A BOOLEAN
$results = Database::getConnection()->query("EXEC SCHEMA.SP_SP_NAME", $options)->execute();

```

### Execute a Stored Procedure and get data as a return

1. The PHP File we're working with

```php

  // We don't use execute(), we directly run fetchAssoc() to get an associative array or fetchObject() to get a an object
  $sp = Database::getConnection()->query("EXECUTE DRU.SP_NAME", $options)->fetchAssoc();

  // OR
  // $sp = Database::getConnection()->query("EXECUTE DRU.SP_NAME", $options)->fetchObject();

  return [
    '#sp' => $sp,
  ]
```

2. we get the data from the return and pass it onto the twig file

```php

<!-- Since it's an associative array, we need the key and value -->

{% for key,value in sp %}
    Key :
    {{ key }}
    Value :
    {{ value }}
{% endfor %}

```

## SQL

### Grouping query conditions + Using SQL LIKE operator

```php

// We use orConditionGroup() to groupe multiple Conditions
// We use escapeLike() to use the LIKE operator

Database::setActiveConnection('external');
$db = Database::getConnection();

$query = $db->select('TABLEPREFIX.TABLE_NAME', 'ALIAS');
$query->fields('ALIAS', ['FIELDS']);
 $group = $query->orConditionGroup()
            ->condition('FIELD_NAME', "%" . $db->escapeLike($palabra_clave) . "%", 'LIKE')
            ->condition('FIELD_NAME', "%" . $db->escapeLike($palabra_clave) . "%", 'LIKE');
$query->condition($group);
$results = $pager->execute()->fetchAll();
Database::setActiveConnection();
```

### Query Pagination

#### Using PagerSelectExtender

```php

// We use PagerSelectExtender to limit the what we get on our page
// $pager should be passed later to the twig file as a variable

use Drupal\Core\Database\Query\PagerSelectExtender;

Database::setActiveConnection('external');
$db = Database::getConnection();

$query = $db->select('TABLEPREFIX.TABLE_NAME', 'ALIAS');
$query->fields('ALIAS', ['FIELDS']);
 $group = $query->orConditionGroup()
            ->condition('FIELD_NAME', "%" . $db->escapeLike($palabra_clave) . "%", 'LIKE')
            ->condition('FIELD_NAME', "%" . $db->escapeLike($palabra_clave) . "%", 'LIKE');
$query->condition($group);
$pager = $query->extend(PagerSelectExtender::class)->limit(5);
$results = $pager->execute()->fetchAll();
Database::setActiveConnection();
```

#### Using Stored Procedure + PHP

```php
// Execute the Sp to get the count result
$count = Database::getConnection()->query("EXECUTE SP_Count", $options);

// Count_result
$count_result = $count->fetchAll();
// Count_total
$count_total = current($count_result)->contador;
// Items Per page
$per_page = 10;
// All Results
$total_count = (int) $count_total;
// Total_Pages.
$total_pages = ceil($total_count / $per_page);
// Current_page
$current_page = (int) ($_GET['page'] ?? 1);
// Pagina != 0.
if ($current_page < 1 || $current_page > $total_pages) {
    $current_page = 1;
}
// offset.
$offset = $per_page * ($current_page - 1);


// Remove number from page url
$pattern = '/page=(\d+)/i';
$replacement = 'page=';
$rest = preg_replace($pattern, $replacement, $current_url);


// Showing XX results of XX
 $my_offset = $offset + $per_page;
  $current_offset = $offset + 1;

  if ($count_total == 0) {
      echo t('No results found for the selected search.');
  } else if ($count_total >= 10) {
      if ($current_page == $total_pages) {
          echo "Showing $current_offset - $count_total of $count_total";
      } else {
          echo "Showing $current_offset - $my_offset of $count_total";
      }
  } else {
      echo "Showing $current_offset - $count_total of $count_total";
  }

   // Primera Pagina
   if ($current_page > 1) :

    if (!$page) : ?>
      <li class="pager__item pager__item--first">
       <?php echo "<a href=\"/home/?page=1\" title=\"Go to first page\" rel=\"first\">
               <span class=\"visually-hidden\">First Page</span>
               <span aria-hidden=\"true\">« First</span>
               </a>"; ?>
   </li>
   <?php else : ?>
   <li class="pager__item pager__item--first">
       <?php echo "<a href=" . $rest . "1" . " title=\"Go to first page\" rel=\"first\">
               <span class=\"visually-hidden\">First Page</span>
               <span aria-hidden=\"true\">« First</span>
               </a>"; ?>
   </li>
   <?php endif;
   endif;
   ?>

   <?php
   if ($total_pages != 1) {
       $win = 2; // window size
       $gap = false;
       for ($i = 1; $i <= $total_pages; $i++) {
           if ($i > 1 + $win && $i < $total_pages - $win && abs($i - $current_page) > $win) {
               if (!$gap) {
                   $gap = true;
               }
               continue;
           }
           $gap = false;
           if ($current_page == $i) {
               echo "<li class=\"pager__item is-active active\">
               <a href=\"/home/?page=$i\" title=\"Current Page\">
               <span class=\"visually-hidden\">Current Page</span>
               $i
               </a>
               </li>";
           } else if (!$page) {
               echo "<li class=\"pager__item\">
               <a href=\"/home/?page=$i\" title=\"Got to page $i\">
               <span class=\"visually-hidden\">Page</span>$i</a>
               </li>";
           } else {
               echo "<li class=\"pager__item\">
               <a href=" . $rest . $i . " title=\"Go to page $i\">
               <span class=\"visually-hidden\">Page</span>$i</a>
               </li>";
           }
       }
   }
   ?>

   <?php
   // Ultima Pagina
   if ($current_page < $total_pages) : // current page < total pages
       if (!$page) :
   ?>
   <li class="pager__item pager__item--last">
       <?php echo "<a href=\"/home/?page=" . $total_pages . "\" title=\"Go To Last page\" rel=\"last\">
               <span class=\"visually-hidden\">Last Page</span>
           <span aria-hidden=\"true\">Last »</span>
           </a>"; ?>
   </li>
   <?php else :
       ?>
   <li class="pager__item pager__item--last">
       <?php echo "<a href=" . $rest . $total_pages . " title=\"Go To Last page\" rel=\"last\">
               <span class=\"visually-hidden\">Last Page</span>
           <span aria-hidden=\"true\">Last »</span>
       </a>"; ?>
   </li>
   <?php endif; // !page
   endif; // current page < total pages
   ?>
```

## Forms

### Create a form with Ajax Callback - namespace + buildForm()

```php
/**
 * @file
 * Contains \Drupal\datos_convocatoria\Form\FormularioEdit.
 * Formulario de edicion de datos
 */

namespace Drupal\MODULENAME\Form;

use Drupal\Core\Form\FormBase;
use Drupal\Core\Form\FormStateInterface;
use Drupal\Core\Url;
use Drupal\Core\Database\Database;

use Drupal\Core\Ajax\AjaxResponse;
use Drupal\Core\Ajax\OpenModalDialogCommand;

use Drupal\file\Entity\File;


// Create a class that extends from FormBase
class FORMNAME extends FormBase
{

  protected $files;
  /**
   * {@inheritdoc}
   */
  public function getFormId()
  {
    // We give the form an ID
    return 'FORMID';
  }

// Create the required buildForm() function
  public function buildForm(array $form, FormStateInterface $form_state)
  {
    Database::setActiveConnection('external');


// Text
$form['FIELD'] = array(
  '#type' => 'textarea',
  '#title' => t('Titulo:'),
  '#maxlength' => 1000,
  '#required' => TRUE,
  '#default_value' => $result[0]->FIELD_NAME
);
// Date
$form['FIELD'] = array(
  '#type' => 'date',
  '#title' => t('TITLE'),
  '#required' => FALSE,
  '#default_value' => $result[0]->FIELD_NAME
);

// Select
$form['FIELD'] = array(
  '#type' => 'select',
  '#title' => t('TITLE'),
  '#options' => $estados_convocatoria,
  '#default_value' => $result[0]->FIELD_NAME,
);
// Number
$form['FIELD'] = array(
  '#type' => 'number',
  '#step' => 'any',
  '#title' => t('TITLE:'),
  '#required' => FALSE,
  '#default_value' => $result[0]->FIELD_NAME
);


// File Upload
$ext_permitidas = 'jpg png';
$max_upload = 16000000;

$form['FIELD'] = array(
  '#type' => 'managed_file',
  '#title' => t('TITLE'),
  '#description' => $this->t('Allowed extensions: @allowed_ext', ['@allowed_ext' => $ext_permitidas]),
  '#upload_location' => 'public://PATH',
  '#multiple' => FALSE,
  '#required' => FALSE,
  '#default_value' => [$result[0]->FIELD_NAME],
  '#upload_validators' => [
    'file_validate_extensions' => [
      $ext_permitidas,
    ],
    'file_validate_size' => [
      $max_upload,
    ],
  ],
);

// Buttons
$form['actions']['#type'] = 'actions';
$form['actions']['submit'] = [
  '#type' => 'submit',
  '#default_value' => $this->t('Save'),
  '#button_type' => 'primary',
];

$form['actions']['delete'] = [
  '#type' => 'button',
  '#value' => t('Borrar'),
  '#attributes' => [
    'class' => ['btn', 'btn-danger', 'use-ajax'],
  ],
  '#ajax' => [
    'callback' => '::myAjaxCallback', // don't forget :: when calling a class method.
    //'callback' => [$this, 'myAjaxCallback'], //alternative notation
    'disable-refocus' => FALSE, // Or TRUE to prevent re-focusing on the triggering element.
    // 'event' => 'click',
    'wrapper' => 'drupal-modal', // This element is updated with this AJAX callback.
    'effect' => 'fade',
    'progress' => [
      'type' => 'throbber',
    ],
  ]
];

return $form;
  }
```

### Create a form with Ajax Callback - submitForm()

```php
// Create the required submitForm() function
public function submitForm(array &$form, FormStateInterface $form_state)
{

// Get values with condition
if (!empty($form_state->getValue('FIELD'))) {
  $FIELD_NAME = $form_state->getValue('FIELD');
} else {
  $FIELD_NAME =  NULL;
}


// Float Values
if (!empty($form_state->getValue('FIELD'))) {
  $FIELD_NAME = floatval($form_state->getValue('FIELD'));
} else {
  $FIELD_NAME = NULL;
}


// Integer Values
if (!empty($form_state->getValue('FIELD'))) {
  $FIELD_NAME = intval($form_state->getValue('FIELD'));
} else {
  $FIELD_NAME = NULL;
}


// Files
if (!empty($form_state->getValue('FIELD'))) {
  $FIELD_NAME = reset($form_state->getValue('FIELD'));
  $file = File::load($FIELD_NAME);
  $file->setPermanent();
  $file->save();
  $file_name = $file->getFilename();
  $file_url = 'PATH' . $file->createFileUrl();
} else {
  $FIELD_NAME = null;
  $file_url = 'SPECIFIC_URL/NAME.PNG';
}

// Create a message to display once redirected
\Drupal::messenger()->addMessage('Convocatoria Modificada');
$form_state->setRedirect('convocatorias.convocatorias');
}

// Create the OPTIONAL ajax callback function
public function myAjaxCallback(array &$form, FormStateInterface $form_state)
{
  $response = new AjaxResponse();
  return $response;
}
```

### Create a form with Ajax Callback - validateForm()

```php
// Create the OPTIONAL validateForm() function
public function validateForm(array &$form, FormStateInterface $form_state)
{

  $FIELD_NAME = $form_state->getValue('FIELD_NAME');
  if (!empty($FIELD_NAME)) {
    $regex = preg_match("/^(?:2[0-3]|[01][0-9]):[0-5][0-9]$/", $FIELD_NAME);

    if ($regex == 0) {
      $form_state->setErrorByName('$FIELD_NAME', $this->t('Please indicate a valid time. The format must be hh:mm'));
    }
  }
}
```

### Button with other function

```php
 $form['actions']['BUTTONNAME'] = [
  '#type' => 'submit',
  '#default_value' => $this->t('BUTTON LABEL'),
  '#submit' => array('::FUNCTION'), // we execute this function
];
```

### Form elements

```php

$form['name'] =
    '#type' => '',
    '#value' => t(''),
    '#prefix' => '',
    '#placeholder' => t(''),
    '#size' => ,
    '#maxlength' => ,
    '#title_display' => 'before',
    '#wrapper_attributes' => [],
    '#label_attributes' => [
      'class' => [ 'my-class', 'other-class']
    ];
```

### Add classes to form

```php

// Form Class
$form['#attributes']['class'][] = 'form-inline';
```

### Add class to form element Label

```php
// Method 1
$form['name'] =
    '#label_attributes' => [
      'class' => [ 'my-class', 'other-class']
    ];

// Method 2
 $form['name'] = array(
    '#label_class' => ['something']
);
```

### Disable advanced search and help links from search form

```php
  if ($form_id == 'search_form') {
    $form['help_link'] = [];
    $form['advanced'] = [];
    $form['basic']['keys'] += [
      '#label_attributes' => [
        'class' => ['sr-only']
      ],
    ];
    $form['basic']['submit'] = [
      '#type' => 'submit',
      '#value' => t('Buscar')
    ];
  }
```

### Get the element name instead of value

```php
if (!empty($form_state->getValue('name'))) {

$value =  $form_state->getValue('name');

$FIELD_NAME = $form['FIELD_NAME']['#options'][$value]; //This
 } else {
  $FIELD_NAME = NULL;
 }
```

## Files & Images

### Create CSV

```php
public function FUNCTION_NAME(array &$form, FormStateInterface $form_state)
{


    $bdd = Database::getConnection();

    $query = $bdd->select('SCHEMA.TABLE_NAME', 'c');
    $query->fields('c', [
        'TITLE',
        'NAME',
    ]);
    $result = $query->execute()->fetchAll();

    $delimiter = "|";
    $filename = "file_name" . date('Y-m-d') . ".csv";

    $uri = 'public://FOLDER/' . $filename;

    // Create a file pointer
    $f = fopen($uri, 'w');

    // Set column headers
    $fields = [
        'HEADER1',
        'HEADER2',
    ];

    fputcsv($f, $fields, $delimiter);


    foreach ($result as $row) {
        $lineData = [
            $row->TITLE,
            $row->NAME
        ];
        fputcsv($f, $lineData, $delimiter);
    }

    // Move back to beginning of file
    fseek($f, 0);
    //output all remaining data on a file pointer
    fpassthru($f);
    fclose($f);

    $file = file_save_data($f, 'public://' . $filename);

    $uri_final = '/sites/default/files/FOLDER/' . $filename;

    \Drupal::messenger()->addStatus(['#markup' => '<a href=' . $uri_final . ' target="_blank" rel= noopener noreferrer" >DOWNLOAD FILE</a>']);

     // Create url from route in .routing.yml
    $url1 = Url::fromRoute('route');

     // Redirect to url
    $form_state->setRedirectUrl($url1);
}
```

### Create the url of an image

```php
use Drupal\file\Entity\File;

$mid = $node->field_image->target_id;
if (!empty($mid)) {
   // Crear enlace de la imagen
   $file = File::load($mid);
   $url = $file->getFileUri();
   $m_url = file_create_url($url);
}
```

## Messages

1. Create a link as a message

```php
   \Drupal::messenger()->addError('');
   \Drupal::messenger()->addWarning('');
   \Drupal::messenger()->addStatus(['#markup' => '<a href=' . $enlace_convocatoria_nueva . ' target="_blank" rel= noopener noreferrer" >OPEN LINK</a>']);
```

## Add Metatags

```php
function MODULE_NAME_page_attachments_alter(array &$page)
{

    $meta_title = "";
    $meta_description = "";
    $meta_og_img = \Drupal::request()->getSchemeAndHttpHost() . '/themes/bootstrap/image.png';


    $meta_title_html = [
      '#type' => 'html_tag',
      '#tag' => 'title',
      '#value' => $meta_title
    ];

    $meta_description = [
      '#type' => 'html_tag',
      '#tag' => 'meta',
      '#attributes' => [
        'name' => 'description',
        'content' => $meta_description,
      ],
    ];

    $meta_og_title = [
      '#type' => 'html_tag',
      '#tag' => 'meta',
      '#attributes' => [
        'name' => 'og:title',
        'content' => $meta_title,
      ],
    ];

    $meta_og_img_html = [
      '#type' => 'html_tag',
      '#tag' => 'meta',
      '#attributes' => [
        'name' => 'og:image',
        'content' => $meta_og_img,
      ],
    ];

    $tw_card = [
      '#type' => 'html_tag',
      '#tag' => 'meta',
      '#attributes' => [
        'name' => 'twitter:card',
        'content' => 'summary_large_image',
      ],
    ];

    $tw_title = [
      '#type' => 'html_tag',
      '#tag' => 'meta',
      '#attributes' => [
        'name' => 'twitter:title',
        'content' => $meta_title,
      ],
    ];

    $tw_image = [
      '#type' => 'html_tag',
      '#tag' => 'meta',
      '#attributes' => [
        'name' => 'twitter:image',
        'content' => $meta_og_img,
      ],
    ];


    $page['#attached']['html_head'][] = [$meta_title_html, 'title'];
    $page['#attached']['html_head'][] = [$meta_description, 'description'];
    $page['#attached']['html_head'][] = [$meta_og_title, 'og:title'];
    $page['#attached']['html_head'][] = [$meta_og_img_html, 'og:image'];
    $page['#attached']['html_head'][] = [$tw_card, 'twitter:card'];
    $page['#attached']['html_head'][] = [$tw_title, 'twitter:title'];
    $page['#attached']['html_head'][] = [$tw_image, 'twitter:image'];
  }
```

## Create Next/Previous Post links

```php

function THEMENAME_preprocess_pages(&$variables){
$node = \Drupal::routeMatch()->getParameter('node');
if ($node instanceof \Drupal\node\NodeInterface) {
   $tipo_contenido = $node->getType(); // Tipo de contenido
   $nid = $node->id(); // Id del nodo
   if ($tipo_contenido == 'blog') {
     // Next
      $query_next = \Drupal::entityTypeManager()->getStorage('node');
      $query_result = $query_next->getQuery();

      $next = $query_result->condition('nid', $nid, '>')
        ->condition('type', 'blog')
        ->condition('status', 1)
        ->sort('nid', 'ASC')
        ->range(0, 1)
        ->execute();

      if (!empty($next) && is_array($next)) {
        $next = array_values($next);
        $next = $next[0];

        $titulo_siguiente =  Node::load($next)->getTitle(); // Titulo
        $enlace_siguiente = \Drupal::service('path_alias.manager')
          ->getAliasByPath('/node/' . $next);  // enlace relativo

        $variables['titulo_siguiente'] = $titulo_siguiente;
        $variables['enlace_siguiente'] = $enlace_siguiente;
      }

      // Back
      $query_prev = \Drupal::entityTypeManager()->getStorage('node');
      $query_result = $query_prev->getQuery();
      $prev = $query_result->condition('nid', $nid, '<')
        ->condition('type', 'blog')
        ->condition('status', 1)
        ->sort('nid', 'DESC')
        ->range(0, 1)
        ->execute();

      if (!empty($prev) && is_array($prev)) {
        $prev = array_values($prev);
        $prev = $prev[0];

        $titulo_previo =  Node::load($prev)->getTitle(); // Titulo
        $enlace_previo = \Drupal::service('path_alias.manager')
          ->getAliasByPath('/node/' . $prev);  // enlace relativo

        $variables['titulo_previo'] = $titulo_previo;
        $variables['enlace_previo'] = $enlace_previo;
      }
   }
}
}

// page--blog.html.twig
{% if titulo_previo %}
	<div class="c-paginationdetail__content">
		<a href="{{enlace_previo}}">
			<p>ANTERIOR</p>
			<span>{{titulo_previo}}</span>
		</a>
	</div>
{% endif %}
{% if titulo_siguiente %}
	<div class="c-paginationdetail__content c-paginationdetail__content--right">
		<a href="{{enlace_siguiente}}">
			<p>POSTERIOR</p>
			<span>{{titulo_siguiente}}</span>
		</a>
	</div>
{% endif %}
```

## Other useful functions

### Create Permalink without accents

```php
/**
 * Funcion crear un permalink/slug a partir de un titulo
 */
function createSlug($str, $delimiter = '-')
{

    $unwanted_array =
        [
            'à' => 'a', 'á' => 'a', 'é' => 'e', 'è' => 'e', 'ì' => 'i', 'í' => 'i', 'ó' => 'o', 'ò' => 'o', 'ú' => 'u', 'ù' => 'u', 'ç' => 'c', 'ñ' => 'n', 'Á' => 'a', 'À' => 'a', 'È' => 'e', 'É' => 'e', 'Ì' => 'i', 'Í' => 'i', 'Ò' => 'o', 'Ó' => 'o', 'Ú' => 'u', 'Ù' => 'u', 'Ñ' => 'n', 'Ç' => 'c'
        ];
    $str = strtr($str, $unwanted_array);

    $slug = strtolower(trim(preg_replace('/[\s-]+/', $delimiter, preg_replace('/[^A-Za-z0-9-]+/', $delimiter, preg_replace('/[&]/', 'and', preg_replace('/[\']/', '', iconv('UTF-8', 'ASCII//TRANSLIT', $str))))), $delimiter));
    return $slug;
}
```

### Check if string Starts With

```php
// Function to check string starting
// with given substring
function startsWith($string, $startString)
{
    $len = strlen($startString);
    return (substr($string, 0, $len) === $startString);
}
// If the search term doesn't start with ' and the legnth is not 0
if (!startsWith($palabra_clave_sp, "'") && strlen(trim($palabra_clave_sp)) !== 0) {
    $count = Database::getConnection()->query("EXECUTE SP", $options);
} else {
    $count = Database::getConnection()->query("EXECUTE SP", $options);
}
```


### Group Array by Date

```php
private function groupByDate( $array ) {
    $grouped_array = [];
    foreach ( $array as $value ) {
      $grouped_array[ $value->FIELD_NAME ][] = $value;
    }

    return $grouped_array;
  }
```

### Flatten Array

```php
private function array_flatten( $array = null, $depth = 1 ) {
    $result = [];
    if ( ! is_array( $array ) ) {
      $array = func_get_args();
    }
    foreach ( $array as $key => $value ) {
      if ( is_array( $value ) && $depth ) {
        $result = array_merge( $result, $this->array_flatten( $value, $depth - 1 ) );
      } else {
        $result = array_merge( $result, [ $key => $value ] );
      }
    }

    return $result;
}
```

## Drush

### Instalation

```shell
composer require drush/drush:10.6.2
```

### Drush Commands i use most

```shell
# Cache clear
drush cc

# Cache and rebuild, same as domain.com/admin/rebuild.php
drush cr

# Export configuration, creates a configuration file in /config/sync
drush cex

# List modules/themes installed, can add flags to be more specific
# --status
# --type
drush pml

# Enable module
drush en MODULENAME

# Disable module
drush pmu MODULENAME
```
