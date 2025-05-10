<template>
    <form class="mt-3" @submit.prevent="submitForm">
      <div class="container mb-3">
        <div class="row">
          <div class="col-md-6">
            <div class="mb-3">
              <label for="pageTitle" class="form-label">Page Title</label>
              <input 
                type="text"
                class="form-control"
                id="pageTitle"
                v-model="pageTitle"
              >
            </div>
            <div class="mb-3">
              <label for="content" class="form-label">Content</label>
              <textarea 
                class="form-control"
                id="content"
                rows="5"
                v-model="content"
              ></textarea>
            </div>
          </div>
  
          <div class="col-md-6">
            <div class="mb-3">
              <label for="linkText" class="form-label">Link Text</label>
              <input 
                type="text"
                class="form-control"
                id="linkText"
                v-model="linkText"
              >
            </div>
            <div class="mb-3 form-check">
                <input class="form-check-input" type="checkbox" v-model="published">
                <label class="form-check-label" for="gridCheck1">
                    Published
                </label>
            </div>
          </div>
        </div>
  
        <div class="row">
          <div class="col">
            <button
              class="btn btn-primary mt-3"
              :disabled="isFormInvalid"
            >
              Create Page
            </button>
          </div>
        </div>
      </div>
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
  if(!pageTitle || !content || !linkText) {
      alert('Please fill the form.');
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

  router.push({path: '/pages'});
}

const isFormInvalid = computed(() => !pageTitle || !content || !linkText);

watch(pageTitle, (newTitle, oldTitle) => {
  if (linkText.value === oldTitle) {
      linkText.value = newTitle;
  }
});

</script>

<!-- <script>
    export default {
        emits used to define the events that the component can emit and here I write validation for the event
        emits: {
            pageCreated({pageTitle, content, link}) {
                if (!pageTitle) {
                    return false;   
                }
                if (!content) {
                    return false;   
                }
                if (!link || !link.text || !link.url) {
                    return false;
                }

                return true;
            }
        },
        computed: {
            isFormInvalid() {
                return !this.pageTitle || !this.content || !this.linkText || !this.linkUrl;
            }
        },
        data() {
            return {
                pageTitle: '',
                content: '',
                linkText: '',
                linkUrl: '',
                published: true
            }
        },
        methods: {
            submitForm() {
                if(!this.pageTitle || !this.content || !this.linkText || !this.linkUrl) {
                    alert('Please fill the form.');
                    return;
                }

                this.$emit('pageCreated', {
                    pageTitle: this.pageTitle,
                    content: this.content,
                    link: {
                        text: this.linkText,
                        url: this.linkUrl
                    },
                    published: this.published
                })

                this.pageTitle = '';
                this.content = '';
                this.linkText = '';
                this.linkUrl = '';
                this.published = true;
            }
        },
        watch: {
            pageTitle(newTitle, oldTitle) {
              if (this.linkText === oldTitle) {
                  this.linkText = newTitle;
              }
            }
        }
    }
</script> -->