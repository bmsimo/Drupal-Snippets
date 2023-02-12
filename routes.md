# Routes

## Passing variables in GET requests

In Drupal 9, you can add variables to the routing file in two ways:

1. Route parameters: You can add variables to the URL path by using curly braces {} around the variable name. In the routing file, you'll need to specify the type of the parameter using the _type option.

```yaml
my_route:
  path: '/my-route/{variable1}/{variable2}'
  defaults:
    _controller: '\Drupal\mymodule\Controller\MyController::myMethod'
  options:
    parameters:
      variable1:
        type: 'string'
      variable2:
        type: 'int'
```

2. Route defaults: You can add default values for variables in the routing file by using the defaults section. If a value is not provided in the URL, the default value will be used.

```yaml
my_route:
  path: '/my-route'
  defaults:
    _controller: '\Drupal\mymodule\Controller\MyController::myMethod'
    variable1: 'default_value1'
    variable2: 'default_value2'
```

3. In the controller, you can access these variables using the $route object, which is passed to the controller method as an argument.

```php
public function myMethod(RouteMatchInterface $route_match) {
  $variable1 = $route_match->getParameter('variable1');
  $variable2 = $route_match->getParameter('variable2');
}
```

## Passing variables in POST requests

1. You can pass parameters to a controller without showing them in the URL. One way to do this is by using a hidden form field.

```php
<form action="{{ path('route_name') }}" method="post">
  <input type="hidden" name="variable1" value="{{ variable1_value }}">
  <input type="submit" value="Submit">
</form>
```

2. In the controller, you can access the hidden field value using the $request object:

```php
public function myMethod(Request $request) {
  $variable1 = $request->request->get('variable1');
}
```

### Note

- Creating a form in this way is not recommended. Instead, you should use the Form API to create forms.

```php
public function buildForm(array $form, FormStateInterface $form_state) {
  $node = \Drupal::routeMatch()->getParameter('node');
  
  $form_state->set('my_field', $node->my_field->value);

  $form['my_field'] = [
    '#type'  => 'hidden',
    '#value' => $form_state->get('my_field'),
  ];
  $form['submit'] = [
    '#type'                    => 'submit',
    '#value'                   => 'Submit',
    '#limit_validation_errors' => [],
  ];
  return $form;
}
```