# ToDoList - Gerenciador de Tarefas em Java Spring Boot

## Introdução

O ToDoList é um aplicativo de gerenciamento de tarefas desenvolvido em Java Spring Boot que oferece funcionalidades para criar, editar, excluir e listar tarefas. Cada tarefa possui um título, descrição, data de início, data de término e prioridade. Além disso, a aplicação permite a criação de usuários com autenticação e associa cada tarefa ao seu criador.

abaixo você tem uma visão geral das funcionalidades do projeto e destacará algumas vantagens do uso da linguagem Java para o desenvolvimento de aplicativos web.

### Reference Documentation

For further reference, please consider the following sections:

* [Official Apache Maven documentation](https://maven.apache.org/guides/index.html)
* [Spring Boot Maven Plugin Reference Guide](https://docs.spring.io/spring-boot/docs/3.0.11/maven-plugin/reference/html/)
* [Create an OCI image](https://docs.spring.io/spring-boot/docs/3.0.11/maven-plugin/reference/html/#build-image)
* [Spring Web](https://docs.spring.io/spring-boot/docs/3.0.11/reference/htmlsingle/index.html#web)

### Guides

The following guides illustrate how to use some features concretely:

* [Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/)
* [Serving Web Content with Spring MVC](https://spring.io/guides/gs/serving-web-content/)
* [Building REST services with Spring](https://spring.io/guides/tutorials/rest/)

## Funcionalidades

### Usuário

* **Criar Usuário (POST):** Crie um novo usuário fornecendo o nome, nome de usuário e senha.

   ```json
   POST: http://localhost:8080/users/
   Body:
   {
       "name": "Andrelino Silva",
       "username": "andrelinos",
       "password": "123456+"
   }
   ```

### Tarefa

* **Criar Tarefa (POST):** Crie uma nova tarefa especificando título, descrição, prioridade, data de início e data de término.

   ```json
   POST: http://localhost:8080/tasks/
   Body:
   {
       "title": "Minha nova tarefa 3",
       "description": "Tarefa para gravar aula de task do curso spring boot",
       "priority": "ALTA",
       "startAt": "2023-10-15T14:20:04",
       "endAt": "2023-10-15T18:20:04"
   }
   ```

* **Editar Tarefa (PUT):** Atualize uma tarefa existente especificando o ID da tarefa e os novos detalhes.

   ```json
   PUT: http://localhost:8080/tasks/{task_id}
   Body:
   {
       "title": "Minha nova tarefa Editada",
       "description": "Tarefa para gravar aula de task do curso spring boot",
       "priority": "ALTA"
   }
   ```

* **Excluir Tarefa (DELETE):** Exclua uma tarefa fornecendo o ID da tarefa.

   ```json
   DELETE: http://localhost:8080/tasks/{task_id}
   ```

* **Obter Tarefas por Usuário (GET):** Liste todas as tarefas associadas ao usuário autenticado.

   ```json
   GET: http://localhost:8080/tasks/
   ```

## Vantagens do Java

O Java é uma escolha sólida para o desenvolvimento de aplicativos web, e o projeto ToDoList demonstra algumas das vantagens dessa linguagem de programação:

1. **Plataforma Poderosa:** O Java é uma linguagem versátil que pode ser usada em uma variedade de plataformas, incluindo desenvolvimento web, desktop e móvel.

2. **Confiabilidade e Segurança:** Java é conhecido por sua confiabilidade e segurança. A máquina virtual Java (JVM) executa o código de forma segura e ajuda a prevenir vulnerabilidades comuns.

3. **Ecossistema Rico:** Java possui um vasto ecossistema de bibliotecas e frameworks, como o Spring Boot, que aceleram o desenvolvimento de aplicativos.

4. **Desempenho:** Java oferece um bom desempenho, especialmente em aplicativos de médio e grande porte, e é conhecido por sua capacidade de escalabilidade.

5. **Portabilidade:** Aplicativos Java podem ser executados em diferentes sistemas operacionais, garantindo que seu aplicativo funcione em diversas plataformas.

6. **Comunidade Ativa:** A comunidade de desenvolvedores Java é grande e ativa, o que significa que você encontrará suporte, recursos e atualizações regulares.

## Conclusão

O projeto ToDoList em Java Spring Boot oferece um sistema de gerenciamento de tarefas com as funcionalidades de POST, PATH e DELETE. O uso do Java como linguagem de programação traz benefícios significativos em termos de desempenho, confiabilidade e escalabilidade. Se você está procurando uma base sólida para o desenvolvimento de aplicativos web, o Java é uma escolha excelente e [Rocketseat](https://app.rocketseat.com.br/) é seu porto seguro de aprendizado. Eu recomendo! 💜

## Adicional - Arquivo Dockerfile

```docker
FROM ubuntu:latest AS build

RUN apt-get update
RUN apt-get install openjdk-17-jdk -y

COPY . .

RUN apt-get install maven -y
RUN  mvn clean install

FROM openjdk:17-jdk-slim

EXPOSE 8080

COPY --from=build /target/todolist-1.0.0.jar app.jar

ENTRYPOINT ["java", "-jar", "app.jar"]
```

Esse arquivo Dockerfile descreve a configuração de uma imagem Docker que cria um ambiente para executar uma aplicação Java. Vou explicar cada parte desse arquivo:

1. `FROM ubuntu:latest AS build`:
   * Este comando define a imagem base para a construção da imagem Docker e a nomeia como `build`. Ele usa a imagem oficial do Ubuntu como ponto de partida. O `AS build` é uma etiqueta opcional que dá um nome ao estágio atual, que pode ser referenciado posteriormente.

2. `RUN apt-get update` e `RUN apt-get install openjdk-17-jdk -y`:
   * Esses comandos são executados dentro do contêiner da imagem "build". Eles atualizam o repositório de pacotes do Ubuntu (`apt-get update`) e, em seguida, instalam o OpenJDK 17 (versão mais recente do Java Development Kit) no contêiner "build".

3. `COPY . .`:
   * Este comando copia todos os arquivos do diretório de contexto (o diretório onde o Dockerfile está localizado) para o diretório atual dentro do contêiner "build".

4. `RUN apt-get install maven -y` e `RUN mvn clean install`:
   * Estes comandos instalam o Maven (uma ferramenta de automação de compilação e gerenciamento de projetos Java) no contêiner "build" e, em seguida, executam o comando `mvn clean install`, que compila e empacota o projeto Java.

5. `FROM openjdk:17-jdk-slim`:
   * Este comando define uma nova etapa (estágio) na construção da imagem Docker, usando a imagem oficial do OpenJDK 17 "slim" como base. Isso significa que a imagem resultante não terá as bibliotecas e utilitários adicionais que a imagem completa do Ubuntu possui, tornando-a mais leve.

6. `EXPOSE 8080`:
   * Este comando instrui o Docker a abrir a porta 8080 para comunicações externas. No entanto, ele não faz com que a porta esteja acessível fora do contêiner; é necessário mapear a porta ao executar o contêiner com o comando `docker run`.

7. `COPY --from=build /target/todolist-1.0.0.jar app.jar`:
   * Esse comando copia o arquivo `todolist-1.0.0.jar` do estágio "build" para o estágio atual, criando um arquivo chamado `app.jar` no diretório raiz da imagem.

8. `ENTRYPOINT ["java", "-jar", "app.jar"]`:
   * Este comando define o ponto de entrada padrão para a imagem Docker. Quando você executar um contêiner a partir dessa imagem, ele executará o comando `java -jar app.jar`, iniciando a aplicação Java contida no arquivo `app.jar`.

Em resumo, esse Dockerfile cria uma imagem Docker que constrói uma aplicação Java a partir de um código-fonte, usando o Maven e o OpenJDK 17 como ferramentas de construção e tempo de execução, e em seguida, a imagem finaliza com a configuração da aplicação Java para ser executada quando um contêiner é iniciado. É comum dividir a construção da imagem em várias etapas para otimizar o tamanho e a eficiência da imagem resultante.
