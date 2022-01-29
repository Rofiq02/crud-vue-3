<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <h4>Data Post</h4>
                        <hr>
                        <router-link 
                            :to="{ name: 'post.create' }"
                            class="btn btn-md btn-success">
                            Tambah Post
                        </router-link>

                        <table class="table table-striped table-bordered mt-4">
                            <thead class="thead-dark">
                                <tr>
                                    <th scope="col">Title</th>
                                    <th scope="col">Content</th>
                                    <th scope="col">Options</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(post, index) in posts" :key="index">
                                    <td>{{ post.title }}</td>
                                    <td>{{ post.content }}</td>
                                    <td>
                                        <router-link :to="{ name: 'post.edit', params:{id: post.id }}"
                                                    class="btn btn-sm btn-primary"
                                        >
                                            Edit
                                        </router-link>
                                        <button @click="postDelete(post.id)" class="btn btn-sm btn-danger mx-2">
                                            Delete
                                        </button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { onMounted, ref, inject } from 'vue' 

export default {
    setup(){
        const url = inject('siteUrl')
        let posts = ref([])

        onMounted(() => {
            axios.get(`${url}/api/post`)
                .then(response => {
                    posts.value = response.data.data
                }).catch(error => {
                    console.log(error)
                })
        })

        function postDelete(id)
        {
            axios.delete(`${url}/api/post/${id}`)
                .then(() => {

                    posts.value.splice(posts.value.indexOf(id), 1)

                }).catch(error => {
                    console.log(error.response.data)
                })
        }

        return {
            posts,
            postDelete
        }
    }
}
</script>

<style>

</style>

