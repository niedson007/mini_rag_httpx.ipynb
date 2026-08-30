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
## 5. Decisões técnicas

### Divisão em chunks

A documentação do HTTPX foi dividida em pequenos trechos de texto
(chunks) para facilitar a recuperação das informações mais relevantes.

Ao final do processamento, foram gerados 277 chunks.

Cada chunk possui os seguintes metadados:

- texto
- arquivo
- chunk_id
- título

### Embeddings

Foi utilizado o modelo:

`sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`

Os embeddings transformam os textos em representações numéricas
(vetores). Isso permite comparar semanticamente a pergunta do usuário
com os conteúdos da documentação.

### Busca semântica

A busca utiliza similaridade de cosseno (cosine similarity) para
comparar o embedding da pergunta com os embeddings dos chunks.

O sistema recupera os 3 chunks com maior similaridade (`top_k=3`).

### Modelo generativo

Após recuperar os trechos mais relevantes, eles são enviados ao
Gemini como contexto.

O modelo foi instruído a responder utilizando apenas as informações
presentes na documentação recuperada.

Quando não há informação suficiente na documentação, o sistema é
instruído a informar que não encontrou informação suficiente.

### Fontes e scores

Além da resposta gerada, o sistema apresenta as fontes utilizadas,
mostrando:

- ranking do resultado;
- score de similaridade;
- arquivo de origem;
- título;
- identificador do chunk.

Isso permite verificar quais documentos foram recuperados para gerar
a resposta.

## 6. Perguntas de teste

Foram realizadas três perguntas para verificar o funcionamento do RAG.

### Teste 1 — Requisição GET

**Pergunta:**

> Como fazer uma requisição GET usando HTTPX?

**Resultado:**

O sistema recuperou trechos da documentação e o Gemini respondeu
explicando que é possível utilizar a função `httpx.get()`.

**Exemplo apresentado pelo sistema:**

```python
import httpx

r = httpx.get('https://httpbin.org/get')

## 7. Limitações

Apesar de o sistema funcionar corretamente nos testes realizados,
existem algumas limitações.

### Recuperação dos chunks

A busca utiliza similaridade de cosseno entre os embeddings e recupera
os 3 chunks com maior pontuação.

Em algumas perguntas, um trecho semanticamente semelhante pode aparecer
antes de um trecho que possui a informação mais diretamente relacionada
à pergunta.

Isso foi observado no teste sobre requisições GET, no qual
`docs/advanced/proxies.md` apareceu como primeiro resultado, enquanto
`docs/quickstart.md` continha um exemplo diretamente relacionado a
`httpx.get()`.

### Dependência da qualidade dos embeddings

A qualidade da recuperação depende do modelo de embeddings utilizado.
Se a pergunta e o conteúdo estiverem semanticamente distantes, o sistema
pode recuperar trechos pouco relevantes.

### Títulos dos documentos

Nem todos os chunks possuem um título identificado automaticamente.
Por isso, alguns resultados podem aparecer com o título "Sem título".

### Quantidade de resultados

O sistema utiliza `top_k=3`, recuperando somente os três resultados com
maior similaridade.

Isso torna o sistema simples e rápido, mas pode fazer com que informações
relevantes presentes em outros chunks não sejam utilizadas pelo modelo.

### Base de conhecimento

O Gemini é orientado a responder utilizando somente a documentação
recuperada.

Portanto, se a informação solicitada não estiver presente na
documentação, o sistema pode informar que não encontrou informação
suficiente, como ocorreu no teste sobre a Copa do Mundo de 2022.

### Ausência de banco vetorial

Esta versão utiliza os embeddings diretamente em memória e realiza a
comparação com os chunks disponíveis.

Uma versão mais avançada poderia utilizar um banco de dados vetorial
para armazenar e consultar os embeddings.
