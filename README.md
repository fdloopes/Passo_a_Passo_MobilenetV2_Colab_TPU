<h2>Passo a passo com a MobilenetV2 utilizando TPUs no Google Colab</h2>

- Este repositório é destinado ao armazenamento de um passo a passo para utilizar a MobilenetV2 com a API de detecção de objetos do Tensorflow, aproveitando as TPUs disponibilizadas no Google Colab para o treinamento.

- Está dividido em um diretório contendo o dataset utilizado no passo a passo e dois arquivos no formato do jupyter notebook(ipynb), onde um dos aquivos é destinado ao treinamento do modelo ```(TPU_Train)``` e o outro ao uso do modelo já treinado ```(GPU_Predict)```. 

- A escolha por separar em dois arquivos diferentes foi por poder utilizar os dois ao mesmo tempo em instâncias diferentes de maquinas oferecidas pelo Colab, visto que realizar inferências utilizando o modelo treinado não é prático com TPUs, assim acaba sendo necessário fazer a troca para o uso de GPU, o que gera uma perda das configurações já realizadas para o ambiente com uso de TPU.

- O dataset presente neste repositório pode ser encontrado nesse link https://www.kaggle.com/alvarole/asirra-cats-vs-dogs-object-detection-dataset e está disponível de forma pública.

- Optei por disponibilizar o dataset aqui por ter organizado os dados de uma forma diferente e ter incluído os arquivos de conversão de ```xml``` para ```csv``` e de ```csv``` para ```record``` que é o formato utilizado pelo Tensorflow, além disso também disponibilizei junto um txt contendo as classes dentro da pasta ```dataset/data```. Fora essa adições, nenhum rótulo ou imagem foi alterado, assim como não foram adicionadas imagens novas, mantendo a integridade original dos dados.

<h2>Passos iniciais</h2>

- Antes de mais nada, é necessário criar uma conta no ```Google Cloud```. Após criada a conta e estar logado na plataforma é necessário acessar o console do ```Google Cloud``` e criar um novo projeto, para isso é possível seguir este tutorial https://cloud.google.com/resource-manager/docs/creating-managing-projects. Com o projeto criado, vá no menu de navegação e selecione a opção de faturamento, fique tranquilo, para utilizar os ```buckets```, que é o que queremos, não é gerado nenhum custo, porém é necessário vincular uma conta de faturamento para poder utiliza-los.

- Com a conta de faturamento já ativada, vamos criar um ```bucket```, para isso é possível seguir este tutorial disponibilizado pelo Google: https://cloud.google.com/storage/docs/creating-buckets.
> Obs: É essencial realizar a criação e uso de buckets, pois é necessário os arquivos estarem disponiveis em um bucket do Google Cloud para podermos utilizar as TPUs no Colab.

- Outro passo importante é disponibilizar o dataset em algum local que permita acesso via Google Colab, por exemplo: github, bitbucket, google drive, etc. No arquivo ```TPU_Train``` existe uma sessão específica para conectar ao google drive, então, por facilidade, recomendo gerar um zip do dataset e upar para o seu drive.

- Também existe a possibilidade de upar o dataset diretamente para a instância do google colab, contudo o upload para o ```Colab``` costuma ser mais lento e se, por alguma eventualidade, você for desconectado, pode acontecer de ter que fazer todo processo novamente.

- E há ainda uma terceira possibilidade, que seria gerar os arquivos ```record``` para o Tensorflow localmente e enviar estes para o seu bucket no google cloud juntamente com o arquivo contendo as classes disponível em ```dataset/data```.
- Fique a vontade para utilizar o método que preferir, porém nos arquivos será abordada a opção com google drive.

<h2>Considerações extras</h2>

- Para permitir que a TPU do google colab tenha acesso ao seu bucket é necessário adicionar permissão para o membro ```allAuthenticatedUsers``` com papel de ```administrador do storage```, então é importante se certificar que este projeto/bucket seja exclusivamente para essa tarefa, evitando assim comprometer a segurança de outros projetos/buckets.

- Além disso, é importante lembrar de alterar o ```tipo do ambiente de execução``` no google colab ao carregar algum dos arquivos, sendo TPU para o arquivo ```TPU_Train``` e GPU para o arquivo ```GPU_Predict```. Para isso basta ir no menu ```Ambiente de execução/Alterar o tipo de ambiente de execução```.

- Outra questão importante é que mesmo se você tiver definido o nome de seu projeto e/ou bucket com letras em caixa alta, é importante passar os nomes em caixa baixa onde for requisitado dentro dos arquivos.

<p align="center">
    <img src="https://user-images.githubusercontent.com/15859532/114289220-808f5080-9a4c-11eb-93da-6d9c1ef61fd5.png"/><br/>
    <em>Definição das variaveis de projeto e bucket</em>
</p>

- No caso, se esse fosse o nome do seu projeto e bucket ficaria assim:
> PROJECT = "your_project_gcs_here" e YOUR_GCS_BUCKET = "your_gcs_bucket_here"

- Durante o primeiro treinamento será utilizado os pesos obtidos com o treinamento sobre o dataset ```COCO```, em muitos casos utilizar os pesos de um pre treinamento para um modelo agiliza muito a conversão sobre novos dados, que é o caso aqui.

- Contudo, o modelo e os pesos disponibilizados estão salvos como um checkpoint de classificação e não de detecção, de maneira que para o primeiro treinamento é necessário definir ```fine_tune_checkpoint_type = "classification"```. Após esse primeiro treinamento, caso queira seguir a treinar, é necessário alterar para ```fine_tune_checkpoint_type = "detection"```, pois os novos pesos serão salvos dessa maneira. 

- Além disso, lembre de alterar a variavel ```fine_tune_checkpoint``` para apontar para o último checkpoint salvo em seu bucket.
 
<p align="center">
    <img src="https://user-images.githubusercontent.com/15859532/114343158-b78c6180-9b33-11eb-8de7-5e392b6dfaf3.png"/><br/>
    <em>Parâmetros de treinamento</em>
</p>

<h2>Resultados esperados</h2>

- Se você conseguiu chegar até aqui e conseguiu executar todas as sessões do arquivo ```TPU_Train``` então provavelmente terá resultados semelhantes aos seguintes.

<p align="center">
    <img src="https://user-images.githubusercontent.com/15859532/114345116-83b33b00-9b37-11eb-9300-284b3a5f0e3e.png"/><br/>
    <em>Loss Total do treinamento</em>
</p><br/>

- Na imagem acima temos em azul escuro a Loss de treinamento próximo a 0,2 e em azul claro temos a Loss de avaliação com valor próximo a 0,65.

<p align="center">
    <img src="https://user-images.githubusercontent.com/15859532/114346772-5b790b80-9b3a-11eb-940c-a931f4dfd7af.png"/><br/>
    <em>Avaliação lado a lado. A esquerda inferência do modelo e a direita imagem rotulada</em>
</p>

- Possivelmente o tensorboard selecionará para você a mesma imagem que está acima para avaliar seus resultados.

- Se estiver satisfeito com os resultados alcançados pode utilizar o arquivo ```GPU_Predict``` para exportar seu modelo treinado e realizar inferências em imagens diferentes que tiver.

<p align="center">
    <img src="https://user-images.githubusercontent.com/15859532/114348567-1bffee80-9b3d-11eb-9ba7-9f67aa6ce67e.png"/><br/>
    <em>Inferência em uma imagem com um gato</em>
</p>

<p align="center">
    <img src="https://user-images.githubusercontent.com/15859532/114348570-1dc9b200-9b3d-11eb-9448-62b1998ca9bd.png"/><br/>
    <em>Inferência em uma imagem com um cachorro</em>
</p>

<h2>Final</h2>

- Este repositório deve passar por atualizações rotineiras e seguirá sempre de forma pública.

- Espero que com este passo a passo consiga facilitar o uso de TPUs no Colab para mais pessoas, pois apesar de o uso de TPUs no Colab possuir bastante documentação, se trata de uma tecnologia relativamente nova e quando necessitei utilizar tive algumas dificuldades por conta de erros pouco expostos ainda pela comunidade. Dito isso, não hesite de entrar em contato pra tirar dúvidas, relatar algum erro ou até mesmo para trocar conhecimento. 
