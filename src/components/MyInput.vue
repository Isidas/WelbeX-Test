<template>
    <input :value="modelValue" @input="updateInput" class="input" type="text" placeholder="Поиск...">
</template>

<script>

export default {
    props: {
        modelValue: [String, Number]
    },
    methods: {
        updateInput(event) {
            this.$emit('update:modelValue', event.target.value)
        }
    }
}
</script>

<style >
.input {
    display: inline-block;
    width: 20%;
    border: 2px solid teal;
    padding: 10px 15px;
    margin-top: 15px;
    outline: none;
}
</style>