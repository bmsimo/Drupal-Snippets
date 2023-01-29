# Drupal As a Framework

Installing Drupal

- Normal Drupal Download
- Composer

### Installation Steps

- Extract zip and continue with the installation on the browser
- Execute `drush site-install standard --account-name=admin --account-pass=admin --db-url=mysql://root:root@localhost:3306/drupal --site-name="Drupal" --site-mail="` to install a standard site or you can use the shortcut `drush si standard -y`
- Execute `drush site-install minimal --account-name=simobm --account-pass=10543910 --db-url=mysql://root:root@localhost:3306/dfm --site-name="Drupal" --site-mail=simobermaki@gmail.com` to install a minimal site, or you can use the shortcut `drush si minimal -y`

## Working with Drupal

### Content Entities

- to create a custom content entity in a module:
  - Create a folder in a custom module called `src/Entity`
- To create a custom content entity, you can use the shortcut `drush generate:entity:content`
- If a content entity is going to have revisions, it should extend `EditorialContentEntityBase` instead of `ContentEntityBase`

#### Content Entities Access

- To create a custom access control handler for a content entity, you can use the shortcut `drush generate:entity:access`

1. Create a permissions file in the module folder called `offer.permissions.yml`
2. 