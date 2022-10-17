<template>
  <div class="app">
    <h1 class="app__title">Тестовое задание на позицию разработчика</h1>
    <div class="app__header">
      <div class="app__select">
        <SelectCard
        v-model="selectCard"
        :options="searchOptions"
      ></SelectCard>
      <SelectCardInput
        v-model="selectCardInput"
        :options="searchInput"
      >
      </SelectCardInput>
      </div>
      <MyInput
        v-model="searchQuery"
      ></MyInput>
      <Header></Header>
    </div>
    <div>
      <CardList
        :card="sortedCard"/>
    </div>
  </div>
</template>

<script>

import data from '@/data/data'
import CardList from './components/CardList.vue';
import Header from './components/Header.vue';
import SelectCard from './components/SelectCard.vue';
import MyInput from './components/MyInput.vue';
import SelectCardInput from './components/SelectCardInput.vue';


export default {
    components: { CardList, Header, SelectCard, MyInput, SelectCardInput, },
    data() {
        return {
            card: [],
            selectCard: '',
            selectCardInput: '',
            searchQuery: '',
            searchOptions: [
              {value: 'body', name: 'По названию'},
              {value: 'quantity', name: 'По количеству'},
              {value: 'distance', name: 'По расстоянию'}
            ],
            searchInput: [
              {value: 'contains', name: 'Содержит'},
              {value: 'more', name: 'Больше'},
              {value: 'less', name: 'Меньше'}
            ],
            currentPage: 1,
        };
    },
    methods: {
      createData(){
         data.card.forEach(item => {
          this.card.push(item)
        })
      },
      onPageChange(page) {
        console.log(page)
        this.currentPage = page;
      }
      
    },
    mounted() {
      this.createData()
      
    },
    computed: {
      sortedCard() {
        if(this.selectCard === '' || this.selectCardInput === '') {
          return [...this.card]
        } 
         else if ((this.selectCard === 'quantity' || this.selectCard === 'distance') && this.selectCardInput === 'less') {
          return [...this.card].sort( (a, b) => a[this.selectCard] - b[this.selectCard] )
        }
         else if ((this.selectCard === 'quantity' || this.selectCard === 'distance') && this.selectCardInput === 'more') {
          return [...this.card].sort( (a, b) => b[this.selectCard] - a[this.selectCard] )
        }
        else if((this.selectCard === 'body' || this.selectCard === 'quantity' || this.selectCard === 'distance') && this.selectCardInput === 'contains') {
          return [...this.card].filter(card => card[this.selectCard].toString().toLowerCase().includes(this.searchQuery.toLowerCase()))
        }
        else if(this.selectCard === 'body' && this.selectCardInput === 'more') {
          return [...this.card].sort( (a, b) => b[this.selectCard]?.localeCompare(a[this.selectCard]) )
        }
        else if(this.selectCard === 'body' && this.selectCardInput === 'less') {
          return [...this.card].sort( (a, b) => a[this.selectCard]?.localeCompare(b[this.selectCard]) )
        }
      },    
    }
    
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.app {
  max-width: 1200px;
  margin: 0 auto;
}
.app__title {
  text-align: center;
}
.app__header {
  display: flex;
  flex-direction: column;
}
</style>
