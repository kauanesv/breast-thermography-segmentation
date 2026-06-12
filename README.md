# 🌡️ Segmentação Automática em Termografia Médica (Mamas)

## 📋 Sobre o Projeto
Este repositório contém o código desenvolvido para um projeto de **Iniciação Científica (IC)** focado no processamento e análise de imagens de **Termografia Médica**. 

O objetivo principal deste algoritmo é realizar a **segmentação automática das mamas** a partir de matrizes de temperatura (arquivos `.txt`). A pipeline foi construída para atuar de forma autônoma, isolando a região de interesse (ROI), removendo ruídos, detectando limites anatômicos (como o sulco inframamário) e separando o tronco/abdômen da região mamária utilizando técnicas avançadas de Visão Computacional.

O dataset de origem utilizado para os testes faz parte dos bancos de dados da UFF (Universidade Federal Fluminense).

## 🛠️ Tecnologias e Bibliotecas Utilizadas
O projeto foi desenvolvido em **Python** (via Google Colab/Jupyter Notebook) e faz uso intensivo das seguintes bibliotecas:
* **OpenCV (`cv2`)**: Filtros morfológicos, detecção de bordas (Canny) e operações lógicas em imagens binárias.
* **Scikit-Image (`skimage`)**: Limiarização de Otsu e morfologia matemática.
* **SciPy / NumPy**: Manipulação das matrizes de temperatura, cálculos espaciais, aproximação de parábolas e localização de picos.
* **Pandas**: Leitura e estruturação dos dados brutos dos sensores térmicos.
* **Matplotlib / Seaborn**: Visualização dos resultados etapa por etapa.

## ⚙️ Arquitetura da Pipeline
A função principal `pipeline(image_np)` orquestra o processamento da imagem do início ao fim. O fluxo de dados segue estas etapas:

1. **Pré-Processamento**: Aplicação de Filtro Gaussiano (15x15) para suavização e conversão térmica, gerando a primeira máscara do corpo.
2. **Crop Automático**: Corte dinâmico focado no tórax utilizando análise de projeções de histograma.
3. **Limiarização Adaptativa**: Uso do método de Otsu para isolar o corpo do fundo de forma refinada.
4. **Cálculo do Eixo de Simetria**: Algoritmo para separação matemática das mamas esquerda e direita (`centro_x`).
5. **Clusterização K-Means**: Isolamento térmico focado nas regiões de maior temperatura (Cluster 5).
6. **Morfologia e Tratamento de Ruídos**: Erosão, conexão de clusters, verificação espacial e uso do Filtro de Canny para refinar bordas isoladas.
7. **Modelagem Anatômica (Sulco)**: Aproximação matemática (parábola) para prever e desenhar o limite inferior das mamas.
8. **Preenchimento de Máscaras**: Varredura horizontal e vertical inteligente para fechar os contornos gerados.
9. **Remoção Abdominal**: Operações lógicas (NAND mask) para apagar a região do abdômen, restando apenas o tecido mamário segmentado.

## 🚀 Como Executar

### Pré-requisitos
Certifique-se de ter as bibliotecas instaladas em seu ambiente:
```bash
pip install numpy pandas opencv-python scikit-image matplotlib scipy seaborn
