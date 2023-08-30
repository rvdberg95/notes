<template>
    <div v-if="title" :style="`--element-color: ${color}; --rotation-value: ${rotateValue}deg;`" :class="{
        darkmodetext: bgIsDark,
        checkedpostit: isDone
    }">
        <h2>{{ title }}</h2>
        <p>{{ description }}</p>
        <button id="check" @click="changePostItStatus('check')">✓</button>
        <button id="delete" @click="changePostItStatus('delete')">X</button>
        <p id="timestamp">Made on {{ timestamp }}</p>
    </div>
</template>

<script>
import tinycolor from "tinycolor2";
export default {
    emits: ['changePostItStatus'],
    props: {
        id: String,
        title: String,
        description: String,
        color: String,
        rotateValue: Number,
        isDone: Boolean,
    },
    beforeMount() {
        this.checkColor();
        this.formatDate();
    },
    data() {
        return {
            bgIsDark: true,
            timestamp: '',
        }
    },
    methods: {
        changePostItStatus(what) {
            this.$emit('changePostItStatus', this.id, what);
        },
        checkColor() {
            const bgColor = tinycolor(this.color);
            const bgBrightness = bgColor.getBrightness();
            if (bgBrightness > 75) this.bgIsDark = false;
        },
        formatDate() {
            const originalDate = new Date(this.id);

            const months = [
                "Jan", "Feb", "Mar", "Apr", "May", "Jun",
                "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
            ];

            const formattedDate = `${originalDate.getDate()} / ${originalDate.getMonth()} /  ${originalDate.getFullYear()}`;
            this.timestamp = formattedDate;
            console.log(formattedDate); // Output: Aug 30 2023

        }
    }
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Kalam:wght@300;400;700&display=swap');

* {
    font-family: 'Kalam', cursive;
    color: rgba(0, 0, 0, 0.75);
}

.darkmodetext h2,
.darkmodetext p {
    color: rgba(255, 255, 255, 0.90);
}


div {
    position: relative;
    background-color: var(--element-color);
    border: 0px solid var(--element-color);
    box-shadow: 0px 15px 10px 0px rgba(0, 0, 0, 0.25), -0px 20px 25px 1px inset rgba(0, 0, 0, 0.2);
    /* box-shadow: 5px 5px 0px 0px var(--element-color); */
    /* border-radius: 5px; */
    width: 15rem;
    min-height: 10rem;
    height: fit-content;
    margin: 1rem;
    padding: 2rem;
    display: grid;
    grid-template-columns: repeat(1, 1fr);
    grid-template-rows: auto 1fr;
    transform: rotate(var(--rotation-value));
    color: rgb(16, 16, 16);
}

div::after {
    content: '';
    position: absolute;
    top: 5px;
    left: 50%;
    width: 2rem;
    aspect-ratio: 1/1;
    border-radius: 999px;
    background-color: rgb(184, 48, 48);
    box-shadow: 0px 3px 7px 0px rgba(0, 0, 0, 0.5), -0px 3px 5px 1px inset rgba(255, 255, 255, 0.2), 0px -4px 2px 1px inset rgba(0, 0, 0, 0.4);
}

div::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(211, 215, 255, 0.75);
    z-index: -999;
    mix-blend-mode: overlay;
}

div h2 {
    height: min-content;
}

div p {
    height: min-content;
    inline-size: 15rem;
    word-wrap: break-word;
}

div button {
    position: absolute;
    top: 3%;
    right: 3%;
    border: 0px;
    background-color: transparent;
    font-size: 1rem;
    font-weight: 900;
    color: rgba(0, 0, 0, 0.5);
    mix-blend-mode: multiply;
}

#check {
    right: 3%;
    bottom: -83%;
}

div .checkedpostit {
    background-color: green;
    text-decoration: line-through;
}

#timestamp {
    font-size: 0.6rem;
    color: rgba(0, 0, 0, 0.5);

    position: absolute;
    top: 0;
    left: 3%;
}
</style>