####################################################################################################
### Virtual Host for <dns_prefix>.<domain> subdomain
####################################################################################################
server {
    listen *:80;
    server_name <dns_prefix>.<domain>;

    ### SSL configuration
    #listen *:443 ssl;
    #ssl_certificate     <ssl_certs_path>/<ssl_cert_pem_file>;
    #ssl_certificate_key <ssl_certs_path>/<ssl_cert_pem_file>;

    #ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    #ssl_prefer_server_ciphers on;
    #ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256:DHE-DSS-AES128-GCM-SHA256:kEDH+AESGCM:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-DSS-AES128-SHA256:DHE-RSA-AES256-SHA256:DHE-DSS-AES256-SHA:DHE-RSA-AES256-SHA:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:AES:CAMELLIA:DES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!DES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DES-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KRB5-DES-CBC3-SHA';

    # Rewrite Rule for HTTP to HTTPS redirection
    #if ($scheme = http) {
    #    return 301 https://$host$request_uri;
    #}
    ###

    index app.php;

    root <app_docroot_path>;

    # Set origin client IP for logs when the web application is behind a load balancer
    real_ip_header X-Forwarded-For;
    set_real_ip_from <private_network_cidr_block>;

    access_log /var/log/nginx/<dns_prefix>-access.log;
    error_log  /var/log/nginx/<dns_prefix>-error.log;

    rewrite ^/app\.php/?(.*)$ /$1 permanent;

    try_files $uri @rewriteapp;

    location / {
        index app.php;
        if (-f $request_filename) {
            break;
        }
        rewrite ^(.*)$ /app.php last;
    }

    # Deny all . files
    location ~ /\. {
        deny all;
    }

    # Rewrite Rules
    location ~ ^/app\.php(/|$) {
        include                     fastcgi_params;
        fastcgi_pass                127.0.0.1:9000;
        fastcgi_split_path_info     ^(.+\.php)(/.*)$;

        fastcgi_param               SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param               FACTER_vm_env <env>;
        fastcgi_index               app.php;
        fastcgi_param               SCRIPT_NAME $fastcgi_script_name;
        fastcgi_param               HTTP_PROXY "";
        fastcgi_buffer_size         128k;
        fastcgi_buffers             4 256k;
        fastcgi_busy_buffers_size   256k;
        fastcgi_read_timeout        600;
    }

    # DEV Rewrite Rules
    location ~ ^/app_dev\.php(/|$) {
        include                     fastcgi_params;
        fastcgi_pass                127.0.0.1:9000;
        fastcgi_split_path_info     ^(.+\.php)(/.*)$;

        fastcgi_param               SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param               FACTER_vm_env <env>;
        fastcgi_index               app_dev.php;
        fastcgi_param               SCRIPT_NAME $fastcgi_script_name;
        fastcgi_param               HTTP_PROXY "";
        fastcgi_buffer_size         128k;
        fastcgi_buffers             4 256k;
        fastcgi_busy_buffers_size   256k;
        fastcgi_read_timeout        600;
    }

    location ~ \.php(/|$) {
        deny all;
    }
}
