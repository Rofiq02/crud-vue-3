<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <h4>Edit Post</h4>
                        <hr>

                        <form @submit.prevent="update()">
                            <div class="form-group">
                                <label for="title" class="font-weight-bold">Title</label>
                                <input type="text"
                                       class="form-control"
                                       v-model="post.title"
                                       placeholder="Masukkan Judul Post"
                                >

                                <div v-if="validation.title"
                                     class="mt-2 alert alert-danger"
                                >
                                    {{ validation.title[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="content" class="font-weight-bold">Content</label>
                                <textarea class="form-control" 
                                          rows="4"
                                          v-model="post.content"
                                          placeholder="Masukkan Kontent Post"
                                >
                                </textarea>

                                <div v-if="validation.content"
                                     class="mt-2 alert alert-danger"
                                >
                                    {{ validation.content[0] }}
                                </div>
                            </div>
                            <button type="submit" class="btn btn-primary">Simpan</button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { reactive, ref, onMounted, inject } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import axios from 'axios'

export default {

    setup() {

        const url = inject('siteUrl');

        const post = reactive({
            title: '',
            content: ''
        })

        const validation = ref([])

        const router = useRouter()

        const route = useRoute()

        onMounted(() => {
            axios.get(`${url}/api/post/${route.params.id}`)
            .then(response => {

                post.title      = response.data.data.title
                post.content    = response.data.data.content

            }).catch(error => {
                console.log(error.response.data)
            })
        })

        function update()
        {
            let title       = post.title
            let content     = post.content

            axios.put(`${url}/api/post/${route.params.id}`, {
                title:  title,
                content: content
            }).then(() => {

                router.push({
                    name: 'post.index'
                })

            }).catch(error => {
                validation.value = error.response.data
            })
        }

        return {
            post,
            route,
            router,
            validation,
            update
        }
    }
}
</script>

<style>
    body{
        background: lightgrey;
    }
</style>