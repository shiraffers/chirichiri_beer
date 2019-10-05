<script>
import { Bar } from 'vue-chartjs'

export default {
    extends: Bar,
    props: ['items'],
    data() {
        return {
            chartData: {
                labels: this.items.map(value => value.title),
                datasets: [{
                    label: 'sample',
                    backgroundColor:'rgba(255, 60, 60, 0.3)',
                    data: [20, 40, 60]
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
    mounted: function() {
        this.renderChart(this.chartData, this.options)
        console.log(this.items);
    }
}
</script>
