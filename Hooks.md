## Hooks

Hooks are functions that are called at specific times in the Drupal core. For example, when a node is saved, a hook is called. When a user logs in, a hook is called. When a page is loaded, a hook is called. There are many hooks that are called in Drupal and they are all documented in the [Drupal API](https://api.drupal.org/api/drupal/core%21core.api.php/group/hooks/9.1.x).

Hooks are called from the `module_name.module` file or from the `THEME_NAME.theme` file. The `module_name.module` file is where you put all the code that is related to the module. The `THEME_NAME.theme` file is where you put all the code that is related to the theme.


## Templates

hooks that interact with templates

### Create template suggestions

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