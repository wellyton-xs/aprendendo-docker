# Passo-a-passo com docker e nodejs

nesse artigo iremos criar ver um step by step de como criar um ambiente docker para rodar uma aplicação simples com nodejs

## requisitos:

para esse exemplo usaremos:

* yarn - gerenciador de pacotes
* docker - gerador de containers
* typescript - linguagem (superset de javascript com tipagem)
* express - biblioteca para criar rotas do servidor
* cors - dependência, sempre baixo sei que dá problema se não tiver mas não faço ideia do que essa jossa faz

opcionais (eu usei mas você não precisa se não quiser):

* vscode (visual studio code)
* ESlint - força padrões e boas práticas de código
* Prettier - formata o código automaticamente e pode ser integrado com ESlint

## Iniciando o projeto

se estiver no yarn execute:

    yarn init (-y opcional, não faz perguntas e da yes em tudo) 

crie a pasta <src> e o arquivo main.ts que é onde será criada a aplicação de exemplo

baixe as dependências de desenvolvimento:

    yarn add -D typescript tsx @types/node @types/express tsup

agora as dependências do projeto:

    yarn add express cors

gere o arquivo de configuração do typescript:

    yarn tsx --init

por fim configure alguns scripts no seu package.json:

    "scripts": {
        "start": "tsx src/main.ts",
        "build": "tsup src/"
    },

## Criando aplicação de exemplo

no main.ts faça as importações: 

    import express from "express"
    import { Request, Response } from "express"

crie as instancias da aplicação:

    const app = express()
    const port = 3000

crie a rota:

    app.get("/", (req: Request, res: Response) => {
        res.send("olá docker :) ")
    })

faça o express utilizar a porta que criamos:

    app.listen(port, () => {
        console.log(`app rodando na porta: http://localhost:${port}`)
    })

para testar a aplicação localmente rode:

    yarn start

com isso, se você acessar "http://localhost:3000" deve ver a mensagem olá docker escrita em uma singela página web.

## compilando e preparando projeto para criação de uma imagem

para criar uma imagem e por fim um container, é necessário compilar/transpilar o código Typescript para Javascript. O Typescript não é entendido pelo node, que é utilizado na hora de desenvolver para que possamos criar códigos mais seguros e mais fáceis de debugar graças a tipagem. Portanto iremos utilizar o `tsx` que irá compilar esse código e colocá-lo em uma pasta chamada dist, a qual iremos passar para dentro do container.

para isso execute no terminal: 

    yarn build

agora vou pedir que apague a pasta node_modules, as dependências essenciais para a execução da aplicação como o express serão baixadas dentro do container. As dependências ficam isoladas e não instaladas diretamente na nossa máquina, assim podemos distribuir essa imagem para vários membros de nossa equipe e as dependências serão as mesmas, não teremos um "nossa, na minha máquina funcionou normal" quando um bug aparecer.

## Criando arquivo Dockerfile

O Dockerfile não é nenhum bicho de sete cabeças, basta criar um arquivo com esse nome na raiz do seu projeto. O docker tem uma linguagem própria para criar as instruções de como a sua imagem deve ser criada a qual é bem simples.

para começar devemos saber que na maioria das vezes iremos usar uma imagem já existente como base para a nossa, no nosso caso atual iremos usar o Node.js como base.

para isso utilizamos o comando `FROM`

ex:

    FROM node

agora que já definimos a base vamos definir um diretório de trabalho dentro do container para executar a aplicação.


    WORKDIR /usr/src/app

vamos copiar o package.json para o diretório de trabalho para concluir a configuração e instalação das dependências 

    COPY package.json .

logo abaixo utilizamos o `RUN` para rodar o yarn e instalar as dependências:

    RUN yarn

o próximo passo é copiar o código fonte em Javascript que compilamos anteriormente para esse diretório de trabalho que criamos:

    COPY dist/ ./dist

temos que expor uma porta lógica para podermos acessar e associar com uma porta do nosso computador e então acessar a aplicação no localhost.

    EXPOSE 3000

e por fim rodar o projeto.

    CMD ["node", "./dist/main.js"]

## construindo a imagem

para construir a imagem, iremos abrir um terminal no diretório do nosso projeto e rodar:

    docker build . -t <tag_que_quiser_dar_a_imagem>

o "." serve para dizer que nesse diretório raiz temos um dockerfile e é para o docker utilizar ele para gerar a imagem.

## rodando um container e testando:

para rodar o container iremos utilizar o seguinte comando:

    docker run -d -p 3000:3000 --name <nome_para_o_container> <nome_que_você_deu_a_imagem>

esse comando parece extenso, mas nas anotações e no resumo explicamos cada parâmetro.

basicamente:

    -d: roda em background

    -p: define uma porta para expor

    3000:3000 - significa que estamos associando a porta lógica 3000 (do container) com a porta 3000 (do nosso sistema)

    --name: nomeia o container