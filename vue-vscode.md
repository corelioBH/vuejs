# Usando Vue no código do Visual Estúdio

O [Vue.js](https://vuejs.org/) é uma biblioteca JavaScript popular para a construção de interfaces de usuário de aplicativos web e de código Visual Studio tem suporte embutido para os blocos de construção Vue.js de [HTML](https://code.visualstudio.com/docs/languages/html) , [CSS](https://code.visualstudio.com/docs/languages/css) e [JavaScript](https://code.visualstudio.com/docs/languages/javascript) . 

Para um ambiente mais rico desenvolvimento do Vue.js, você pode instalar a extensão [Vetur](https://marketplace.visualstudio.com/items?itemName=octref.vetur) no VSCode que suporta Vue.js IntelliSense, trechos de código, formatação e muito mais.

------

![bem-vindo ao Vue](https://code.visualstudio.com/assets/docs/nodejs/vuejs/welcome-to-vue.png)

------

## Bem-vindo ao Vue

Vamos usar o [Vue CLI](https://cli.vuejs.org/) para este tutorial. 

Para instalar e usar o Vue CLI, bem como executar o servidor de aplicativos Vue, você vai precisar do [Node.js](https://nodejs.org/) e do  [npm](https://www.npmjs.com/) (o gerenciador de pacotes Node.js) instalado. 

> **Dica** : Para testar se você tem Node.js e npm corretamente instalado em sua máquina, você pode digitar `node --version`e `npm --version`.

Para instalar o `vue/cli` em um terminal ou prompt de comando digite:

```
npm install -g @vue/cli
```

Isso pode demorar alguns minutos para instalar. Agora você pode criar um novo aplicativo Vue.js digitando:

```
vue create my-app
```

onde `my-app`é o nome da pasta para a sua aplicação. Você irá selecionar uma predefinição e você pode manter o padrão `(babel, eslint)`, que usará [Babel](https://babeljs.io/) para transpile o JavaScript para o navegador ES5 compatível e instalar o [ESLint](http://eslint.org/) linter para detectar erros de codificação. Pode demorar alguns minutos para criar o aplicativo Vue e instalar suas dependências.

Vamos executar rapidamente a nossa aplicação Vue ao navegar para a nova pasta e digitando `npm run serve`para iniciar o servidor web e abrir o aplicativo em um navegador:

```
cd my-app
npm run serve
```

Você deverá ver "Wlecome to your Vue.js App" em [http: // localhost: 8080](http://localhost:8080/) no seu browser. Você pode pressionar Ctrl + C para parar o `vue-cli-service`server.

Para abrir o aplicativo Vue no Código VS, a partir de um terminal (ou prompt de comando), navegue até a `my-app`pasta e tipo `code .`:

```
cd my-app
code .
```

Código VS vai lançar e apresentar a sua candidatura Vue no File Explorer.

## Extensão VSCode

Agora abra a `src`pasta e selecione o `App.vue`arquivo. Você vai notar que o Código VS não mostra qualquer destaque de sintaxe e trata o arquivo como **texto simples** como você pode ver na barra de status inferior direito. Você também verá uma notificação recomendando a [Vetur](https://marketplace.visualstudio.com/items?itemName=octref.vetur) extensão para o `.vue`tipo de arquivo.

![vetur recomendação extensão](https://code.visualstudio.com/assets/docs/nodejs/vuejs/vetur-extension-recommendation.png)

A extensão suprimentos Vetur Vue.js recursos de linguagem (destaque de sintaxe, IntelliSense, trechos, formatação) para Código VS.

![extensão temporada](https://code.visualstudio.com/assets/docs/nodejs/vuejs/vetur-extension.png)

A partir da notificação, pressione **Instalar** para baixar e instalar a extensão Vetur. Você deverá ver a extensão Vetur  no modo de exibição de extensões. Quando a instalação estiver completa (pode demorar alguns minutos), você será solicitado a atualizar o VSCode com a extensão habilitado.

Agora você deve ver que `.vue`é um tipo de arquivo reconhecido para o idioma Vue e você tem recursos de linguagem, como destaque de sintaxe, correspondente suporte e descrições pairar.

![recursos de linguagem vue](https://code.visualstudio.com/assets/docs/nodejs/vuejs/vue-language-features.png)

## IntelliSense

Como você começar a digitar `App.vue`, você verá sugestões inteligentes ou conclusões, tanto para HTML e CSS, mas também para Vue.js itens específicos como declarações ( `v-bind`, `v-for`) na seção de templates do Vue:

![sugestões Vue.js](https://code.visualstudio.com/assets/docs/nodejs/vuejs/suggestions.png)

Você também terá acesso a propriedades Vue ( `methods`, `computed`) na seção de `scripts`:

![sugestões Vue.js JavaScript](https://code.visualstudio.com/assets/docs/nodejs/vuejs/javascript-suggestions.png)

## Olá Mundo!

Vamos atualizar o aplicativo de exemplo para "Olá mundo!". Em `App.vue`substituir o componente HelloWorld `msg`atributo personalizado texto com "Olá mundo!".

```
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <HelloWorld msg="Hello World!"/>
  </div>
</template>
```

Uma vez que você salvar o `App.vue`arquivo ( ⌘S ), reinicie o servidor com `npm run serve`e você verá "Olá mundo!". Deixe o servidor em execução, enquanto nós vamos para aprender como depurar uma aplicação Vue.js .

> **Dica** : O código VS suporta Auto Save, que por padrão salva seus arquivos após um atraso. Verifique o **Auto Save** opção no **arquivo** menu para ligar Auto Save ou diretamente configurar o `files.autoSave`usuário [ajuste](https://code.visualstudio.com/docs/getstarted/settings) .

------

![Olá Mundo](https://code.visualstudio.com/assets/docs/nodejs/vuejs/hello-world.png)

------

## Linting

Linters analisar o seu código fonte e pode avisá-lo sobre possíveis problemas antes de executar o aplicativo. Os Vue ESLint plug-in ( [eslint-plugin-vue](https://www.npmjs.com/package/eslint-plugin-vue) ) verifica erros de sintaxe específica Vue.js que são mostrados no editor como squigglies vermelho e também são exibidas no painel de **Problemas** ( **Exibir** > **Problemas** ⇧⌘M ).

Abaixo você pode ver um erro quando o linter Vue detecta mais de um elemento raiz em um modelo:

![Ver linting](https://code.visualstudio.com/assets/docs/nodejs/vuejs/vue-linting.png)

## Depuração

Você pode depurar código Vue.js no lado do cliente com o [depurador para o Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome). Você pode aprender mais com a [depuração Vue.js no Chrome e Código VS](https://github.com/Microsoft/vscode-recipes/tree/master/vuejs-cli)  [aqui](https://github.com/Microsoft/vscode-recipes) site.

> Nota: Atualmente problemas com o sourcemaps gerados por vue-cli, que causam problemas com a experiência de depuração no código VS. Veja https://github.com/vuejs/vue-loader/issues/1163 .

Outra ferramenta popular para depurar Vue.js é o [vue-devtools](https://github.com/vuejs/vue-devtools) plug-in.

## Outras extensões

Vetur é apenas uma das muitas extensões Vue.js disponíveis para Código VS. Você pode pesquisar na vista Extensions ( ⇧⌘X ) digitando 'vue'.

![extensões Vue.js](https://code.visualstudio.com/assets/docs/nodejs/vuejs/vue-extensions.png)

Existem também pacotes de extensão que empacotam extensões que outras pessoas têm encontrado útil para o desenvolvimento Vue.js.

![pacote de extensão Vue.js](https://code.visualstudio.com/assets/docs/nodejs/vuejs/vue-extension-pack.png)

Pronto. Agora você está minimamente habilitado para instalar e rodar aplicações Vue.JS no VSCode.