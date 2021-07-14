# dockerfiles #

images docker créées pour différents usages.

## images dockers ##

### php7.2-with-ext ###
image apache/php7.2 (avec toutes les extensions nécessaires pour prestashop) pour développer et installer prestashop 1.7.6.7 par exemple

#### Pour construire l'image ####
```
cd php7.2-with-ext/
docker build -t php7.2-with-ext .
```

## docker-compose ##

### xampp ###
pour créer php7.2-with-ext, db et phpmyadmin (idéal pour développer prestashop 1.7.6.7)  
Les `data` pour `db` seront placées dans ~/VM/docker/mysql  
Le `www` pour `php` sera placé dans ~/VM/docker/www  
Le 'db' sera acceessible depuis le host sur le port 3306 avec le user `root`


#### Pour lancer le docker-compose ####
```
cd docker-compose/xampp
docker-compose start
```
(ou up pour voir les logs dans la console)

#### Pour vérifier l'état des containers ####
```
cd docker-compose/xampp
docker-compose ps
```

#### Pour arrêter le docker-compose ####
```
cd docker-compose/xampp
docker-compose stop
```
