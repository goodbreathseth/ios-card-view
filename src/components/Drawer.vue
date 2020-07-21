<template>
    <div ref="container" :style="getHeight(1)" :class="getOpacity" class="container fixed top-0 left-0 right-0 bg-black bg-opacity-25 overflow-y-scroll transition-opacity duration-300">
        <div ref="0" class=" w-full" :style="getHeight(.2)" @click="$emit('close')"></div>
        <div ref="1" class=" w-full" :style="getHeight(.4)" @click="$emit('close')"></div>
        <div ref="2" class=" w-full" :style="getHeight(.4)" @click="$emit('close')"></div>
        <!-- White drawer -->
        <div ref="3" class="h-11/12-screen bg-white rounded-t-lg shadow-lg">

            <!-- Grey drag bar -->
            <button ref="btn" id="btn" @click="scroll(3)" class="p-3">
                <div class="bg-gray-300 h-2 w-20 rounded-full shadow-sm"></div>
            </button>

            <!-- Content of drawer -->
            <div ref="content" class="bg-orange-300 overflow-y-auto">

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
    data: function () {
        return {
            drawerMounted: false,
            windowHeight: window.innerHeight,
        }
    },
    mounted() {
        this.drawerMounted = true;
        
        let view = this;
        setTimeout(() => {
            // Scrolls to wherever the parent component defines where it should go
            this.$refs[1].scrollIntoView();
            if (this.position === 'top')
                this.scroll(3);
            else if (this.position == 'bottom')
                this.scroll(1);
            else
                this.scroll(2);


            // Set observer for the visibility of button
            let observer = new IntersectionObserver((entries) => {
                if(entries[0].intersectionRatio === 0)
                    view.$emit('close');
                else
                    console.log('second')

                // if(entries[0].intersectionRatio !== 1)
                //     view.$emit('close');
            }, { threshold: .00001 });
    
            observer.observe(view.$refs[3]);
        }, 200);
    },
    beforeDestroy() {
        // document.exitFullscreen();
    },
    computed: {
        getOpacity() {
            if (this.drawerMounted) 
                return 'opacity-100'
            else
                return 'opacity-0'
        }
    },
    methods: {
        scroll(elemId) {
            this.$refs[elemId].scrollIntoView({ 
                behavior: 'smooth' 
            });
        },
        togglePosition() {

        },
        getHeight(percent) {
            return `height: ${this.windowHeight * percent}px;`
        },
    }
}
</script>

<style scoped>
.container {
    scroll-snap-type: y mandatory;
    /* scroll-behavior: smooth; */
}

.container > div {
    scroll-snap-align: start;
    scroll-snap-stop: always;
}
</style>