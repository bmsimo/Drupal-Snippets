# Files & Images

Example of how we work with files and images in Drupal 8/9.

## Create CSV

```php
public function FUNCTION_NAME(array &$form, FormStateInterface $form_state)
{


    $bdd = Database::getConnection();

    $query = $bdd->select('SCHEMA.TABLE_NAME', 'c');
    $query->fields('c', [
        'TITLE',
        'NAME',
    ]);
    $result = $query->execute()->fetchAll();

    $delimiter = "|";
    $filename = "file_name" . date('Y-m-d') . ".csv";

    $uri = 'public://FOLDER/' . $filename;

    // Create a file pointer
    $f = fopen($uri, 'w');

    // Set column headers
    $fields = [
        'HEADER1',
        'HEADER2',
    ];

    fputcsv($f, $fields, $delimiter);


    foreach ($result as $row) {
        $lineData = [
            $row->TITLE,
            $row->NAME
        ];
        fputcsv($f, $lineData, $delimiter);
    }

    // Move back to beginning of file
    fseek($f, 0);
    //output all remaining data on a file pointer
    fpassthru($f);
    fclose($f);

    $file = file_save_data($f, 'public://' . $filename);

    $uri_final = '/sites/default/files/FOLDER/' . $filename;

    \Drupal::messenger()->addStatus(['#markup' => '<a href=' . $uri_final . ' target="_blank" rel= noopener noreferrer" >DOWNLOAD FILE</a>']);

     // Create url from route in .routing.yml
    $url1 = Url::fromRoute('route');

     // Redirect to url
    $form_state->setRedirectUrl($url1);
}
```

## Get the url of an image

In this example, we get the url of an image that is added via a MEDIA field called "field_image".

```php
use Drupal\file\Entity\File;

$mid = $node->field_image->target_id;
if (!empty($mid)) {
   // Crear enlace de la imagen
   $file = File::load($mid);
   $url = $file->getFileUri();
   $m_url = file_create_url($url);
}
```
