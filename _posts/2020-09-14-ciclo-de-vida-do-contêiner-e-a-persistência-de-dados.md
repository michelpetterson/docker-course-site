---
layout: post
title: "Ciclo de vida do contêiner e a persistência de dados"
date: 2020-08-15 16:21:01
image: '/assets/img/'
description:
tags:
- docker
categories:
- Docker
twitter_text:
presentation: '/assets/pp/Docker_Dia06.ppsx'
exercise: 'https://docs.google.com/forms/d/e/1FAIpQLSf67i0CbToziG1gPOTJPx0pg0JVK0nsJC9suOZWGO9GlTBK9g/viewform'
lab: 'https://docs.google.com/forms/d/e/1FAIpQLSe15f5GanGlpkCaXl0i-3D_PzQ9lCEI4Ut88nMsdPsSpsIFzw/viewform'
---

Nessa sessão veremos como podemos fazer para que o docker armazene os dados de forma persistente, permitindo que eles não sejam perdidos quando o contêiner for recriado. Entenderemos também as principais formas de se configurar a persistência de dados, e quanto a sua melhor aplicabilidade.
{: .text-justify}


## Tipos de montagens e persistência de dados

Por padrão todos os dados que são criados dentro do contêiner são armazenados na camada de contêiner, com isso quando ele é recriado, os dados que foram gravados pela aplicação se perdem junto com o contêiner. Isso se dá pelo conceito de imutabilidade e efemericidade com os quais o docker foi concebido.
Para tratar dessa necessidade o docker permite trabalhar com volumes e anexá-los aos contêineres. 
Esses volumes podem ser de três tipos:
{: .text-justify}

* <strong>Bind mounts</strong> - Armazena os arquivos do contêiner em diretório definido no docker host. É possível também compartilhar qualquer diretório ou arquivo existente no docker host (servidor) para dentro do contêiner.
{: .text-justify}

* <strong>Tmpfs mounts</strong> - Esse metódo armazena os dados do contêiner na memória do docker host e nunca os escrevem para o sistema de arquivos. Os dados **NÃO** são persistidos no disco, caso o host ou o contêiner seja reiniciado, os dados se perdem.
{: .text-justify}

* <strong>Volumes</strong> - Quando é utilizado esse tipo de montagem, o docker armazenará por padrão os dados no diretório "/var/lib/docker/volumes/\<nome do volume\>/_data" do docker host.
Nessa forma pode ser utilizado outros "drivers" de armazenamento de dados, mas por padrão o docker utiliza o driver "local". Se desejarmos utilizar outro driver, é necessário criar o volume explicitamente com o comando "docker volume create -o/--opt <driver>" antes de executar um contêiner.
{: .text-justify}

Para um melhor entendimento, abaixo está uma imagem ilustrativa dos tipos de montagens do docker:
{: .text-justify}

![Docker Types Mounts](/assets/img/docker-types-of-mounts.png)

## Comandos para gerenciamento de volumes

* Comando para criação de volume utilizando o driver padrão "local":
{% highlight bash %}
server# docker volume create <nome do volume>
{% endhighlight %}

<em> Pode ser utilizado a opção --driver (-d) para especificar um driver de armazenamento diferente</em> 

* Listar volumes criados
{% highlight bash %}
server# docker volume ls
{% endhighlight %}

Exemplo de execução de um contêiner com "bind mount":
{% highlight bash %}
server# docker container run --volume /opt/bindmount:/dados nginx
{% endhighlight %}

Exemplo de execução de um contêiner com montagem "volume":
{% highlight bash %}
server# docker ontainer run --volume teste-vol:/dados nginx
{% endhighlight %}
