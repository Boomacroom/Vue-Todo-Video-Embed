<template>

  <form @submit.prevent="handleSubmit">
      <label for="titleText">Title:</label>
      <input type="text" v-model="title"  name="titleText" id="" required>
      <label for="detailsText">Details:</label>
      <ckeditor name="detailsText" :editor="editor" v-model="details" :config="editorConfig"></ckeditor>
      <button>Add Project</button>
  </form>
</template>

<script>
import ClassicEditor from '@ckeditor/ckeditor5-build-classic';

export default {
    data() {
        return {
            title: '',
            details: '',
            editor: ClassicEditor,
            editorConfig: {
                
                plugins: ['MediaEmbed'],
                toolbar: [ 'heading', '|', 'bold', 'italic', 'link', 'bulletedList', 'numberedList', '|', 'insertTable', '|', 'imageUpload', 'mediaEmbed', '|', 'undo', 'redo' ],
                table: {
                    toolbar: [ 'tableColumn', 'tableRow', 'mergeTableCells' ]
                },
                mediaEmbed: {previewsInData: true}
            },
    
        }
    },
    components: {
    },
    methods: {
        handleSubmit() {
            let project = {
                title: this.title,
                details: this.details,
                complete: false
            }
            fetch('http://localhost:3000/projects', {
                method: 'POST',
                headers: {'Content-Type': 'application/json'},
                body: JSON.stringify(project)
            }).then(() => {
                this.$router.push('/')
            }).catch((err) => console.log(err))
            
        }
    }

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