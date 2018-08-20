<template>
    <div class="col-lg-3">
        <div class="card" :class="{ scrollable: scrollable} ">
            <div class="card-body">
                <h5 class="card-title">
                    <i class="fa fa-fw fa-book"></i> History
                    <button class="btn btn-default" 
                            @click="clearHistory" 
                            title="This will delete all game history, but keep your scores.">
                        Clear
                    </button>
                </h5>
                <ul class="list-unstyled">
                    <li v-for="(hist, index) in history" :key="index">
                        {{ hist.outcome }} 
                        <small class="text-muted">({{ hist.timestamp }})</small>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
import event from '../event.js';
import moment from 'moment';
export default {
    name: 'History',
    data() {
        return {
            history: [],
            scrollable: false
        }
    },
    methods: {
        init() {
            if(localStorage.getItem('history')) {
                this.history = JSON.parse(localStorage.getItem('history'));
            }

            this.addScrollableClass();
        },
        clearHistory() {
            this.history = [];
            this.scrollable = false;
        },
        addScrollableClass() {
            if(this.history.length > 16) {
                this.scrollable = true;
            }
        }
    },
    mounted() {
        this.init();

        event.$on('game_results', result => {
            let history = {
                outcome: result,
                timestamp: moment().format('h:mm:ss a')
            };

            this.history.unshift(history);

            this.addScrollableClass();
        });

        event.$on('save', () => {
            localStorage.setItem('history', JSON.stringify(this.history));
        });

        event.$on('restart', () => {
            this.clearHistory();
        });
    }
}
</script>

<style>
    .scrollable {
        max-height: 30.9rem;
        overflow-y: scroll;
    }
</style>