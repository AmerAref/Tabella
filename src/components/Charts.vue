<template>
  <div class="post">
  <div>
       <b-card-group id ="bcard" deck
                  class="mb-3">
        <b-card bg-variant="info"
                text-variant="white"
                header="INFO"
                class="text-center">
            <p class="card-text">Messaggi totali di info  {{AllInfo}}</p>
        </b-card>
        <b-card bg-variant="warning"
                text-variant="white"
                header="WARNING"
                class="text-center">
            <p class="card-text">Messaggi totali di warn  {{AllWarn}}</p>
        </b-card>
        <b-card bg-variant="danger"
                text-variant="white"
                header="Error"
                class="text-center">
            <p class="card-text">Messaggi totali di Error  {{AllError}}</p>
        </b-card>
    </b-card-group>
    </div>
  <h1>Diagramma relativo a tutti i messaggi</h1>
<pie-chart :data="[['Info', AllInfo], ['Error', AllError], ['Warn', AllWarn]]" :library="{backgroundColor: 'yellowgreen'}"  ></pie-chart>
  <h1>Diagramma a colonna suddiviso per data</h1>
<column-chart id="aaa" :data="chartData" :colors="['red', 'orange', 'blue']" :stacked="true" :library="{backgroundColor: 'yellowgreen'}"legend="bottom" xtitle="Date" ytitle="Level"> </column-chart>

  </div>
</template>

<script>
import Chartkick from 'chartkick'
import VueChartkick from 'vue-chartkick'
import Vue from 'vue'
import axios from 'axios'
import { countBy } from 'vue-computed-helpers'
import lodash from 'lodash'
import VueLodash from 'vue-lodash'

Vue.use(VueChartkick, { Chartkick })
Vue.use(VueLodash, lodash)
export default {
  name: 'VueChartKick',
  data () {
    return {
      logs: [ ]
    }
  },
  mounted () {
    axios
    .get('../static/Log.json')
    .then(response => { this.logs = response.data })
  },
  methods: {
    getChartData: function () {
      let groupsByDate = Vue._.groupBy(this.logs, 'date')
      let groupsByLevel = Vue._.groupBy(this.logs, 'level')

      let chartData = []
      let logTypes = {}

      Vue._.forEach(groupsByLevel, function (logs, type) {
        logTypes[type] = {
          name: type,
          data: []
        }
      })

      Vue._.forEach(groupsByDate, function (group, date) {
        let counts = Vue._.countBy(group, 'level')

        Vue._.forEach(counts, function (value, key) {
          logTypes[key].data.push([date, value])
        })
      })

      Vue._.forEach(logTypes, function (type) {
        chartData.push(type)
      })

      return chartData
    }
  },
  computed: {
    AllWarn: countBy('logs', 'level', 'WARNING'),
    AllError: countBy('logs', 'level', 'ERROR'),
    AllInfo: countBy('logs', 'level', 'INFO'),
    chartData: function () {
      return this.getChartData()
    }
  }
}
</script>

<style scoped>

#aaa{
  margin-top: 20px;
  margin-right: 20px;
  }

#bcard {
  margin-top: 20px;
  margin-left: 20px;
  margin-right: 20px;
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