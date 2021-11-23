<template>
  <section>
    <header>
      <h1>My books</h1>
    </header>
    <book-form @add-book="addBook"></book-form>
    <ul>
      <book-card 
        v-for="book of bookList"
        :key="book.id"
        :book="book"
        @delete-book="deleteBook"
        ></book-card>
    
    </ul>
  </section>
</template>

<script>
import axios from 'axios';
const API_URL = "http://127.0.0.1:8000/api/books";
export default {
  data() {
    return {
      bookList:[
        {id: 1, title: 'Book1', description: 'My book 1'},
        {id: 2, title: 'Book2', description: 'My book 2'},
        {id: 3, title: 'Book3', description: 'My book 3'},
      ]
    };
  },
  methods: {
    deleteBook(bookId){
      axios.delete(API_URL + "/" + bookId).then(() => {
        this.bookList = this.bookList.filter((book) => book.id !== bookId);
       
      })
    
    },
    addBook(title, description){
      const newBook = {
        title: title,
        description: description
      }
     
      // this.bookList.push(newBook);
      axios.post(API_URL, newBook).then(res => {
        this.bookList.push(res.data.newBook);
        console.log(this.bookList)
      })
     
    }
  },
  mounted() {
    axios.get(API_URL).then(res => {
      this.bookList = res.data;
      console.log(res.data)
    })
  },
};
</script>

<style>
:root {
  --main-color: #3a2dfc;
}
* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}
header {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  margin: 3rem auto;
  border-radius: 10px;
  padding: 1rem;
  background-color: var(--main-color);
  color: white;
  text-align: center;
  width: 90%;
  max-width: 40rem;
}
ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
button {
  cursor: pointer;
  border: 1px solid var(--main-color);
  background-color: var(--main-color);
  color: white;
  padding: 0.5rem 1rem;
  box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.26);
}
button:hover,
button:active {
  box-shadow: 1px 2px 6px rgba(0, 0, 0, 0.26);
}
</style>