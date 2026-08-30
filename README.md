# mini_rag_httpx.ipynb
projeto fabrica, sistema de RAG simples utilizando a documentação do HTTPX e Google Gemini.

# Mini RAG HTTPX

## 1. Objetivo

Este projeto apresenta a construção de um sistema simples de RAG
(Retrieval-Augmented Generation) utilizando a documentação do HTTPX
como base de conhecimento.

O sistema realiza a leitura dos documentos Markdown, divide o conteúdo
em chunks, transforma os textos em embeddings e utiliza busca semântica
para recuperar os trechos mais relevantes para uma pergunta.

Após a recuperação dos trechos, o modelo Gemini utiliza esses conteúdos
como contexto para gerar uma resposta.

O sistema também apresenta as fontes utilizadas, incluindo arquivo,
título, chunk e score de similaridade.

## 2. Arquitetura

O fluxo do sistema é:

Documentação HTTPX
↓
Chunks
↓
Embeddings
↓
Busca semântica
↓
3 melhores resultados
↓
Google Gemini
↓
Resposta + fontes

## 3. Tecnologias utilizadas

- Python
- Google Colab
- Sentence Transformers
- scikit-learn
- Google Gemini API
- GitHub
- Documentação oficial do HTTPX

### Modelo de embeddings

Foi utilizado o modelo:

`sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`

Esse modelo foi utilizado para transformar os textos dos chunks
e as perguntas do usuário em vetores, permitindo realizar a busca
por similaridade semântica.

### Modelo generativo

Foi utilizado o modelo:

`gemini-2.5-flash`

O Gemini recebe os trechos recuperados pelo mecanismo de busca
e gera a resposta final utilizando esses trechos como contexto.

## 4. Como executar

### 1. Abrir o notebook

O arquivo principal do projeto é:

`mini_rag_httpx.ipynb`

O notebook pode ser aberto utilizando o Google Colab.

### 2. Executar as células

Execute as células do notebook na ordem em que aparecem.

O projeto realiza as seguintes etapas:

1. Instalação das bibliotecas.
2. Obtenção da documentação do HTTPX.
3. Leitura dos documentos Markdown.
4. Divisão dos documentos em chunks.
5. Criação dos metadados.
6. Geração dos embeddings.
7. Busca semântica.
8. Integração com o Google Gemini.
9. Geração da resposta.
10. Apresentação das fontes e scores.

### 3. Configurar a API Key

Para utilizar o Gemini, é necessário configurar uma chave da API do Google AI.

No Google Colab, a chave deve ser armazenada nos Secrets com o nome:

`GEMINI_API_KEY`

A chave não deve ser publicada diretamente no código ou no repositório do GitHub.

### 4. Executar uma pergunta

Depois de executar as células anteriores, utilize a função:

```python
responder_rag("Como fazer uma requisição GET usando HTTPX?")
