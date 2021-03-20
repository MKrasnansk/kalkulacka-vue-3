<template>
    <div class="calculator">
        <div class="result">{{ answer }}</div>
        <div class="input">{{ inputLog + Input }}</div>

        <div @click="clear()" class="btn operator">C</div>
        <div @click="plusminus()" class="btn operator">+/-</div>
        <div @click="percent()" class="btn operator">%</div>
        <div @click="divide()" class="btn operator">/</div>

        <div @click="addInput('7')" class="btn">7</div>
        <div @click="addInput('8')" class="btn">8</div>
        <div @click="addInput('9')" class="btn">9</div>
        <div @click="multiply()" class="btn operator">X</div>

        <div @click="addInput('4')" class="btn">4</div>
        <div @click="addInput('5')" class="btn">5</div>
        <div @click="addInput('6')" class="btn">6</div>
        <div @click="substract()" class="btn operator">-</div>

        <div @click="addInput('1')" class="btn">1</div>
        <div @click="addInput('2')" class="btn">2</div>
        <div @click="addInput('3')" class="btn">3</div>
        <div @click="plus()" class="btn operator">+</div>

        <div @click="addInput('0')" class="btn">0</div>
        <div @click="decimal()" class="btn">.</div>
        <div class="btn">**</div>
        <div @click="equal()" class="btn operator">=</div>
    </div>
</template>

<script>
export default {
    name: "App",
    components: {},
    data() {
        return {
            //hodnota v inpute
            Input: "",
            //vysledok
            answer: "",
            //hodnota na preratavanie
            inputLog: "",
            //blokovanie pridavania duplicitneho operatora
            operator: true,
        };
    },
    methods: {
        //ak je operator true input nadstavy na prazdnu hodnotu a zmeni operator na false
        //prima argument number
        addInput(number) {
            if (this.operator) {
                this.Input = "";
                this.operator = false;
            }
            this.Input = `${this.Input}${number}`;
        },
        clear() {
            this.Input = "";
            this.inputLog = "";
            this.answer = "";
        },
        plus() {
            this.addToInputLog("+");
        },
        divide() {
            this.addToInputLog("/");
        },
        substract() {
            this.addToInputLog("-");
        },
        multiply() {
            this.addToInputLog("*");
        },
        decimal() {
            if (this.Input.indexOf(".") === -1) {
                this.addInput(".");
            }
        },
        //ak je operator true tak nebude reagovat
        //eval funkcia na vyratanie vysledku a toFixed na 2 desatinene miesta
        equal() {
            if (this.operator === false) {
                this.answer = eval(this.inputLog + this.Input).toFixed(2);
            }
        },
        //ak input nieje prazdny povoli funkciu
        //hodnota v inpute ked sa rovna prvy char - tak sa zmaze
        //inak nadstavi - pred hodnotou inputu
        plusminus() {
            if (this.Input !== "") {
                this.Input =
                    this.Input.charAt(0) === "-"
                        ? this.Input.slice(1)
                        : `-${this.Input}`;
            }
        },
        //ak input nieje prazdny povoli funkciu
        //sparsuje na float a vydeli 100 cim vznikne 1% z hodnoty v inpute
        percent() {
            if (this.Input !== "") {
                this.Input = `${parseFloat(this.Input) / 100}`;
            }
        },
        //argument operator
        //ak je false inputLog dostane hodnotu false
        //input nadstavy na prazdny
        // a operator zmeni na true
        addToInputLog(operator) {
            if (this.operator === false) {
                this.inputLog += `${this.Input}${operator}`;
                this.Input = "";
                this.operator = true;
            }
        },
    },
};
</script>

<style>
#app {
    font-family: Helvetica, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}
body {
    background-color: #2c3e50;
}
.calculator {
    display: grid;
    grid-template-columns: 50px 50px 50px 50px;
    grid-template-rows: 50px 50px;
    background-color: black;
    height: 500px;
    gap: 0.5em;
    padding: 1em;
    border-radius: 0.5em;
    position: absolute;
    top: 3em;
    left: 50%;
    transform: translate(-50%);
    justify-content: center;
}
.btn {
    background-color: #131b23;
    color: white;
    border-radius: 25%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: bold;
}
.operator {
    background-color: #263f58;
}
.result,
.input {
    grid-column: span 4;
    font-size: 40px;
    text-align: right;
    color: red;
    overflow: hidden;
}
.input {
    color: white;
}
</style>
