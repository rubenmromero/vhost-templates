####################################################################################################
### Virtual Host for <dns_prefix>.<domain> subdomain
####################################################################################################
<VirtualHost *:80>
    ServerAdmin support@<domain>
    ServerName <dns_prefix>.<domain>
    DocumentRoot <app_docroot_path>

    # Rewrite Rule for HTTP to HTTPS redirection
    #RewriteEngine on
    #RewriteCond %{SERVER_PORT} !^443$
    #RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} [R,L]

    <Directory <app_docroot_path> >
        AddType application/json .json
        AllowOverride All
        Options +FollowSymLinks
        SetEnv FACTER_vm_env <env>
        Require all granted
    </Directory>

    # For CentOS
    ErrorLog  logs/<dns_prefix>-error.log
    CustomLog logs/<dns_prefix>-access.log combined
    # For Debian
    #ErrorLog  ${APACHE_LOG_DIR}/<dns_prefix>-error.log
    #CustomLog ${APACHE_LOG_DIR}/<dns_prefix>-access.log combined
</VirtualHost>
