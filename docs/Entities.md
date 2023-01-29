## Entities

Entities are the main building blocks of Drupal. They are the objects that are stored in the database. For example, a node is an entity, a user is an entity, a taxonomy term is an entity, etc.

### Nodes

Nodes are the main content type in Drupal. It's the most common entity that we use in Drupal.

Here are some of the methods that I use to work with nodes.

#### Get Current Node information

```php
$node = \Drupal::routeMatch()->getParameter('node'); // get node

$node_type = $node->getType(); // Node type
$node_bundle = $node->bundle(); // Node type

$nid = $node->id(); // Node id

$title = $node->getTitle(); // Node title

```

#### Get current Node information on preview page

```php
  $routeMatch   = \Drupal::routeMatch();
  $node_preview = $routeMatch->getParameter( 'node_preview' );
```

#### Get info of a specific Node - NODE::load()

```php
$NODE_TITLE =  Node::load(ID)->getTitle(); // Node title
```

#### Query Nodes

There are two methods that I use to query nodes, the first one is the `EntityQuery` and the second one is the `EntityTypeManager`. These two are almost exactly the same, if you were to look at the code, you would see that they are using the same methods. The only difference is that the `EntityTypeManager` is a bit more flexible and it's easier to use.

##### Get info of a specific Nodes - EntityQuery

```php
$node_ids = Drupal::entityQuery('node')
      ->condition('type', CONTENT_TYPE)
      ->condition('FIELD_NAME', FIELD_ID)
      ->execute(); // Get all the node ids that match these conditions
$nodes = Node::loadMultiple($node_ids); // All the nodes in an assoc array
```

##### Get info of a specific Nodes - EntityTypeManager

```php
$node = \Drupal::entityTypeManager()->getStorage('node')
        ->getQuery()
        ->condition('nid', $nid, '=')
        ->condition('type', 'blog')
        ->condition('status', 1)
        ->sort('nid', 'ASC')
        ->range(0, 1)
        ->execute();

$node = array_values($node);
$node = $node[0];

$titulo =  Node::load($node)->getTitle(); // Titulo

$link = \Drupal::service('path_alias.manager')
          ->getAliasByPath('/node/' . $node);  // Relative url

```

#### Reference

For more information about the Node class, check the [Node Methods](https://api.drupal.org/api/drupal/core%21modules%21node%21src%21Entity%21Node.php/class/Node/9.1.x)

### Taxonomy Terms

Taxonomy terms are the main way to categorize content in Drupal. They are used to group content in a specific category. For example, you can create a taxonomy term called "News" and then you can create a taxonomy term called "Sports" and then you can create a taxonomy term called "Politics". These are the main categories that you can use to group content. As opposed to nodes, taxonomy terms are not content, they are just categories.

#### Get Taxonomy Term Names

##### Method 1 - entityTypeManager - Gets Taxonomy Terms

```php
$tree = \Drupal::entityTypeManager()->getStorage('taxonomy_term')->loadTree(
  'grafica_ambito', // The taxonomy term vocabulary machine name.
  0,                 // The "tid" of parent using "0" to get all.
  1,                 // Get only 1st level.
  TRUE               // Get full load of taxonomy term entity.
);
$results = [];

foreach ($tree as $term) {
  $results[] = $term->getName();
}
```

##### Method 2 - EntityQuery + load - Get Taxonomy Terms sorted by custom field

```php
$results = \Drupal::entityQuery('taxonomy_term')
    ->condition('vid', "TAXONOMY_TERM")
    ->sort('FIELD')
    ->execute();

  $term_names_array = [];

  $term = \Drupal\taxonomy\Entity\Term::load($result);
  $term_names_array[] = $term->getName();
```

##### Method 3 - Same as method 3 but using entityTypeManager

```php

$taxonomy_terms = \Drupal::entityQuery('taxonomy_term')
    ->condition('vid', "TAXONOMY_TERM")
    ->sort('FIELD');

$results = $taxonomy_terms->execute();

$term_names_array = [];

foreach ($results as $result) {
  $term = \Drupal::entityTypeManager()
    ->getStorage('taxonomy_term')
    ->load($result);
  $term_names_array[] = $term->getName();
}
```
