# Projeto para poder falar sobre o Registry no Docker Rio

## O que é, onde vivem, você aprenderá hoje no Docker Rio

### O que é

O Registry é um aplicativo do lado do servidor sem estado e altamente escalável que armazena e permite distribuir imagens do Docker. O Registry é de código aberto, sob a licença permissiva do Apache . Você pode encontrar o código-fonte no [GitHub](https://github.com/distribution/distribution).

### Por que usar

Você deve usar o Registro se quiser:
- Controle rígido onde suas imagens estão sendo armazenadas;
- Possuir totalmente seu pipeline de distribuição de imagens;
- Integre o armazenamento e a distribuição de imagens firmemente em seu fluxo de trabalho de desenvolvimento interno

### Alternativas

Os usuários que procuram uma solução pronta para uso e sem manutenção são incentivados a acessar o Docker Hub , que fornece um registro hospedado gratuito, além de recursos adicionais (contas da organização, compilações automatizadas e muito mais).

### Requisitos

O Registry é compatível com o mecanismo Docker versão 1.6.0 ou superior.

- [Docker HUB](https://hub.docker.com/)
- [Amazon Registry](https://aws.amazon.com/pt/ecr/)
- [Google Container Registry](https://cloud.google.com/container-registry/)
- [Github Registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-docker-registry)

### Como funciona o Registry

O Registry tem funcionalidade muito parecida com o GIT.
Quando damos um build em um Dockerfile para construirmos um container podemos utilizar o argumento “-t” para que o container tenha uma TAG
Podemos enviar o container para o Registry com o PUSH valor_da_tag assim como podemos recuperar um container com o comando PULL.
Os exemplos que utilizaremos terão como base o Registry oficial da Docker que é o https:/hub.docker.com.

### Recuperando um Container

Imagine que você quer utilizar o container do ubuntu na última versão.
o comando é:

```
$ docker pull ubuntu:latest
```

### Enviar um container para o Registry

Antes de enviar um container para o Registry precisamos de um conteiner

```DOCKER
FROM ubuntu:latest
WORKDIR /app
RUN echo "MEU PRIMEIRO CONTAINER" > /app/text.txt
```

Tendo o Dockerfile conforme o texto acima, temos que compilar a imagem do container.

```
$ docker build . --file=Dockerfile -t meu_primeiro_container:latest
```

Depois do build para testarmos a imagem compilada rodamos o comando abaixo:
```
$ docker run --rm meu_primeiro_container:latest cat /app/text.txt
```

E ele tem que retornar o texto `“MEU PRIMEIRO CONTAINER”`

Com a imagem gerada temos que colocar uma TAG para identificar ela.

```
$ docker tag meu_primeiro_container:latest rfpereira/docker-rio-registry:latest
$ docker push rfpereira/docker-rio-registry:latest
```

A TAG tem que conter a identificação do usuário no docker hub para poder ser enviado para lá, caso você utilize o linux e não tenha o Docker Desktop instalado para fazer o login no docker hub você poderá se autenticar com o comando abaixo.

```
$ docker login
```
### Criando o nosso próprio Registry

A Docker disponibiliza a imagem do Registry.

Para criarmos o nosso próprio Registry basta rodarmos o container com o comando abaixo:

```
$ docker run -d -p 5000:5000 --name registry registry:2
```

### Utilizando o nosso Registry

Com o container do Registry rodando podemos pegar a imagem criada anteriormente e colocar outra TAG agora para fazer o PUSH para o nosso Registry

```
$ docker image tag meu_primeiro_container localhost:5000/meu_primeiro_container
```

Agora já podemos enviar para o nosso Registry

```
$ docker push localhost:5000/meu_primeiro_container
```

### Destruindo nosso Registry

Agora pare nosso registry e remova todos os dados

```
$ docker container stop registry && docker container rm -v registry
```

Claro que isso não é o melhor cenário, tendo em vista que se armazenamos uma imagem de container em um Registry é para podermos utilizarmos ela posteriormente.

Mas este é só um exemplo para fins didáticos.

## Referências

 - [Registry](https://docs.docker.com/registry/)

 ## Vídeo do processo descrito acima.

 [![Docker Rio Registry](https://img.youtube.com/vi/rB62C1UeRZA/0.jpg)](https://youtu.be/rB62C1UeRZA)