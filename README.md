# Drupal

- [Drupal](#drupal)
  - [Get Current Url](#get-current-url)
  - [Attach library to specific page](#attach-library-to-specific-page)
  - [Modals](#modals)
    - [Dynamic Way with content](#dynamic-way-with-content)
    - [Manual with API](#manual-with-api)
  - [Add link to form](#add-link-to-form)
  - [Execute a Stored Procedure](#execute-a-stored-procedure)
  - [Execute a Stored Procedure and get data as a return](#execute-a-stored-procedure-and-get-data-as-a-return)
  - [Get query parameters from current URL](#get-query-parameters-from-current-url)
  - [Grouping query conditions + Using SQL LIKE operator](#grouping-query-conditions--using-sql-like-operator)
  - [Query Pagination](#query-pagination)
  - [Forms](#forms)
  - [Pass Parameters to another Route](#pass-parameters-to-another-route)

## Get Current Url

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

## Attach library to specific page

```php
function hello_world_preprocess_page(&$variables){
  $current_path = \Drupal::service('path.current')->getPath();
  if ($current_path === '/buscar-convocatorias') {
    $variables['#attached']['library'][] = 'hello_world/hello_form';
  }
}
```



## Modals

### Dynamic Way with content

1. Create a Block or a page with the info

```html

<!-- Custom Block -->
<p><a class="use-ajax" data-dialog-options="{&quot;width&quot;:800}" data-dialog-type="modal" href="/search/node">Search</a></p>
```

2. Add a btn or a link with class 'use-ajax' to your twig or php code

```php

// Btn -> Can Use absolute Url values
$form['actions']['delete'] = array(
      '#type' => 'button',
      '#value' => t('Borrar'),
      '#attributes' => array(
        'class' => ['btn', 'btn-danger', 'use-ajax'],
        'data-dialog-options' => '{"width":800}',
        'data-dialog-type' => 'modal',
        'href' => "/prueba-modal",
      ),
    );

// Link, use Url::fromRoute()
$form['actions']['delete'] = [
      '#type' => 'link',
      '#title' => t('Borrar'),
      '#url' => Url::fromRoute('hello.modal'),
      // '#url' => Url::fromRoute('entity.node.canonical', ['node' => 1]),
      '#attributes' => [
        'target' => '_blank',
        'class' => ['btn', 'btn-danger', 'use-ajax', 'js-form-submit', 'form-submit'],
        'data-dialog-options' => '{"width":800}',
        'data-dialog-type' => 'modal',
      ],
    ];
```

3. [OPTIONAL] Create a route in .routing.yml

```yaml
hello.modal:
 path: '/prueba-modal'
 defaults:
   _title: 'Popup'
   _form: '\Drupal\hello_world\Form\FormularioEdit'
 requirements:
   _permission: 'access content'
```

4. Add the modal dependencies to the libraries file

### Manual with API

```php

use Drupal\Core\Ajax\AjaxResponse;
use Drupal\Core\Ajax\ReplaceCommand;
use Drupal\Core\Ajax\OpenModalDialogCommand;

// Btn with ajax event and ajax callback
function submitform (){
 $form['actions']['delete'] = [
      '#type' => 'button',
      '#value' => t('Borrar'),
      '#attributes' => [
        'class' => ['btn', 'btn-danger'],
      ],
      '#ajax' => [
        'callback' => '::myAjaxCallback', // don't forget :: when calling a class method.
        //'callback' => [$this, 'myAjaxCallback'], //alternative notation
        'disable-refocus' => FALSE, // Or TRUE to prevent re-focusing on the triggering element.
        'event' => 'click',
        'wrapper' => 'edit-output', // This element is updated with this AJAX callback.
        'progress' => [
          'type' => 'throbber',
        ],
      ]
    ];
}
// Ajax callback function that executes our code
  public function myAjaxCallback(array &$form, FormStateInterface $form_state)
  {

    // Prepare the text for our dialogbox.
    $dialogText['#markup'] = "You selected";
    $response = new AjaxResponse();

    // Show the dialog box.

    $options =     [
     'width' => '700px',
      'modal' => TRUE,
      'appendTo' => '#' . $this->wrapperId,
      'dialogClass' => 'my-dialog-class',

      'buttons' => [
        [
          'text' => 'Aceptar',
          'class' => 'btn btn-success',
          'click' => "function() { $(this).dialog('close'); }",
        ],
        [
          'text' => 'Cancelar',
          'class' => 'btn btn-danger',
          'click' => "function() { $(this).dialog('close'); }",
        ]
      ]
    ];

    $response->addCommand(new OpenModalDialogCommand('Confirme Borrado de Convocatoria', $dialogText, $options));

    // Finally return the AjaxResponse object.
    return $response;
  }

```


```yaml
# Libraries
  dependencies:    
    - core/jquery #Just to be safe
    - core/drupal.dialog.ajax #Required for dialogs
    - core/jquery.form #If you also want to use Ajax for form operations
```

## Add link to form

```php
// Normal field
$form['test_link'] = [
'#type' => 'link',
'#title' => t('Link title'),
'#url' => Url::fromRoute('entity.node_type.collection')
];

// Actions field
$form['actions']['preview'] = [
'#type' => 'link',
'#title' => t('Link title'),
'#url' => Url::fromRoute('entity.node_type.collection')
// '#attributes' => ['target' => '_blank', 'class' => array('button', 'button--primary', 'js-form-submit', 'form-submit')],
];

```

## Execute a Stored Procedure

```php

// Connect to external database.
Database::setActiveConnection('external');

// OPTIONAL: Getting a query Parameter
$convocatoriaID = $_GET['cid'] ?? NULL;

 // Placeholder for the paremeters that we pass on the query
 $options = array(
     ":ConvocatoriaId" =>  $convocatoriaID,
 );

 // Executing the query --> RETURNS A BOOLEAN
 $results = Database::getConnection()->query("EXEC DRU.SP_DeleteConvocatoriaById @ConvocatoriaId = :ConvocatoriaId", $options)->execute();

 Database::setActiveConnection();

 // Redirect or return
 return $this->redirect('convocatorias.convocatorias');

```

## Execute a Stored Procedure and get data as a return

1. The PHP File we're working with

```php

  // OPTIONAL - If there's a query parameter
  $queryParams = (isset($_GET["palabra_clave"]));

  // Placeholder that we put on the query options/arguments
  $options = array(
      ':Titulo' => $palabra_clave,
  );

  // We don't use execute(), we directly run fetchAssoc() to get an associative array or fetchObject() to get a an object
  $sp = Database::getConnection()->query("EXECUTE DRU.SP_BuscarConvocatoriasAdmin @Titulo = :Titulo", $options)->fetchAssoc();
  // $sp = Database::getConnection()->query("EXECUTE DRU.SP_BuscarConvocatoriasAdmin @Titulo = :Titulo", $options)->fetchObject();


  // ....
  return [
    '#sp' => $sp,
  ]

```

2. we get the data from the return and pass it onto the twig file

```

<!-- Since it's an associative array, we need the key and value -->

{% for key,value in sp %}
	Key :
	{{ key }}
	Value :
	{{ value }}
{% endfor %}

```

## Get query parameters from current URL

```php

// Using \Drupal::request()->query->get()
$palabra_clave = \Drupal::request()->query->get('QUERY_PARAMETER');

// Using $_GET[]
$queryParams = $_GET["palabra_clave"] || NULL;

```

## Grouping query conditions + Using SQL LIKE operator

```php

// We use orConditionGroup() to groupe multiple Conditions
// We use escapeLike() to use the LIKE operator

Database::setActiveConnection('external');
$db = Database::getConnection();

$query = $db->select('TABLEPREFIX.TABLE_NAME', 'ALIAS');
$query->fields('ALIAS', ['FIELDS']);
 $group = $query->orConditionGroup()
            ->condition('TITULO', "%" . $db->escapeLike($palabra_clave) . "%", 'LIKE')
            ->condition('ACTIVIDAD', "%" . $db->escapeLike($palabra_clave) . "%", 'LIKE');
$query->condition($group);
$results = $pager->execute()->fetchAll();
Database::setActiveConnection();
```


## Query Pagination

```php

// We use PagerSelectExtender to limit the what we get on our page
// $pager should be passed later to the twig file as a variable

use Drupal\Core\Database\Query\PagerSelectExtender;

Database::setActiveConnection('external');
$db = Database::getConnection();

$query = $db->select('TABLEPREFIX.TABLE_NAME', 'ALIAS');
$query->fields('ALIAS', ['FIELDS']);
 $group = $query->orConditionGroup()
            ->condition('TITULO', "%" . $db->escapeLike($palabra_clave) . "%", 'LIKE')
            ->condition('ACTIVIDAD', "%" . $db->escapeLike($palabra_clave) . "%", 'LIKE');
$query->condition($group);
$pager = $query->extend(PagerSelectExtender::class)->limit(5);
$results = $pager->execute()->fetchAll();
Database::setActiveConnection();

```

## Forms


```php

// Form attributes and input attributes

// Form Classs
$form['#attributes']['class'][] = 'form-inline';

// Form Element Attributes
 $form['PALABRA_CLAVE'] = array(
            '#type' => 'textfield',
            '#title' => t('Palabra Clave:'),
            '#required' => FALSE,
            '#size' => 20,
            '#default_value' => $default_palabra_clave || NULL,
            '#label_class' => ['something'] 
        );

// Get the element NAME instead of value 
if (!empty($form_state->getValue('ESTADO'))) {
   $estado =  $form_state->getValue('ESTADO');
   $estado_name = $form['ESTADO']['#options'][$estado]; //This
     } else {
      $estado = NULL;
     }

```


## Pass Parameters to another Route

```php


// On form submit, do this

 $url = Url::fromRoute('ROUTE')->setRouteParameters([
            'palabra_clave' => $palabra_clave,
            'estado' => $estado_name,
            'tipo_convocatoria' => $tipo_convocatoria_name,
            'borrado' => $borrado,
        ]);

  $form_state->setRedirectUrl($url);

```