## Libraries

Libraries are a way to group and manage JavaScript, CSS, and other assets. They are used to provide a consistent way to include assets in Drupal. Libraries are defined in a .libraries.yml file, which is placed in the module or theme directory. The .libraries.yml file defines the library name, version, and the files that are included in the library. Libraries can be attached to a page or a specific element in a page.

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

