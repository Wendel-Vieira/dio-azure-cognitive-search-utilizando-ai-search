# ☁️ Azure Cognitive Search: Indexação e Consulta de Dados

Este repositório contém o passo a passo prático documentando a minha experiência com a configuração e exploração do **Azure AI Search** (antigo Azure Cognitive Search), baseado no laboratório oficial da Microsoft e desenvolvido como parte do desafio de projeto da **DIO (Digital Innovation One)**.

## 🎯 Objetivo do Laboratório

O objetivo principal deste laboratório é aplicar técnicas de **organização e pesquisa de documentos** por meio da ingestão de dados e indexação utilizando inteligência artificial na nuvem do Azure.

### Metas Atingidas:
- ✅ **Ingestão de Dados**: Extração de conhecimento a partir de fontes de dados.
- ✅ **Criação de Índice**: Estruturação de um índice inteligente para busca rápida e refinada.
- ✅ **Exploração Prática**: Testes de busca utilizando queries para minerar e extrair informações relevantes de grandes volumes de documentos.

---

## 🛠️ Tecnologias e Serviços Utilizados

- **[Azure AI Search](https://azure.microsoft.com/en-us/products/ai-services/ai-search/)**: Serviço de pesquisa em nuvem com recursos integrados de inteligência artificial.
- **Azure Cognitive Services**: Conjunto de APIs de IA para enriquecimento do conhecimento (análise de sentimentos, extração de frases-chave, reconhecimento ótico de caracteres - OCR).
- **Azure Blob Storage**: Armazenamento em nuvem usado como fonte dos documentos brutos (ex: arquivos PDF, DOCX, imagens).
- **Portal do Azure**: Interface gráfica para provisionamento e gerenciamento dos recursos.

---

## 🚀 Passo a Passo (Como foi feito)

### Passo 1: Provisionamento dos Recursos
Para começar, foi necessário criar os recursos fundamentais no Portal do Azure:
1. **Azure AI Search**: Provisionado na camada (tier) Basic para habilitar a pesquisa cognitiva.
2. **Azure AI Services (Multi-service account)**: Necessário para fornecer as habilidades cognitivas (skills) durante a fase de ingestão, permitindo que a IA leia e entenda textos em imagens ou extraia entidades.
3. **Storage Account**: Provisionamento de um contêiner (Blob) contendo os dados brutos de exemplo fornecidos pelo laboratório da Microsoft (ex: reviews de clientes de uma rede de cafés).

### Passo 2: O Processo de Ingestão e Indexação (Import Data)
No painel do Azure AI Search, utilizamos o assistente **"Import data"** para conectar a fonte de dados (Blob Storage) ao nosso índice de pesquisa. 

O fluxo executado foi:
- **Data Source**: Conectamos ao Azure Blob Storage onde os documentos de texto e imagens estavam armazenados.
- **Add Cognitive Skills**: Habilitamos o enriquecimento cognitivo. Configuramos para extrair pessoas, organizações, localizações e frases-chave (key phrases). Também ativamos o OCR (Optical Character Recognition) para extrair texto de imagens.
- **Customize Target Index**: Definimos a estrutura do índice. Marcamos quais campos seriam `Retrievable` (recuperáveis na resposta), `Filterable` (filtráveis), `Sortable` (ordenáveis), `Facetable` (categorizáveis) e `Searchable` (pesquisáveis em texto livre).
- **Create an Indexer**: O indexador foi agendado para rodar uma única vez (`Once`), varrendo o storage, aplicando as skills cognitivas e populando o índice.

### Passo 3: Exploração e Busca de Dados (Search Explorer)
Com o índice populado e status de "Success", utilizamos a ferramenta **Search Explorer** no portal do Azure para realizar consultas em formato JSON.

#### Exemplos de Queries Realizadas:

1. **Busca Simples (Pesquisa em todo o texto):**
   ```json
   {
     "search": "coffee",
     "count": true
   }
   ```
   *Retornou todos os documentos que mencionam café, além do count total de hits.*

2. **Filtro Específico (Filtrando por Sentimento):**
   ```json
   {
     "search": "*",
     "filter": "sentiment eq 'negative'"
   }
   ```
   *Retornou documentos (reviews) que a Inteligência Artificial classificou com sentimento negativo.*

3. **Facetamento (Categorização):**
   ```json
   {
     "search": "*",
     "facets": ["locations"]
   }
   ```
   *Agrupou os resultados extraindo as cidades/locais mais mencionados nos documentos, útil para dashboards analíticos.*

---

## 🧠 Insights e Aprendizados

- O **enriquecimento cognitivo (Cognitive Skills)** é a parte mais poderosa do processo. Ao invés de apenas indexar o texto bruto, o Azure automaticamente aplicou modelos de NLP (Processamento de Linguagem Natural) para entender sentimentos e entidades, o que normalmente exigiria a construção de pipelines complexos de Machine Learning.
- A configuração correta dos atributos de campo (`Searchable` vs `Filterable`) impacta diretamente na performance e no tamanho do índice. É uma decisão arquitetural crucial.
- O Azure AI Search atua de forma excelente como motor de busca back-end para sistemas de Retrieval-Augmented Generation (RAG) em aplicações modernas de IA Generativa.

---
## 👨‍💻 Autor

- **Wendel Vieira**
- Desafio prático elaborado para a Formação e Bootcamp da DIO.
