# Developer Guide

![Roadmap](.gitbook/assets/plano_comlegenda%20%281%29.png)

### Instalação

1. instalar nativescript \(docs.nativescript.org\)
2. instalar dependências com `npm install` no root do projecto\(não esquecer que temos um registo interno npm [https://npm.dev.mysns.pt/](https://npm.dev.mysns.pt/)\)
3. usar nativescript para executar o projecto

### WebPack

Isto torna toda a aplicação muito mais rápida

Para construir com antecedência, deve executar comandos npm \(ver o projeto package.json\)

ex:`npm run build-android-bundle`

**Nota 1:** ISTO VAI MUDAR NA PRÓXIMA VERSÃO DE NATIVECRIPT \(3.4\)

**Nota 2:** \* para o cartão com pdf\(Testamento vital\) temos que especificar algumas entradas no webpack config\(`webpack.config.js` da linha 166 a 171\)

**Nota 3:** É necessário validar se é preciso atualizar dependências regularmente \(sempre que possivel, **diariamente**\)

### Git,Testes, Continuous Integration

## Git

O workflow do repositorio de git é muito simples, é composto por duas branches principais, a `master` tem a última versão que esta nas stores, a `development` é a proxima iteração da aplicação.

Cada feature que é implementada ou bug corrigido tem uma branch nova com um nome sugestivo em relação ao conteudo da mesma.

No final da implementação o developer submete um pull request que postriormente tem de ser aprovado por outro membro da equipa, a cada pull request a Continuous Integration corre testes unitarios e testes e2e!

Ao fazer uma release a branch development dá merge com a master e cria-se uma tag com a versao que foi released

## Testes

O project tem suporte a testes unitarios, há ja algum testes feitos \(app/tests\) o suficiente para enterder como se fazem o resto dos testes

Tem tambem testes end-to-end, a documentaçao da ferramenta usada\(plugin `nativescript-appium`\), e basicamente deve-se mimicar o comportamento do utilizador ao usar a aplicaçao, isto nao esta de momento implementado porqur o backend ainda nao suporta na totalidade um ambiente de qualidade/devenvolvimento i.e. nao ha utilizador de demos com todos os cartões. O plugin tem tambem a funcionalidade de fazer `screenshot diffing`

Executar e2e tests

```text
npm run e2e
```

Executar Unit tests

```text
test <Platform>
```

## CI

Neste momento pode ser usada a ci do VSTS, o mac mini tem um [agente](https://github.com/Microsoft/vsts-agent)\(script executado na consola\) que se liga ao vsts, apartir dai quando ha um pull request pode ser especificado no vsts o que correr, neste caso queremos correr os testes unitarios e e2e, nao esquecer de ter dois emuladores, ou dispositivos ligados para os testes correrem nos mesmoS

### Releasing process

#### iOS:

Para fazer release para iOS é necessária uma conta itunes e executar `npm run publish-ios-bundle`

#### Android:

* _build_ com webpack\(`npm run build-android-bundle --release --key-store-path ./my-release-key.keystore --key-store-password 916265505 --key-store-alias MySNSstore --key-store-alias-password 916265505`\)
* isto assinará automaticamente \(_auto sign_\) o apk e agora precisa fazer upload para a playstore console

## Notas Prioritárias

* Alguns dos plugins estão no nosso registro npm interno porque precisavam de modificações
* O plugin `nativescript-hook-debug-production` não funciona bem \(não deteta o debug no modo de lançamento, mas recebemos um _pull request_ para corrigir. Esperar pela nova versão do nativescript porque precisamos ter o Webpack a trabalhar com ele\)
* há muitas coisas que podem ser otimizadas para o ngrx
* a dependência `hoek` serve para usar a mesma engine de validação de campos\(comunicaçao cliente Servidor\) que os packages `@ces-sec/*`, internamente temos uma versão\(4.1.99\) publicada no resgistro interno de npm, o ambiente de Nativescript nao suporta algumas primitivas de node, então o package tem apenas retiradas algumas linhas que não eram necessárias
* nativescript `advanced-webview-interface` também possui uma versão interna \(internal release, pois o autor está inativo\)
* Vendor.ts precisa de algumas otimizações, agora existem algumas bibliotecas repetidas nos diferentes módulos do aplicativo \([https://docs.nativescript.org/best-practices/bundling-with-webpack](https://docs.nativescript.org/best-practices/bundling-with-webpack)\)
* ngrx e componentes normais precisam de testes
* Testes e2e precisam de uma conta demo do backend

**Cartão Atividade Fisica / Medições de Saúde**

* o plugin `nativescript-health-data` também tem um _pull request_ pendente para retornar dados uniformes \(foi libertada internamente uma versão com as mudanças desse _pull request_\)
* estamos a usar uma versão interna do circle plugin na **atividade fisica** por causa de um _pull request_ que fizemos. Enquanto isso o autor lançou uma versão mais recente \(então isso precisa ser atualizado\)

Plugin `nativescript-health-data` - [https://github.com/filipemendes1994/nativescript-health-data](https://github.com/filipemendes1994/nativescript-health-data) Se tiverem problemas, abram um issue no github.

