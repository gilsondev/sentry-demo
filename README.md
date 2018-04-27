# Monitorando erros da sua aplicaçào com o Sentry

Esse projeto foi usado como exemplo na palestra sobre Sentry. Faça o fork e altere a vontade.

Slides: https://speakerdeck.com/gilsondev/como-monitorar-os-erros-da-sua-aplicacao-com-o-sentry

## Requisitos

* Vagrant
* Docker e Docker Compose

### Instalação

* Execute os serviços do Sentry para preparar a aplicação:

```shell
$ docker-compose -f sentry/docker-compose.yml up -d
```

* Inicie as outras VMs:

```shell
$ vagrant up django laravel springboot
```

Para acessar o painel do sentry, acesse: `http://localhost:9000` e siga as instruções.

### Preparando as aplicações

Os projetos são exemplos do próprio Sentry. Para verificar de outras tecnologias é só acessar o repositório [getsentry/examples](https://github.com/getsentry/examples).

Com o sentry preparado e com sua conta criada, vamos preparar as aplicações para enviar erros ao painel.

* Crie os seguintes projects no Sentry:

  * Django
  * Laravel
  * Java

Com suas [respectivas DSNs](https://docs.sentry.io/quickstart/#configure-the-dsn), configure as aplicações de acordo com o README de cada projeto.

* [Django](django/README.md)
* [Laravel](laravel/README.md)
* [Spring Boot](spring-boot/README.md)

Com tudo configurado é só acessar as aplicações:

* Django: http://192.168.10.3:8000
* Laravel: http://192.168.10.4:8000
* Spring Boot: http://192.168.10.5:8080
