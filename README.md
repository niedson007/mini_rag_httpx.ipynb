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
