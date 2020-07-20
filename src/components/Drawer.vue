<template>
    <div class="container fixed top-0 bottom-0 left-0 right-0 bg-black bg-opacity-25 overflow-y-auto">
        <div id="1" class="h-2/5-screen w-full bg-green-400" @click="$emit('close')"></div>
        <div id="2" class="h-2/5-screen w-full bg-red-300" @click="$emit('close')"></div>
        <!-- White drawer -->
        <div class="h-11/12-screen bg-white">

            <!-- Grey drag bar -->
            <button @click="scroll(1)" class="p-3">
                <div class="bg-gray-300 h-2 w-20 rounded-full shadow-sm"></div>
            </button>

            <!-- Content of drawer -->
            <div id="3" class="bg-orange-300 overflow-y-auto">

            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        position: {
            type: String,
            default: 'middle',
        }
    },
    mounted() {
        if (this.position === 'top')
            this.scroll(3);
        else if (this.position == 'bottom')
            this.scroll(1);
        else
            this.scroll(2);
    },
    methods: {
        scroll(elemId) {
            console.log("scrollNum:", elemId)
            console.log('position:', this.position)
            document.getElementById(elemId).scrollIntoView({ 
                behavior: 'smooth' 
            });
        },
        
    }
}
</script>

<style scoped>
.container {
    scroll-snap-type: y mandatory;
    scroll-behavior: smooth;
}

.container > div {
    scroll-snap-align: start;
    scroll-snap-stop: always;
}
</style>