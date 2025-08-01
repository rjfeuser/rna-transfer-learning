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

```python
# Importar bibliotecas necessárias
import tensorflow as tf
import os
import zipfile
import urllib.request

# Fazer o download do dataset de exemplo do GitHub
url = "[https://github.com/seu-usuario/seu-repositorio/raw/main/dataset.zip](https://github.com/seu-usuario/seu-repositorio/raw/main/dataset.zip)"
file_name = "dataset.zip"

print("Baixando o dataset...")
urllib.request.urlretrieve(url, file_name)
print("Download concluído!")

# Extrair os arquivos
with zipfile.ZipFile(file_name, 'r') as zip_ref:
    zip_ref.extractall('dataset')

# Configurar diretórios
train_dir = 'dataset/train'

# Pré-processar as imagens e criar geradores de dados
IMG_WIDTH = 224
IMG_HEIGHT = 224
BATCH_SIZE = 32

train_datagen = tf.keras.preprocessing.image.ImageDataGenerator(
    rescale=1./255,
    shear_range=0.2,
    zoom_range=0.2,
    horizontal_flip=True,
    validation_split=0.2  # Usar parte dos dados para validação
)

train_generator = train_datagen.flow_from_directory(
    train_dir,
    target_size=(IMG_WIDTH, IMG_HEIGHT),
    batch_size=BATCH_SIZE,
    class_mode='categorical',
    subset='training'
)

validation_generator = train_datagen.flow_from_directory(
    train_dir,
    target_size=(IMG_WIDTH, IMG_HEIGHT),
    batch_size=BATCH_SIZE,
    class_mode='categorical',
    subset='validation'
)

# Carregar o modelo pré-treinado (MobileNetV2)
base_model = tf.keras.applications.MobileNetV2(
    input_shape=(IMG_WIDTH, IMG_HEIGHT, 3),
    include_top=False,
    weights='imagenet'
)

# Congelar as camadas do modelo base para não serem treinadas
base_model.trainable = False

# Criar um novo modelo em cima do modelo base
model = tf.keras.models.Sequential([
    base_model,
    tf.keras.layers.GlobalAveragePooling2D(),
    tf.keras.layers.Dense(train_generator.num_classes, activation='softmax')
])

# Compilar o modelo
model.compile(
    optimizer='adam',
    loss='categorical_crossentropy',
    metrics=['accuracy']
)

# Exibir um resumo do modelo
model.summary()

# Treinar o modelo
epochs = 10
history = model.fit(
    train_generator,
    epochs=epochs,
    validation_data=validation_generator
)

# Avaliar o modelo
loss, accuracy = model.evaluate(validation_generator)
print(f"Acurácia final: {accuracy * 100:.2f}%")

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
