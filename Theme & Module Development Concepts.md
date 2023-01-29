# Drupal Theme and Module Development Concepts

## Preprocess Functions

- In the .theme file, you should only use preprocess functions to add or modify UI elements or alter variables for a template file. You should not use preprocess functions to add or modify data. If you need to add or modify data, use a module's hook implementation instead.

- You can be very specific with the preprocess functions, for example, if you want to add a variable to a specific template file, you can use the template name as the function name, for example, if you want to add a variable to the page.html.twig template, you can use the following function:

```php
function MYTHEME_preprocess_page(&$variables) {
  $variables['my_variable'] = 'Hello World';
}
```

- If you want to add a variable to a node template with a specific ID, you can use the following function:

```php
function MYTHEME_preprocess_node__42(&$variables) {
    $variables['my_variable'] = 'Hello World';
}
```

- When adding new elements to the $variables array that represent content which should be displayed as HTML it's best practice to define a render array instead of hard-coding the markup.

### Preprocess Call Stack

In Drupal, the preprocess call stack is the sequence of functions that are executed in order to preprocess variables before they are passed to the theme layer for rendering.

The preprocess call stack consists of the following steps:

1. template_preprocess()
2. template_preprocess_node()
3. MODULE_preprocess()
4. MODULE_preprocess_node()
5. ENGINE_preprocess()
6. ENGINE_preprocess_node()
7. theme_preprocess()
8. theme_preprocess_node()

Each preprocess function in the stack can modify the variables that are passed to it, and can add new variables to the stack for use in later preprocess functions or in the theme layer.

It is worth mentioning that the preprocess function calls are made according to the "preprocess function" in the theme hook's theme_hook_suggestions and the order in which they are defined in the .info.yml file.