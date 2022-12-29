# DEVOPS CONTAINERS ![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)

## Docker: Criando e gerenciado containers
https://cursos.alura.com.br/course/docker-criando-gerenciando-containers

## 02 Os primeiros comandos:

https://hub.docker.com/ Repositorio Docker com imagens pre-prontas (selo de imagem oficial)

https://docs.docker.com/engine/reference/commandline/cli/  Use the Docker command line docker

```docker run --help```

```docker pull ubuntu``` //baixa, extrai a imagem e não executa

```docker run ubuntu``` //não vai baixar, e vai apenas executar o que ja foi extraido


```docker run ubuntu``` // baixa a imagem, executa ubuntu e para, pois nao tem mais nenhum processo segurando a execução do container. Para ficasse ON teria que ter pelo menos 1 processo dentro dele vivo.

## Fluxo da criação de containers 
https://cursos.alura.com.br/course/docker-criando-gerenciando-containers/task/100389

```sh
docker ps
docker container ls 
>>>>> ambos os comandos acima, fazem a mesma coisa
docker container ls -a // -a mostra os executados no passado
```



```docker run ubuntu sleep 1d``` // tecnicamente esse comando vai executar o comando sleep 1dia dentro da imagem oficial ubuntu
O comando acima, trava o terminal, e precisa-se abrir outro e pode executar o comando abaixo para ver o container no ar.

```docker ps``` //informando o CONTAINER ID, IMAGE, COMMAND, CREATED, STATUS, PORTS, NAMES


## Outros comandos importantes
https://cursos.alura.com.br/course/docker-criando-gerenciando-containers/task/100390

```docker stop xxxxxx``` // xxx é o ID e pára o processo
```docker start xxxxxx``` // xxx é o ID e reexecutar o processo

Como interagir com ele??
```docker exec -it xxxxxx bash``` // -it de interativo e o bash será executado dentro do container xxxxx, ficando isolado, por conta do namespayce, filesystem isolados.

### TOP
Dando o comando ```top``` dentro do container, e iniciando e parando o mesmo, ele incerra os PIDs e arvores de processos (KILL). Caso não se queira isso, pode-se dar uma pausa:
```docker pause xxxxxx``` // xxx é o ID e pausa o container
```docker unpause xxxxxx``` // xxx é o ID e despausa o container sem matar os processos rodandos

```docker stop -t=0 xxxxxx``` // -t=0 pára imediatamente, visto que por default ele executa o pause de maneira saudavel.
```docker rm xxxxxx``` //remove o container e apaga todas as modificações de criação de arquivo eventual dentro do ubuntu, por exemplo, caso voce volte a executar o comando ```docker exec -it xxxxxx bash``` não gerando persistencia de arquivos neste caso.

## Otimizando

```docker run -it ubuntu bash```  //não tem nenhum terminal travando a execução, mas tbm quando encerrrar, os dados alterados no root@xxxxx:~# ..... não serão persistidos, por serem devidamentes isolados.

# O ```docker run``` cria um novo container e o executa. O ```docker exec``` permite executar um comando em um container que já está em execução.


### Mapeando portas
https://cursos.alura.com.br/course/docker-criando-gerenciando-containers/task/100391

"dockersamples/static-site" //https://hub.docker.com/r/dockersamples/static-site

```docker run -d dockersamples/static-site``` //-d para travar o terminal, vai baixar esse container, nao-oficial. Faz os downloads e extrações e executa na maquina.
 
## >>> ```docker run -d -P dockersamples/static-site``` // -P essa flag inicia o mapeamento de portas
```docker port xxxxxxx``` // xxxx sendo o id do dockersamples baixado
mostrando as portas mapeadas do container, dentro do ```localhost:yyyy``` sendo yyy o numero da porta 80/tcp -> :::yyyy

## >> Ou então >> se matarmos o container com: ```docker rm xxxxxxxxx --force```
Com o comando acima, voce mata o container e conteudo e abaixo será possivel especificar qual porta do containter, voce quer mapear e dar mais detalhes deste mapeamento.

## >>> ```docker run -d -p 8080:80 dockersamples/static-site``` // -p minusculo, essa flag  especifica que que a porta 8080 do container -> [deve refletir] a porta 80 do localhost.
Com isso valida-se o isolamento da aplicação do container.

```docker port``` //comando responsável pela visualização de como o mapeamento de portas de um container está sendo feito.



# 03 Entendendo imagens
https://cursos.alura.com.br/course/docker-criando-gerenciando-containers/task/100392


```docker images``` ou ```docker images ls```  //lista as imagens baixadas


## Criando e compreendendo imagens:

- Imagens são imutáveis, ou seja, depois de baixadas, múltiplos containers conseguirão reutilizar a mesma imagem;
- Imagens são compostas por uma ou mais camadas. Dessa forma, diferentes imagens são capazes de reutilizar uma ou mais camadas em comum entre si;
- Podemos criar nossas imagens através de Dockerfiles e do comando ```docker build```;
- Para subir uma imagem no Docker Hub, utilizamos o comando ```docker push```.



## 04 Persistindo dados:
https://cursos.alura.com.br/course/docker-criando-gerenciando-containers/task/100604

Quando containers são removidos, nossos dados são perdidos;
- Podemos persistir dados em definitivo através de volumes e bind mounts;
- Bind mounts dependem da estrutura de pastas do host;
- Volumes são gerenciados pelo Docker;
- Tmpfs armazenam dados em memória volátil
