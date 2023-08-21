# Vagrantfile et scripts pour automatiser Kubernetes à l'aide de Kubeadm

[![made-with-bash](https://img.shields.io/badge/Made%20with-Bash-1f425f.svg)](https://www.gnu.org/software/bash/)
[![made-for-VSCode](https://img.shields.io/badge/Made%20for-VSCode-1f425f.svg)](https://code.visualstudio.com/)
[![GPLv3 license](https://img.shields.io/badge/License-GPLv3-blue.svg)](http://perso.crans.org/besson/LICENSE.html)
[![Ruby](https://badgen.net/badge/icon/ruby?icon=ruby&label)](https://https://ruby-lang.org/)

Language : [**Anglais**](https://github.com/CheickOumarDiaby/vagrant-kubeadm-k8s/README.md) or [**Français**](https://github.com/CheickOumarDiaby/vagrant-kubeadm-k8s/README.fr.md)

## Prérequis

1. Avoir: Vagrant et VirtualBox
2. Avoir plus de 8 Giga RAM sur votre machine car les Vms utilisent 3 vCPUS et plus de 4 GB RAM

## Pour les utilisateurs de MAC/Linux

La dernière version de Virtualbox pour Mac/Linux peut causer des problèmes.

Créer ou  éditer le fichier _/etc/vbox/networks.conf_  et ajouter le code suivant pour éviter les erreurs réseaux :
<pre>* 0.0.0.0/0 ::/0</pre>

ou exécutez les commandes ci-dessous :

```shell
sudo mkdir -p /etc/vbox/
echo "* 0.0.0.0/0 ::/0" | sudo tee -a /etc/vbox/networks.conf
```
Pour que les réseaux hôtes uniquement puissent être dans n'importe quelle plage, pas seulement 192.168.56.0/21 comme décrit ici :
https://discuss.hashicorp.com/t/vagrant-2-2-18-osx-11-6-cannot-create-private-network/30984/23

## Installation du Cluster

Pour provisionner le cluster, exécutez les commandes suivantes.

```shell
git clone https://github.com/CheickOumarDiaby/vagrant-kubeadm-k8s.git
cd vagrant-kubeadm-k8s
vagrant up
```
## Définir la variable de fichier _Kubeconfig_

```shell
cd vagrant-kubeadm-kubernetes
cd configs
export KUBECONFIG=$(pwd)/config
```

ou vous pouvez copier le fichier de configuration dans le répertoire _.kube_.

```shell
cp config ~/.kube/
```

## Installer le tableau de bord Kubernetes

Le tableau de bord est automatiquement installé par défaut, mais il peut être ignoré en commentant la version du tableau de bord dans _configuration.yaml_ avant d'exécuter `vagrant up`.

Si vous ignorez l'installation du tableau de bord, vous pouvez le déployer plus tard en l'activant dans _configuration.yaml_ et en exécutant ce qui suit :

```shell
vagrant ssh -c "/vagrant/scripts/dashboard.sh" master
```

## Accès au tableau de bord Kubernetes

Pour obtenir le token de connexion, copiez-le depuis _config/token_ ou exécutez la commande suivante :

```shell
kubectl -n kubernetes-dashboard get secret/admin-user -o go-template="{{.data.token | base64decode}}"
```

Proxy le tableau de bord :
```shell
kubectl proxy
```

Ouvrez le site dans votre navigateur :
```shell
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/overview?namespace=kubernetes-dashboard
```

## Pour arrêter le cluster,

```shell
vagrant halt
```

## Pour redémarrer le cluster,

```shell
vagrant up
```

## Pour supprimer le cluster,

```shell
vagrant destroy -f
```