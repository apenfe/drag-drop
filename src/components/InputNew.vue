<script setup>

    import {ref, defineEmits} from 'vue';

    const text = ref("");

    const emits = defineEmits(['onNewItem']);

    function handleSubmit(){

        if(text.value != ""){
            // llamo al emit
            emits('onNewItem',text.value);
            text.value = "";
        }

    }
  
</script>

<template>

    <form action="" @submit.prevent="handleSubmit">

        <input class="input" type="text" v-model="text" placeholder="Inserte nueva tarea">

    </form>

</template>

<style scoped>

form{
    margin: 1rem;
}

.input{

    text-align: center;
    width: 100%;
    height: 3rem;
    border: none;
    outline: none;
    border-radius: 0.5rem;
    color: #333;

}

.input::placeholder{
    color: #333;

}

</style>