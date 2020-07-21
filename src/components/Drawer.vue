<template>
    <div ref="container" :style="getHeight(1)" class="container fixed top-0 left-0 right-0 bg-black bg-opacity-25 overflow-y-scroll">
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
                Content
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
            windowHeight: window.innerHeight,
        }
    },
    mounted() {
        // Scrolls to wherever the parent component defines where it should go
        this.$nextTick(function () {
            // Code that will run only after the
            // entire view has been rendered
            this.$refs[1].scrollIntoView();
            if (this.position === 'top')
                this.scroll(3);
            else if (this.position == 'bottom')
                this.scroll(1);
            else
                this.scroll(2);
        })
        
        let view = this;
        setTimeout(() => {


            // Set observer for the visibility of button
            let observer = new IntersectionObserver((entries) => {
                if(entries[0].intersectionRatio <= .01) {
                    console.log('Closing')
                    view.$emit('close');
                }

            }, { threshold: .01 });
    
            observer.observe(view.$refs[3]);
        }, 200);
    },
    computed: {
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

/* Hide scrollbar for Chrome, Safari and Opera */
.container::-webkit-scrollbar {
  display: none;
}

/* Hide scrollbar for IE, Edge and Firefox */
.container {
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
}

.container > div {
    scroll-snap-align: start;
    scroll-snap-stop: always;
}
</style>