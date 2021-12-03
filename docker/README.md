# Instalação e Configuração Stack ELK APM em Docker

## Projeto de Monitoramento em tempo Real de Aplicações Django.


Tabela de conteúdos
=================
<!--ts-->   
   * [Instalação](#instalacao)
      * [Pre Requisitos](#pré-requisitos-e-como-rodar-a-aplicação)
   * [Features](#Features)	  
   * [Como usar](#como-usar)
      * [Pre Requisitos](#pre-requisitos)
      * [Executar Docker Compose](#executar-docker-compose)
      * [Demostração da Aplicação](#demonstração-da-aplicação)   
   * [Tecnologias](#-tecnologias-utilizadas)
   * [Contribuição](#contribuição)
   * [Autor](#autor)
   * [Licença](#licença)
<!--te-->


### Features

- [x] Criação de uma Maquina Virtual 
	- [x] Instalação do docker
	- [x] Instalação do docker-compose

- [x] Criação do Docker Composer
	- [x]	Configuração do Elastisearch
	- [x]	Configuração do Kiabana
	- [x]	Configuração do Apm-server

- [x] Instalação do cliente python para coleta dos logs
	- [x]	Instalação via pip
	- [x]	Configuraçãos settings.py

### Pré-requisitos e como rodar a aplicação

		
- Virtualbox instalado
- Hardware Necessário
  - Virtual Machine ou Computador com 2 Núcleos de CPU 
  - 4 Gb Memória Ram
- Fazer o Git Clone do Repositório.
- Instalar o Docker - [Documentação](https://docs.docker.com/engine/install/)
- Instalar o Docker Compose - [Documentação](https://docs.docker.com/compose/install/)

### Executar Docker Compose

No diretório raiz terá o arquivo "docker-compose.yml" que é apenas executalo que o ambiente ira ser disponibilizado.

	docker-compose up -d

### Principais configurações do docker-compose.


Foi necessário criação de uma rede privada para conectar o Apm-server e Kibana ao Elasticsearch.
Para essa Stack foi criada para testes, as configurações abaixo tem que ser revisadas para utilização em ambiantes de Produção.
A variavel "dicovery.type" é responsavel pela formação do cluster onde foi configurado para uso "single-mode" ou seja com apenas um nó para o Elasticsearch.
Foi criado um volume persistente para salvar os dados dos containers caso seja reiniciado.
Foi criado arquivos de configuração ".yml" fora do container para melhor gerencia dos testes, e montado com somente leitura.

#### Netowork


	networks:
	  elastic:
	    driver: bridge


#### Elasticsearch

	services:
  	  es01:
    	    image: docker.elastic.co/elasticsearch/elasticsearch:7.15.2
	    container_name: es01 
	    environment:
	      - discovery.type=single-node        # MODO DO CLUSTER ELASTICSEARCH
	      - ES_JAVA_OPTS=-Xms512m -Xmx512m
	    ulimits:
	      memlock:
	        soft: -1
	        hard: -1
	    volumes:
	      - data01:/usr/share/elasticsearch/data    # VOLUME PARA ARMAZENAR DADOS 
	      - ./elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml:ro # ARQUIVO DE CONFIGURAÇÃO
	    networks:
	      - elastic


#### kibana

	  kibana:
	    container_name: kibana
	    image: docker.elastic.co/kibana/kibana:7.15.2
	    environment:
	      SERVER_NAME: 172.27.10.122
	      ELASTICSEARCH_HOSTS: '["http://es01:9200"]'
	    volumes:
	      - ./kibana.yml:/usr/share/kibana/config/kibana.yml:rw
	    networks:
	      - elastic
	    ports:
	      - 5601:5601


#### apm-server

	apm-server:
	    container_name: apm-server
	    image: docker.elastic.co/apm/apm-server:7.15.2
	    environment:
	      - output.elasticsearch.hosts=["es01:9200"]
	    volumes:  
	      - ./apm-server.docker.yml:/usr/share/apm-server/apm-server.yml:ro
	    networks:
	      - elastic
	    ports:
	      - 8200:8200



### Demonstração da Aplicação



### 🛠 Tecnologias Utilizadas

As seguintes ferramentas foram usadas na construção do projeto:

- [Elasticsearch v7.15.2](https://www.elastic.co/guide/en/elasticsearch/reference/master/docker.html)
- [Kibana v7.15.2](https://www.elastic.co/guide/en/kibana/7.15/docker.html)
- [Apm-server v7.15.2](https://www.elastic.co/guide/en/apm/server/7.15/running-on-docker.html)


### Contribuição
### Autor
### Licença

