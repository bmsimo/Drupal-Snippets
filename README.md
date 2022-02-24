# Drupal

- [Drupal](#drupal)
  - [Get Current Url](#get-current-url)
  - [Libraries](#libraries)
    - [Attach library to specific page](#attach-library-to-specific-page)
    - [Attach library to specific form](#attach-library-to-specific-form)
    - [Attach library from twig file](#attach-library-from-twig-file)
  - [Modals](#modals)
    - [Dynamic Way with content](#dynamic-way-with-content)
  - [Add link to form](#add-link-to-form)
  - [Execute a Stored Procedure](#execute-a-stored-procedure)
  - [Execute a Stored Procedure and get data as a return](#execute-a-stored-procedure-and-get-data-as-a-return)
  - [Get query parameters from current URL](#get-query-parameters-from-current-url)
  - [Grouping query conditions + Using SQL LIKE operator](#grouping-query-conditions--using-sql-like-operator)
  - [Query Pagination](#query-pagination)
    - [Using PagerSelectExtender](#using-pagerselectextender)
    - [Using Stored Procedure + PHP](#using-stored-procedure--php)
  - [Forms](#forms)
    - [Default form with Ajax Callback](#default-form-with-ajax-callback)
    - [Button with other function](#button-with-other-function)
    - [Other Form CheatSheets](#other-form-cheatsheets)
  - [Create a file](#create-a-file)
  - [Pass Parameters to another Route](#pass-parameters-to-another-route)
  - [Messages](#messages)
  - [Create Permalink](#create-permalink)
  - [Check if string Starts With](#check-if-string-starts-with)
  - [Add Metatags](#add-metatags)

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

## Libraries

### Attach library to specific page

```php
function hello_world_preprocess_page(&$variables){
  $current_path = \Drupal::service('path.current')->getPath();
  if ($current_path === '/buscar-convocatorias') {
    $variables['#attached']['library'][] = 'hello_world/hello_form';
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
{{ attach_library('datos_convocatoria/convocatoria-detalle') }}
```

## Modals

### Dynamic Way with content

1. Create a Delete Btn, give it the 'use-ajax' class and a callback function

```php
 $form['actions']['delete'] = [
   '#type' => 'button',
   '#value' => t('Borrar'),
   '#attributes' => [
     'class' => ['btn', 'btn-danger', 'use-ajax'],
   ],
   '#ajax' => [
     'callback' => '::myAjaxCallback', // don't forget :: when calling a class method.
     //'callback' => [$this, 'myAjaxCallback'], //alternative notation
     'disable-refocus' => FALSE, // Or TRUE to prevent 
     'wrapper' => 'drupal-modal', // This element is updated with this AJAX callback.
     'effect' => 'fade',
     'progress' => [
       'type' => 'throbber',
     ],
   ]
 ];
```

2. Create an ajax callback function to handle the delete btn

```php
use Drupal\Core\Ajax\AjaxResponse;
use Drupal\Core\Ajax\ReplaceCommand;
use Drupal\Core\Ajax\OpenModalDialogCommand;

  public function myAjaxCallback(array &$form, FormStateInterface $form_state)
  {

    // Prepare the text for our dialogbox.
    // $dialogText['#markup'] = "You selected";
    $response = new AjaxResponse();

    // Show the dialog box.

    $dialogText = '';

    $options = [
      'width' => '700px',
      'modal' => TRUE,
      'dialogClass' => 'modal-dialog',
      'create' => 'function() {console.log("created")}',
      'buttons' => [
        [
          'text' => 'Aceptar',
          'class' => 'btn btn-success btn-aceptar',
        ],
        [
          'text' => 'Cancelar',
          'class' => 'btn btn-danger btn-cancelar',
        ]
      ]
    ];
    $response->addCommand(new OpenModalDialogCommand('Confirme Borrado de Convocatoria', $dialogText, $options));

    return $response;
  }
```

3. Create the JS file to handle the modal creation and event listener

```JS
jQuery(document).ready(function ($) {
  // Obtenemos las query parameters
  const urlParams = new URLSearchParams(window.location.search);
  const cid = urlParams.get("cid");

  (function ($, Drupal) {
    Drupal.behaviors.custom_behavior = {
      attach: function (context) {
        $(window)
          .once("custom-behavior")
          .on(
            "dialog:aftercreate",
            function (event, dialog, $element, settings) {
              // Definimos nuestras variables y pasamos el modal via arguments
              const modalAceptar = document.querySelector(
                ".modal-dialog .btn-success"
              );
              const modalCancelar = document.querySelector(
                ".modal-dialog .btn-danger"
              );
              //   Events Listeners al hacer Click
              modalAceptar.addEventListener("click", (e) => {
                e.preventDefault();
                e.stopImmediatePropagation();
                // Cerramos el modal
                dialog.close();
                // Redireccionamos a la pagina borrar convocatoria con el query parameter
                window.location.replace(
                  `/admin/content/borrar-convocatoria?cid=${cid}`
                );
              });
              modalCancelar.addEventListener("click", (e) => {
                e.preventDefault();
                e.stopImmediatePropagation();
                dialog.close();
              });
            }
          );
      },
    };
  })(jQuery, Drupal);
});
```

4. Add the modal dependencies to the libraries file

```yaml
# Libraries
convocatoria-library:
  js:
    js/modal.js: {}
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

```php
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
$palabra_clave_value = $_GET["palabra_clave"] ?? "";
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

### Using PagerSelectExtender

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

### Using Stored Procedure + PHP

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
      echo t('No se han encontrado resultados para la búsqueda seleccionada.');
  } else if ($count_total >= 10) {
      if ($current_page == $total_pages) {
          echo "Mostrando $current_offset - $count_total de $count_total";
      } else {
          echo "Mostrando $current_offset - $my_offset de $count_total";
      }
  } else {
      echo "Mostrando $current_offset - $count_total de $count_total";
  }

   // Primera Pagina
   if ($current_page > 1) :

       if (!$page) : ?>
   <li class="pager__item pager__item--first">
       <?php echo "<a href=\"/como-acceder-a-los-fondos/convocatorias?page=1\" title=\"Ir a la primera página\" rel=\"first\">
               <span class=\"visually-hidden\">Primera página</span>
               <span aria-hidden=\"true\">« Primero</span>
               </a>"; ?>
   </li>
   <?php else : ?>
   <li class="pager__item pager__item--first">
       <?php echo "<a href=" . $rest . "1" . " title=\"Ir a la primera página\" rel=\"first\">
               <span class=\"visually-hidden\">Primera página</span>
               <span aria-hidden=\"true\">« Primero</span>
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
               <a href=\"/como-acceder-a-los-fondos/convocatorias?page=$i\" title=\"Página actual\">
               <span class=\"visually-hidden\">Página actual</span>
               $i
               </a>
               </li>";
           } else if (!$page) {
               echo "<li class=\"pager__item\">
               <a href=\"/como-acceder-a-los-fondos/convocatorias?page=$i\" title=\"Ir a pagina $i\">
               <span class=\"visually-hidden\">Pagina</span>$i</a>
               </li>";
           } else {
               echo "<li class=\"pager__item\">
               <a href=" . $rest . $i . " title=\"Ir a pagina $i\">
               <span class=\"visually-hidden\">Pagina</span>$i</a>
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
       <?php echo "<a href=\"/como-acceder-a-los-fondos/convocatorias?page=" . $total_pages . "\" title=\"Ir a la última página\" rel=\"last\">
               <span class=\"visually-hidden\">Última página</span>
           <span aria-hidden=\"true\">Último »</span>
           </a>"; ?>
   </li>
   <?php else :
       ?>
   <li class="pager__item pager__item--last">
       <?php echo "<a href=" . $rest . $total_pages . " title=\"Ir a la última página\" rel=\"last\">
               <span class=\"visually-hidden\">Última página</span>
           <span aria-hidden=\"true\">Último »</span>
       </a>"; ?>
   </li>
   <?php endif; // !page
   endif; // current page < total pages
   ?>
```

## Forms

### Default form with Ajax Callback

```php
/**
 * @file
 * Contains \Drupal\datos_convocatoria\Form\FormularioEdit.
 * Formulario de edicion de datos
 */

namespace Drupal\datos_convocatoria\Form;

use Drupal\Core\Form\FormBase;
use Drupal\Core\Form\FormStateInterface;
use Drupal\Core\Url;
use Drupal\Core\Database\Database;

use Drupal\Core\Ajax\AjaxResponse;
use Drupal\Core\Ajax\OpenModalDialogCommand;

use Drupal\file\Entity\File;


// Create a class that extends from FormBase
class FormularioEdit extends FormBase
{

  protected $files;
  /**
   * {@inheritdoc}
   */
  public function getFormId()
  {
    // We give the form an ID
    return 'convocatoria_edit';
  }

// Create the required buildForm() function
  public function buildForm(array $form, FormStateInterface $form_state)
  {
    Database::setActiveConnection('external');


// Text
$form['TITULO'] = array(
  '#type' => 'textarea',
  '#title' => t('Titulo:'),
  '#maxlength' => 1000,
  '#required' => TRUE,
  '#default_value' => $result[0]->TITULO
);
// Date
$form['FECHA_APERTURA_PLAZO'] = array(
  '#type' => 'date',
  '#title' => t('Fecha apertura plazo:'),
  '#required' => FALSE,
  '#default_value' => $result[0]->FECHA_APERTURA_PLAZO
);

// Select
$form['ESTADO_ID'] = array(
  '#type' => 'select',
  '#title' => t('Estado'),
  '#options' => $estados_convocatoria,
  '#default_value' => $result[0]->ESTADO_ID,
);
// Number
$form['IMPORTE'] = array(
  '#type' => 'number',
  '#step' => 'any',
  '#title' => t('Importe:'),
  '#required' => FALSE,
  '#default_value' => $result[0]->IMPORTE
);


// File Upload
$ext_permitidas = 'jpg png';
$max_upload = 16000000;

$form['ENLACE_RRSS'] = array(
  '#type' => 'managed_file',
  '#title' => t('Imagen RRSS'),
  '#description' => $this->t('Extensiones Validas: @allowed_ext', ['@allowed_ext' => $ext_permitidas]),
  '#upload_location' => 'public://rrss/',
  '#multiple' => FALSE,
  '#required' => FALSE,
  '#default_value' => [$result[0]->ID_RRSS],
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

  // Create the required submitForm() function
public function submitForm(array &$form, FormStateInterface $form_state)
{
// Get the values
$titulo = $form_state->getValue('TITULO');
$body =  $form_state->getValue('BODY');

// Get values with condition
if (!empty($form_state->getValue('ACTIVIDAD'))) {
  $actividad = $form_state->getValue('ACTIVIDAD');
} else {
  $actividad =  NULL;
}
// Float Values
if (!empty($form_state->getValue('IMPORTE'))) {
  $importe = floatval($form_state->getValue('IMPORTE'));
} else {
  $importe = NULL;
}
// Integer Values
if (!empty($form_state->getValue('SI_PYMES_PUBLICAR_CATEGORIA2'))) {
  $si_pymes_2 = intval($form_state->getValue('SI_PYMES_PUBLICAR_CATEGORIA2'));
} else {
  $si_pymes_2 = NULL;
}

// Files
if (!empty($form_state->getValue('ENLACE_RRSS'))) {
  $id_rrss = reset($form_state->getValue('ENLACE_RRSS'));
  $file = File::load($id_rrss);
  $file->setPermanent();
  $file->save();
  $file_name = $file->getFilename();
  $file_url = 'https://planderecuperacion.gob.es' . $file->createFileUrl();
} else {
  $id_rrss = null;
  $file_url = 'https://planderecuperacion.gob.es/themes/bootstrap/images/fondos_social.png';
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


// Create the OPTIONAL validateForm() function
public function validateForm(array &$form, FormStateInterface $form_state)
{

  $hora_cierre_plazo = $form_state->getValue('HORA_CIERRE_PLAZO');
  if (!empty($hora_cierre_plazo)) {
    $regexHora = preg_match("/^(?:2[0-3]|[01][0-9]):[0-5][0-9]$/", $hora_cierre_plazo);

    if ($regexHora == 0) {
      $form_state->setErrorByName('HORA_CIERRE_PLAZO', $this->t('Porfavor indique una hora valida. El formato debe ser hh:mm'));
    }
  }
}
}
```

### Button with other function

```php
 $form['actions']['nueva_convocatoria'] = [
  '#type' => 'submit',
  '#default_value' => $this->t('Nueva convocatoria'),
  '#submit' => array('::nuevaConvocatoria'),
];
```

### Other Form CheatSheets

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

## Create a file

```php
public function descargarConvocatorias(array &$form, FormStateInterface $form_state)
{


    Database::setActiveConnection('external');
    $bdd = Database::getConnection();

    $query = $bdd->select('DRU.CONVOCATORIA', 'c');
    $query->fields('c', [
        'TITULO',
        'BODY',
        'ACTIVIDAD',
        'FECHA_APERTURA_PLAZO',
        'FECHA_CIERRE_PLAZO',
        'HORA_CIERRE_PLAZO',
        'FECHA_PUBLICACION',
        'ENLACE_CONVOCATORIA',
        'TEXTO_ENLACE_CONVOCATORIA',
        'ENLACE_MAS_INFO',
        'TEXTO_ENLACE_MAS_INFO',
        'ORIENTADA_PYMES',
        'ESTADO_ID',
        'IMPORTE',
        'NOTA_IMPORTE',
        'INFO_CONTACTO',
        'LUGAR_EJECUCION',
        'PROCEDIMIENTO',
        'REFERENCIA',
        'SI_PYMES_PUBLICAR_CATEGORIA1',
        'SI_PYMES_PUBLICAR_CATEGORIA2',
        'TIPO_CONVOCATORIA_ID',
        'A_QUIEN_DIRIGIDA1',
        'A_QUIEN_DIRIGIDA2',
        'A_QUIEN_DIRIGIDA3',
        'ORGANO_CONVOCANTE',
        'ENLACE_RRSS',
        'ID_RRSS',
    ]);
    $result = $query->execute()->fetchAll();
    Database::setActiveConnection();



    $delimiter = "|";
    $filename = "convocatorias_" . date('Y-m-d') . ".csv";

    $uri = 'public://convocatori/' . $filename;

    // Create a file pointer
    $f = fopen($uri, 'w');

    // Set column headers
    $fields = [
        'TITULO',
        'BODY',
        'ACTIVIDAD',
        'FECHA_APERTURA_PLAZO',
        'FECHA_CIERRE_PLAZO',
        'HORA_CIERRE_PLAZO',
        'FECHA_PUBLICACION',
        'ENLACE_CONVOCATORIA',
        'TEXTO_ENLACE_CONVOCATORIA',
        'ENLACE_MAS_INFO',
        'TEXTO_ENLACE_MAS_INFO',
        'ORIENTADA_PYMES',
        'ESTADO_ID',
        'IMPORTE',
        'NOTA_IMPORTE',
        'INFO_CONTACTO',
        'LUGAR_EJECUCION',
        'PROCEDIMIENTO',
        'REFERENCIA',
        'SI_PYMES_PUBLICAR_CATEGORIA1',
        'SI_PYMES_PUBLICAR_CATEGORIA2',
        'TIPO_CONVOCATORIA_ID',
        'A_QUIEN_DIRIGIDA1',
        'A_QUIEN_DIRIGIDA2',
        'A_QUIEN_DIRIGIDA3',
        'ORGANO_CONVOCANTE',
        'ENLACE_RRSS',
        'ID_RRSS',
    ];
    fputcsv($f, $fields, $delimiter);


    foreach ($result as $row) {
        $lineData = [
            $row->TITULO,
            $row->BODY,
            $row->ACTIVIDAD,
            $row->FECHA_APERTURA_PLAZO,
            $row->FECHA_CIERRE_PLAZO,
            $row->HORA_CIERRE_PLAZO,
            $row->FECHA_PUBLICACION,
            $row->ENLACE_CONVOCATORIA,
            $row->TEXTO_ENLACE_CONVOCATORIA,
            $row->ENLACE_MAS_INFO,
            $row->TEXTO_ENLACE_MAS_INFO,
            $row->ORIENTADA_PYMES,
            $row->ESTADO_ID,
            $row->IMPORTE,
            $row->NOTA_IMPORTE,
            $row->INFO_CONTACTO,
            $row->LUGAR_EJECUCION,
            $row->PROCEDIMIENTO,
            $row->REFERENCIA,
            $row->SI_PYMES_PUBLICAR_CATEGORIA1,
            $row->SI_PYMES_PUBLICAR_CATEGORIA2,
            $row->TIPO_CONVOCATORIA_ID,
            $row->A_QUIEN_DIRIGIDA1,
            $row->A_QUIEN_DIRIGIDA2,
            $row->A_QUIEN_DIRIGIDA3,
            $row->ORGANO_CONVOCANTE,
            $row->ENLACE_RRSS,
            $row->ID_RRSS
        ];
        fputcsv($f, $lineData, $delimiter);
    }

    // Move back to beginning of file
    fseek($f, 0);
    //output all remaining data on a file pointer
    fpassthru($f);
    fclose($f);

    $file = file_save_data($f, 'public://' . $filename);

    $uri_final = '/sites/default/files/convocatori/' . $filename;

    \Drupal::messenger()->addStatus(['#markup' => '<a href=' . $uri_final . ' target="_blank" rel= noopener noreferrer" >Descargar Convocatorias</a>']);

    $url1 = Url::fromRoute('convocatorias.convocatorias');

    $form_state->setRedirectUrl($url1);
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

## Messages

1. Create a link as a message
   
   ```php
   \Drupal::messenger()->addStatus(['#markup' => '<a href=' . $enlace_convocatoria_nueva . ' target="_blank" rel= noopener noreferrer" >Abrir Convocatoria</a>']);
   ```

## Create Permalink

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

## Check if string Starts With

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

## Add Metatags

1. Adding specific metatags from the .module file

```php
function datos_convocatoria_page_attachments_alter(array &$page)
{
  $cid = \Drupal::routeMatch()->getParameter('cid');
  $ruta = \Drupal::service('current_route_match')->getRouteName();

  if ($ruta == 'convocatorias.detalle') {
    Database::setActiveConnection('external');
    $options = [
      ':ConvocatoriaId' => (int)$cid,
    ];
    $query = Database::getConnection()->query("EXECUTE DRU.SP_BuscarConvocatoriaById @ConvocatoriaId = :ConvocatoriaId", $options);
    $result = $query->fetchAll();
    // Connect back to default database.
    $meta_title = $result[0]->TITULO;
    $meta_title .= " | Plan de Recuperación, Transformación y Resiliencia Gobierno de España.";
    $meta_description = $result[0]->BODY;

    if ($result[0]->ENLACE_RRSS !== NULL) {
      $meta_og_img = $result[0]->ENLACE_RRSS;
    } else {
      $meta_og_img = \Drupal::request()->getSchemeAndHttpHost() . '/themes/bootstrap/images/fondos_social.png';
    }


    Database::setActiveConnection();

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
}
```

## Crear Links Next/Previous post/pagina

`template_preprocess_page()`

```php
/*
  Pagina Detalle de blog
  */
  $node = \Drupal::routeMatch()->getParameter('node');
  $tipo_contenido = $node->bundle(); // Tipo de contenido
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
```

`page--blog.html.twig`

```twig
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