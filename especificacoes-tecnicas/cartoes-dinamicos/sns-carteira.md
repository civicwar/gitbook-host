# SNS Carteira

## Conceito Base

A aplicação SNS Carteira, será maioritariamente uma aplicação _'Burra',_ ou seja, irá tratar principalmente da autenticação do utilizador, demonstração de conteúdos e como _frontend para os cartões dinâmicos._

A aplicação terá também tratar da parte _client _relativamente à cache local de todos os cartões de que faça download.

Será definido pela SPMS, qual o número máximo de cartões que possam estar instanciados na aplicação ao mesmo tempo — 10 max — de modo que não haja uma grande necessidade de cache e processamento pela parte do telemóvel.

## Funcionalidades da aplicação

* `MySNS Carteira` — Aplicação móvel que terá as seguintes funcionalidades:
  * Autenticação
    * RNU \[API\] — Autenticação através da Rede Nacional do Utente 
      * Nº telemóvel
      * Data de Nascimento
      * Nº Utente
    * CMD \[AMA\] — Autenticação através da Chave Móvel Digital
  * Conteúdo
    * Área do Cidadão \[link\]
    * Sobre
    * MySNS \[link - AppStore\]
    * MySNS Tempos \[link - AppStore\]
  * Definições
    * Envio de Telemetria
    * Terminar Sessão
  * Cartões dinâmicos
    * Cartões Disponíveis — Lista com todos os cartões disponíveis na rede, divididos em duas categorias:
      * SNS — Cartões desenvolvidos pela SPMS
      * Outros — _possibilidade_ de pesquisar
    * Carteira — Carteira de cartões selecionados, onde cada cartão terá
      * sumário informativo do cartão — _modo cartão de carteira _
      * Informação completa do cartão — _ver mais_

## Processo de instanciação de um novo cartão

Um utilizador da aplicação SNS carteira, quando quiser — desde que esteja ligado à rede — poderá selecionar um cartão que necessite. E este será o processo por baixo:

1. Ao entrar na lista de cartões disponíveis
   1. Download do ficheiro all-metadata.json
   2. Validar all-metadata.json com um json-schema
   3. all-metadata.json, contém todos os cartões disponíveis de momento na plataforma de cartões dinâmicos. \( ver estrutura \)



## Estrutura JSON do all-metadata.json



