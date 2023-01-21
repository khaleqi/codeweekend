<template>
  <div class="container">
    <Header @toggle-add-book="toggleAddTask" :showAddBook="showAddBook" />
    <div v-show="showAddBook">
      <AddBook @add-book="addBook" />  
    </div>
    <Books :books="books" @toggle-read="toggleRead" @delete-book="deleteBook"/>
  </div>
</template>

<script>
import Header from './Header.vue'
import Books from './Books.vue'
import AddBook from './AddBook.vue'

export default {
  name: 'App',
  components: {
    Header,
    Books,
    AddBook
  },
  data() {
    return {
      books: [],
      showAddBook: false
    }
  },
  methods: {
    addBook(book) {
      this.books = [...this.books, book]
    },
    deleteBook(id) {
      if(confirm('Are you sure you want to delet book?')) {
        this.books = this.books.filter((book) => book.id !== id);
      }
    },
    toggleRead(id) {
      this.books = this.books.map((book) => book.id === id? {...book, read: !book.read } : book)
    },
    toggleAddTask() {
      this.showAddBook = !this.showAddBook
    },
    async fetchBooks() {
      console.log('test');
      

      var config = {
        method: 'get',
        url: 'https://q6t57xov6d.execute-api.eu-west-1.amazonaws.com/dev/books',
        headers: { }
      };

      axios(config)
      .then(function (response) {
        console.log(JSON.stringify(response.data));
      })
      .catch(function (error) {
        console.log(error);
      });
      
    },
    async oldFetchBooks() {
      // const res = await fetch('https://q6t57xov6d.execute-api.eu-west-1.amazonaws.com/dev/books?bookId=7')x
      let res = await fetch('https://q6t57xov6d.execute-api.eu-west-1.amazonaws.com/dev/books', {
        method: 'GET',
        headers: {
          // 'Content-Type': 'application/json',
          'Access-Control-Allow-Origin': '*',
          'Access-Control-Allow-Headers': '*',
          'Access-Control-Allow-Methods': '*'
        }
      });
      const data = await res.json()
      return data
    }
  },
  
  
  async created() {
    var myHeaders = new Headers();
    myHeaders.append("Access-Control-Allow-Origin", "*");
    myHeaders.append("Access-Control-Allow-Headers", "*");
    // myHeaders.append("Access-Control-Allow-Credentials", true);
    

    var requestOptions = {
      method: 'GET',
      headers: myHeaders,
      redirect: 'follow'
    };

    const res = fetch("placeHolder", requestOptions)
      .then(response => response.text())
      .then(result => console.log(result))
      .catch(error => console.log('error', error));
    return res;
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Poppins', sans-serif;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}
</style>
