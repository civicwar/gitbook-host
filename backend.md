# Backend - Serviços

#### Arquitectura {#arq}

Nesta aplicação utilizou-se o **Docker Swarm** \(ver a secção [Serviços](#Serviços) para mais informações\) no **Azure Container Service**. O Docker Swarm, utilizando a API do Docker nativa, fornece um ambiente para a implementação de cargas de trabalho de conteúdo através de um conjunto agrupado de anfitriões de Docker.

![Diagrama](/images/diagrama.png)

No diagrama acima, é possivel identificar duas secções no Container Service: **Core** e **Integrações**  
A aplicação comunica diretamente com o Reverse Proxy CeS. Cada serviço é executado num contentor Docker. Todos os contentores Docker são independentes \(possuindo cada um o seu Dockerfile\), pelo que a inoperabilidade de um não afeta os restantes.  
A orquestração é feita segundo um ficheiro de configuração – `docker-compose.yaml`.  

**Contentores Docker:**
* **Autenticação RNU** - responsável pela validação dos dados. Se forem validos, responde OK e envia uma sms com codigo TOTP pelo serviço da SPMS e faz um novo reply com o codigo TOTP (2-step authentication).
* **Autenticação CMD** - redirecciona o cidadão para a pagina [Autenticação.GOV](https://www.autenticacao.gov.pt). Depois de o cidadão se autenticar, é gerada uma chave.
* **Dispatcher** - toda a comunicação da CeS, excepto Telemetria e Autenticação. No futuro, irá fazer a avaliação de Requests e Replies.
* **Notificações** -
* **Telemetria** - dados estatísticos \(quantas pessoas iniciaram sessão, descarregaram  cartões, etc\)
* **Logging** - auditorias e logs

As integrações são os **serviços externos**,  que facilitam o acesso aos servidores SPMS e ADSE. Para tal, criou-se uma interface para cada serviço acedido \(RNU, PEM, ADSE\). Cada serviço é executado num contentor Docker próprio e isolado. É criada uma rede virtual no Docker para isolar os contentores de integração dos de serviços Core.

No **Core** do Azure Container Service, encontram-se os serviços que comunicam com a base de dados **DocumentDB**, responsável pelo armazenamento de _logs_ encriptados  para diagnóstico de problemas de funcionamento da aplicação, telemetria, associação \(encriptada\) do dispositivo ao cidadão e ainda metadatas de cartões, notificações e dispatcher. Estes dados não são armazenados no dispositivo móvel, mas sim da base de dados de backend \(DocumentDB\).

**Dados armazenados na DocumentDB**

* **PublicKeys NSNS Collection** - 
* **Dispatcher Metadate Collection** - 
* **CartõesMetadata Collection** - Apenas guarda dados de _Look and Feel_ - cor, logo etc, não armazenando quaisquer dados pessoais
* **NotificaçõesMetadata Collection** 
* **Telemetria Collection** - dados estatísticos 
* **LogAudition Collection** - auditorias e logs

---

## Serviços e Tecnologias {#servicos}

![Arquitetura CeS](images/servicos.png)

### Operations support systems \(OSS\)

* [Node.js](#nodejs)
* [Hapi.js](#happyjs)
* [PM2](#pm2)
* [Docker](#docker)
* [DocumentDB / MongoDB](#docDB)
* [NGINX](#nginx)
* [NPM modules](#npm) 

#### Node.js {#nodejs}

É um runtime de Javascript Server-side. Permite trabalhar na mesma linguagem e ambiente no frontend e backend \(equipas mais compactas\).  
É amplamente suportado por muitas empresas comerciais e Node.js Foundation \(Linux Foundation\), tendo a maior comunidade no GitHub  
É utilizado por: PayPal, Netflix, LinkedIn, Uber, IBM

* [mais informações](https://nodejs.org/en/)
* [Github](https://github.com/nodejs)

#### Hapi.Js {#happyjs}

Web and services application framework - framework de Node.js para desenvolver serviços baseado na configuração. Criado no Wallmart Labs para responder às necessidade do Wallmart.  
É utilizado por: PayPal, Disney, GOV.UK, mozzila, Yahoo, etc

* [mais informações](https://hapijs.com/)
* [github - Hapi.Js + TypeScript](https://github.com/dwyl/hapi-typescript-example)
* [Biblioteca Joi](https://github.com/hapijs/joi) - Object schema description language and validator for JavaScript objects.

##### Plugins Hapi.js criados:

**Autenticação**

* auth-ces-auth
* auth-ces-comm
* auth-ces-dispatcher
* auth-ces-other

**Resposta**

* reply-ces-auth
* reply-ces-comm
* reply-ces-handler

#### PM2 {#pm2}

PM2 é um Process Manager de Node.js para aplicações em produção, com balanceador de carga incorporado.  
PM2 é um gerenciador de processos de produção para aplicativos Node.js com load balancer incorporado. Facilita  as tarefas comuns do administrador do sistema, tais como: criar Cluster aplicacional, Workflows de deployment, Gestão de Logs e Monitorização  
É utilizado por: PayPal, Microsoft, IBM, etc

* [mais informações](http://pm2.keymetrics.io/)
* [Documentação](http://pm2.keymetrics.io/docs/usage/cluster-mode/)
* [Github](https://github.com/Unitech/pm2)

#### Docker {#docker}

Docker é um projeto open-source que permite criar um contentor \(container\) com a infraestrutura que se pretender por forma a poder partilha-la em qualquer máquina e manter sempre o mesmo comportamento. Este contentor contem um sistema operativo Linux lightweight, com o tradicional sistema de ficheiros e politicas de segurança que caracterizam o sistema operativo Linux, utilizando assim menos recursos de memória e espaço em disco. Em suma, permite compartimentalizar serviços e aplicações em qualquer tecnologia e executar em qualquer ambiente. É suportado pelos maiores fornecedores cloud \(Amazon, Microsoft, Google\).

É utilizado por: PayPal, BBC, eBay, GE, Spotify, ING, etc

* [mais informações](https://www.docker.com/)
* [Documentação](http://pm2.keymetrics.io/docs/usage/cluster-mode/)
* [Github](https://github.com/Unitech/pm2)

#### DocumentDB / MongoDB {#docDB}

**MongoDB**  
Base de Dados NoSQL open-source e cross-platform, sem esquemas \(_schemaless_\), orientado a documentos. Tem um bom suporte de JavaScript e uma grandde comunidade.  
A informação é guardada no formato JSON.  
É utilizado por: Forbes, BOSCH, Facebook, GOV.UK, McAfee, etc.

* [Documentação](https://docs.mongodb.com/)

**Azure DocumentDB**  
Base de Dados semelhante ao MongoDB, mas gerida na Cloud. Pertence à Microsoft Azure platform.

* [Documentação](https://docs.microsoft.com/pt-pt/azure/documentdb/)

#### NGINX {#nginx}

NGINX é um servidor HTTP e reverse proxy open-source e de alta performance. Utilizado na CeS como reverse proxy, possui uma boa comunidade, com suporte comercial.  
É utilizado por: Netflix, Hulu, GitHub, Airbnb, WorkPress, Eventbrite, etc

* [Mais Informações](https://www.nginx.com/)
* [Documentação](https://www.nginx.com/resources/wiki/)

#### NPM modules {#npm}

Node Package Manager. Todos os módulos criados para as aplicações MySNS podem ser consultados aqui:

* [MySNS - Available Packages](https://npm.dev.mysns.pt/)

---

### Segurança - Bibliotecas e Tools {#seg}

De modo a facilitar o entendimento da CeS, optou-se por descrever algumas bibliotecas e ferramentas.

#### Azure Key Vault:

O cofre de chave do Azure ajuda a salvaguardar as chaves criptográficas e os segredos utilizados pelas aplicações em nuvem e pelos serviços.

* [mais informações](https://docs.microsoft.com/pt-pt/azure/key-vault/key-vault-whatis)
* [npm](https://www.npmjs.com/package/azure-keyvault)
* [API docs](http://azure.github.io/azure-sdk-for-node/azure-keyvault/latest/)

#### \(RSA\) jsrsasign

'jsrsasign' \(RSA-Sign JavaScript Library\) é uma biblioteca criptográfica opensouce que suporta RSA/RSAPSS/ECDSA/DSA signing/validation, ASN.1, PKCS\#1/5/8 private/public key, X.509 certificate, CRL, CMS SignedData, TimeStamp & CAdES e JSON Web Signature\(JWS\)/Token\(JWT\)/Key\(JWK\)

* [site](http://kjur.github.io/jsrsasign/)
* [npm](https://www.npmjs.com/package/jsrsasign)
* [API docs](https://kjur.github.io/jsrsasign/api/index.html)

* certificados CA:

  * [JS Certification Authority](https://kjur.github.io/jsrsasign/tool_ca.html)

#### Crypt AES - Crypto-js

Crypto-js é uma biblioteca JavaScript de padrões de criptografia.

* [github](https://github.com/brix/crypto-js)
* [npm](https://www.npmjs.com/package/crypto-js)

#### Validação de JSON

* [site](http://json-schema.org)
* [npm](https://www.npmjs.com/package/jsonschema)
* [conversão de tipos typescript e JSON Schema - github](https://github.com/YousefED/typescript-json-schema)

#### TOTP

Para a gestão de passwords, utilizou-se o One-Time Password manager. Compativel com  HOTP \(counter based one time passwords\) e TOTP \(time based one time passwords\). Suporta autenticação de dois fatores.

* [npm](https://www.npmjs.com/package/otp.js)

#### JSON Web Key \(JWK\) - para Storage

* [Docs](https://tools.ietf.org/html/rfc7515)

#### JSON Web Signature \(JWS\) - Payload JWE

* [Docs](https://tools.ietf.org/html/rfc7515)

#### JSON Web Encryption \(JWE\) - Payload MessageAction\(CeS\)

* [Docs](https://tools.ietf.org/html/rfc7516)



