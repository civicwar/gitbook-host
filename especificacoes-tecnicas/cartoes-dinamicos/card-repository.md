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



## JSON Schema:metadata.json

```javascript
{
  "$schema": "http://json-schema.org/draft-06/schema#",
  "title": "CardMetadata",
  "description": "Card Information, Required for the ",
  "type": "object",
  "definitions": {
    "osTypes": {
      "enum": ["ios", "android"]
    },
    "sourceTypes": {
      "enum": ["sns", "other"]
    },
    "categoryTypes": {
      "enum": ["pem", "clinic", "sns"]
    },
    "contactTypes": {
      "enum": ["email", "phone", "mobile", "skype", "slack"]
    }
  },
  "properties": {
    "version": {
      "description": "Version of the Metadata file",
      "type": "string"
    },
    "id": {
      "type": "string",
      "description": "Id used to get the API Key for the Setup, UUID"
    },
    "checksum-min": {
      "description": "CheckSum of the Small Card Template file",
      "type": "string"
    },
    "checksum-full": {
      "description": "CheckSum of the Full Card Template file",
      "type": "string"
    },
    "os": {
      "description": "Target Mobile OS",
      "type": "string",
      "oneOf": [{ "$ref": "#/definitions/osTypes" }]
    },
    "source": {
      "type": "string",
      "description": "Type of Entity that developed the card",
      "oneOf": [{ "$ref": "#/definitions/sourceTypes" }]
    },
    "category": {
      "type": "array",
      "description": "Category in which the card fits",
      "items": {
        "type": "string",
        "description": "Category Name",
        "oneOf": [{ "$ref": "#/definitions/categoryTypes" }]
      }
    },
    "title": {
      "type": "string",
      "description": "Title of the Card"
    },
    "description": {
      "type": "string",
      "description": "Description of the Card"
    },
    "developer": {
      "type": "object",
      "description": "Information about the Entity Responsable for Maintaining the Card",
      "required": ["name", "address", "country", "contact"],
      "properties": {
        "name": {
          "type": "string",
          "description": "Name of the Entity"
        },
        "address": {
          "type": "string",
          "description": "Address of the Entity"
        },
        "country": {
          "type": "string",
          "description": "Country of the Entity"
        },
        "contact": {
          "type": "array",
          "minItems": 1,
          "items": {
            "type": "object",
            "description": "Information to Contact the Entity",
            "required": ["type", "value"],
            "properties": {
              "type": {
                "type": "string",
                "description": "Type of Contact",
                "oneOf": [{ "$ref": "#/definitions/contactTypes" }]
              },
              "value": {
                "type": "string"
              }
            }
          }
        }
      }
    }
  },
  "required": [
    "version",
    "checksum-min",
    "checksum-full",
    "os",
    "source",
    "category",
    "title",
    "description",
    "developer"
  ]
}

```

