# Helpful Functions and solutions

These are some helpful functions and solutions to issues i've encountered while developing.

## Add Metatags

Add specific metatags to a page, for example, if you want to add a specific image to a page, you can use the following function, this function should be placed in the .module file.

```php
function MODULE_NAME_page_attachments_alter(array &$page)
{

    $meta_title = "";
    $meta_description = "";
    $meta_og_img = \Drupal::request()->getSchemeAndHttpHost() . '/themes/bootstrap/image.png';


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
```

## Create Next/Previous Post links

Create next and previous post links, this function should be placed in the .theme file or .module file.

```php

function THEMENAME_preprocess_pages(&$variables){
$node = \Drupal::routeMatch()->getParameter('node');
if ($node instanceof \Drupal\node\NodeInterface) {
   $content_type = $node->getType(); // Tipo de contenido
   $nid = $node->id(); // Id del nodo
   if ($content_type == 'blog') {
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

        $next_title =  Node::load($next)->getTitle(); // Titulo
        $next_link = \Drupal::service('path_alias.manager')
          ->getAliasByPath('/node/' . $next);  // enlace relativo

        $variables['next_title'] = $next_title;
        $variables['next_ling'] = $next_link;
      }

      // Back
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

        $previous_title =  Node::load($prev)->getTitle(); // Titulo
        $previous_link = \Drupal::service('path_alias.manager')
          ->getAliasByPath('/node/' . $prev);  // enlace relativo

        $variables['previous_title'] = $previous_title;
        $variables['previous_link'] = $previous_link;
      }
   }
}
}

// page--blog.html.twig
{% if previous_title %}
	<div class="c-paginationdetail__content">
		<a href="{{previous_link}}">
			<p>ANTERIOR</p>
			<span>{{previous_title}}</span>
		</a>
	</div>
{% endif %}
{% if next_title %}
	<div class="c-paginationdetail__content c-paginationdetail__content--right">
		<a href="{{next_ling}}">
			<p>POSTERIOR</p>
			<span>{{next_title}}</span>
		</a>
	</div>
{% endif %}
```

## Create Permalink without accents

```php
/**
 * Spanish: Funcion crear un permalink/slug a partir de un titulo
 * English: Function to create a permalink/slug from a title
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
/**
 * Function to check if a string starts with specific characters
 * @param string $string
 * @param string $startString
 * @return bool
 */
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

## Group Array by Date

```php
private function groupByDate( $array ) {
    $grouped_array = [];
    foreach ( $array as $value ) {
      $grouped_array[ $value->FIELD_NAME ][] = $value;
    }

    return $grouped_array;
  }
```

## Flatten Array

```php
/**
* Function to flatten an array
* @param array $array
* @param int $depth
* @return array
*/
private function array_flatten( $array = null, $depth = 1 ) {
    $result = [];
    if ( ! is_array( $array ) ) {
      $array = func_get_args();
    }
    foreach ( $array as $key => $value ) {
      if ( is_array( $value ) && $depth ) {
        $result = array_merge( $result, $this->array_flatten( $value, $depth - 1 ) );
      } else {
        $result = array_merge( $result, [ $key => $value ] );
      }
    }

    return $result;
}
```
