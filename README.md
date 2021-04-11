<h2>Passo a passo com a MobilenetV2 utilizando TPUs no Google Colab</h2>

- Este repositório é destinado ao armazenamento de um passo a passo para utilizar a MobilenetV2 com a API de detecção de objetos do Tensorflow, rodando o treinamento utilizando as TPUs disponbilizadas no Google Colab.
- Está dividido em um diretório contendo o dataset utilizado no passo a passo e dois arquivos no formato do jupyter notebook(ipynb), onde um dos aquivos é destinado ao treinamento do modelo ```MobilenetV2_TPU_Train``` e o outro ao uso do modelo já treinado ```MobilenetV2_Predict```. 
- A escolha por separar em dois arquivos diferentes foi por poder utilizar os dois ao mesmo tempo em instâncias diferentes de maquinas oferecidas pelo Colab, visto que para realizar inferências utilizando o modelo treinado não é prático utilizando TPUs, assim acaba sendo necessário fazer a troca para o uso de GPU, o que acaba gerando uma perda das configurações já realizadas para o ambiente com uso de TPU.
- O dataset presente neste repositório pode ser encontrado nesse link https://www.kaggle.com/alvarole/asirra-cats-vs-dogs-object-detection-dataset e está disponível de forma pública. 
- Optei por disponibilizar o dataset aqui por ter separado de uma forma diferente os dados e por disponibilizar junto os arquivos de conversão de ```xml``` para ```csv``` e de ```csv``` para o formato ```record``` utilizado pelo Tensorflow, além disso também disponibilizei junto um txt contendo as classes dentro da pasta ```dataset/data```. Fora essa adições, nenhum rótulo ou imagem foi alterado, assim como não foram adicionadas imagens novas, mantendo a integridade original dos dados.

<h2>Passos iniciais</h2>

- Antes de mais nada, é necessário criar uma conta no ```Google Cloud```. Após criada a conta e estar logado na plataforma é necessário acessar o console do ```Google Cloud``` e criar um novo projeto, para isso é possível seguir este tutorial https://cloud.google.com/resource-manager/docs/creating-managing-projects. Com o projeto criado, vá no menu de navegação e selecione a opção de faturamento, fique tranquilo, para utilizar os ```buckets```, que é o que queremos, não é gerado nenhum custo, porém é necessário vincular uma conta de faturamento.
- Com a conta de faturamento já ativada, vamos criar um ```bucket```, para isso é possível seguir este tutorial disponibilizado pelo Google: https://cloud.google.com/storage/docs/creating-buckets.
> Obs: É essencial realizar a criação e uso de buckets, pois é necessário os arquivos que vamos utilizar estarem disponiveis em um bucket do google para podermos utilizar as TPUs no Colab.

- Outro passo importante é disponibilizar o dataset em algum local que permita acesso via google colab, por exemplo: github, bitbucket, google drive, etc. No arquivo ```MobilenetV2_TPU_Train``` existe uma sessão específica para conectar ao google drive, então, por facilidade, recomendo gerar um zip do dataset e upar para o seu drive.
- Também existe a possibilidade de upar o dataset diretamente para a instância do google colab, contudo o upload para o ```colab``` costuma ser mais lento e se, por alguma eventualidade, você for desconectado, pode acontecer de ter que fazer todo proccesso novamente.
- E há ainda uma terceira possibilidade, que seria gerar os arquivos ```record``` para o Tensorflow localmente e enviar estes para o seu bucket no google cloud juntamente com o arquivo contendo as classes disponível em ```dataset/data```.
- Fique a vontade para utilizar o método que preferir, porém aqui será abordada a opção com google drive.

<h2>Considerações extras</h2>

- Para permitir que a TPU do google colab tenha acesso ao seu bucket é necessário adicionar permissão para o membro ```allAuthenticatedUsers``` com papel de ```administrador do storage```, então é importante se certificar que este projeto/bucket seja exclusivamente para essa tarefa, evitando assim comprometer a segurança de outros projetos/buckets.

- Além disso, é importante lembrar de alterar o tipo do ambiente de execução no google colab ao carregar algum dos arquivos, sendo TPU para o arquivo ```MobilenetV2_TPU_Train``` e GPU para o arquivo ```MobilenetV2_Predict```.

- Outra questão importante é que mesmo se você tiver definido o nome de seu projeto e/ou bucket com letras em caixa alta, é importante passar os nomes em caixa baixa onde for requisitado dentro do aquivos do google colab.
<p align="center">
    <img src="https://user-images.githubusercontent.com/15859532/114289220-808f5080-9a4c-11eb-93da-6d9c1ef61fd5.png"/><br/>
    <em>Definição das variaveis de projeto e bucket</em>
</p>

