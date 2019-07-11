#### Consumo de Informações da API da Marvel com o Vue, Material Design e o NPM.

A partir de agora, usaremos bibliotecas que estão no [NPM](https://www.npmjs.com/) (Gerenciador de Pacotes Node). Sendo assim, teremos acesso a diversos pacotes prontos para adicionar em nosso projeto.

Vamos criar a aplicação rodando os seguintes comandos:

```shell
$ npm install --global vue-cli
# Vamos criar um projeto preparado para o "webpack"
# Obs: Aceite todas as sugestões ao ser perguntado (vue-router, eslint, etc)
$ vue init webpack nome-projeto
# Entre no projeto, instale as dependências e execute-o:
$ cd nome-projeto
$ npm install
$ npm run dev
```

Pronto, estamos agora com o nosso ambiente de desenvolvimento funcionando!

#### A aplicação

Nesse ano de grandes lançamentos de filmes vamos criar uma aplicação que irá consumir uma API da Marvel e listar alguns quadrinhos.

Veja como conseguir sua **API Key no link abaixo:**

[**Getting started**
*Marvel.com is the source for Marvel comics, digital comics, comic strips, and more featuring Iron Man, Spider-Man…*developer.marvel.com](https://developer.marvel.com/documentation/getting_started)

[**Marvel Developer Portal - Interactive Documentation**
*The Marvel developer portal gives Marvel fans, partners and other technologists access to an array of powerful APIs…*developer.marvel.com](https://developer.marvel.com/docs)

------

#### Trabalhando a Usabilidade

Além de fazermos integração com a Marvel, vamos deixar nossa aplicação com uma interface melhor e proporcionar uma experiência boa ao usuário. Gosto muito de seguir na linha do Material Design, sendo assim, vou utilizar uma biblioteca chamada Vue Material que já disponibiliza vários componentes para que possamos focar apenas no nosso objetivo: listar os quadrinhos! Veja sua [documentação ](https://vuematerial.io/)e a variedade de componentes disponíveis.

Vamos instalar então o Vue Material em nosso projeto executando o seguinte comando:

```sh
npm install vue-material --save
```

Como a biblioteca instalada, vamos importá-la no arquivo **main.js** que se encontra dentro da pasta **src**:

```js
import Vue from 'vue';
import App from './App';
import router from './router';
import VueMaterial from 'vue-material';
import 'vue-material/dist/vue-material.min.css';

Vue.use(VueMaterial);

Vue.config.productionTip = false;

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>',
});
```



Agora nosso projeto está preparado para utilizarmos todos os componentes disponíveis da biblioteca.

#### Efetuando chamadas para a API

Antes de escrevermos a nossa primeira requisição, vamos instalar o [**axios**](https://github.com/axios/axios) para facilitar as requisições:

```sh
npm install axios --save
```

Com o axios instalado, vamos criar uma nova pasta chamada **services** dentro de src e adicionar um arquivo com o nome **MarvelAPI.js.** Ao adicionar, vamos importar o axios e desenvolver uma função em que vamos passar o **total** de quadrinhos que queremos e uma função de **callback** para recuperar o valor de retorno:

```js
import axios from 'axios';

const urlBaseMarvel = 'https://gateway.marvel.com:443/v1/public/';
const apiKey = 'CHAVE-PUBLICA-MARVEL-API';

export default {
    getAllComics: (limit, callback) => {
        const urlComics = urlBaseMarvel + 'comics?apikey=' + apiKey + '&limit=' + limit;
        axios.get(urlComics).then((comics) => {
            if (callback) {
                callback(comics);
            }
        })
    }
}
```



#### Criando um componente

Já que agora temos uma função que irá fazer um requisição, vamos criar um componente que irá receber alguns dados e exibir de forma amigável.

Na pasta **components**, vamos criar um arquivo chamado **Quadrinho.vue**, o qual irá renderizar uma imagem do quadrinho, o título e uma descrição.

Obs: Lembre-se que estou usando o **vue-material** e consigo utilizar alguns componentes prontos, como o **md-card**.

```vue
<template>
    <md-card class="card-default">
        <h2>{{titulo}}</h2>
        <img class="imagem-quadrinho" :src="imagem"/>
        <p>{{descricao}}</p>
    </md-card>
</template>

<script>
export default {
    name: 'quadrinho',
    props: {
        imagem: {
            type: String
        },
        titulo: {
            type: String,
            required: true
        },
        descricao: {
            type: String
        }
    }
}
</script>

<style scoped>
    .imagem-quadrinho {
        width: 120px;
    }
    .card-default {
        padding: 28px;
        margin: 18px;
        justify-content: center;
        align-items: center;
        display: flex;
        flex-direction: column;
        background: #FFF !important;
    }
</style>
```



#### Executando a requisição e mostrando o resultado no componente criado

Vamos para a parte mais interessante! Iremos ajustar o arquivo existente **App.vue** para que realize a chamada da função que fizemos para buscar os quadrinhos e, posteriormente, percorrer a lista e popular cada item em nosso componente **Quadrinho.vue**.

Então importaremos o arquivo que tem a função de requisição, o MarvelAPI.js, juntamente com o nosso componente Quadrinho.vue. Como estamos importanto um componente, precisaremos adicioná-lo na propriedade **components** do Vue.js.

Também é importante adicionarmos uma variável na propriedade **data** para que ela receba o retorno da requisição e utilizá-la para listar os quadrinhos. Vamos executar a chamada para a API da Marvel, passando um número total de 10 quadrinhos, uma função que receberá os quadrinhos e alimentar nossa variável dentro do ciclo de vida da aplicação **created**:

Assim, tudo ficará da seguinte forma:

```vue

<script>
import MarvelApi from '@/services/MarvelAPI';
import Quadrinho from '@/components/Quadrinho';
export default {
  name: 'App',
  components: {
    Quadrinho
  },
  data() {
    return {
      quadrinhos: []
    }
  },
  created() {
    var self = this
    MarvelApi.getAllComics(10, comics => {
      self.quadrinhos = comics.data.data.results;
    })
  }
}
</script>
```



Ao fazer uma requisição dos quadrinhos, também é retornado um caminho em que se encontra a imagem do quadrinho. Porém, precisaremos adicionar um valor no final do caminho que indicará o tamanho da imagem que queremos. Sendo assim, vamos criar um atributo **methods** e adicionar uma função que fará todo esse tratamento para nós:

```vue
<script>
import MarvelApi from '@/services/MarvelAPI';
import Quadrinho from '@/components/Quadrinho';
export default {
  name: 'App',
  components: {
    Quadrinho
  },
  data() {
    return {
      quadrinhos: []
    }
  },
  created() {
    var self = this
    MarvelApi.getAllComics(10, comics => {
      self.quadrinhos = comics.data.data.results;
    })
  },
  methods: {
    getImagem: function(quadrinho) {
      if (quadrinho.images.length) {
        return quadrinho.images[0].path + '/portrait_medium.jpg';
      }
    }
  }
};
</script>
```



Feito isso, vamos atuar em nosso template do **App.vue**. Utilizaremos algumas classes CSS da biblioteca vue-material e iremos fazer um laço de repetição que irá agir em nossa variável quadrinhos que, consequentemente, gerará nossos componentes.

```vue
<template>
  <div id="app" class="md-layout">
    <div class="md-layout-item md-size-33" v-for="quadrinho in quadrinhos" :key="quadrinho.id">
      <quadrinho
        :titulo="quadrinho.title"
        :descricao="quadrinho.description"
        :imagem="getImagem(quadrinho)"
    ></quadrinho>
    </div>
  </div>
</template>

<script>
import MarvelApi from '@/services/MarvelAPI';
import Quadrinho from '@/components/Quadrinho';
/// Continuacao do arquivo Vue.JS que voce construiu antes  
```



Com todos os passos e alguns ajustes de CSS, teremos 10 quadrinhos sendo mostrados em nossa página!

Observaçao: Nem todos os quadrinhos da API possuem  imagem e descrição completa

![img](https://cdn-images-1.medium.com/max/1600/1*GCJAuEjZO_zlTkcxqo3mjQ.png)

---

## Exercício Livre

Desenvolva uma aplicação similar que faça o consumo de informações dos heróis da Marvel e exiba informações básicas sobre eles. Para isso você precisará explorar a API da Marvel de personagens (Characters) e estudar o código acima.

