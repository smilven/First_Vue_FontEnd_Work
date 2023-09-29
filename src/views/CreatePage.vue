<template>
  <form action="" class="container mb-3">
    <div class="row">
      <div class="col">
        <div class="mb-3">
          <label for="" class="form-label"> Page Title </label>
          <input type="text" class="form-control" name="" v-model="pageTitle" />
        </div>
        <div class="mb-3">
          <label for="" class="form-label"> Content </label>
          <textarea name="" type="text" v-model="content" id="" cols="30" class="form-control" rows="5"></textarea>
        </div>
      </div>

      <div class="col">
        <div class="mb-3">
          <label for="" class="form-label"> Link text </label>
          <input type="text" class="form-control" v-model="linkText" />
        </div>


        <div class="row mb-3">
          <div class="form-check">
            <input type="checkbox" class="form-check-input" v-model="published" />
            <label class="form-check-label" for="gridCheck1"> Published </label>
          </div>
        </div>
      </div>
    </div>
    <button class="btn btn-primary" @click.prevent="submitForm" :disabled="isFormInvalid">
      Create Page
    </button>
  </form>
</template>



<script setup>
import { inject, ref, computed, watch } from 'vue';
import { useRouter } from 'vue-router';
const bus = inject('$bus');
const pages = inject('$pages');
const router = useRouter();


let pageTitle = ref('');
let content = ref('');
let linkText = ref('');
let published = ref(true);


function submitForm() {
  if (!pageTitle || !content || !linkText) {
    alert('Please fill the form');
    return;
  }

  let newPage = {
    pageTitle: pageTitle.value,
    content: content.value,
    link: {
      text: linkText.value,

    },
    published: published.value

  }

  pages.addPage(newPage)



  bus.$emit('page-created', newPage);

  router.push({ path: '/pages' });

}

const isFormInvalid = computed(() => !pageTitle || !content || !linkText);
watch(pageTitle, (newTitle, oldTitle) => {
  if (linkText.value == oldTitle) {
    linkText.value = newTitle;
  }
});
</script>


