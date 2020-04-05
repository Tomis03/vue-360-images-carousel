<template>
    <div id="app">
        <div class="main-image">
            <transition name="fade">
                <v-img
                    :src="images[mainImageIndex].large"
                    :lazy-src="images[mainImageIndex].small"
                    :key="`image${mainImageIndex}`"
                    contain
                    width="100%"
                    height="400"
                >
                    <template v-slot:placeholder>
                        <div class="progress-circular">
                            <v-progress-circular indeterminate></v-progress-circular>
                        </div>
                    </template>
                </v-img>
            </transition>
            <div class="buttons">
                <v-btn large icon outlined @click="prevSlide">
                    <v-icon large>mdi-chevron-left</v-icon>
                </v-btn>
                <v-btn icon large outlined @click="nextSlide">
                    <v-icon large>mdi-chevron-right</v-icon>
                </v-btn>
            </div>
        </div>
        <div v-show="$vuetify.breakpoint.smAndUp" class="perspective">
            <div class="carousel" ref="carousel">
                <div
                    v-for="(image, index) in images"
                    :key="`image${index}`"
                    class="image-container"
                    @click="changeSlide(index)"
                >
                    <v-img :src="image.small" contain max-width="186" max-height="116"></v-img>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "app",
    data() {
        return {
            images: [
                {
                    small: "https://picsum.photos/id/11/160/90",
                    large: "https://picsum.photos/id/11/1600/900"
                },
                {
                    small: "https://picsum.photos/id/120/160/90",
                    large: "https://picsum.photos/id/120/1600/900"
                },
                {
                    small: "https://picsum.photos/id/480/160/90",
                    large: "https://picsum.photos/id/480/1600/900"
                },
                {
                    small: "https://picsum.photos/id/500/100/90",
                    large: "https://picsum.photos/id/500/1000/900"
                },
                {
                    small: "https://picsum.photos/id/15/160/90",
                    large: "https://picsum.photos/id/15/1600/900"
                },
                {
                    small: "https://picsum.photos/id/88/160/90",
                    large: "https://picsum.photos/id/88/1600/900"
                },
                {
                    small: "https://picsum.photos/id/620/160/90",
                    large: "https://picsum.photos/id/620/1600/900"
                },
                {
                    small: "https://picsum.photos/id/621/160/90",
                    large: "https://picsum.photos/id/621/1600/900"
                },
                {
                    small: "https://picsum.photos/id/123/160/90",
                    large: "https://picsum.photos/id/123/1600/900"
                },
                {
                    small: "https://picsum.photos/id/435/160/90",
                    large: "https://picsum.photos/id/435/1600/900"
                }
            ],
            el: null,
            mainImageIndex: 0,
            size: null,
            panelSize: null,
            translateZ: null,
            rotateY: 0,
            rotateInt: 0,
            ry: null
        };
    },
    methods: {
        carousel3d() {
            this.el = this.$refs["carousel"];
            this.size = this.el.children.length;
            this.panelSize = this.el.clientWidth;
            this.translateZ = Math.round(
                this.panelSize / 2 / Math.tan(Math.PI / this.size)
            );
            this.el.style.transform = `rotateY(0deg) translateZ(-${this.translateZ}px)`;
            this.ry = 360 / this.size;
            this.animateSlider();
        },
        animateSlider() {
            this.rotateY = this.ry * this.rotateInt;
            for (let i = 0; i < this.size; i++) {
                let z = this.rotateInt * this.ry + i * this.ry;
                this.el.children[
                    i
                ].style.transform = `rotateY(${z}deg) translateZ(${this.translateZ}px)`;
            }
            this.rotateY = 0;
        },
        changeSlide(index) {
            this.mainImageIndex = index;
            let newInt = -1 * index;
            if (newInt < this.rotateInt + -1 * (this.size / 2)) {
                this.rotateInt = this.size + newInt;
            } else {
                this.rotateInt = newInt;
            }
            this.animateSlider();
        },
        nextSlide() {
            this.rotateInt -= 1;
            if (this.mainImageIndex == this.images.length - 1) {
                this.mainImageIndex = 0;
            } else {
                this.mainImageIndex += 1;
            }
            this.animateSlider();
        },
        prevSlide() {
            this.rotateInt += 1;
            if (this.mainImageIndex == 0) {
                this.mainImageIndex = this.images.length - 1;
            } else {
                this.mainImageIndex -= 1;
            }
            this.animateSlider();
        }
    },
    mounted() {
        this.mainImage = this.images[0].large;
        this.carousel3d();
    }
};
</script>

<style lang="scss">
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.3s;
}

.fade-enter,
.fade-leave-to {
    opacity: 0;
}

#app {
    margin: 30px auto 0;
    max-width: 800px;

    .main-image {
        position: relative;
        width: 100%;
        max-height: 400px;
        overflow: hidden;

        .progress-circular,
        .buttons {
            width: 100%;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;

            .v-progress-circular {
                color: #fafafa;
            }
        }

        .buttons {
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            padding: 0 24px;
            justify-content: space-between;

            button {
                background: #ffffff;
            }
        }
    }

    .perspective {
        margin: 0 auto;
        margin-top: 30px;
        width: 210px;
        height: 140px;
        position: relative;
        -webkit-perspective: 1100px;
        perspective: 1100px;

        .carousel {
            width: 100%;
            height: 100%;
            position: absolute;
            transform-style: preserve-3d;
            -webkit-transform-style: preserve-3d;
            -moz-transform-style: preserve-3d;
            transform: rotateY(0deg) translateZ(-288px);
            -webkit-transform: rotateY(0deg) translateZ(-288px);

            .image-container {
                display: flex;
                align-items: center;
                position: absolute;
                width: 186px;
                height: 116px;
                cursor: pointer;
                -webkit-transition: opacity 1s, -webkit-transform 1s;
                -moz-transition: opacity 1s, -moz-transform 1s;
                -o-transition: opacity 1s, -o-transform 1s;
                transition: opacity 1s, transform 1s;
                background-color: rgba(30, 30, 30, 0.7);
            }
        }
    }
}
</style>
