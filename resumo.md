# RESUMO DE DOCKER

* container: um processo isolado no seu computador que pode ser utilizado para rodar todo um ambiente de desenvolvimento ou aplicação para produção.

* imagem docker: modelo para criação de um container, você pode criar utilizando um Dockerfile ou baixar uma imagem pronta da internet

* você pode ter cada parte essencial da sua aplicação separada, assim dá pra criar várias instâncias do banco de dados e da aplicação web e balancear a carga entre elas, fazendo assim com que fique mais difícil dela cair

## comandos básicos CLI

* docker run <imagem>: 
    roda um container a partir de uma imagem 

    -it: a opção <-it> permite acesso ao prompt da imagem que está sendo executada.

    -d: roda container em background

    -p: expõe uma porta do container para acesso externo

    --name: nomeia o container (caso não use essa flag o container terá nome aleatório dificultando a gestão)


* docker ps:
    lista os containers que estão rodando
 
    -a: lista todos os containers que já foram executados

* docker stop <id_container>:

* docker start <id_container:
    reinicia um container já criado anteriormente

* docker logs <id_container>:
    exibe logs de um container especificado

* docker rm <id_container:
    remove um container criado anteriormente e libera espaço.

docker build <arquivo>:
    constrói uma imagem a partir de um arquivo Dockerfile

    .: constrói a imagem a partir de um arquivo Dockerfile na raiz do projeto
    
    -t nomeia uma tag pra imagem criada (facilita gerenciamento)