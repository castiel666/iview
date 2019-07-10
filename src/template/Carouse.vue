<template>
<Row >
    <Carousel 
        v-model="value3"
        :autoplay="setting.autoplay"
        :autoplay-speed="setting.autoplaySpeed"
        :dots="setting.dots"
        :radius-dot="setting.radiusDot"
        :trigger="setting.trigger"
        :arrow="setting.arrow">
        <CarouselItem  >
            <div class="demo-carousel" ><img src="..\images\swiper1.jpg"  width="100%"></div>
        </CarouselItem>
        <CarouselItem>
            <div class="demo-carousel"><img src="..\images\swiper2.jpg"  width="100%"></div>
        </CarouselItem>
        <CarouselItem>
            <div class="demo-carousel"><img src="..\images\swiper3.jpg"  width="100%"></div>
        </CarouselItem>
    </Carousel>
</Row>
</template>
<script>
    export default {
        data () {
            return {
                value3: 0,
                setting: {
                    autoplay: false,
                    autoplaySpeed: 2000,
                    dots: 'inside',
                    radiusDot: false,
                    trigger: 'click',
                    arrow: 'hover'
                }
            }
        },
    }
</script>