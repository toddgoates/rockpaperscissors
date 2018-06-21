<template>
    <div class="col-sm-4">
        <div class="card">
            <div class="card-body text-center">
                <div class="btn-group">
                    <button class="btn btn-secondary" @click="play('rock')">
                        <i class="fa fa-fw fa-hand-rock-o"></i> rock
                    </button>
                    <button class="btn btn-warning" @click="play('paper')">
                        <i class="fa fa-fw fa-hand-paper-o"></i> paper
                    </button>
                    <button class="btn btn-danger" @click="play('scissors')">
                        <i class="fa fa-fw fa-hand-scissors-o"></i> scissors
                    </button>
                </div>
            </div>
            <div class="card-footer">
                <button class="btn btn-default float-left"
                        @click="save"
                        title="This will save your game progress">
                    Save
                </button>
                <button class="btn btn-default float-right"
                        @click="restart"
                        title="This will start your game over. All saved scores and history will be deleted">
                    Restart
                </button>
            </div>
        </div>
    </div>
</template>

<script>
import event from '../event.js';
export default {
    name: 'Gameplay',
    data() {
        return {
            options: ['rock', 'paper', 'scissors']
        }
    },
    methods: {
        play(choice) {
            this.logAction('You', choice);
            let compChoice = this.compTurn();

            this.determineWinner(choice, compChoice);
        },
        compTurn() {
            let choice = this.getRandomMove();
            this.logAction('Computer', choice);

            return choice;
        },
        getRandomMove() {
            return this.options[Math.floor(Math.random()*this.options.length)];
        },
        logAction(player, choice) {
            let action = player + ' chose ' + choice;
            if(player === 'You') {
                event.$emit('your_choice', action);
            } else {
                event.$emit('comp_choice', action);
            }
        },
        logOutcome(result) {
            event.$emit('game_results', result);
        },
        determineWinner(choice, compChoice) {
            if(choice == 'rock' && compChoice == 'rock')
                this.logOutcome('You tied!');
            else if(choice == 'rock' && compChoice == 'paper')
                this.logOutcome('You lost!');
            else if(choice == 'rock' && compChoice == 'scissors')
                this.logOutcome('You won!');
            if(choice == 'paper' && compChoice == 'rock')
                this.logOutcome('You won!');
            else if(choice == 'paper' && compChoice == 'paper')
                this.logOutcome('You tied!');
            else if(choice == 'paper' && compChoice == 'scissors')
                this.logOutcome('You lost!');
            if(choice == 'scissors' && compChoice == 'rock')
                this.logOutcome('You lost!');
            else if(choice == 'scissors' && compChoice == 'paper')
                this.logOutcome('You won!');
            else if(choice == 'scissors' && compChoice == 'scissors')
                this.logOutcome('You tied!');
        },
        save() {
            event.$emit('save');
        },
        restart() {
            event.$emit('show_modal');
        }
    }
}
</script>
