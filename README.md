# elasticstack

Aqui vamos mostrar como foi instalado uma stack de Elasticsearch, Kibana, metricbeat, e APM em uma VM com Rehl 7.

Recursos Destinados:

> 8 Vcpus
> 
> 8 Gb Memória Ram
> 
> 300 Gb Disco



### Primeiro vamos Instalar o Elasticsearch:


Baixe e instale a chave de assinatura pública:
```
rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
```

crie o arquivo elastic.repo dentro de /etc/yum.repos.d/ e copie as linhas abaixo:

```
[elasticsearch]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=0
autorefresh=1
type=rpm-md
```

logo após execute os comandos para carregar a lista de pacotes do novo repositório.

```
#yum clean all
#yum repolist
```
logo após execute a instalação:

```
sudo yum install --enablerepo=elasticsearch elasticsearch kibana metricbeat apm-server -y
```


### Agora vamos a configuração:

No Elasticsearch no arquivo elasticsearch.yml que fica dentro de /etc/elasticsearch foi apenas alterado esses parametros:

```
network.host: 0.0.0.0
http.port: 9200
```

No apm-server no arquivo apm-server.yml que fica dentro de /etc/apm-server foi apenas alterado esses parametros:

O bind para deixar a porta exposta para os outros servidores acessarem.
E a conexão com elasticsearch, como a instalação foi feita no mesmo servidor ficou como localhost.

```
apm-server:
  host: "0.0.0.0:8200"
  
output.elasticsearch:
  hosts: ["localhost:9200"]  
```
