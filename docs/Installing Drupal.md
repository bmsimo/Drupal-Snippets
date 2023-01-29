## Installing Drupal

A quick guide on how to install Drupal on your local machine.

### Reference

- You can refer to the official documentation [here](https://www.drupal.org/docs/installing-drupal).

### Requirements

- Composer

### Steps

Here's a quick guide on how to install Drupal 9 on your local machine:

On you terminal, navigate to the directory where you want to install Drupal and exectute the following command:

```bash
composer create-project drupal/recommended-project my_site_name_dir
```

As of Drupal 9.4, the recommended project will create two folders, `web` and `vendor`. The `web` folder will contain all the Drupal files and the `vendor` folder will contain all the dependencies. Make sure that your apache server is pointing to the `web` folder.

Once the installation is complete, you can access your site by going to `http://localhost/my_site_name_dir/web`. You will be prompted to install Drupal. Follow the steps and you're good to go.

There other ways to continue with the installation from the terminal itself, for example if you're using Drush, you can use the following command to install Drupal:

```bash
drush site-install minimal --account-name=admin --account-pass=admin --db-url=mysql://root:root@localhost:3306/drupal --site-name="Drupal" --site-mail=""`
```

This will install Drupal with the minimal profile and create an admin user with the username `admin` and password `admin`.

I always go with the minimal profile, because it's the fastest way to get started and more useful when i want to do migrations. The minimal profile doesn't come with any default content, so you can start from scratch, also, it will help us later import our configuration from our source site.

You can also use the standard profile, which is the default profile that comes with Drupal.

