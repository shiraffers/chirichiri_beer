<script>
import { Pie } from 'vue-chartjs'

export default {
    extends: Pie,
    props: ['graphData'],
    data () {
        return {
            chartData: {
                labels: this.graphData.type,
                datasets: [{
                    backgroundColor: this.graphData.color,
                    data: [this.graphData.data1, this.graphData.data2]
                }],
            },
            options: {
                title: {
                    display: true,
                    text: 'Pie chart'
                },
            }
        }
    },
    mounted: function() {
        this.renderChart(this.chartData, this.options)
    }
}
</script>
