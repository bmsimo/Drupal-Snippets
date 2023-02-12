## Menus


## Add a menu link

```yaml
# module_name.links.menu.yml
toolbar.user:
  title: User
  description: 'User menu items'
  weight: 10
  route_name: 'user.page'
  parent: system.admin_toolbar
  menu_name: toolbar
```