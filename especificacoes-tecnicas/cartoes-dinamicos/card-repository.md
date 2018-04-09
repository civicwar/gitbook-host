---
description: Repositório de Cartões dinâmicos
---

# Card Repository

## Conceito Base

Este repositório, terá todos os cartões dinâmicos suportados pela plataforma, na sua forma compilada, após passar pelos processos de teste e validação.

## Estrutura do Repositório

Cada cartão terá **duas** versões, **inerentes do mesmo código fonte**, mas com estrutura adaptada para **iOS** ou para **Android**.

Utilizando como exemplo o cartão da PEM, o repositório terá as seguintes entradas:

* `pem_v1.0.0_android.zip`
* `pem_v1.0.0_ios.zip`

Quando um utilizador escolher um cartão na lista de cartões disponíveis, o SNS Carteira irá fazer download do pacote referente ao tipo de sistema operativo, onde está a ser corrido a aplicação.

## Estrutura dos Pacotes Compilados

Um pacote de cartões dinâmicos terá como estrutura:

#### Android & iOS

* `metadata.json`
* `min-template.js`
* `full-template.js`
* `resources`
  * `image1.png`
  * `image1@2x.png`
  * `image1@3x.png`
  * `...`

Deste modo, na submissão de um cartão, a entidade interessada poderá desenvolver assets diferentes para cada tipo de plataforma.



## Estrutura do Código de Submissão

// TODO!!!

