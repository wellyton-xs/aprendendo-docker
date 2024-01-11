# DOCKER

## INTRODUÇÃO

docker é uma poderosa ferramenta que nos permite ter várias instâncias de uma aplicação rodando simultaneamente além de permitir isolar várias aplicações e separar cada parte essencial de uma aplicação com suas dependências. Podemos ter uma instancia com a aplicação web e suas dependências e uma instância com o banco de dados. 

resumindo as vantagens:

    separação de dependências
    separação de aplicações (se uma cai a outra pode continuar o seu trabalho)
    múltiplas instâncias para mais disponibilidade e segurança (aplicação não cai fácil nem fudendo com mais de uma instancia dela rodando, essa redundância é ótima pra construir aplicações sólidas e difíceis de cair)

    
## instalação
para instalar o docker basta ir na documentação oficial e utilizar os métodos lá apresentados, no arch Linux basta utilizar: 

yay -S docker
sudo systemctl start docker

após isso será possível utilizar docker normalmente, porém no Linux é necessário utilizar sudo antes de fazer certas operações com containers.

## O quão poderoso é o docker?

um exemplo simples de comando que demonstra a utilidade do docker é o comando:

docker run -it ubuntu

se você rodou esse comando deve ter visto um terminal bash do ubuntu aparecendo após um download ter ocorrido. O que aconteceu é que o docker baixou uma imagem do seu repositório online e então executou. Vamos comparar esse processo com o processo de se baixar uma maquina virtual ubuntu

maquina virtual:
    processo de instalação mais complicado
    sistema inflado (cheio de coisas que não precisa)
    há como baixar uma maquina virtual pronta

container docker:
    processo de instalação rápido e simples
    sistema leve (só tem o que precisa)
    já vem pronto, instala e roda.

usar maquinas virtuais para rodar aplicações grandes sempre foram uma dor de cabeça para Devs, por isso que hoje em dia em produção os containers junto a ferramentas como Kubernetes e Nginx tornam as aplicações muito mais escaláveis permitindo que seja possível aumentar as instâncias da aplicação de acordo com a demanda, balancear a carga para suportar alto tráfego de dados e então descartar as instâncias no lixo quando não mais necessárias.

## o que é uma imagem docker?

uma imagem docker é um pacote ou ambiente que pode ser executado em um container

## comandos básicos

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


