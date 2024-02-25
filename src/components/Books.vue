<template>
    <div>
      <h2>Books</h2>
      <ul>
        <li v-for="book in books" :key="book.id">
          {{ book.title }} by {{ book.author }}
          <button @click="borrowBook(book)">Borrow</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        books: []
      };
    },
    mounted() {
      
      fetch('http://localhost:8080/api/books')
        .then(response => response.json())
        .then(data => {
          this.books = data;
        })
        .catch(error => {
          console.error('Error fetching books:', error);
        });
    },
    methods: {
      borrowBook(book) {
        
        fetch('http://localhost:8080/api/borrow', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ book })
        })
        .then(response => {
          if (response.ok) {
            alert('Book borrowed successfully');
            
            this.loadBooks();
          } else {
            throw new Error('Failed to borrow book');
          }
        })
        .catch(error => {
          console.error('Error borrowing book:', error);
          alert('Failed to borrow book');
        });
      },
      loadBooks() {
        
        fetch('http://localhost:8080/api/books')
          .then(response => response.json())
          .then(data => {
            this.books = data;
          })
          .catch(error => {
            console.error('Error fetching books:', error);
          });
      }
    }
  };
  </script>
  