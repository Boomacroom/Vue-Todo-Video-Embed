<template>

  <form @submit.prevent="handleSubmit">
      <label for="titleText">Title:</label>
      <input type="text" v-model="title"  name="titleText" id="" required>
      <label for="detailsText">Details:</label>
      <ckeditor name="detailsText" :editor="editor" v-model="details" :config="editorConfig"></ckeditor>
      <button>Update Project</button>
  </form>
</template>

<script>
import ClassicEditor from '@ckeditor/ckeditor5-build-classic';

export default {
    props: ['id'],
    data() {
        return {
            
            editor: ClassicEditor,
            editorConfig: {
                toolbar: [ 'heading', '|', 'bold', 'italic', 'link', 'bulletedList', 'numberedList', '|', 'insertTable', '|', 'imageUpload', 'mediaEmbed', '|', 'undo', 'redo' ],
                table: {
                    toolbar: [ 'tableColumn', 'tableRow', 'mergeTableCells' ]
                },
            plugins: ['MediaEmbed', 'Heading', 'Paragraph', 'Bold', 'Italic', 'Link', 'List','Table'],
            mediaEmbed: {previewsInData: true}
            },
            title: '',
            details: '',
            uri: 'http://localhost:3000/projects/' + this.id          
        }
    },
    methods:{
        handleSubmit() {
            fetch(this.uri, { 
                method: 'PATCH',
                headers: { 'Content-Type': 'application/json'},
                body: JSON.stringify({ 
                    title: this.title,
                    details: this.details
                    })
                }).then(() => {
                this.$router.push('/')
                
        }).catch(err => console.log(err))
    }
    },
    components: {
    },
    mounted() {
        fetch(this.uri)
          .then(res => res.json())
          .then(data => {
              this.title = data.title
              this.details = data.details
          })
    },
}
</script>

<style>
  form {
    background: white;
    padding: 20px;
    border-radius: 10px;
  }
  label {
    display: block;
    color: #bbb;
    text-transform: uppercase;
    font-size: 14px;
    font-weight: bold;
    letter-spacing: 1px;
    margin: 20px 0 10px 0
  }
  input {
    padding: 10px;
    border: 0;
    border-bottom: 1px solid #ddd;
    width: 100%;
    box-sizing: border-box;
  }
  textarea {
    border: 1px solid #ddd;
    padding: 10px;
    width: 100%;
    box-sizing: border-box;
    height: 100px;
  }
  form button {
    display: block;
    margin: 20px auto 0;
    background: #00ce89;
    color: white;
    padding: 10px;
    border: 0;
    border-radius: 6px;
    font-size: 16px;
  }
</style>