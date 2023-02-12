# Drupal 9 Call stack

Drupal 9 uses a hierarchical call stack, which consists of several layers:

The HTTP layer, which handles incoming HTTP requests.
The Routing layer, which maps the request to a specific controller.
The Controller layer, which processes the request and returns a response.
The Theme layer, which renders the response as HTML.
Each layer communicates with the layer below it, passing information down the stack and receiving data back up. The order of the stack is not interchangeable.

The call stack is the sequence of function calls that are executed in order to handle a request in Drupal.

It starts with the index.php file, which bootstraps Drupal and sets up the environment. Next, the request is routed to the appropriate controller, which processes the request and returns a response. Finally, the response is passed to the theme layer, which renders it as HTML and sends it back to the browser.

n Drupal, the preprocess call stack is the sequence of functions that are executed in order to preprocess variables before they are passed to the theme layer for rendering.

The preprocess call stack consists of the following steps:

The theme_preprocess() function, which is the main entry point for preprocessing in Drupal.
The specific preprocess function for the theme hook being used, such as template_preprocess_page() or node_preprocess().
The specific preprocess function for the specific template file being used, such as my_module_preprocess_page() or my_module_preprocess_node().
Each preprocess function in the stack can modify the variables that are passed to it, and can add new variables to the stack for use in later preprocess functions or in the theme layer.

It is worth mentioning that the preprocess function calls are made according to the "preprocess function" in the theme hook's theme_hook_suggestions and the order in which they are defined in the .info.yml file.