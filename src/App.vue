<template>
    <h1>Hello There :)</h1>
    <h2>Please select a range</h2>
    <div>
        <select v-model="startNum" name="" id="">
            <option v-for="num in numTo100" :key="num">
                {{num}}
            </option>
        </select>
        <select v-model="endNum" name="" id="">
            <option v-for="num in numTo100" :key="num">
                {{num}}
            </option>
        </select>
        <button @click="updateRange">Select range</button>
    </div>
    <p>I have selected a number between {{ chosenStartNum }} and {{ chosenEndNum }}, can you guess the number? </p>
    <input type="number">
    
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
                randNum: Math.floor(Math.random() * (this.endNum - this.startNum + 1)) + this.startNum
            };
        },
    created() {
            for (let i = 1; i <= 100; i++) {
                this.numTo100.push(i)
                
            }
    },
    methods: {
        updateRange() {
            const start = parseInt(this.startNum, 10)
            const end = parseInt(this.endNum, 10)
            
            alert("start: " + this.startNum + "  end: " + this.endNum)
            if (start >= end) { 
                alert("The beginning number cant be larger than the ending number of the renge")
            } else {
                this.chosenStartNum = start
                this.chosenEndNum = end
                this.randNum = Math.floor(Math.random() * (this.chosenEndNum - this.chosenStartNum + 1)) + this.chosenStartNum
                
                alert("Randon number: " + this.randNum)
            }

            
            }
        } 
    }
</script>