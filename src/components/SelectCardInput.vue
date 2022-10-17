<template>
    <select v-model="modalValue" @change="conditionOption">
        <option disabled value="">Выберите из списка</option>
        <option 
            v-for="option in options" 
            :key="option.value" 
            :value="option.value"
            >
            {{ option.name }}
            </option>
    </select>
</template>

<script>
export default {
    props: {
        modalValue: {
            type: String
        },
        options: {
            type: Array,
            default: () => []
        }
    },
    methods: {
        conditionOption(event) {
            this.$emit('update:modelValue', event.target.value)
        }
    }
}
</script>

<style scoped>
select {
    margin: 50px 20px 15px 0;
    width: 200px;
    height: 30px;
    font-size: 16px;
    border: 2px solid teal;
    outline: none;
}
</style>