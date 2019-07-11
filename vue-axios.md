## Consumo de APIs com o Vue.JS

Existem diversos momentos quando você está desenvolvendo uma aplicação Web que podem necessitar consumir e exibir dados de uma API. Há várias maneiras de se fazer isso, mas a maneira mais popular é usando [axios](https://github.com/axios/axios), um cliente HTTP baseado em [Promises](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Promise).

Neste exercício, usaremos a [API CoinDesk](https://www.coindesk.com/api/) para exibir os preços do Bitcoin, atualizados a cada minuto. Primeiramente, precisamos instalar o axios via npm/yarn ou através do endereço CDN. Um esqueleto que você pode usar para o restante do exercício é disponibilizado abaixo.

```html
<html>
  <!-- seu codigo html vem aqui -->
  
  <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
  <script src="https://unpkg.com/vue"/></script>
  <script type="text/javascript">
      /// Seu codigo JS vem aqui
  </script>
</html>
```

Temos várias maneiras de recuperarmos informações de uma API, mas primeiro é interessante descobrir o formato dos dados, para sabermos o que mostraremos. Para fazer isso, faremos uma requisição para o *endpoint* da API, para podermos ver os dados. Podemos ver na documentação da API CoinDesk que esta chamada será feita para `https://api.coindesk.com/v1/bpi/currentprice.json`. Assim, criaremos uma propriedade de dados que eventualmente abrigará nossa informação e, então, recuperaremos os dados e atribuiremos usando o gatilho de ciclo de vida `mounted`:

```javascript
new Vue({
  el: '#app',
  data () {
    return {
      info: null
    }
  },
  mounted () {
    axios
      .get('https://api.coindesk.com/v1/bpi/currentprice.json')
      .then(response => (this.info = response))
  }
})
```

```html
<div id="app">
  {{ info }}
</div>
```

O resultado base é resumido no HTML, para facilitar o trabalho para você.

```html
<html>
  <div id="app">
  {{ info }}
  </div>
  
  <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
  <script src="https://unpkg.com/vue"/></script>
  <script type="text/javascript">
    new Vue({
    el: '#app',
    data () {
      return {
        info: null
      }
    },
    mounted () {
      axios
        .get('https://api.coindesk.com/v1/bpi/currentprice.json')
        .then(response => (this.info = response))
    }
  })
  </script>
</html>
```



E o que temos é isso:

```json
{ "data": { "time": { "updated": "Jul 11, 2019 15:12:00 UTC", "updatedISO": "2019-07-11T15:12:00+00:00", "updateduk": "Jul 11, 2019 at 16:12 BST" }, "disclaimer": "This data was produced from the CoinDesk Bitcoin Price Index (USD). Non-USD currency data converted using hourly conversion rate from openexchangerates.org", "chartName": "Bitcoin", "bpi": { "USD": { "code": "USD", "symbol": "&#36;", "rate": "11,769.2850", "description": "United States Dollar", "rate_float": 11769.285 }, "GBP": { "code": "GBP", "symbol": "&pound;", "rate": "9,387.6996", "description": "British Pound Sterling", "rate_float": 9387.6996 }, "EUR": { "code": "EUR", "symbol": "&euro;", "rate": "10,458.8928", "description": "Euro", "rate_float": 10458.8928 } } }, "status": 200, "statusText": "", "headers": { "cache-control": "max-age=15", "content-type": "application/javascript", "expires": "Thu, 11 Jul 2019 15:13:07 UTC" }, "config": { "transformRequest": {}, "transformResponse": {}, "timeout": 0, "xsrfCookieName": "XSRF-TOKEN", "xsrfHeaderName": "X-XSRF-TOKEN", "maxContentLength": -1, "headers": { "Accept": "application/json, text/plain, */*" }, "method": "get", "url": "https://api.coindesk.com/v1/bpi/currentprice.json" }, "request": {} }
```



Excelente! Já temos algumas informações reais da cotação do BitCoin!!

Mas parece um pouco confuso agora, pois o JSON não foi tratado. Então, vamos exibir adequadamente a informação desejada e também adicionar algum tratamento de exceção no caso de alguma coisa não funcionar conforme esperado ou levar mais tempo do que esperávamos para recuperar as informações.

## [Exemplo do Mundo Real: Trabalhando os Dados](https://br.vuejs.org/v2/cookbook/using-axios-to-consume-apis.html#Exemplo-do-Mundo-Real-Trabalhando-os-Dados)

### [Mostrando o Dado Desejado](https://br.vuejs.org/v2/cookbook/using-axios-to-consume-apis.html#Mostrando-o-Dado-Desejado)

É muito comum que as informações que precisaremos estejam internamente na resposta obtida, então teremos que analisar o que acabamos de obter para conseguirmos acessar adequadamente. Neste caso, podemos ver que o preço informado está em `response.data.bpi`. Se usarmos isso, nossa saída seria a seguinte:

```javascript
axios
  .get('https://api.coindesk.com/v1/bpi/currentprice.json')
  .then(response => (this.info = response.data.bpi))
```



```json
{ "USD": { "code": "USD", "symbol": "&#36;", "rate": "11,769.2850", "description": "United States Dollar", "rate_float": 11769.285 }, "GBP": { "code": "GBP", "symbol": "&pound;", "rate": "9,387.6996", "description": "British Pound Sterling", "rate_float": 9387.6996 }, "EUR": { "code": "EUR", "symbol": "&euro;", "rate": "10,458.8928", "description": "Euro", "rate_float": 10458.8928 } }
```

Isso é muito mais fácil para nós exibirmos, então podemos atualizar o HTML para mostrar somente a informação que recebemos. Criaremos um [filter](https://br.vuejs.org/v2/api/#Vue-filter) para ter certeza que a casa decimal estará no lugar correto.

```html
<div id="app">
  <h1>Preços do Bitcoin</h1>
  <div
    v-for="currency in info"
    class="currency">
    {{ currency.description }}:
    <span class="lighten">
      <span v-html="currency.symbol"></span>{{ currency.rate_float | currencydecimal }}
    </span>
  </div>
</div>
```



```javascript
filters: {
  currencydecimal (value) {
    return value.toFixed(2)
  }
},
```



![image-20190711122227804](/Users/marco.mendes/Library/Application Support/typora-user-images/image-20190711122227804.png)



### [Lidando com Erros](https://br.vuejs.org/v2/cookbook/using-axios-to-consume-apis.html#Lidando-com-Erros)

Às vezes, podemos não receber os dados que precisamos da API. Há vários motivos para que nossa chamada assíncrona possa falhar, seguem alguns exemplos:

- A API está fora do ar.
- A requisição foi realizada de forma incorreta.
- A API não retornou as informações da maneira que esperávamos.

Ao realizar a requisição, devemos checar todas essas circustâncias, obtendo informações em todos os casos para que possamos lidar com o problema. Em uma chamada *axios*, fazemos isso usando `catch`.

```javascript
axios
  .get('https://api.coindesk.com/v1/bpi/currentprice.json')
  .then(response => (this.info = response.data.bpi))
  .catch(error => console.log(error))
```

Assim, saberemos se algo falhou durante a requisição da API. Mas, se o dado está desconfigurado ou a API está fora do ar? Neste caso, o usuário não verá nada. Podemos exibir uma informação de carregamento (um *loader*) durante a requisição e, em seguida, avisar o usuário se não conseguirmos recuperar os dados.

```javascript
new Vue({
  el: '#app',
  data () {
    return {
      info: null,
      loading: true,
      errored: false
    }
  },
  filters: {
    currencydecimal (value) {
      return value.toFixed(2)
    }
  },
  mounted () {
    axios
      .get('https://api.coindesk.com/v1/bpi/currentprice.json')
      .then(response => {
        this.info = response.data.bpi
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
  }
})

```



```html
<div id="app">
  <h1>Preços do Bitcoin</h1>

  <section v-if="errored">
    <p>Pedimos desculpas, não estamos conseguindo recuperar as informações no momento. Por favor, tente novamente mais tarde.</p>
  </section>

  <section v-else>
    <div v-if="loading">Carregando...</div>

    <div
      v-else
      v-for="currency in info"
      class="currency">
      {{ currency.description }}:
      <span class="lighten">
        <span v-html="currency.symbol"></span>{{ currency.rate_float | currencydecimal }}
      </span>
    </div>

  </section>
</div>
```



Você pode clicar no botão reexecutar no exemplo abaixo, para poder ver rapidamente o estado de carregamento, enquanto buscamos os dados da API:

![image-20190711122431349](/Users/marco.mendes/Library/Application Support/typora-user-images/image-20190711122431349.png)



## [Considerações Finais](https://br.vuejs.org/v2/cookbook/using-axios-to-consume-apis.html#Consideracoes-Finais)

Existem vários caminhos para trabalhar com Vue e *axios*, além de consumir e mostrar um *endpoint* de uma API. Você também pode realizar comunicação com *Serveless Functions*, postar/editar/deletar de uma API que você tenha permissão de escrita, e entre outros casos. Devido à integração direta dessas duas bibliotecas, acabou se tornando uma escolha comum entre os desenvolvedores que precisam integrar clientes HTTP em seu fluxo de trabalho.