# elasticstack

Aqui vou mostrar como foi instalado uma stack de Elasticsearch, Kibana, metricbeat, e APM.

# Primeiro vamos Instalar o Elasticsearch:


Baixe e instale a chave de assinatura pública:

rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch


crie o arquivo elasticsearch.repo dentro de /etc/yum.repos.d/ e copie as linhas abaixo:


[elasticsearch]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=0
autorefresh=1
type=rpm-md


logo após execute os comandos para carregar a lista de pacotes do novo repositório.

#yum clean all
#yum repolist

logo após execute a instalação:

sudo yum install --enablerepo=elasticsearch elasticsearch




