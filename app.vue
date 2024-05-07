<template>
  <div>
      <button @click="handleClick">
          click
      </button>
  </div>
</template>

<script setup lang="ts">
let ws: WebSocket | undefined;
const connect = async () => {
    const isSecure = location.protocol === "https:";
    const url = (isSecure ? "wss://" : "ws://") + location.host + "/_ws";
    if (ws) {
        ws.close();
    }

    ws = new WebSocket(url);

    ws.addEventListener("message", (event) => {
        console.log(event.data)
    });

    await new Promise((resolve) => ws!.addEventListener("open", resolve));
};

const handleClick = () => {
    ws!.send("ping");
}

onMounted(async () => {
    connect();
});
</script>
