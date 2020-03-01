

<script>
  //Importing Line class from the vue-chartjs wrapper
  import { Line, mixins } from 'vue-chartjs'
  const {reactiveProp} = mixins
  
  const chartOptions =  {
          scales: {
            yAxes: [{
              id: 'T',
              ticks: {
                beginAtZero: true
              },
              type: 'linear',
              gridLines: {
                display: true
              }
            },{
              id: 'P',
              ticks: {
                beginAtZero: true
              },
              type: 'linear',
              gridLines: {
                display: true
              }
            }
            
            
            ],
            xAxes: [ {
              gridLines: {
                display: false
              }
            }]
          },
          legend: {
            display: true
          },
          responsive: true,
          maintainAspectRatio: false
        }


  //Exporting this so it can be used in other components
  export default {
    extends: Line,
    mixins: [reactiveProp],
    data() {
      return {
        options: chartOptions
      }
    },
    mounted() {
      this.renderChart(this.chartData, this.options)
    },
  
  }
</script>
