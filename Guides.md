# Guides

A collection of Drupal Guides.

## Disable advanced search and help links from search form

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