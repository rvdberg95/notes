<template>
    <div id="form-container">
        
        <form @submit.prevent="submitNote(); clearInput();"
        :style="`--form-color: ${colorInput}; --form-color-2: ${colorInput}99;` "
        :class="{darkmodetext: bgIsDark}"
        >
        <div >
                <!-- <label for="title">Title</label> -->
                <input v-model="titleInput" type="text" id="title" name="title" ref="title"  placeholder="Title">
            </div>
            <div>
                <!-- <label for="description">Description</label> -->
                <textarea v-model="descriptionInput" id="description" name="description" placeholder="Description"></textarea>
            </div>
            <div>
                <!-- <label for="color">Color Post-it</label> -->
                <input type="color" id="color" 
                @input="updateFormColor"
                :value="colorInput"
                >
            </div>
            <button class="confirmbutton">Confirm</button>
        </form>
        <!-- <button @click="clearNotes">Remove All your notes</button> -->
    </div>
</template>

<script>
import { inject, onMounted, getCurrentInstance, registerRuntimeCompiler } from 'vue'
import tinycolor from "tinycolor2";

export default {

    emits: ['refresh'],
    data() {
        return {
            titleInput: '',
            descriptionInput: '',
            colorInput: '#F6F26B',
            bgIsDark: false,

        };
    },

    setup() {
        const emitter = inject('emitter');
        const submitNote = () => {
            const title = document.querySelector('#title').value;
            const description = document.querySelector('#description').value.toString();
            const color = document.querySelector('#color').value;
            const rotateValue = Math.floor(Math.random() * 10) - 5;
            const isDone = false;

            const newNote = {
                title: title,
                description: description,
                color: color,
                rotateValue: rotateValue,
                isDone: isDone,
            }
            emitter.emit('addNote', newNote);
            // document.querySelector('#title').value = '';
            // document.querySelector('#description').value = '';
        };
        return {
            submitNote
        }
    },
    beforeMount() {
        this.checkColor();
    },
    methods: {
        clearNotes() {
            window.localStorage.clear(`notes`);
            location.reload();
        },
        clearInput() {
            this.titleInput = '';
            this.descriptionInput = '';
        },
        updateFormColor(){
            const color = document.querySelector('#color').value;
            this.colorInput = color;
            this.checkColor();
        },
        checkColor() {
            const bgColor = tinycolor(this.colorInput);
            const bgBrightness = bgColor.getBrightness();
            if (bgBrightness < 180) 
            {
                this.bgIsDark = true;
            } else {
                this.bgIsDark = false;
            }

            console.log('checking color', bgColor.getBrightness(), this.bgIsDark);
        },
    },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Gothic+A1:wght@100;200;300;400;500;600;700;800;900&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Kalam:wght@300;400;700&display=swap');

* {
    color: rgba(0, 0, 0, 0.75);
}



.darkmodetext ::placeholder
    , button, .darkmodetext input, .darkmodetext textarea
  {
    color: rgba(255, 255, 255, 0.90) !important;
}

p {
    margin-bottom: 2rem;
    color: white;
    margin: 2rem;
    font-family: 'Gothic A1', sans-serif;
    font-size: 1rem;
}

#form-container {
    font-family: 'Kalam', cursive;
    color: rgba(0, 0, 0, 0.75);
    font-size: 2rem;
    display: block;
    height: 100vh;
    min-width: 25rem;
    background-color: rgb(41, 41, 41);
    background: linear-gradient(135deg, #0e0e0e 0%, #353535 50%, #0d0d0d 100%);
    background-size: 200% 200%;
    background-position: center;
    box-shadow: 0px 0px 25px 10px inset black;
    border-right: 3px solid rgba(229, 229, 229, 0.1);
}

form {
    display: grid;
    grid-template-columns: 1fr;
    gap: 1rem;
    text-justify: center;
    align-items: center;
    background-color: var(--form-color);
    mix-blend-mode: normal;
    filter: brightness(1) saturate(1);
    margin: 2rem;
    padding: 2rem;
    position: relative;
    height: fit-content;
    box-shadow: 0px 15px 10px 0px rgba(0, 0, 0, 0.25), -0px 20px 25px 1px inset rgba(0, 0, 0, 0.2);
    filter: saturate(0.9) brightness(1.1);

}

label {
    width: auto;
}

form::after {
    content: '';
    position: absolute;
    top: 100%;
    left: 0;
    width: 100%;
    height: 20%;
    z-index: 0;
    background: repeating-linear-gradient(0deg, var(--form-color), var(--form-color) 3px, var(--form-color-2) 1px, var(--form-color-2) 6px);
    background-size: 100% 100%, 12px 12px;
}

#color {
    padding: 0px;
    margin: 0px;
    border: none;
    width: 103%;
    height: 3rem;
    background-color: transparent;
}

#color::-webkit-color-swatch {
    border-radius: 5px;
    border: 1px solid rgba(0, 0, 0, 0.1);
}

textarea {
    resize: none;
    background-color: var(--form-color);
    border: 1px solid rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    height: 4rem;
    width: 100%;
    font-family: 'Kalam', cursive;
    color: rgba(0, 0, 0, 0.75);
    font-size: 1rem;
}

input {
    background-color: var(--form-color);
    border: 1px solid rgba(0, 0, 0, 0.1);
    border-radius: 5px;
    width: 100%;
    font-family: 'Kalam', cursive;
    color: rgba(0, 0, 0, 0.75);
    font-size: 2rem;
}

form div {
    display: grid;
    grid-template-columns: 1fr;
    height: fit-content;
    text-justify: center;
    align-items: center;
}

.confirmbutton {
    font-family: 'Kalam', cursive;
    color: rgba(0, 0, 0, 0.25);
    font-size: 2rem;
    border: 0px solid rgba(0, 0, 0, 0.01);
    background-color: rgba(0, 0, 0, 0.05);
    border-radius: 5px;
    width: 103%;
    transition: 0.3s all;
}

.confirmbutton:hover {
    background-color: var(--form-color);
}

.confirmbutton:active {
    color: transparent;
}

</style>