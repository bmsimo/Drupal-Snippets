# Drupal Core Classes & Methods

These are some of the Drupal core classes and their methods that help me in my day to day work

## Url Drupal::URL

### Get current Url

```php
$url = Url::fromRoute('<current>'); //Route Object, if no options were added we get a relative URL
$url = Url::fromRoute('<current>',['absolute'=> 'true']); //Route Object, 'Absolute' parameter was passed, we get a an absolute url
$url = $url->toString(); // String url
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

## Messenger

> Drupal::messenger() helps us show messages on the page 

### Create a link as a message

```php
   \Drupal::messenger()->addError('');
   \Drupal::messenger()->addWarning('');
   \Drupal::messenger()->addStatus(['#markup' => '<a href=' . $enlace_convocatoria_nueva . ' target="_blank" rel= noopener noreferrer" >OPEN LINK</a>']);
```

## Services

The Drupal::service() method is used to get a service from the container. Services are objects that are used to perform a specific task. For example, the path.alias_manager service is used to get the alias of a path. The container is a registry of all the services that are available in Drupal. You can get a list of all the services by going to http://localhost/my_site_name_dir/web/core/modules/system/src/ServiceProviders/ServiceProvider.php.

### path.current - get current path

```php
$current_path = \Drupal::service('path.current')->getPath();
```

### path_alias.manager - get current path alias

```php
$path_alias = \Drupal::service('path_alias.manager')->getAliasByPath($current_path);
```

### path.matcher

The path.matcher service is used to check if the current path is the frontpage.

```php
Drupal::service('path.matcher')->isFrontPage();
```