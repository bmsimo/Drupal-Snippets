# SQL

## Grouping query conditions + Using SQL LIKE operator

```php

// We use orConditionGroup() to groupe multiple Conditions
// We use escapeLike() to use the LIKE operator

Database::setActiveConnection('external');
$db = Database::getConnection();

$query = $db->select('TABLEPREFIX.TABLE_NAME', 'ALIAS');
$query->fields('ALIAS', ['FIELDS']);
 $group = $query->orConditionGroup()
            ->condition('FIELD_NAME', "%" . $db->escapeLike($palabra_clave) . "%", 'LIKE')
            ->condition('FIELD_NAME', "%" . $db->escapeLike($palabra_clave) . "%", 'LIKE');
$query->condition($group);
$results = $pager->execute()->fetchAll();
Database::setActiveConnection();
```

## Query Pagination

### Using PagerSelectExtender

```php

// We use PagerSelectExtender to limit the what we get on our page
// $pager should be passed later to the twig file as a variable

use Drupal\Core\Database\Query\PagerSelectExtender;

Database::setActiveConnection('external');
$db = Database::getConnection();

$query = $db->select('TABLEPREFIX.TABLE_NAME', 'ALIAS');
$query->fields('ALIAS', ['FIELDS']);
 $group = $query->orConditionGroup()
            ->condition('FIELD_NAME', "%" . $db->escapeLike($palabra_clave) . "%", 'LIKE')
            ->condition('FIELD_NAME', "%" . $db->escapeLike($palabra_clave) . "%", 'LIKE');
$query->condition($group);
$pager = $query->extend(PagerSelectExtender::class)->limit(5);
$results = $pager->execute()->fetchAll();
Database::setActiveConnection();
```

### Using Stored Procedure + PHP

```php
// Execute the Sp to get the count result
$count = Database::getConnection()->query("EXECUTE SP_Count", $options);

// Count_result
$count_result = $count->fetchAll();
// Count_total
$count_total = current($count_result)->contador;
// Items Per page
$per_page = 10;
// All Results
$total_count = (int) $count_total;
// Total_Pages.
$total_pages = ceil($total_count / $per_page);
// Current_page
$current_page = (int) ($_GET['page'] ?? 1);
// Pagina != 0.
if ($current_page < 1 || $current_page > $total_pages) {
    $current_page = 1;
}
// offset.
$offset = $per_page * ($current_page - 1);


// Remove number from page url
$pattern = '/page=(\d+)/i';
$replacement = 'page=';
$rest = preg_replace($pattern, $replacement, $current_url);


// Showing XX results of XX
 $my_offset = $offset + $per_page;
  $current_offset = $offset + 1;

  if ($count_total == 0) {
      echo t('No results found for the selected search.');
  } else if ($count_total >= 10) {
      if ($current_page == $total_pages) {
          echo "Showing $current_offset - $count_total of $count_total";
      } else {
          echo "Showing $current_offset - $my_offset of $count_total";
      }
  } else {
      echo "Showing $current_offset - $count_total of $count_total";
  }

   // Primera Pagina
   if ($current_page > 1) :

    if (!$page) : ?>
      <li class="pager__item pager__item--first">
       <?php echo "<a href=\"/home/?page=1\" title=\"Go to first page\" rel=\"first\">
               <span class=\"visually-hidden\">First Page</span>
               <span aria-hidden=\"true\">« First</span>
               </a>"; ?>
   </li>
   <?php else : ?>
   <li class="pager__item pager__item--first">
       <?php echo "<a href=" . $rest . "1" . " title=\"Go to first page\" rel=\"first\">
               <span class=\"visually-hidden\">First Page</span>
               <span aria-hidden=\"true\">« First</span>
               </a>"; ?>
   </li>
   <?php endif;
   endif;
   ?>

   <?php
   if ($total_pages != 1) {
       $win = 2; // window size
       $gap = false;
       for ($i = 1; $i <= $total_pages; $i++) {
           if ($i > 1 + $win && $i < $total_pages - $win && abs($i - $current_page) > $win) {
               if (!$gap) {
                   $gap = true;
               }
               continue;
           }
           $gap = false;
           if ($current_page == $i) {
               echo "<li class=\"pager__item is-active active\">
               <a href=\"/home/?page=$i\" title=\"Current Page\">
               <span class=\"visually-hidden\">Current Page</span>
               $i
               </a>
               </li>";
           } else if (!$page) {
               echo "<li class=\"pager__item\">
               <a href=\"/home/?page=$i\" title=\"Got to page $i\">
               <span class=\"visually-hidden\">Page</span>$i</a>
               </li>";
           } else {
               echo "<li class=\"pager__item\">
               <a href=" . $rest . $i . " title=\"Go to page $i\">
               <span class=\"visually-hidden\">Page</span>$i</a>
               </li>";
           }
       }
   }
   ?>

   <?php
   // Ultima Pagina
   if ($current_page < $total_pages) : // current page < total pages
       if (!$page) :
   ?>
   <li class="pager__item pager__item--last">
       <?php echo "<a href=\"/home/?page=" . $total_pages . "\" title=\"Go To Last page\" rel=\"last\">
               <span class=\"visually-hidden\">Last Page</span>
           <span aria-hidden=\"true\">Last »</span>
           </a>"; ?>
   </li>
   <?php else :
       ?>
   <li class="pager__item pager__item--last">
       <?php echo "<a href=" . $rest . $total_pages . " title=\"Go To Last page\" rel=\"last\">
               <span class=\"visually-hidden\">Last Page</span>
           <span aria-hidden=\"true\">Last »</span>
       </a>"; ?>
   </li>
   <?php endif; // !page
   endif; // current page < total pages
   ?>
```

## Stored Procedures

### Execute a Stored Procedure

```php

// Executing the query --> RETURNS A BOOLEAN
$results = Database::getConnection()->query("EXEC SCHEMA.SP_SP_NAME", $options)->execute();

```

### Execute a Stored Procedure and get data as a return

1. The PHP File we're working with

```php

  // We don't use execute(), we directly run fetchAssoc() to get an associative array or fetchObject() to get a an object
  $sp = Database::getConnection()->query("EXECUTE DRU.SP_NAME", $options)->fetchAssoc();

  // OR
  // $sp = Database::getConnection()->query("EXECUTE DRU.SP_NAME", $options)->fetchObject();

  return [
    '#sp' => $sp,
  ]
```

2. we get the data from the return and pass it onto the twig file

```php

<!-- Since it's an associative array, we need the key and value -->

{% for key,value in sp %}
    Key :
    {{ key }}
    Value :
    {{ value }}
{% endfor %}

```

3. Add the variable used in the controller to the render array in the module file

```php
function module_theme(){
    return [
    'variables' => [
      '#sp' => $sp,
    ],
  ];
}
```


