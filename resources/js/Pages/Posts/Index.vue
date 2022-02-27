<template>
    <Head title="Posts" />

    <BreezeAuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Dashboard
            </h2>
        </template>

        <section class="bg-white">
            <div class="max-w-screen-xl px-4 py-16 mx-auto sm:px-6 lg:px-8">
                <div class="grid grid-cols-1 gap-x-16 gap-y-8 lg:grid-cols-5">
                    <div class="lg:py-12 lg:col-span-2">
                        <p class="max-w-xl font-extrabold text-4xl text-pink-600">
                            "Nothing is ever really lost to us as long as we remember it."
                        </p>

                        <div class="mt-2">
                            <address class="mt-2 not-italic">L.M. Montgomery</address>
                        </div>
                    </div>

                    <div class="p-8 bg-white rounded-lg shadow-lg lg:p-12 lg:col-span-3">
                        <form @submit.prevent="form.post('/posts', options)" :disabled="form.processing" class="space-y-4">
                            <div>
                                <label class="sr-only" for="title">Title</label>
                                <input v-model="form.title" class="w-full p-3 text-sm rounded-lg" placeholder="Title" type="text" id="title"/>
                                <div class="text-red-500" v-if="form.errors.title">{{ form.errors.title }}</div>
                            </div>

                            <div>
                                <label class="sr-only" for="content">Content</label>
                                <textarea
                                    v-model="form.content"
                                    class="w-full p-3 text-sm border-gray-200 rounded-lg"
                                    placeholder="Content"
                                    rows="8"
                                    id="Content"
                                ></textarea>
                                <div class="text-red-500" v-if="form.errors.content">{{ form.errors.content }}</div>
                            </div>

                            <div class="mt-4">
                                <button
                                    type="submit"
                                    class="inline-flex items-center justify-center w-full px-5 py-3 text-white bg-pink-400 hover:bg-pink-600 rounded-lg sm:w-auto">
                                    <span class="font-medium"> Save </span>

                                    <svg
                                        xmlns="http://www.w3.org/2000/svg"
                                        class="w-5 h-5 ml-3"
                                        fill="none"
                                        viewBox="0 0 24 24"
                                        stroke="currentColor"
                                    >
                                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3" />
                                    </svg>
                                </button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </section>

        <section class="text-gray-600 body-font bg-white">
            <div class="container px-5 py-24 mx-auto">
                <div class="flex flex-wrap -mx-4 -my-8">
                    <div class="py-8 px-4 lg:w-1/3" v-for="post in posts.data">
                        <div class="h-full flex items-start bg-gray-100 p-10 shadow-lg rounded">
                            <div class="w-12 flex-shrink-0 flex flex-col text-center leading-none">
                                <span class="text-gray-500 pb-2 mb-2 border-b-2 border-pink-500">{{ post.month }}</span>
                                <span class="font-medium text-lg text-gray-800 title-font leading-none">{{ post.day }}</span>
                            </div>
                            <div class="flex-grow pl-6">
                                <h2 class="text-xl title-font font-medium text-pink-600 mb-1">{{ post.title }}</h2>
                                <p class="leading-relaxed mb-5">{{ post.content }}</p>
                            </div>
                        </div>
                    </div>
                </div>
                <pagination :links="posts.meta.links" />
            </div>
        </section>



    </BreezeAuthenticatedLayout>
</template>

<script>
import BreezeAuthenticatedLayout from '@/Layouts/Authenticated.vue';
import { Head } from '@inertiajs/inertia-vue3'
import { useForm } from '@inertiajs/inertia-vue3'
import Pagination from "@/Components/Pagination";

export default {
    components: {
        Head,
        Pagination,
    },
    layout: BreezeAuthenticatedLayout,
    props: {
        posts: Object
    },
    setup () {
        const form = useForm({
            title: null,
            content: null,
        })

        const options = {
            preserveScroll: true,
            onSuccess: () => form.reset('title', 'content'),
        }

        return { form, options }
    },
}
</script>

<style scoped>

</style>
