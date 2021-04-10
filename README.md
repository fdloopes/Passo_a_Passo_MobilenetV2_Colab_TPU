<h1>Passo a passo com MobilenetV2 utilizando TPUs no Google Colab</h1>

- Este repositório é destinado ao armazenamento de um passo a passo para utilizar a MobilenetV2 com a API de detecção de objetos do Tensorflow, rodando o treinamento utilizando as TPUs disponbilizadas no Google Colab.
- Está dividido em um diretório contendo o dataset utilizado no passo a passo e dois arquivos no formato do jupyter notebook(ipynb), onde um dos aquivos é destinado ao treinamento do modelo ```MobilenetV2_TPU_Train``` e o outro ao uso do modelo já treinado ```MobilenetV2_Predict```. 
- A escolha por separar em dois arquivos diferentes foi por poder utilizar os dois ao mesmo tempo em instâncias diferentes de maquinas oferecidas pelo Colab, visto que para realizar inferências utilizando o modelo treinado não é prático utilizando TPUs, assim acaba sendo necessário fazer a troca para o uso de GPU, o que acaba gerando uma perda das configurações já realizadas para o ambiente com uso de TPU.
- O dataset presente neste repositório pode ser encontrado nesse link https://www.kaggle.com/alvarole/asirra-cats-vs-dogs-object-detection-dataset e está disponível de forma pública. 
- Optei por disponibilizar o dataset aqui por ter separado de uma forma diferente os dados e por disponibilizar junto os arquivos de conversão de ```xml``` para ```csv``` e de ```csv``` para o formato ```record``` utilizado pelo Tensorflow, além disso também disponibilizei junto um txt contendo o labelmap dentro da pasta ```dataset/data```. Fora essa adições, nenhum rótulo ou imagem foi alterado, assim como não foram adicionadas imagens novas, mantendo a integridade original dos dados.

<h2>Exercícios de lógica de 1 a 10</h2>

- Possui os exercicios E.1 até E.10 propostos na primeira parte da lista de desafios.
- Implementação realizada no codesandbox utilizando o template fornecido.
- Link: https://codesandbox.io/s/teste-estagio-template-forked-p6uk9
- As soluções e prints são exibidos através do console.log(), de maneira que para visualizar o resultado é necessário abrir o console no navegador.

<h2>Desafio Back-end</h2>

- Possui a solução para o desafio back-end, implementação de uma API em node.js utilizando o padrão RESTful.
- Para implementação da API foi utilizado banco MySQL e as seguintes bibliotecas: Express, Knex, Objection e Nodemon.
- Inicialmente para colocar em execução a API é necessário editar o arquivo ```src/database/config.js```, nele estão as configurações de conexão ao banco de dados.
  
  ```javascript
  database: 'base_nave',
  user: 'user',
  password: '123456'
  ```
No trecho acima estão as entradas que necessitam de configuração de acordo com o ambiente, sendo que o database informado necessita ser criado previamente e passado seu nome nesse campo. Os campos user e password são as credenciais de acesso ao banco.<br/>
Em seguida, é possível utilizar os scripts definidos no arquivo ```package.json``` para utilizar a migration(criação das tabelas) e a seed(inserção de dados nas tabelas).<br/>
  - Comandos para execução dos scripts:
    ```
    npm run migrate:latest
    npm run seed
    ```
Com as tabelas criadas podemos colocar a API em execução através do comando: `npm run dev`.<br/>
> OBS¹: A API está definida para responder as solicitações na porta 8080, ou seja, localhost:8080 ou 127.0.0.1:8080, a porta pode ser editada dentro do arquivo ´server.js´.<br/>
- Dentro da pasta API_NodeJs existe um arquivo exportado dos testes realizados no Insomnia, de maneira que é necessário apenas importar este arquivo para facilitar os testes as rotas.
> OBS²: Por utilizar o tipo UUID para chave primária não é possível deixar setado os id's nas rotas que necessitam informar, assim, é necessário alterar esses campos conforme os UUID's gerados ao criar os dados.<br/>

<h3>Dificuldades</h3>

Ao longo do desenvolvimento da API encontrei dificuldade em conseguir implementar as rotas de Store utilizando os relacionamentos fornecidos pela biblioteca Objection. Ao tentar utilizar a função insertGraph(), a qual, teoricamente, insere dados na tabela relacionada ao Model e ao relacionamento previamente implementado, reparei que o ID, por exemplo, do naver criado, não estava sendo retornado, sendo assim impossivel a função realizar o insert na tabela de relacionamento. Após pesquisar em fóruns relacionados a biblioteca, descobri que na verdade o problema é com o banco de dados MySQL que não possui suporte a função returning(), assim não retornava a entrada inserida na tabela, assim como o campo ID.<br/>
Para contornar esse empecilho necessitei utilizar um novo select() após inserir o naver, buscando pela última inserção na tabela, para conseguir recuperar o ID e poder inserir na tabela de relacionamento com os projetos que este participa.<br/>
Acredito que a forma que contornei o problema não é a ideal, afinal a biblioteca oferece uma forma mais otimizada de realizar esta operação. Contudo, foi a primeira vez que utilizei as bibliotecas Knex e Objection juntamente com o banco MySQL, e não encontrei outra forma de contornar o problema. De qualquer maneira, em uma futura implementação usarei o PostgreSQL.<br/>

<h2>Exercícios de Banco de Dados</h2>

- Exercicios E.B.1 até E.B.5 propostos como desafio bônus.
- As soluções são scripts sql, de maneira que para executa-los é necessário utilizar um banco de dados, para os desafios utilizei o MySQL.
> - Obs: para facilitar é interessante abrir um terminal para acessar o BD de dentro do diretório onde estão os scripts.
- Após estar logado no ambiente(MySQL) basta utilizar o comando: `source "script a executar"`.
- Importante: O script E.B.1.sql é o de criação do BD e das tabelas, de maneira que deve ser o primeiro a ser executado, seguido do script E.B.2.sql que é o responsável por popular as tabelas.
