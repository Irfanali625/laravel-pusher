<script setup>
import { ref, onMounted } from "vue";

defineProps({
    canLogin: {
        type: Boolean,
    },
    canRegister: {
        type: Boolean,
    },
    laravelVersion: {
        type: String,
        required: true,
    },
    phpVersion: {
        type: String,
        required: true,
    },
});

Pusher.logToConsole = true;

const messages = ref("");

const pusher = new Pusher("270a2fe7115609061c33", {
    cluster: "ap2",
});

const channel = pusher.subscribe("my-channel-test");
channel.bind("my-event-test", (data) => {
    // messages.value.push(JSON.stringify(data));
    messages.value = data.message;

    setTimeout(() =>{
        messages.value = ''
    }, 5000)
});

onMounted(() => {});
</script>

<template>
    <Head title="Welcome" />
    <h1>Pusher Test</h1>
    <p>
        Publish an event to channel <code>my-channel</code> with event name
        <code>my-event</code>; it will appear below:
    </p>

    <div class="flex justify-end p-4 fixed top-0 right-0" v-if="messages">
        <div class="bg-blue-500 text-white p-4 rounded shadow">
            {{ messages }}
        </div>
    </div>
</template>

<style></style>
