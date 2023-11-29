# Part de l'OS ubuntu/ Recherche dans ubuntu
FROM ubuntu:20.04
# Lance téléchargement
RUN apt update
# Lance l'installation demandée
RUN apt install -y nginx php-fpm supervisor
# Définir variables d'environnement (nginx_vhost)
ENV nginx_vhost /etc/nginx/sites-available/default
# Définir variables d'environemment (php)
ENV php_conf /etc/php/7.4/fpm/php.ini
# Définir variables d'environnement (configurer nginx)
ENV nginx_conf /etc/nginx/nginx.conf
# Définir variables d'environnement (configurer supervisord)
ENV supervisor_conf /etc/supervisor/supervisord.conf
# Copy php dans le nginx virtualhost
COPY default ${nginx_vhost}
# Permet d'afficher l'ancien fichier et le nouveau-nom fichierEntrée>nom fichierSortie
RUN sed -i -e 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' ${php_conf} && \
    echo "\ndaemon off;" >> ${nginx_conf} 
# Copy supervisor config dans l'image en cours
COPY supervisord.conf ${supervisor_conf}
# Démarre la création et modifie la propriété du répertoire racine
RUN mkdir -p /run/php && \
    chown -R www-data:www-data /var/www/html && \
    chown -R www-data:www-data /run/php
# Définir le volume de l'image personnalisé afin de monter tous les ces répertoires sur la machine hôte
VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx", "/var/www/html"]
# Ajoute un script start.sh et définit la commande CMD de conteneur par défaut
COPY start.sh /start.sh
CMD ["./start.sh"]
# Ouvre les ports par défaut sur le conteneur
EXPOSE 80 443






