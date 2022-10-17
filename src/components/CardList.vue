<template>
    <div class="card__list" v-if="card.length > 0">
        <TransitionGroup name="list">
            <CardItem
                v-for="card in card"
                :key="card.id"
                :card="card"/>
        </TransitionGroup>
    </div>
    <h2 v-else>Не найдено...</h2>
</template>

<script>
import CardItem from './CardItem.vue';
export default {
    props: {
        card: {
            type: Array,
            required: true
        }
    },
    components: { CardItem }
}
</script>

<style scoped>
.list-move,
.list-enter-active,
.list-leave-active {
  transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
}
h2 {
    margin-top: 20px;
    color: teal;
    font-size: 24px;
}
</style>