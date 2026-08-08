🩸 Laboratorio_Verifica_Anemia (Edge-AI)
Este é um aplicativo de triagem médica experimental desenvolvido em Flutter. Ele utiliza Visão Computacional e Inteligência Artificial (Edge-AI) rodando diretamente no celular (offline) para estimar os níveis de hemoglobina do paciente através de uma foto da conjuntiva palpebral.

O projeto foi construído com foco em Offline-First, permitindo o diagnóstico em áreas remotas, mas conta com sincronização inteligente na nuvem (FastAPI) para envio de dados e telemetria (GPS e Bateria) quando há conexão disponível.

🚀 Principais Funcionalidades
Diagnóstico Offline (Edge-AI): Processamento do modelo Vision Transformer (ViT) via onnxruntime direto no hardware do dispositivo, sem depender de internet.

Validação de Qualidade de Imagem: Algoritmo de segurança que analisa a contagem de pixels avermelhados, impedindo a geração de laudos a partir de fotos incorretas ou sem iluminação adequada.

Guia de Enquadramento: Interface de câmera customizada com máscara de recorte oval para auxiliar o usuário na captura perfeita da pálpebra.

Classificação OMS: Cálculo automático do status de anemia (Normal, Leve, Moderada, Grave) cruzando a hemoglobina estimada com a idade e o sexo do paciente, seguindo as diretrizes da Organização Mundial da Saúde.

Banco de Dados Local: Salvamento do histórico de exames no próprio aparelho utilizando shared_preferences.

Sincronização Nuvem & Telemetria: Coleta de dados do GPS e nível de bateria do celular, enviando o payload completo via HTTP POST para um servidor FastAPI no momento do diagnóstico.

🛠️ Tecnologias Utilizadas
Linguagem & Framework: Dart & Flutter (SDK >=3.0.0)

IA & Visão Computacional: ONNX Runtime (onnxruntime), Processamento de Imagem (image)

Integração Nativa: Controle de Câmera (camera), Permissões (permission_handler)

Telemetria: GPS (geolocator), Monitoramento de Hardware (battery_plus)

Comunicação & Armazenamento: HTTP/REST (http), Armazenamento Chave-Valor (shared_preferences)

📱 Como Executar o Projeto
1. Clone o repositório:

Bash
git clone https://github.com/joseremedios-max/app_verifica_anemia.git
2. Acesse a pasta do projeto:

Bash
cd app_verifica_anemia
3. Baixe as dependências:

Bash
flutter pub get
4. Execute o aplicativo (conecte um aparelho físico ou emulador):

Bash
flutter run
⚙️ Configuração de Nuvem (Opcional)
Para testar a sincronização remota, altere o IP no arquivo correspondente à tela da câmera (_syncWithCloud). O servidor esperado é uma API em Python/FastAPI configurada para receber requisições POST com a estrutura JSON correspondente.

⚠️ Aviso Legal (Disclaimer)
Este laboratorio de aplicao tem caráter estritamente educacional, experimental e de pesquisa em engenharia de software e IA. O teste de visão computacional serve apenas como triagem preliminar e não substitui, sob nenhuma hipótese, exames de sangue laboratoriais (hemograma) ou a avaliação de um profissional de saúde qualificado. Em caso de suspeita de anemia, procure orientação médica.

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

**[autonomo]**
Engenheiro de Software | Especialista estrategico  em IA e Visão Computacional


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

