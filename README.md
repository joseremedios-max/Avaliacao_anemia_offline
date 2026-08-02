# 🩸 Sangue-Virtual-AI

Um pipeline de Inteligência Artificial de ponta a ponta (Edge-Cloud) projetado para a triagem não invasiva de anemia. O sistema utiliza a câmera de smartphones e modelos avançados de Visão Computacional e Deep Learning para estimar níveis de hemoglobina, operando com alta eficiência mesmo em cenários de conectividade restrita (áreas remotas).

---

## 📌 Visão Geral do Projeto

O **Sangue-Virtual-AI** propõe uma abordagem moderna para diagnósticos preliminares em campanhas de saúde móvel. Superando as limitações de análises estatísticas globais, o projeto evoluiu para o uso de **Vision Transformers (ViT)**, capazes de capturar a topologia espacial fina e padrões cromáticos sutis do tecido conjuntivo palpebral associados à palidez anêmica.

A arquitetura foi desenhada com foco em **Edge Computing**, garantindo inferência local em tempo real no dispositivo móvel, com sincronização offline-first para a nuvem.

---

## 🚀 Stack Tecnológica e Arquitetura

O ecossistema do projeto é dividido em camadas especializadas:

*   **Processamento e Calibração Óptica:** `OpenCV`, `NumPy`, `Scikit-Image`. Implementação de algoritmos de balanço de branco (*Gray World*) para mitigar vieses de iluminação (flash vs. luz ambiente).
*   **Detecção e Segmentação (Teacher-Student):** `SAM 2` (Segment Anything Model) para rotulação automática (Auto-Labeling) guiada por prompts, gerando *Bounding Boxes* para o treinamento cirúrgico de modelos leves como o `YOLOv11-Nano`.
*   **Deep Learning Core (Regressão):** `PyTorch` e `timm`. Substituição de classificadores densos tradicionais por um modelo **Vision Transformer** (`vit_tiny_patch16_224`) otimizado para regressão contínua de hemoglobina (g/dL).
*   **Edge Computing (Mobile):** Conversão de modelos via `ONNX` (Opset 14) rodando no motor `onnxruntime`, atingindo um tempo de inferência impressionante de **< 60ms**, com um *payload* final otimizado de **~21 MB**.
*   **Backend e Orquestração (Nuvem):** `FastAPI`, `Uvicorn` e validação via `Pydantic`. Gerenciamento de versionamento de dados com `DVC` e armazenamento no `AWS S3`, suportando sincronização assíncrona assim que a rede é restabelecida.

---

## 📊 Desempenho Clínico e Técnico Preliminar

*   **Alta Revocação (Recall):** Na fase de classificação inicial (PyTorch V2), o modelo atingiu **89% de recall** para a classe anêmica (F1-score de 0.85). Em triagens médicas, isso minimiza drasticamente os falsos negativos, garantindo segurança ao paciente.
*   **Eficiência de Hardware:** O pipeline nativo empacotado para o celular prova que não é necessária uma GPU em nuvem para o diagnóstico primário, viabilizando o uso em larga escala na saúde pública.

---

## 🛣️ Roadmap e Próximos Passos (Regulamentação)

Para evoluir de um protótipo avançado para validação em campo (foco em padrões ANVISA/FDA), os próximos marcos incluem:

1.  **Isolamento Estrito de ROI:** Aprimorar o pipeline *Gray World* aplicando a normalização cromática *apenas* na Região de Interesse segmentada, evitando distorções causadas pelo fundo da imagem.
2.  **Diversidade de Dataset:** Validar o modelo contra um espectro mais amplo de tons de pele (escala de Fitzpatrick) e artefatos gerados por lentes de smartphones de baixo custo.
3.  **Métricas de Erro Clínico:** Transição das métricas estatísticas clássicas de IA para o **Erro Absoluto Médio (MAE)** na regressão de hemoglobina (g/dL), comparando diretamente com o padrão-ouro (hemograma completo laboratorial).

---

## ⚠️ Aviso Legal (Disclaimer Médico)

Este projeto tem finalidade estritamente **educacional, experimental e de pesquisa em engenharia de software e IA**. 

A análise visual realizada por este sistema **NÃO substitui, de forma alguma, um diagnóstico médico profissional ou exames laboratoriais**. Os criadores e desenvolvedores deste projeto não assumem responsabilidade por decisões clínicas baseadas nas predições do algoritmo.

---

# 🩸 OcularAI: Triagem Não-Invasiva de Anemia via Visão Computacional e Edge Computing

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat&logo=pytorch&logoColor=white)
![ONNX](https://img.shields.io/badge/ONNX-005CED?style=flat&logo=onnx&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white)
![YOLO](https://img.shields.io/badge/YOLOv11-00FFFF?style=flat&logo=yolo&logoColor=black)

## 📌 Sobre o Projeto
Este projeto de **HealthTech** propõe um sistema fim a fim (End-to-End) para a triagem não-invasiva de anemia. Utilizando fotografias da conjuntiva palpebral (parte interna do olho), o sistema processa imagens ópticas e cruza esses dados com metadados clínicos (Idade e Sexo) para prever níveis de hemoglobina sem a necessidade de coleta de sangue.

O sistema foi desenhado para atuar em regiões remotas (offline) e sincronizar dados com a nuvem quando houver conexão, otimizando o fluxo de diagnósticos em locais com infraestrutura laboratorial limitada.

## 🚀 Arquitetura do Sistema

A solução foi dividida em 4 pilares principais:

1. **Computer Vision Automática (YOLOv11):** Segmentação e recorte cirúrgico automático da conjuntiva palpebral.
2. **Machine Learning Clássico (Random Forest / XGBoost):** Classificador primário de triagem com calibração de *threshold* focado na redução de falsos positivos (**Acurácia de ~81%**).
3. **Deep Learning no Edge (Vision Transformer - ViT):** Modelo PyTorch de regressão de hemoglobina, convertido e otimizado para o formato **ONNX**.
4. **Cloud API (FastAPI):** Servidor assíncrono para telemetria e recepção de exames realizados offline.

---

## 🔬 Principais Conquistas Técnicas

- **Edge Computing Extremo:** O modelo Vision Transformer (ViT) foi reduzido a um arquivo portátil de **21.21 MB**, capaz de rodar inferências em CPU (celulares comuns) em **37 milissegundos**.
- **Descoberta de Biomarcadores:** Análises de *Feature Importance* revelaram que a textura vascular (desvio padrão das cores) tem maior peso preditivo clínico do que a média de palidez geral.
- **Integração Clínico-Óptica:** O pipeline funde dados biométricos (RGB, HSV, YCbCr) com fisiológicos (One-Hot Encoding para Idade e Sexo).
- **Zero Dependência do PyTorch em Produção:** Inferência 100% nativa utilizando `onnxruntime` e `numpy`, garantindo compatibilidade com Android/iOS.

---

## 🛠️ Tecnologias Utilizadas

- **Visão Computacional:** OpenCV, MediaPipe, Ultralytics (YOLOv11).
- **Processamento de Dados:** Pandas, NumPy, Scikit-Learn.
- **Deep Learning:** PyTorch, TIMM (Torch Image Models).
- **Implantação (Deployment):** ONNX Runtime, FastAPI, Pydantic, Uvicorn.
- **Monitoramento/Versionamento:** DVC (Data Version Control).

---

## 📂 Estrutura do Repositório (Resumo)

```text
├── data/                  # Conjunto de dados e metadados clínicos
├── models/                # Modelos salvos (.pkl e .onnx)
│   ├── classificador_anemia_rf.pkl
│   └── vit_hemoglobina_mobile_final.onnx
├── src/
│   ├── etl_unificacao_dados.py    # Pipeline de limpeza e extração de cores
│   ├── train_rf_classifier.py     # Treinamento do Random Forest
│   ├── train_vit_pytorch.py       # Arquitetura e treino do Vision Transformer
│   ├── export_to_onnx.py          # Script de conversão PyTorch -> ONNX
│   ├── onnx_mobile_sim.py         # Simulador de inferência Edge (37ms)
│   └── api_nuvem.py               # Servidor FastAPI
└── README.md



## 👨‍💻 Autor e Contato

**[anonimo]**
Engenheiro de Software | Especialista em IA e Visão Computacional



Documento de Arquitetura de Software: Sistema Edge-AI para Diagnóstico de Anemia.

1. Resumo Executivo
Este documento descreve a arquitetura de um sistema inteligente de grau médico (Medical-Grade) focado no diagnóstico não invasivo de anemia infantil em áreas remotas. A solução elimina a dependência de conectividade constante, utilizando processamento Edge Computing em dispositivos móveis (smartphones) para inferência clínica instantânea, sincronizando os dados com a nuvem apenas quando há cobertura de rede.

2. Visão Geral da Arquitetura
O ecossistema é dividido em duas frentes independentes:

Módulo Offline (Edge): Aplicativo móvel responsável por capturar a imagem, isolar a região de interesse (pálpebra inferior) e calcular a taxa de hemoglobina em tempo real sem uso de internet.

Módulo Cloud (Nuvem): Backend assíncrono projetado para receber e armazenar os dados consolidados quando o dispositivo retorna a uma área com conectividade (Wi-Fi ou 5G).

3. Pipeline de Inteligência Artificial (Visão e Regressão)
O motor de diagnóstico foi concebido em uma arquitetura de múltiplos estágios para garantir precisão e leveza:

Estágio A: Pré-processamento e Normalização
Algoritmos de correção de luz (Gray World) e ajustes de Gamma são aplicados para padronizar as imagens capturadas, mitigando variações de iluminação ambiente (ex: luz solar direta vs. luz incandescente).

Estágio B: Auto-Labeling com Zero-Shot AI
Para a construção do dataset, o Segment Anything Model (SAM) foi utilizado em conjunto com injeção de coordenadas, automatizando a criação de máscaras de segmentação da conjuntiva palpebral, substituindo meses de anotação manual.

Estágio C: Extração Biométrica (YOLOv11)
O modelo ultraleve YOLOv11-Nano foi treinado para realizar a detecção instantânea da pálpebra no celular, extraindo apenas a região contendo a colorimetria capilar necessária para o diagnóstico.

Estágio D: Regressão Espectral (Vision Transformer - ViT)
Uma arquitetura ViT Tiny substitui as tradicionais redes convolucionais. Em vez de classificação, a última camada foi otimizada para regressão linear contínua, analisando a palidez do tecido e retornando o nível exato de hemoglobina (ex: 12.4 g/dL).

4. Engenharia de Deploy e Edge Computing
Para viabilizar a execução de redes neurais complexas em hardwares limitados (smartphones de baixo custo), o modelo PyTorch foi convertido para o formato ONNX (Open Neural Network Exchange).

Desafio de Compilação: Motores experimentais do PyTorch 2.x falharam na exportação de modelos baseados em Transformers.

Solução Implementada: Realizamos um bypass utilizando o compilador C++ legado (TorchScript e dynamo=False), garantindo a preservação total dos pesos

