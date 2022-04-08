<template>
  <b-modal ref="bookModal" id="bookModal" hide-footer :title="title">
    <b-form @submit="onSubmit" @reset="onReset" class="w-100">
      <b-form-group id="form-title-group" label="Title:" label-for="form-title-input">
        <b-form-input id="form-title-input" type="text" v-model="book.title" required
                      placeholder="Enter title"></b-form-input>
      </b-form-group>
      <b-form-group id="form-author-group" label="Author:" label-for="form-author-input">
        <b-form-input id="form-author-input" type="text" v-model="book.author" required
                      placeholder="Enter author"></b-form-input>
      </b-form-group>
      <b-form-group id="form-read-group">
        <b-form-checkbox-group v-model="book.read" id="form-checks">
          <b-form-checkbox value="true">Read?</b-form-checkbox>
        </b-form-checkbox-group>
      </b-form-group>
      <b-button type="submit" variant="primary">Submit</b-button>
      <b-button type="reset" variant="danger">Reset</b-button>
    </b-form>
  </b-modal>
</template>

<script>

export default {
  name: 'AddUpdateBookModalComponent',
  data() {
    return {
      title: '',
      book: {
        id: '',
        title: '',
        author: '',
        read: [],
      },
    };
  },
  components: {
  },
  methods: {
    show(book) {
      if (book) {
        this.title = 'Update book';
        this.book.id = book.id;
        this.book.title = book.title;
        this.book.author = book.author;
        this.book.read[0] = book.read;
      } else {
        this.title = 'Add book';
        this.book.id = '';
        this.book.title = '';
        this.book.author = '';
        this.book.read = [];
      }
      this.$refs.bookModal.show();
    },

    onReset(evt) {
      evt.preventDefault();
      this.$refs.bookModal.hide();
    },

    onSubmit(evt) {
      evt.preventDefault();
      this.$refs.bookModal.hide();
      this.$emit('submitted', this.book);
    },
  },
};
</script>

<style scoped>
.btn {
  margin-right: 5px;
}
</style>
