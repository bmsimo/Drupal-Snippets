# Custom Fields

Sometimes you need to create your own custom fields or group of fields to your models. This is a guide on how to do that. Although there is an example at the [Examples for developers](https://git.drupalcode.org/project/examples/-/tree/3.x/modules/field_example) module, i found it lacking in some areas. So i decided to create my own example.

We will be leveraging the [Field API](https://www.drupal.org/docs/8/api/entity-api/field-api) to create our custom fields. which means we will need to create a `FieldType`, `FieldWidget` and `FieldFormatter`. we will create these files in the `src/Plugin/Field/FieldType`, `src/Plugin/Field/FieldWidget` and `src/Plugin/Field/FieldFormatter` directories.

The first example will be creating a field that has both an entity reference field and an integer field.

## FieldType

The `FieldType` is the class that will handle the storage of the field. It will be responsible for storing the data in the database and retrieving it. It will also be responsible for validating the data.

```php
<?php

namespace Drupal\MODULE_NAME\Plugin\Field\FieldType;

use Drupal\Core\Entity\EntityInterface;
use Drupal\Core\Field\FieldStorageDefinitionInterface;
use Drupal\Core\Field\Plugin\Field\FieldType\EntityReferenceItem;
use Drupal\Core\StringTranslation\TranslatableMarkup;
use Drupal\Core\TypedData\DataDefinition;

/**
 * @FieldType(
 *   id = "YOUR_FIELD_TYPE_ID",
 *   label = @Translation("THE LABEL THAT WILL BE DISPLAYED"),
 *   description = @Translation("A SIMPLE DESCRIPTION OF THE FIELD"),
 *   category = @Translation("THE CATEGORY WHERE IT WILL BE DISPLAYED"),
 *   default_widget = "YOUR_DEFAULT_WIDGET",
 *   default_formatter = "YOUR_DEFAULT_FORMATTER",
 * )
 */
class EntityReferenceQuantity extends EntityReferenceItem {
  public static function propertyDefinitions( FieldStorageDefinitionInterface $field_definition ) {

    $properties              = parent::propertyDefinitions( $field_definition );
    
    $priority_definition     = DataDefinition::create( 'integer' )
                                             ->setLabel( t( 'Priority' ) )
                                             ->setRequired( true ); // If we want the field to be required
    $properties['priority'] = $priority_definition;

    return $properties;
  }

  public static function schema( FieldStorageDefinitionInterface $field_definition ) {
    $schema                         = parent::schema( $field_definition );

    // The schema normally used in Drupal for integer fields
    $schema['columns']['priority'] = array(
      'type'     => 'int',
      'size'     => 'normal',
      'unsigned' => true,
    );

    return $schema;
  }

  /**
   * {@inheritdoc}
   */
  public function isEmpty() {
    if ( ! empty( $this->priority ) ) {
    // If the field is not empty, we return false and the information is saved
      return false;
    }
    return true;
  }

}
```

## FieldWidget

The `FieldWidget` is the class that will handle the display of the field in the form. It will be responsible for displaying the field in the form and handling the data that is sent from the form.

```php
<?php

namespace Drupal\MODULE_NAME\Plugin\Field\FieldWidget;

use Drupal\Core\Field\FieldItemListInterface;
use Drupal\Core\Field\Plugin\Field\FieldWidget\EntityReferenceAutocompleteWidget;
use Drupal\Core\Form\FormStateInterface;

/**
 * @FieldWidget(
 *   id = "YOUR_DEFAULT_WIDGET",
 *   label = @Translation("THE LABEL THAT WILL BE DISPLAYED"),
 *   description = @Translation("A SIMPLE DESCRIPTION OF THE FIELD"),
 *   field_types = {
 *     "YOUR_FIELD_TYPE_ID"
 *   }
 * )
 */
class EntityReferenceAutocompleteQuantity extends EntityReferenceAutocompleteWidget {
  public function formElement( FieldItemListInterface $items, $delta, array $element, array &$form, FormStateInterface $form_state ) {
    $widget = parent::formElement( $items, $delta, $element, $form, $form_state );

    $widget['priority'] = array(
      '#title'         => $this->t( 'Priority' ),
      '#type'          => 'number',
      '#default_value' => isset( $items[ $delta ] ) ? $items[ $delta ]->priority : 1, // If no value is set, we set it to 1
      '#min'           => 1,
      '#weight'        => 10,
    );

    return $widget;
  }
}

```

## FieldFormatter

The `FieldFormatter` is the class that will handle the display of the field in the view. It will be responsible for displaying the field in the view.

```php
<?php

namespace Drupal\MODULE_NAME\Plugin\Field\FieldFormatter;

use Drupal\Core\Field\FieldItemListInterface;
use Drupal\Core\Field\Plugin\Field\FieldFormatter\EntityReferenceLabelFormatter;

/**
 * @FieldFormatter(
 *   id = "YOUR_DEFAULT_FORMATTER",
 *   label = @Translation("THE LABEL THAT WILL BE DISPLAYED"),
 *   description = @Translation("A SIMPLE DESCRIPTION OF THE FIELD"),
 *   field_types = {
 *     "YOUR_FIELD_TYPE_ID"
 *   }
 * )
 */
class EntityReferenceQuantityFormatter extends EntityReferenceLabelFormatter {

  public function viewElements( FieldItemListInterface $items, $langcode ) {
    $elements = parent::viewElements( $items, $langcode );
    $values   = $items->getValue();

    foreach ( $elements as $delta => $entity ) {
      $elements[ $delta ]['#suffix'] = ' Priority: ' . $values[ $delta ]['priority'];
    }


    if ( \Drupal::routeMatch()->getRouteName() == 'view.MYVIEW' ) { // OPTIONAL
      foreach ( $elements as $delta => $entity ) {
        $elements[ $delta ]['#prefix'] = '<div class="teaser-title">';
        $elements[ $delta ]['#suffix'] = '</div><div class="teaser-priority">priority: ' . $values[ $delta ]['priority'] . '</div>';
      }
    }// OPTIONAL

    // OPTIONAL: Order by [$values[ $delta ]['priority']]
    usort($elements, function($a, $b) {
      return $a['#suffix'] <=> $b['#suffix'];
    });

    return $elements;
  }
}

```