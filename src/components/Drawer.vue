<!-- 
    Draggable drawer

    @props
        position - values: 'top', 'middle', 'bottom'

    Example usage:
    <Drawer v-if="showDrawer" @close="showDrawer = false">
        <div>
          Content
        </div>
    </Drawer>
 -->
<template>
    <div ref="container" :style="getHeight(1)" :class="getOpacity" class="container fixed top-0 left-0 right-0 bg-black bg-opacity-25 overflow-y-scroll transition-opacity duration-500">
        <div ref="0" class=" w-full" :style="getHeight(.2)" @click="close()"></div>
        <div ref="1" class=" w-full" :style="getHeight(.4)" @click="close()"></div>
        <div ref="2" class=" w-full" :style="getHeight(.4)" @click="close()"></div>
        <!-- White drawer -->
        <div ref="3" class="h-11/12-screen bg-white rounded-t-xl shadow-lg">

            <!-- Grey drag bar -->
            <button ref="btn" id="btn" @click="scroll(3)" class="py-2 px-5 mb-4 focus:outline-none">
                <div class="bg-gray-300 h-2 w-12 rounded-full shadow-sm"></div>
            </button>

            <!-- Content of drawer -->
            <div ref="content" class="h-full bg-white overflow-y-auto">
                <slot></slot>
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
            visible: false,
        }
    },
    mounted() {
        // Scrolls to wherever the parent component defines where it should go
            setTimeout(() => {
                this.visible = true;
                this.$refs[1].scrollIntoView();
                if (this.position === 'top')
                    this.scroll(3);
                else if (this.position == 'bottom')
                    this.scroll(1);
                else
                    this.scroll(2);
                
            }, 50);
        
        let view = this;
        setTimeout(() => {

            // Set observer for the visibility of button
            let observer = new IntersectionObserver((entries) => {
                if(entries[0].intersectionRatio <= .01) {
                    view.$emit('close');
                }
            }, { threshold: .01 });
            observer.observe(view.$refs[3]);


        }, 200);
    },
    computed: {
        getOpacity() {
            if (this.visible) {
                return 'opacity-100'
            }
            else
                return 'opacity-0'
        },
    },
    methods: {
        close() {
            this.$emit('close')
        },
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
    /* scroll-behavior: smooth; */
    scroll-snap-type: y mandatory;

    /* Hide scrollbar for IE, Edge and Firefox */
    -ms-overflow-style: none;  /* IE and Edge */
    scrollbar-width: none;  /* Firefox */
}

.container::-webkit-scrollbar {
    /* Hide scrollbar for Chrome, Safari and Opera */
    display: none !important;
}

.container > div {
    scroll-snap-align: start;
    scroll-snap-stop: always;
}
</style>