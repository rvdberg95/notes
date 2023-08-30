
<template>
  <section>
<InputForm></InputForm>
<TheNotes></TheNotes>
</section>
</template>

  <script>
import InputForm from './components/InputForm.vue';
import TheNotes from './components/TheNotes.vue';

export default {
  components: {
    InputForm, TheNotes
  },
  data() {
    return {
      title: 'Learning Resources',
    };
  },
}
  </script>

<style>
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  overflow: hidden;
}
section {
  display: flex;
}
</style>
