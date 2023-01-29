# Forms

Forms are a very important part of Drupal. They are used to create content, edit content, and even to create users.

## Create a form with Ajax Callback

### buildForm()

```php
/**
 * @file
 * Contains \Drupal\MODULE_NAME\Form\FormClassName.
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
  '#options' => $options,
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
$extensions = 'jpg png';
$max_upload = 16000000; // bytes

$form['FIELD'] = [
  '#type' => 'managed_file',
  '#title' => t('TITLE'),
  '#description' => $this->t('Allowed extensions: @allowed_ext', ['@allowed_ext' => $extensions]),
  '#upload_location' => 'public://PATH',
  '#multiple' => FALSE,
  '#required' => FALSE,
  '#default_value' => [$result[0]->FIELD_NAME],
  '#upload_validators' => [
    'file_validate_extensions' => [
      $extensions,
    ],
    'file_validate_size' => [
      $max_upload,
    ],
  ],
];

// Buttons
$form['actions']['#type'] = 'actions';
$form['actions']['submit'] = [
  '#type' => 'submit',
  '#default_value' => $this->t('Save'),
  '#button_type' => 'primary',
];

$form['actions']['delete'] = [
  '#type' => 'button',
  '#value' => t('Delete'),
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

### submitForm()

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
\Drupal::messenger()->addMessage(t('MESSAGE'));
$form_state->setRedirect('route_name');
}

// Create the OPTIONAL ajax callback function
public function myAjaxCallback(array &$form, FormStateInterface $form_state)
{
  $response = new AjaxResponse();
  return $response;
}
```

### validateForm()

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

## Add a Button with custom submit function

An example of how to add a button to a form and execute a custom function when the button is clicked.

```php
 $form['actions']['BUTTONNAME'] = [
  '#type' => 'submit',
  '#default_value' => $this->t('BUTTON LABEL'),
  '#submit' => array('::FUNCTIONNAME'), // we execute this function
];

//...

function FUNCTIONNAME(){
    // do something
}
```

## Form field Attributes

These are some of the most common attributes you can use to customize your form fields.

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

## Add classes to form

Add a class to the form element

```php

// Form Class
$form['#attributes']['class'][] = 'form-inline';
```

## Add class to form element Label

Add a class to the form element label

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

## Get the element name instead of value

```php
if (!empty($form_state->getValue('name'))) {

$value =  $form_state->getValue('name');

$FIELD_NAME = $form['FIELD_NAME']['#options'][$value]; //This
 } else {
  $FIELD_NAME = NULL;
 }
```
