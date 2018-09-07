location / {
    # First attempt to serve request as file, then
    # as directory, then fall back to displaying a 404.
    try_files $uri $uri/ /index.php?$args;
    index   index.php index.html index.htm;
}
location ~ \.php$ {
    include snippets/fastcgi-php.conf;

    # With php7.0-cgi alone:
    #fastcgi_pass 127.0.0.1:9000;
    # With php7.0-fpm:
    fastcgi_pass unix:/run/php/php7.0-fpm.sock;
}