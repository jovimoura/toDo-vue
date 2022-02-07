<template>
    <div class="new-task">
        <input class="form-element" placeholder="Nova tarefa?" v-model="name" @keydown.enter="add" type="text">
        <button class="form-element" @click="add">+</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            name: ''
        }
    },
    methods: {
        add() {
            this.$emit('taskAdded', { name: this.name })
            this.name = ''
        }
    },    
}
</script>

<style>
    .form-element {
        outline: none;
        font-size: 2rem;
        border: 1px solid #FFF;
        padding: 5px 10px 8px;
        color: #FFF;
    }

    input.form-element {
        width: 80%;
        background: #FFF2;
        border-top-left-radius: 8px;
        border-bottom-left-radius: 8px;
    }

    button.form-element {
        border-left: none;
        background-color: #2196F3;
        border-top-right-radius: 8px;
        border-bottom-right-radius: 8px;
    }

    @media(max-width:768px) {
        .new-task {
            width: auto;
        }

        input.form-element {
            width: 75%;
        }
    }
</style>