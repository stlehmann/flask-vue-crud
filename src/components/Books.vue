<template>
  <div class="container">
    <div class="row">
      <div class="col-sm-10">
        <h1>Books</h1>
        <hr>
        <br><br>
        <alert ref="alert"></alert>
        <button type="button" class="btn btn-success btn-sm" @click="showModal">Add Book</button>
        <br><br>
        <table class="table table-hover">
          <thead>
          <tr>
            <th scope="col">Title</th>
            <th scope="col">Author</th>
            <th scope="col">Read</th>
            <th></th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="(book, index) in books" :key="index">
            <td>{{ book.title }}</td>
            <td>{{ book.author }}</td>
            <td>
              <span v-if="book.read">Yes</span>
              <span v-else>No</span>
            </td>
            <td>
              <div class="btn-group" role="group">
                <button type="button" class="btn btn-warning btn-sm" @click="editBook(book)">
                  Update
                </button>
                <button type="button" class="btn btn-danger btn-sm" @click="deleteBook(book)">
                  Delete
                </button>
              </div>
            </td>
          </tr>
          </tbody>
        </table>
      </div>
    </div>
    <add_update_book_modal ref="add_update_book_modal"
                           @submitted="bookSubmitted"></add_update_book_modal>
  </div>
</template>

<script>
import axios from 'axios';
import Alert from '@/components/Alert.vue';
import AddUpdateBookModal from '@/components/AddUpdateBookModal.vue';

function createPayloadFromBook(book) {
  let read = false;
  if (book.read[0]) read = true;
  const payload = {
    title: book.title,
    author: book.author,
    read,
  };
  return payload;
}

export default {
  name: 'books-item',
  data() {
    return {
      books: [],
    };
  },
  components: {
    alert: Alert,
    add_update_book_modal: AddUpdateBookModal,
  },
  methods: {
    showModal() {
      this.$refs.add_update_book_modal.show();
    },
    editBook(book) {
      this.$refs.add_update_book_modal.show(book);
    },
    deleteBook(book) {
      const path = `http://localhost:5000/books/${book.id}`;
      axios.delete(path)
        .then((res) => {
          this.$refs.alert.show(res.data.message);
          this.getBooks();
        })
        .catch((error) => {
          this.$refs.alert.show(error, 'danger');
        });
    },
    getBooks() {
      const path = 'http://localhost:5000/books';
      axios.get(path)
        .then((res) => {
          this.books = res.data.books;
        })
        .catch((error) => {
          this.$refs.alert.show(error, 'danger');
        });
    },
    bookSubmitted(book) {
      const payload = createPayloadFromBook(book);

      // Update existing book
      if (book.id) {
        const path = `http://localhost:5000/books/${book.id}`;
        axios.put(path, payload)
          .then((res) => {
            this.$refs.alert.show(res.data.message);
            this.getBooks();
          })
          .catch((error) => {
            this.$refs.alert.show(error, 'danger');
          });
      // Add a new book
      } else {
        const path = 'http://localhost:5000/books';
        axios.post(path, payload)
          .then((res) => {
            this.$refs.alert.show(res.data.message);
            this.getBooks();
          })
          .catch((error) => {
            this.$refs.alert.show(error, 'danger');
          });
      }
    },
  },
  created() {
    this.getBooks();
  },
};
</script>

<style scoped>

</style>
