# Microserviços Cards (Cartões) dockerized sem as definições do Dockerfile 
### Acessando ao H2
### - Lembrar:
#### a. Criar um arquivo sql: data.sql
#### b. Configurar o application.properties
#### c. Adicionar no pom a dependência(pode ser pelo starters ou manualmente):
````
<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<scope>runtime</scope>
</dependency>
````

#### 1. Verificar no console o endpoint criado:
````
h2-console
````
#### 2. No navegador colocar o endpoint criado com a porta local cofigurado nas properties. Se não tiver sido configurada, ficará na 8080:
````
http://localhost:8080/h2-console/
````
#### 3. Quando abrir no console, colocar em JDBC URL, se der algum erro, o que saiu no console, por exemplo:
````
 H2 console available at '/h2-console'. Database available at 'jdbc:h2:mem:testdb'
````
#### 4. Testando no Postman:
````
http://localhost:9000/myCards
````
##### - se der um erro 415: Content type 'text/plain;charset=UTF-8' not supported], colocar no Headers:
````
1. KEY: Content-Type
2. VALUE: application/json
````

#### 5. A beleza dos Microservices: Não há nada acoplado da regra de negócio com os microserviços cartões ou contas.


#### 6. Buildpacks (pacotes de compilação) - São conjuntos de scripts que, dependendo da linguagem de programação que você escolher, resolverão dependências,
geração de assets e até mesmo compilação arquivos, dentro da plataforma Heroku.
> - 1. Spring Boot tem uma abordagem aproveitando buildpacks que são introduzidos pela Pivotal e Heroku.
> - 2. Você não precisa escrever uma definição completa.
> - 3. O Buildpacks são inteligentes o suficiente para detectar quais são todas as dependências que você tem dentro da aplicação.
> - 4. Eles criam uma imagem sem a necessidade de mencionar a definição(FROM, ENTRYPOINT, COPY e etc) do Dockerfile.
> - 5. Os buildpacks que eles lhe darão precisam da nuvem para imagens do docker, transformando o código-fonte do seu aplicativo em imagens que você pode correr em qualquer nuvem.
> - 6. Não há necessidade de se preocupar em criar o Dockerfile.
> - 7. Eles possuem um alto nível de abstração para construir as aplicações comparado aos Dockerfiles.
> - 8. Não há a necessidade de fazer todo o trabalho pesado fazendo inúmeras definições em Dockerfiles, que diante dessa ferramenta, se tornam contraproducentes.
> - 9. Eles detectam e reunem tudo o que a aplicação precisa para construir e rodar com sucesso num ambiente in Cloud.
> - 10. Buildpacks é um conceito e eles provém frameworks necessárias para aplicativos relacionados ao Java.
> - 11. O Paketo Buildpacks é usado pelo SpringBoot para gerar imagens baseadas no conceito dos buildpacks. Ele impacta e suporta Java, .NET e outras linguagens.


#### 7. Criando Docker Image com Buildpacks

##### Adicionar:
##### Observação: O nome da imagem alí está apenas como exemplo.
````
<plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <image>
                        <name>tuyosistema/${project.artifactId}</name>
                    </image>
                </configuration>
            </plugin>
</plugins>
````

### Rodar o comando dentro do diretório com o pom.xml, que usa buildpacks, para gerar uma imagem docker para o projeto:
````
mvn spring-boot:build-image
````
### Depois de rodar o comando acima dentro do diretório em que se encontra o pom.xml, ele fará o pull das imagens do repositório do DockerHub e isso ajudará a criar imagens baseadas no conceito de buildpacks.

### Imagem criada: Successfully built image 'docker.io/tuyosistema/loansdockerize:latest'

### O nome da imagem foi colocado aqui como exemplo:
````
    <image>
        <name>tuyosistema/${project.artifactId}</name>
    </image>
````

### Imagem feita sem dockerfile, ou seja, sem as definicoes do mesmo.

### Correndo imagem no terminal ou cmd:
````
## Correndo o docker image com a porta:
````
docker run -p 9000:9000 hendersonporfirio/microservice-cards

````
