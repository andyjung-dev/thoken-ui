<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <h2> Current token: {{ selectedToken }}</h2>
    <h3> Sample Correlation {{ correlation }}</h3>
    <line-chart :chart-data="datacollection"/>
    <button @click="buttonTest('DAI')">DAI</button>
    <button @click="buttonTest('BAT')">BAT</button>
    <button @click="buttonTest('BNB')">BNB</button>
    <button @click="buttonTest('ZRX')">ZRX</button>
    <button @click="buttonTest('LINK')">LINK</button>

    
  </div>
</template>

<script>

import LineChart from '@/components/LineChart';
import axios from 'axios';

  const exampleData = {
  "transfers": [
    0,
    271592078893,
    311237782793,
    312963968902,
    313437211807,
    291112261035,
    216837203920,
    224219295050,
    273648403229,
    335672451361,
    284615824670,
    272961291823,
    261819978681,
    306712301869,
    305327516090,
    351493369578,
    384139614210,
    429651462001,
    308159038082,
    295726481501,
    186902542757,
    229590021542,
    209047241163,
    166917738693,
    156662000549,
    180121012828,
    173143842584,
    179653387818,
    304767435232,
    418741590429,
    269123307908
  ],
  "prices": [
    "0.26182947",
    "0.25012693",
    "0.24627681",
    "0.23111112",
    "0.23229295",
    "0.24455196",
    "0.23764767",
    "0.23888936",
    "0.23240954",
    "0.22625531",
    "0.22127134",
    "0.21688633",
    "0.22115177",
    "0.21191383",
    "0.21267419",
    "0.19389169",
    "0.17966555",
    "0.20170191",
    "0.18740046",
    "0.19453780",
    "0.19216107",
    "0.19441371",
    "0.18562073",
    "0.18515348",
    "0.18194380",
    "0.18228321",
    "0.18586775",
    "0.18612971",
    "0.19647607",
    "0.18865820",
    "0.18545516"
  ]
};


export default {

  name: 'HelloWorld',
  components: { LineChart },
  props: { msg: String },
  data () {
    return {
      selectedToken: 'DAI',
      correlation: "0",
      datacollection: null,
     
    }
  },
  created(){
    this.fetchData()
    this.fillData()
  },
  mounted() {
    this.fillData()
  },
  methods: {
    fillData(){

        const testdatacollection = {
          //Data to be represented on x-axis
          labels: [...Array(30).keys()],
          datasets: [
            {
              label: 'Token Transfer Volume',
              backgroundColor: '#f87979',
              pointBackgroundColor: 'purple',
              fill: false,
              yAxisID: 'T',
              borderWidth: 1,
              pointBorderColor: '#249EBF',
              //Data to be represented on y-axis
              data: exampleData.transfers.slice(1)
            },
            {
              label: 'Token Price',
              backgroundColor: '#087979',
              pointBackgroundColor: 'black',
              fill: false,
              yAxisID: 'P',
              borderWidth: 1,
              pointBorderColor: '#249E0F',
              //Data to be represented on y-axis
              data: exampleData.prices.slice(1)
            }
          ]

        }

      this.datacollection = testdatacollection
      
    },
    fetchData (){},
    buttonTest(token){
        axios.post("https://thoken.herokuapp.com/getDump", {
        token
      })
        .then(result => {
          const data = result.data;
   
          const atestdatacollection = {
          //Data to be represented on x-axis
          labels: [...Array(30).keys()],
          datasets: [
            {
              label: 'Token Transfer Volume',
              backgroundColor: '#f87979',
              pointBackgroundColor: 'purple',
              fill: false,
              yAxisID: 'T',
              borderWidth: 1,
              pointBorderColor: '#249EBF',
              //Data to be represented on y-axis
              data: data.transfers
            },
            {
              label: 'Token Price',
              backgroundColor: '#087979',
              pointBackgroundColor: 'black',
              fill: false,
              yAxisID: 'P',
              borderWidth: 1,
              pointBorderColor: '#249E0F',
              //Data to be represented on y-axis
              data: data.prices
            }
          ]

        }
        this.correlation = data.correlation
        this.datacollection = atestdatacollection
          
          })
      .catch(err => {

        console.log(err)})




       
    }
  }
   
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
