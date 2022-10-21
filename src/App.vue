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
        :card="sortedCard"
        v-if="!isLoading"/>
        <div
          v-else
          >Идет загрузка...
        </div>
    </div>
    <div class="app__pages">
      <div class="page"
      v-for="pageNumber in totalPages"
      :key="pageNumber"
      :class="{
        'current-page': page === pageNumber
      }"
      @click="changePage(pageNumber)"
      >{{ pageNumber }}</div>
    </div>
  </div>
</template>

<script>


import CardList from './components/CardList.vue';
import Header from './components/Header.vue';
import SelectCard from './components/SelectCard.vue';
import MyInput from './components/MyInput.vue';
import SelectCardInput from './components/SelectCardInput.vue';
import axios from 'axios';


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
            isLoading: false,
            page: 1,
            limit: 10,
            totalPages: 0
        };
    },
    methods: {
      changePage(pageNumber) {
        this.page = pageNumber
        this.fetchCard()
      },
      async fetchCard() {
        try {
          this.isLoading = true
          const response = await axios.get('http://localhost:3000/card', {
            params: {
              _page: this.page,
              _limit: this.limit
            }
          })
          this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
          this.card = response.data
        }
        catch(e) {
          alert('Error message')
        }
        finally {
          this.isLoading = false
        }
      }
    },
    mounted() {
     this.fetchCard()
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
.app__pages {
  text-align: center;
  margin: 10px auto;
  display: flex;
}
.page {
  cursor: pointer;
  margin: 0 3px;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 20px;
  height: 20px;
  font-size: 16px;
  padding: 3px;
  border-radius: 50%;
  background-color: teal;
  color: white;
  font-weight: 600;
}
.current-page {
  color: teal;
  background-color: inherit;
  border: 1px solid teal;
}
</style>
