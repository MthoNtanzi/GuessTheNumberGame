<template>
    <h1>Hello There :)</h1>
    <h2>Please select a range</h2>
    <br/>
    <p>Select difficulty</p>
    <select v-model="selectedDifficulty" @change="chooseDifficulty">
        <option v-for="choice in difficulty" :key="choice" :value="choice">
                {{ choice }}
            </option>
    </select>
    <p>You have {{ guessCount }} guesses remaining</p>
    <div>
        <select v-model="startNum" @change="updateRange">
            <option v-for="num in numTo100" :key="num">
                {{ num }}
            </option>
        </select>
        <select v-model="endNum" @change="updateRange">
            <option v-for="num in numTo100" :key="num">
                {{ num }}
            </option>
        </select>
    </div>
    <p>I have selected a number between {{ chosenStartNum }} and {{ chosenEndNum }}, can you guess the number? </p>
    <input v-model="userGuess" type="number" @keyup.enter="getGuess">
    <button @click="getGuess">Guess</button>
    <p v-if="hasUserGuessed">
        <span v-if="isCorrect">Yes!! You win</span>
        <span v-else-if="isHigh">Your guess is too high</span>
        <span v-else-if="isLow">Your guess is too low</span>
    </p>
</template>

<script>
export default {
    data() {
        return {
            difficulty: ["easy", "normal", "hard"],
            numTo100: [],
            startNum: 1,
            endNum: 100,
            chosenStartNum: 1,
            chosenEndNum: 100,
            randNum: null, // Set to null initially
            userGuess: null,
            hasUserGuessed: false,
            isCorrect: false,
            isHigh: false,
            isLow: false,
            guessCount: 5
        };
    },
    created() {
        for (let i = 1; i <= 100; i++) {
            this.numTo100.push(i);
        }
    },
    methods: {
        updateRange() {
            const start = parseInt(this.startNum, 10);
            const end = parseInt(this.endNum, 10);

            if (start >= end) {
                alert("The beginning number can't be larger than the ending number of the range");
            } else {
                this.chosenStartNum = start;
                this.randNum = Math.floor(Math.random() * (this.chosenEndNum - this.chosenStartNum + 1)) + this.chosenStartNum;
                this.chosenEndNum = end;
            }
        },

        getGuess() {
            if (this.randNum === null) {
                alert("Please select a difficulty first.");
                return;
            }

            if (this.guessCount === 0 && !this.isCorrect) {
                alert("You lose! The correct number was " + this.randNum);
                this.resetGame();
            }
            
            this.hasUserGuessed = true;
            const theGuess = parseInt(this.userGuess);
            this.guessCount--

            if (theGuess === this.randNum) {
                this.isCorrect = true;
                this.isHigh = false;
                this.isLow = false;
                alert("Yes!! You win!");
                return;
            } else if (theGuess > this.randNum) {
                this.isCorrect = false;
                this.isHigh = true;
                this.isLow = false;
                return
            } else {
                this.isCorrect = false;
                this.isHigh = false;
                this.isLow = true;
                return
            } 
        },

        chooseDifficulty() {
            if (this.selectedDifficulty === "easy") {
                this.guessCount = 8;
            } else if (this.selectedDifficulty === "normal") {
                this.guessCount = 5;
            } else if (this.selectedDifficulty === "hard") {
                this.guessCount = 3;
            }
            this.resetGame();
            alert("Difficulty changed to " + this.selectedDifficulty + " with " + this.guessCount + " guesses.");
        },

        resetGame() {
            this.hasUserGuessed = false;
            this.isCorrect = false;
            this.isHigh = false;
            this.isLow = false;
            this.userGuess = null;
            this.guessCount = this.selectedDifficulty === "easy" ? 8 : this.selectedDifficulty === "normal" ? 5 : 3;
            this.randNum = Math.floor(Math.random() * (this.chosenEndNum - this.chosenStartNum + 1)) + this.chosenStartNum;
        },
    }
}
</script>
