<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <h4>Tambah Post</h4>
                        <hr>

                        <form @submit.prevent="store()">
                            <div class="form-group">
                                <label for="title" class="font-weight-bold">Title</label>
                                <input
                                  type="text"
                                  class="form-control"
                                  v-model="post.title"
                                  placeholder="Masukkan Judul Post"
                                >

                                <!-- Validation -->
                                <div v-if="validation.title" class="mt-2 alert alert-danger">
                                    {{ validation.title[0] }}
                                </div>
                            </div>

                            <div class="form-group mt-2">
                                <label for="content" class="font-weight-bold">Content</label>
                                <textarea 
                                    class="form-control" 
                                    placeholder="Masukkan Kontent Post" 
                                    rows="4"
                                    v-model="post.content"
                                ></textarea>

                                <!-- Validation -->
                                <div v-if="validation.content" class="mt-2 alert alert-danger">
                                    {{ validation.content[0] }}
                                </div>
                            </div>

                            <button type="submit" class="btn btn-primary mt-3">
                                Simpan
                            </button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { reactive, ref, inject } from 'vue'
import { useRouter } from 'vue-router'
import axios from 'axios'

export default {
    setup() {

        const url = inject('siteUrl')

        const post = reactive({
            title: '',
            content: ''
        })

        //validation
        const validation = ref([])

        //vue router
        const router = useRouter()

        function store()
        {
            let title = post.title
            let content = post.content

            axios.post(`${url}/api/post`, {
                title: title,
                content: content
            }).then(() => {

                router.push({
                    name: 'post.index'
                })

            }).catch(error => {
                validation.value = error.response.data
            })
        }

        return{
            post,
            validation,
            router,
            store
        }
    }
}
</script>

<style>
    body{
        background: lightgrey;
    }
</style>