# Drupal 9 Security

Drupal 9 has built-in security features that help to prevent XSS (Cross-Site Scripting) and SQL injection attacks.

For XSS, Drupal 9 uses the OWASP (Open Web Application Security Project) Secure Input library, which helps to prevent XSS attacks by sanitizing and validating user input. Additionally, Drupal 9 includes the HtmlRouteLink class, which helps to prevent XSS attacks by properly encoding link URLs.

For SQL injection, Drupal 9 uses parameterized queries in its database layer, which helps to prevent SQL injection attacks by properly escaping user input.

However, it's always a good idea to add extra security measures in your custom modules, especially if you're handling sensitive user data or processing sensitive operations. You should always validate user input, and make sure to escape any data that is displayed on the site to prevent XSS attacks. Additionally, you should always use parameterized queries when interacting with the database to prevent SQL injection attacks.

In summary, while Drupal 9 provides built-in security features to prevent XSS and SQL injection attacks, it's always a good idea to take extra steps to secure your custom modules and ensure that your site remains secure.