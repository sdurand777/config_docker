# config_docker

Objectif de ce repo avoir une image de base mis a jour continuellement pour gerer un ensemble de repo git compatibles

Le docker-compose doit permettre de mount different repo git dont les environnements sont deja presents dans l'image docker pour executer les scripts



il faut en premier utiliser config_dev pour obtenir la config dev sur un ordi from scratch
afin de recuperer les drivers nvidia et les outils de base

ensuite on peut build l'image docker

docker build -f dockerfile -t nom_image .


