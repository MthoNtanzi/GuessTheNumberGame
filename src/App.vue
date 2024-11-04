<template>
    <h1>Hello There :)</h1>
    <h2>Please select a range</h2>
    <p>You have {{ guessCount }} guesses remaining</p>
    <div>
        <select v-model="startNum" name="" id="">
            <option v-for="num in numTo100" :key="num">
                {{ num }}
            </option>
        </select>
        <select v-model="endNum" name="" id="">
            <option v-for="num in numTo100" :key="num">
                {{ num }}
            </option>
        </select>
        <button @click="updateRange">Select range</button>
    </div>
    <p>I have selected a number between {{ chosenStartNum }} and {{ chosenEndNum }}, can you guess the number? </p>
    <input v-model="userGuess" type="number">
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
            isLow: false
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

            alert("Start: " + this.startNum + "  End: " + this.endNum);
            if (start >= end) {
                alert("The beginning number can't be larger than the ending number of the range");
            } else {
                this.chosenStartNum = start;
                this.chosenEndNum = end;
                this.randNum = Math.floor(Math.random() * (this.chosenEndNum - this.chosenStartNum + 1)) + this.chosenStartNum;
                alert("Random number: " + this.randNum);
            }
        },

        getGuess() {
            if (this.randNum === null) {
                alert("Please select a range first.");
                return;
            }

            this.hasUserGuessed = true;
            const theGuess = parseInt(this.userGuess);

            if (theGuess === this.randNum) {
                this.isCorrect = true;
                this.isHigh = false;
                this.isLow = false;
            } else if (theGuess > this.randNum) {
                this.isCorrect = false;
                this.isHigh = true;
                this.isLow = false;
            } else {
                this.isCorrect = false;
                this.isHigh = false;
                this.isLow = true;
            }
        }
    }
}
</script>
