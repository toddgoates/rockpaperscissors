<template>
    <div class="col-sm-5">
        <div class="card">
            <div class="card-body">
                <div class="float-left text-primary">
                    <p>You: {{ yourScore }}</p>
                    <p>{{ yourChoice }}</p>
                </div>
                <div class="float-right text-right text-danger">
                    <p>Computer: {{ compScore }}</p>
                    <p>{{ compChoice }}</p>
                </div>
                <div class="clearfix"></div>
                <hr>
                <div class="text-center">
                    <strong>{{ outcome }}</strong>
                </div>
            </div>
        </div>
        <br>
        <div class="card" v-show="yourScore > 0 || compScore > 0">
            <div class="card-body">
                <h3 class="card-title">
                    <i class="fa fa-fw fa-trophy"></i> Wins
                </h3>
                <canvas id="piechart"></canvas>
            </div>
        </div>
    </div>
</template>

<script>
import event from '../event.js';
import Chart from 'chart.js';
export default {
    name: 'Score',
    data() {
        return {
            yourScore: 0,
            compScore: 0,
            yourChoice: '',
            compChoice: '',
            outcome: ''
        }
    },
    methods: {
        init() {
            if(localStorage.getItem('yourScore')) {
                this.yourScore = Number(localStorage.getItem('yourScore'));
            }

            if(localStorage.getItem('compScore')) {
                this.compScore = Number(localStorage.getItem('compScore'));
            }

            this.drawChart();
        },
        drawChart() {
            if(this.yourScore > 0 || this.compScore > 0) {
                let ctx = document.getElementById('piechart').getContext('2d');
                let data = {
                    datasets: [{
                        data: [this.yourScore, this.compScore],
                        backgroundColor: ['#007BFF', '#DC3545']
                    }],
                    labels: ['You', 'Computer']
                };
                new Chart(ctx, {
                    type: 'pie',
                    data: data,
                    options: {}
                });
            }
        }
    },
    mounted() {
        this.init();

        event.$on('your_choice', action => {
            this.yourChoice = action;
        });

        event.$on('comp_choice', action => {
            this.compChoice = action;
        });

        event.$on('game_results', result => {
            if(result === 'You won!') {
                this.yourScore += 1;
            } else if(result === 'You lost!') {
                this.compScore += 1;
            }
            this.outcome = result;
            this.drawChart();
        });

        event.$on('save', () => {
            localStorage.setItem('yourScore', this.yourScore);
            localStorage.setItem('compScore', this.compScore);
        });

        event.$on('restart', () => {
            this.yourScore = 0,
            this.compScore = 0,
            this.yourChoice = '',
            this.compChoice = '',
            this.outcome = ''
        });
    }
}
</script>
