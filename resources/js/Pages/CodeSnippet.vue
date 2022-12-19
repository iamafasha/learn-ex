<template>


    <Head>
    <title>Code Snippet</title>
    <meta name="description" content="Your page description">
    </Head>
    <Editor
        :value="`
    console.log('HelloWorld')
    console.log('HelloWorld')
    console.log('HelloWorld')
    `">
    </Editor>
</template>


<script setup>
import { Head } from '@inertiajs/inertia-vue3'

import Editor from '../components/Editor/Index.vue';

</script>
