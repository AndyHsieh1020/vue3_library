<template>
    <div>
      <h2>Dashboard</h2>
      <h3>Borrowed Books</h3>
      <ul v-if="borrowedBooks.length">
        <li v-for="record in borrowedBooks" :key="record.id">
          {{ record.book.title }} (Borrowed on {{ record.borrowDate }})
          <button @click="returnBook(record)">Return</button>
        </li>
      </ul>
      <p v-else>No books borrowed</p>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        borrowedBooks: []
      };
    },
    mounted() {
      
      fetch('http://localhost:8080/api/borrowed-books')
        .then(response => response.json())
        .then(data => {
          this.borrowedBooks = data;
        })
        .catch(error => {
          console.error('Error fetching borrowed books:', error);
        });
    },
    methods: {
      returnBook(record) {
        
        fetch('http://localhost:8080/api/return', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ record })
        })
        .then(response => {
          if (response.ok) {
            alert('Book returned successfully');
            
            this.loadBorrowedBooks();
          } else {
            throw new Error('Failed to return book');
          }
        })
        .catch(error => {
          console.error('Error returning book:', error);
          alert('Failed to return book');
        });
      },
      loadBorrowedBooks() {
        
        fetch('http://localhost:8080/api/borrowed-books')
          .then(response => response.json())
          .then(data => {
            this.borrowedBooks = data;
          })
          .catch(error => {
            console.error('Error fetching borrowed books:', error);
          });
      }
    }
  };
  </script>
  