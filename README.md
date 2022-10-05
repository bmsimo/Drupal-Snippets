# 1. Drupal Documentation

- [1. Drupal Documentation](#1-drupal-documentation)
  - [1.1. Learn Drupal](#11-learn-drupal)
  - [1.2. Modules](#12-modules)
    - [1.2.1. Developmemt](#121-developmemt)
    - [1.2.2. Productivity](#122-productivity)
    - [1.2.3. Themes](#123-themes)
    - [1.2.4. Search](#124-search)
  - [1.3. Configuration](#13-configuration)
  - [1.4. Nodes](#14-nodes)
    - [1.4.1. Get Current Node information](#141-get-current-node-information)
    - [1.4.2. Get current Node information on preview page](#142-get-current-node-information-on-preview-page)
    - [1.4.3. Get info of a specific Node - NODE::load()](#143-get-info-of-a-specific-node---nodeload)
    - [1.4.4. Get info of a specific Nodes - EntityQuery](#144-get-info-of-a-specific-nodes---entityquery)
    - [1.4.5. Get info of a specific Nodes - EntityTypeManager](#145-get-info-of-a-specific-nodes---entitytypemanager)
    - [1.4.6. Links](#146-links)
    - [1.4.7. Get Taxonomy Term Names](#147-get-taxonomy-term-names)
      - [1.4.7.1. Method 1 - entityTypeManager - Gets Taxonomy Terms](#1471-method-1---entitytypemanager---gets-taxonomy-terms)
      - [1.4.7.2. Method 2 - EntityQuery + load - Get Taxonomy Terms sorted by custom field](#1472-method-2---entityquery--load---get-taxonomy-terms-sorted-by-custom-field)
      - [1.4.7.3. Method 3 - Same as method 3 but using entityTypeManager](#1473-method-3---same-as-method-3-but-using-entitytypemanager)
  - [1.5. Templates](#15-templates)
    - [1.5.1. Use specific Template](#151-use-specific-template)
    - [1.5.2. Add current path as css class](#152-add-current-path-as-css-class)
    - [1.5.3. Get all parameters](#153-get-all-parameters)
    - [1.5.4. Get current language](#154-get-current-language)
    - [1.5.5. Get base path](#155-get-base-path)
  - [1.6. Twig](#16-twig)
    - [1.6.1. Check if variable is not null](#161-check-if-variable-is-not-null)
    - [1.6.2. Render Node elements and fields](#162-render-node-elements-and-fields)
    - [1.6.3. Render Referenced Media Elements from a referenced Item](#163-render-referenced-media-elements-from-a-referenced-item)
    - [1.6.4. Render multiple field elements](#164-render-multiple-field-elements)
    - [1.6.5. Render multiple taxonomy terms field](#165-render-multiple-taxonomy-terms-field)
    - [1.6.6. Render/Access List (Text) Field type](#166-renderaccess-list-text-field-type)
    - [1.6.7. Render Translated content types/Taxonomies](#167-render-translated-content-typestaxonomies)
    - [1.6.8. Render the label of a select field type](#168-render-the-label-of-a-select-field-type)
    - [1.6.9. Links](#169-links)
  - [1.7. Urls](#17-urls)
    - [1.7.1. Get current url](#171-get-current-url)
    - [1.7.2. Get current path](#172-get-current-path)
    - [1.7.3. Get current path alias](#173-get-current-path-alias)
    - [1.7.4. Check current path](#174-check-current-path)
    - [1.7.5. Get current domain](#175-get-current-domain)
    - [1.7.6. Get BaseUrl](#176-get-baseurl)
    - [1.7.7. Pass Parameters to another Route](#177-pass-parameters-to-another-route)
    - [1.7.8. Get query parameters from current URL](#178-get-query-parameters-from-current-url)
  - [1.8. Libraries](#18-libraries)
    - [1.8.1. Attach library to specific page](#181-attach-library-to-specific-page)
    - [1.8.2. Attach library to specific form](#182-attach-library-to-specific-form)
    - [1.8.3. Attach library from twig file](#183-attach-library-from-twig-file)
  - [1.9. Stored Procedures](#19-stored-procedures)
    - [1.9.1. Execute a Stored Procedure](#191-execute-a-stored-procedure)
    - [1.9.2. Execute a Stored Procedure and get data as a return](#192-execute-a-stored-procedure-and-get-data-as-a-return)
  - [1.10. SQL](#110-sql)
    - [1.10.1. Grouping query conditions + Using SQL LIKE operator](#1101-grouping-query-conditions--using-sql-like-operator)
    - [1.10.2. Query Pagination](#1102-query-pagination)
      - [1.10.2.1. Using PagerSelectExtender](#11021-using-pagerselectextender)
      - [1.10.2.2. Using Stored Procedure + PHP](#11022-using-stored-procedure--php)
  - [1.11. Forms](#111-forms)
    - [1.11.1. Create a form with Ajax Callback - namespace + buildForm()](#1111-create-a-form-with-ajax-callback---namespace--buildform)
    - [1.11.2. Create a form with Ajax Callback - submitForm()](#1112-create-a-form-with-ajax-callback---submitform)
    - [1.11.3. Create a form with Ajax Callback - validateForm()](#1113-create-a-form-with-ajax-callback---validateform)
    - [1.11.4. Button with other function](#1114-button-with-other-function)
    - [1.11.5. Form elements](#1115-form-elements)
    - [1.11.6. Add classes to form](#1116-add-classes-to-form)
    - [1.11.7. Add class to form element Label](#1117-add-class-to-form-element-label)
    - [1.11.8. Disable advanced search and help links from search form](#1118-disable-advanced-search-and-help-links-from-search-form)
    - [1.11.9. Get the element name instead of value](#1119-get-the-element-name-instead-of-value)
  - [1.12. Files & Images](#112-files--images)
    - [1.12.1. Create CSV](#1121-create-csv)
    - [1.12.2. Create the url of an image](#1122-create-the-url-of-an-image)
  - [1.13. Messages](#113-messages)
  - [1.14. Add Metatags](#114-add-metatags)
  - [1.15. Create Next/Previous Post links](#115-create-nextprevious-post-links)
  - [1.16. Other useful functions](#116-other-useful-functions)
    - [1.16.1. Create Permalink without accents](#1161-create-permalink-without-accents)
    - [1.16.2. Check if string Starts With](#1162-check-if-string-starts-with)
    - [1.16.3. Group Array by Date](#1163-group-array-by-date)
    - [1.16.4. Flatten Array](#1164-flatten-array)
  - [1.17. Drush](#117-drush)
    - [1.17.1. Instalation](#1171-instalation)
    - [1.17.2. Drush Commands i use most](#1172-drush-commands-i-use-most)

## 1.1. Learn Drupal

- [How to start with Drupal](https://stackoverflow.com/questions/9713806/where-to-learn-drupal-from-scratch/34879758#34879758)
- [Drupalize](https://drupalize.me/)
- [DrupalBook](https://drupalbook.org/about-drupalbook)
- [Heshans Blog](https://www.heididev.com/)
- [Drupal tips](https://codimth.com/)

## 1.2. Modules ##

### 1.2.1. Developmemt ###

- Devel
- Admin Toolbar
- Rabbit Hole => Tested Weekly Passes with D9.3 + PHP 8
- Twig Tweak => Tested Weekly
- Metatag => Tested Weekly
- Vardumper
- Twig Vardumper
- Simple Cron

### 1.2.2. Productivity ###

- Easy Breadcrumb => Tested Weekly
- Bulk update fields _(optional)_
- Coffee
- Better Exposed Filters
- Simple XML Sitemap
- Entity Type Clone

### 1.2.3. Themes ###

- AT Theme 2.0 + AT Theme generator + AT Tool
- Bootstrap Sass

### 1.2.4. Search ###

- Search Api
- Search Api Solr
- Facets

## 1.3. Configuration

- Config Filter
- Config ignore

## 1.4. Nodes

### 1.4.1. Get Current Node information

```php
// Using a preprocessing function we can access information on the current node
$node = \Drupal::routeMatch()->getParameter('node'); // get node
if ($node instanceof \Drupal\node\NodeInterface){} // Check if it's node
$CONTENT_TYPE = $node->getType(); // Node type
$nid = $node->id(); // Node id | nid
$title = $node->getTitle();; // Node title
$node->bundle(); // Node Type same as getType()
```

### 1.4.2. Get current Node information on preview page

```php
  $routeMatch   = \Drupal::routeMatch();
  $node_preview = $routeMatch->getParameter( 'node_preview' );
```

### 1.4.3. Get info of a specific Node - NODE::load()

```php
$NODE_TITLE =  Node::load(ID)->getTitle(); // Node title
```

### 1.4.4. Get info of a specific Nodes - EntityQuery

```php
$node_ids = Drupal::entityQuery('node')
      ->condition('type', CONTENT_TYPE)
      ->condition('FIELD_NAME', FIELD_ID)
      ->execute(); // Get all the node ids that match these conditions
$nodes = Node::loadMultiple($node_ids); // All the nodes in an assoc array
```

### 1.4.5. Get info of a specific Nodes - EntityTypeManager

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

### 1.4.6. Links

[Node Methods](https://api.drupal.org/api/drupal/core%21modules%21node%21src%21Entity%21Node.php/class/Node/9.1.x)

### 1.4.7. Get Taxonomy Term Names

#### 1.4.7.1. Method 1 - entityTypeManager - Gets Taxonomy Terms

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

#### 1.4.7.2. Method 2 - EntityQuery + load - Get Taxonomy Terms sorted by custom field

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

#### 1.4.7.3. Method 3 - Same as method 3 but using entityTypeManager

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

## 1.5. Templates

### 1.5.1. Use specific Template

```php
function THEMENAME_theme_suggestions_page_alter(array &$suggestions, array $variables){
 $suggestions[] = 'page__roads';  //page--roads.html.twig
}
```

### 1.5.2. Add current path as css class

```php
$current_path = \Drupal::service('path.current')->getPath();
$path_alias = \Drupal::service('path_alias.manager')->getAliasByPath($current_path);
$path_alias = ltrim($path_alias, '/');
$variables['attributes']['class'][] = 'path-' . \Drupal\Component\Utility\Html::cleanCssIdentifier($path_alias);
```

### 1.5.3. Get all parameters

```php
$parameters = \Drupal::routeMatch()->getParameters()->all();
$parameters['taxonomy_term'];
```

### 1.5.4. Get current language

```php
$parameters = \Drupal::routeMatch()->getParameters()->all();
$variables['language'] = \Drupal::languageManager()->getCurrentLanguage();
$variables['language'] = \Drupal::languageManager()->getCurrentLanguage()->getId();
```

### 1.5.5. Get base path

```php
$variables['base_path'] = base_path();
```

## 1.6. Twig

### 1.6.1. Check if variable is not null

```php
{% if foo is not null and foo is not empty %}
{% if foo|length %}
```

### 1.6.2. Render Node elements and fields

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

### 1.6.3. Render Referenced Media Elements from a referenced Item

```php
{# Media element = Image #}
{% set IMAGEURL = node.FIELD_NAME.entity.MEDIA_FIELD_NAME|file_uri %}
{% set IMAGEALT = node.FIELD_NAME.entity.MEDIA_FIELD_NAME.entity.field_media_image.alt %}
{{ file_url(IMAGEURL)}}
{{ IMAGEALT }}

{# Using Twig tweak #}
{{ drupal_entity('media', node.FIELD_NAME.entity.MEDIA_FIELD_NAME.entity.id) }}
```

### 1.6.4. Render multiple field elements

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

### 1.6.5. Render multiple taxonomy terms field

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

### 1.6.6. Render/Access List (Text) Field type

```php
{{node.field_name[0].value}}
```

### 1.6.7. Render Translated content types/Taxonomies

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

### 1.6.8. Render the label of a select field type

```php
{% for key, item in NODE.CONTENT_TYPE.entity.FIELD_NAME if key|first != '#' %}
    {% set VALUE = item.value %}
    {{ NODE.CONTENT_TYPE.entity.FIELD_NAME.getSetting('allowed_values')[VALUE] }}
{% endfor %}
```

### 1.6.9. Links

- [Rendereable Elements Attributes](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Render%21Element%21RenderElement.php/class/RenderElement/9.1.x)
- [Render API](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Render%21theme.api.php/group/theme_render/9.1.x)

## 1.7. Urls

### 1.7.1. Get current url

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

### 1.7.2. Get current path

```php
$current_path = \Drupal::service('path.current')->getPath();
```

### 1.7.3. Get current path alias

```php
$path_alias = \Drupal::service('path_alias.manager')->getAliasByPath($current_path);
```

### 1.7.4. Check current path

```php
 \Drupal::service('path.matcher')->isFrontPage();
```

### 1.7.5. Get current domain

```php
\Drupal::request()->getSchemeAndHttpHost()
```

### 1.7.6. Get BaseUrl

```php
\Drupal::request()->baseUrl()
```

### 1.7.7. Pass Parameters to another Route

```php

// On form submit, do this
 $url = Url::fromRoute('ROUTE')->setRouteParameters([
    'name' => $name,
]);

// redirect form to this url with these parameters
$form_state->setRedirectUrl($url);
```

### 1.7.8. Get query parameters from current URL

```php

// Recommended
$keyword = \Drupal::request()->query->get('QUERY_PARAMETER');

// Using $_GET[]
$keyword = $_GET["QUERY_PARAMETER"] ?? "";
```

## 1.8. Libraries

### 1.8.1. Attach library to specific page

```php
function THEMENAME_preprocess_page(&$variables){
  $current_path = \Drupal::service('path.current')->getPath();
  if ($current_path === '/mypath') {
    $variables['#attached']['library'][] = 'library/name';
  }
}
```

### 1.8.2. Attach library to specific form

```php
function MODULE_NAME_form_alter(&$form, &$form_state, $form_id)
{

  if ($form_id == 'FORM_ID') {
    $form['#attached']['library'][] = 'MODULE_NAME/LIBRARY_NAME';
  }
}
```

### 1.8.3. Attach library from twig file

```php
{{ attach_library('library/name') }}
```

## 1.9. Stored Procedures

### 1.9.1. Execute a Stored Procedure

```php

// Executing the query --> RETURNS A BOOLEAN
$results = Database::getConnection()->query("EXEC SCHEMA.SP_SP_NAME", $options)->execute();

```

### 1.9.2. Execute a Stored Procedure and get data as a return

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

## 1.10. SQL

### 1.10.1. Grouping query conditions + Using SQL LIKE operator

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

### 1.10.2. Query Pagination

#### 1.10.2.1. Using PagerSelectExtender

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

#### 1.10.2.2. Using Stored Procedure + PHP

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

## 1.11. Forms

### 1.11.1. Create a form with Ajax Callback - namespace + buildForm()

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

### 1.11.2. Create a form with Ajax Callback - submitForm()

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

### 1.11.3. Create a form with Ajax Callback - validateForm()

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

### 1.11.4. Button with other function

```php
 $form['actions']['BUTTONNAME'] = [
  '#type' => 'submit',
  '#default_value' => $this->t('BUTTON LABEL'),
  '#submit' => array('::FUNCTION'), // we execute this function
];
```

### 1.11.5. Form elements

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

### 1.11.6. Add classes to form

```php

// Form Class
$form['#attributes']['class'][] = 'form-inline';
```

### 1.11.7. Add class to form element Label

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

### 1.11.8. Disable advanced search and help links from search form

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

### 1.11.9. Get the element name instead of value

```php
if (!empty($form_state->getValue('name'))) {

$value =  $form_state->getValue('name');

$FIELD_NAME = $form['FIELD_NAME']['#options'][$value]; //This
 } else {
  $FIELD_NAME = NULL;
 }
```

## 1.12. Files & Images

### 1.12.1. Create CSV

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

### 1.12.2. Create the url of an image

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

## 1.13. Messages

1. Create a link as a message

```php
   \Drupal::messenger()->addError('');
   \Drupal::messenger()->addWarning('');
   \Drupal::messenger()->addStatus(['#markup' => '<a href=' . $enlace_convocatoria_nueva . ' target="_blank" rel= noopener noreferrer" >OPEN LINK</a>']);
```

## 1.14. Add Metatags

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

## 1.15. Create Next/Previous Post links

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

## 1.16. Other useful functions

### 1.16.1. Create Permalink without accents

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

### 1.16.2. Check if string Starts With

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


### 1.16.3. Group Array by Date

```php
private function groupByDate( $array ) {
    $grouped_array = [];
    foreach ( $array as $value ) {
      $grouped_array[ $value->FIELD_NAME ][] = $value;
    }

    return $grouped_array;
  }
```

### 1.16.4. Flatten Array

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

## 1.17. Drush

### 1.17.1. Instalation

```shell
composer require drush/drush:10.6.2
```

### 1.17.2. Drush Commands i use most

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
