<template>
    <div class="container">
        <BaseNote v-for="note in notes" :key="note.id" :id="note.id" :title="note.title" :description="note.description"
            :color="note.color" :rotateValue="note.rotateValue" :isDone="note.isDone"
            @changePostItStatus="onChangePostItStatus">
        </BaseNote>
    </div>
</template>

<script>
import BaseNote from './BaseNote.vue';
import InputForm from './InputForm.vue';
import { inject } from 'vue';

export default {
    created() {
        this.notes = JSON.parse(window.localStorage.getItem('notes'));
        if (!this.notes) {
            window.localStorage.setItem(`notes`, JSON.stringify(this.backupNote))
        }
    },
    mounted() {

        const emitter = inject('emitter');

        emitter.on('addNote', (newNoteInput) => {
            if (!newNoteInput.title || !newNoteInput.description) return;

            const newNote = {
                key: new Date().toString(),
                id: new Date().toString(),
                title: newNoteInput.title,
                description: newNoteInput.description,
                color: newNoteInput.color,
                rotateValue: newNoteInput.rotateValue,
                isDone: false,
            }
            console.log(this.notes);
            if (!this.notes) this.notes = [{}];
            this.notes.unshift(newNote);

            window.localStorage.setItem(`notes`, JSON.stringify(this.notes))
        })
    },
    components: { BaseNote, InputForm },
    data() {
        return {
            notesAreEmpty: false,
            notes: [{
            }
            ],
            backupNote: [{
                title: 'Write your first note',
                description: 'Make it really good!'
            }
            ],
        };
    },
    methods: {
        refresh() {
            this.notes = JSON.parse(window.localStorage.getItem('notes'));
        },
        onChangePostItStatus(seletedPostitId, kindOfChange) {
            console.log(`Changing....`, kindOfChange);
            if (kindOfChange === 'delete') {
                const selectedNote = this.notes.find(note => note.id === seletedPostitId);
                // this.notes = newNotes;
                const index = this.notes.indexOf(selectedNote);
                const x = this.notes.splice(index, 1);
                console.log(this.notes);
            } else if (kindOfChange === 'check') {
                const selectedNote = this.notes.find(note => note.id === seletedPostitId);
                const index = this.notes.indexOf(selectedNote);
                this.notes[index].isDone = !this.notes[index].isDone;

            }

            this.update();
        },
        // onCheckPostIt(seletedPostitId){
        //     const selectedNote = this.notes.find(note => note.id === seletedPostitId);
        //     const index = this.notes.indexOf(selectedNote);
        //     this.notes[index].isDone = !this.notes[index].isDone;
        //     this.update();
        // },
        update() {
            window.localStorage.setItem(`notes`, JSON.stringify(this.notes))
        },
    },
    provide() {
        return {
            notes: this.notes,
            addNote: this.addNote,
            refresh: this.refresh
        };
    },
}
</script>

<style scoped >
.container {
    display: flex;
    flex-wrap: wrap;
    /* grid-template-columns: repeat(auto, 1fr); */
    background-image: url('../assets/bg.jpg');
     background-repeat: repeat ;
     overflow: scroll;
     background-attachment: local;
     min-height: auto;
     height: 100vh;

}
</style>