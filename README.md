# rna-transfer-learning
Transfer Learning para Classificação de Imagens
Este projeto demonstra como usar transfer learning para treinar um modelo de rede neural e classificar novas categorias de imagens com alta precisão e rapidez.

O que é Transfer Learning?
Imagine que você está ensinando uma criança a identificar diferentes raças de cachorros. Ela já sabe o que é um cachorro, uma orelha, um rabo, etc. Você não precisa começar do zero explicando o que é um animal. Você simplesmente aponta para as características específicas de cada raça.

O transfer learning funciona da mesma forma. Em vez de treinar uma rede neural do zero, usamos um modelo pré-treinado em um conjunto de dados muito grande (como o ImageNet, que contém milhões de imagens de diferentes objetos, animais e cenários). Esse modelo já "aprendeu" a identificar bordas, formas, cores e texturas. Nós "transferimos" esse conhecimento e o adaptamos para o nosso novo problema (no nosso caso, a classificação de novas categorias de imagens).

Isso nos permite:

Economizar tempo de treinamento: O modelo já tem uma base sólida de conhecimento.

Obter alta precisão com poucos dados: O modelo já está "esperto" e precisa de menos exemplos para aprender as novas categorias.

Estrutura do Projeto
A estrutura de pastas para este projeto é organizada da seguinte forma:

.
├── dataset/
│   ├── train/
│   │   ├── categoria1/
│   │   │   ├── imagem1.jpg
│   │   │   ├── imagem2.jpg
│   │   │   └── ...
│   │   └── categoria2/
│   │       ├── imagem1.jpg
│   │       ├── imagem2.jpg
│   │       └── ...
└── README.md
A pasta dataset contém as imagens para treinamento.

Dentro de dataset, a pasta train contém subpastas, onde cada subpasta representa uma categoria de imagem a ser classificada.
