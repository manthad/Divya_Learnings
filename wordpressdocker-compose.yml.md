services:
  mysql:
    image: mysql:latest
    container_name: mysql-container-mula
    environment:
      MYSQL_ROOT_PASSWORD: my_root_password
      MYSQL_USER: wordpress_test_ravi
      MYSQL_PASSWORD: wordpress_test_ravi_password
      MYSQL_DATABASE: wordpress_ravi
    ports:
      - 33306:3306
    volumes:
      - wordpress_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    container_name: wordpress-container-mula
    ports:
      - 8081:80
    environment:
      WORDPRESS_CONFIG_EXTRA:
        define('WP_ALLOW_REPAIR', true );
        define('WP_HOME', 'http://localhost:8081');
        define('WP_SITEURL','http://localhost:8081');

volumes:   # add this section
  wordpress_data:    # does not need anything underneath this