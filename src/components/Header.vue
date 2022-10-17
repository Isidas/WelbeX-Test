<template>
    <div class="header">
      <div class="header__item">Дата</div>
      <div class="header__item">Название</div>
      <div class="header__item">Количество</div>
      <div class="header__item">Расстояние</div>
    </div>
</template>

<script>
export default {
    
}
</script>

<style scoped>
.header {
  margin-top: 50px;
  display: flex;
  justify-content: space-between;
  
}
.header__item {
  padding: 20px;
  text-align: center;
  font-size: 24px;
  color: white;
  width: 25%;
  height: 100%;
  background: teal;
  border: 2px solid white;
}
</style>