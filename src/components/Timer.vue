<template>
    <div class="timer">
        Time : {{ timerCount }}
    </div> 
</template>

<script>

    export default {

        data() {
            return {
                timerCount: 60
            }
        },

        watch: {

            timerCount: {
                handler(value) {

                    if (value > 0) {
                        setTimeout(() => {
                            this.timerCount--;
                        }, 1000);
                    }

                },
                immediate: true // This ensures the watcher is triggered upon creation
            }

        }
    }

</script>

<style>
.timer {
  margin: 30px;
  font-weight: bold;
  justify-content: center;
  position: absolute;
}
</style>

