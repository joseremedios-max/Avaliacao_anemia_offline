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
