<template>
    <div class="card" >
        <span class="card__item">{{card.date}}</span>
        <span class="card__item">{{card.body}}</span>
        <span class="card__item">{{card.quantity}}</span>
        <span class="card__item">{{card.distance}}</span>
    </div>
</template>

<script>

export default {
    props: {
        card: {
            type: Object,
            required: true
        }
    }
}
</script>

<style scoped>
.card {
  display: flex;
  flex-wrap: nowrap;
  justify-content: space-between;
  font-size: 24px;
}
.card__item {
  padding: 5px;
  width: 25%;
  text-align: center;
  border: 1px solid teal;
}
</style>