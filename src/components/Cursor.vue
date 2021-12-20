<template>
    <div class="cursors">
        <div class="cursor" :key="playerCursor"> {{ playerCursor }} </div>
    </div>
</template>

<script>
export default {
    name: "Cursor",
    props: ["player"],
    data() {
        return {
            playerCursor: ""
        }
    },
    mounted() {
        document.addEventListener("mousemove", function(event) {
            document.querySelector(".cursor").style.left = event.pageX + "px";
            document.querySelector(".cursor").style.top = event.pageY + "px";
        })
    },
    computed() {
        if (this.$props.player === 1) {
            this.playerCursor = "X";
        } else if (this.$props.player === -1) {
            this.playerCursor = "O";
        } else {
            this.playerCursor = ".";
        }
    }
}
</script>

<style>
div.cursors div {
  position: absolute;
  z-index: 100;
  pointer-events: none;
  transform: translate(-50%, -50%);
  opacity: 0.7;
}
.cursor {
    font-size: 6rem;
}
</style>