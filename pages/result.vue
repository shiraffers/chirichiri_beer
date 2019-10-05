
<script>
import { Bar } from 'vue-chartjs'
import axios from "axios"
export default {
    extends: Bar,
    data() {
        return {
          items: [],
          chartData: {
                labels: [],
                datasets: [{
                    label: 'コスパ度数',
                    backgroundColor:'rgba(255, 60, 60, 0.3)',
                    data: []
                }]
            },
            options: {
                title: {
                    display: true,
                    text: 'Bar chart'
                },
                scales: {
                    yAxes: [{
                            ticks: {
                                beginAtZero: true,
                            }
                    }]
                }
            }
        }
    },
    async created() {
       console.log("大人")
      const { data } = await axios.get(`${process.env.API}/beers`);
      this.items = data.Items.sort((a, b) => (a.id > b.id ? 1 : -1));

          console.log({items: this.items});
          this.chartData.labels = this.items.map(item => {
            console.log("aaa")
            return item.title
          })

           this.chartData.datasets[0].data = this.items.map(item => {
            return item.alcohol / item.price * 100
          })
        this.renderChart(this.chartData, this.options)
  }
}
</script>


<style>
#bar-chart{

}
</style>