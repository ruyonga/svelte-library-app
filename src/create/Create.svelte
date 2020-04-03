<script>
  import { navigate } from 'svelte-routing'
  import BackButtonRow from "../common/BackButtonRow.svelte";
  import BookCover from "../common/BookCover.svelte";
  import Button from "../common/Button.svelte";
  import Header from "../common/Header.svelte";
  import { httpPost } from "../common/api.js";
  import TextInput from "./TextInput.svelte";


  let title = "";
  let author = "";
  let cover = "";
  let about = "";

  $: book  = { title, author, cover, about}

  async function handleSumbit(event) {
       function getRandomInt(min, max) {
        min = Math.ceil(min);
        max = Math.floor(max);
        return Math.floor(Math.random() * (max - min + 1)) + min;
    }
        const newBook  = 
            { ...book,
                 variation: getRandomInt(0,2), favorite: false}
        const {ok} =    await httpPost('/', newBook)
        if(ok) {
            navigate('/')
        }
  }

   

</script>

<style>
  form {
    display: grid;
    grid-auto-rows: auto;
    grid-template-columns: 1fr;
    gap: var(--spacingXLarge);
  }
  .fields {
    display: grid;
    grid-auto-rows: auto;
    gap: var(--spacingMedium);
  }
  .preview {
    display: grid;
    grid-template-columns: minmax(20vw, 10rem);
    grid-template-rows: minmax(32vw, 16rem);
  }
  @media (min-width: 48rem) {
    form {
      grid-template-columns: 60vw 20vw;
    }
  }
</style>

<BackButtonRow />

<Header element="h1" size="large">Create</Header>

<form on:submit|preventDefault={handleSumbit}>
  <div class="fields">
    <TextInput lable="Title" bind:value={title} />
    <TextInput lable="Author" bind:value={author} />
    <TextInput lable="Cover URL" bind:value={cover} />
    <TextInput lable="About" bind:value={about} multiline />
    <div>   
        <Button>Add</Button>
    </div>
  </div>

  <div>
    <Header>Preview</Header>
    <div class="preview" >
        <BookCover {book}/>
    </div>

  </div>
</form>
