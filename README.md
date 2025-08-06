# Transfer Learning para Classificação de Imagens

Este projeto demonstra como usar **transfer learning** para treinar um modelo de rede neural e classificar novas categorias de imagens com alta precisão e rapidez.

## O que é Transfer Learning?

Imagine que você está ensinando uma criança a identificar diferentes raças de cachorros. Ela já sabe o que é um cachorro, uma orelha, um rabo, etc. Você não precisa começar do zero explicando o que é um animal. Você simplesmente aponta para as características específicas de cada raça.

O **transfer learning** funciona da mesma forma. Em vez de treinar uma rede neural do zero, usamos um modelo pré-treinado em um conjunto de dados muito grande (como o **ImageNet**, que contém milhões de imagens de diferentes objetos, animais e cenários). Esse modelo já "aprendeu" a identificar bordas, formas, cores e texturas. Nós "transferimos" esse conhecimento e o adaptamos para o nosso novo problema (no nosso caso, a classificação de novas categorias de imagens).

Isso nos permite:

* **Economizar tempo de treinamento:** O modelo já tem uma base sólida de conhecimento.
* **Obter alta precisão com poucos dados:** O modelo já está "esperto" e precisa de menos exemplos para aprender as novas categorias.

## Estrutura do Projeto

A estrutura de pastas para este projeto é organizada da seguinte forma:

<img width="461" height="330" alt="image" src="https://github.com/user-attachments/assets/1198cbe1-3436-49a5-a253-d8424fcc030e" />



* A pasta `dataset` contém as imagens para treinamento.
* Dentro de `dataset`, a pasta `train` contém subpastas, onde cada subpasta representa uma categoria de imagem a ser classificada.

## Treinamento e Transfer Learning

Você pode executar o código abaixo diretamente no **Google Colab**. Ele fará o download de um conjunto de dados de exemplo do GitHub, configurará o ambiente e executará o treinamento.


### Como o código funciona:

1.  **Baixa e extrai as imagens:**
        O código acessa o seu repositório no GitHub para baixar o arquivo zip com as imagens.
2.  **Pré-processamento:**
        Ele utiliza o `ImageDataGenerator` para carregar as imagens, redimensioná-las e aplicar técnicas de **data augmentation** (aumentar artificialmente o número de imagens com rotações, zoom, etc.).
3.  **Carrega o modelo base:**
        O modelo `MobileNetV2` é carregado com os pesos pré-treinados do ImageNet.
4.  **Congela as camadas:**
        As camadas do `MobileNetV2` são congeladas, ou seja, elas não serão alteradas durante o treinamento. Isso preserva o conhecimento que o modelo já tem.
5.  **Adiciona novas camadas:**
        Uma nova camada de saída é adicionada ao final do modelo. Essa camada é responsável por aprender a classificar as novas categorias de imagens.
6.  **Treina o modelo:**
        Apenas a nova camada de saída é treinada, o que é muito mais rápido e eficiente.


## Projeto para redução de dimensionalidade em imagens (Binarização)
**Como o Código Funciona**
Este notebook demonstra a binarização de imagens usando um modelo de machine learning simples. O processo é dividido em quatro etapas principais:

**Instalação e Importação:** O código garante que as bibliotecas necessárias (opencv-python e scikit-learn) estejam instaladas no ambiente do Google Colab e as importa para o script.

**Carregamento da Imagem:** Uma imagem em tons de cinza é carregada para ser processada. No Colab, o script usa wget para baixar uma imagem de uma URL, mas você pode facilmente adaptá-lo para usar uma imagem que você mesmo fez o upload.

**Preparação dos Dados de Treinamento:** Para treinar um modelo de machine learning, precisamos de um conjunto de dados de **"verdade"** (ground truth). Neste caso, o código cria esses dados de forma simples:

**Features (X):** Os valores de cada pixel da imagem original.

**Labels (y):** Os valores binários (0 ou 1) que o modelo deve aprender a prever. Estes são gerados inicialmente aplicando um limiar fixo.

**Treinamento e Aplicação do Modelo:** Um classificador k-NN (k-Nearest Neighbors), um dos modelos de machine learning mais simples e eficazes, é treinado com os dados de treino. Após o treinamento, o modelo é usado para prever a binarização na imagem completa.
