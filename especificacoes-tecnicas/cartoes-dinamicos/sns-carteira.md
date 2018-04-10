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

Um utilizador da aplicação SNS carteira, quando quiser — desde que esteja ligado à rede — poderá selecionar um cartão que necessite. 

1. Ao entrar na lista de cartões disponíveis
   1. Download do ficheiro `all-metadata.json`
   2. Validar `all-metadata.json` com um json-schema
   3. `all-metadata.json`, contém todos os cartões disponíveis de momento na plataforma de cartões dinâmicos. \( ver [estrutura](sns-carteira.md#estrutura-json-do-all-metadata.json) \)
2. Ao seleccionar um cartão para adicionar
   1. Verificar existência do cartão na cache local
      1. **Se não existir**, ou se a versão do cartão local for anterior à verificada no  `all-metadata.json`, será feito o download do cartão.
   2. Verificar os checksum dos cartões com o que vem no `all-metadata.json` 
      1. **Se não coincidir**, informar o utilizador, perguntar se deve fazer novamente download do cartão e informar a plataforma da CeS desta situação.
   3. Adicionar o cartão na carteira, sendo neste caso utilizado o `min-template.js` para o cartão pequeno, no ato de instanciação, será chamado a função `onInstall(API)` que irá informar a plataforma que houve uma instalação desse cartão, servindo ao mesmo tempo, para validar se o cartão foi instalado como deve ser.
3. Ao seleccionar o _Ver Mais_  num cartão
   1. Instanciar a versão full do cartão, utilizando `full-template.js`, e no ato de instanciação, será chamado a função `onInstall(API)` que irá informar a plataforma que houve uma instalação desse cartão, servindo ao mesmo tempo, para validar se o cartão foi instalado como deve ser.



## Estrutura JSON do all-metadata.json

```javascript
{
  "$schema": "http://json-schema.org/draft-06/schema#",
  "title": "AllCardsMetadata",
  "description": "List of All Cards available for CeS-App",
  "type": "object",
  "properties": {
    "version": {
      "description": "Version of the Metadata file",
      "type": "string"
    },
    "cards": {
      "minItems": 1,
      "description": "Available Cards",
      "type": "array",
      "items": {
        "type": "object",
        "required": ["version", "title", "description", "color", "hasNext"],
        "properties": {
          "version": {
            "description": "Version of the Card",
            "type": "string"
          },
          "checksum_android": {
            "description": "CheckSum Value of the Card Package -- Android",
            "type": "string"
          },
          "checksum_ios": {
            "description": "CheckSum Value of the Card Package -- iOS",
            "type": "string"
          },
          "title": {
            "description": "Title of the Card",
            "type": "string"
          },
          "description": {
            "description": "Short Description of the Card",
            "type": "string"
          },
          "color": {
            "description": "Color of the Card",
            "type": "string"
          },
          "hasNext": {
            "description": "Checks if has Next Step",
            "type": "boolean"
          },
          "onNext": {
            "description": "Method to ask on Next Step",
            "type": "string"
          }
        }
      }
    }
  },
  "required": ["version", "cards"]
}

```

