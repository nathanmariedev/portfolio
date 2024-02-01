<template>
    <img :src="walkingFrames[this.hero.currentFrameIndex]" alt="hero"
        style="height: 770px; width: 480px; position: absolute; bottom: 0; left: 0;">
</template>

<script>
export default {
    props: {
        hero: {
            type: Object,
            required: true,
        },
    },

    data() {
        return {
            walkingFrames: [
                require('./../static/sprites/main.png'),
                require('./../static/sprites/run_1.png'),
                require('./../static/sprites/run_2.png'),
            ],
            lastDirection: 'right',
        };
    },

    methods: {
        moveLeft() {
            const updatedHero = { ...this.hero };
            updatedHero.ax = -0.5;
            updatedHero.friction = 1; 
            updatedHero.isFacingLeft = true;           
            this.$emit('heroMoved', updatedHero);
        },
        moveRight() {
            const updatedHero = { ...this.hero };
            updatedHero.ax = 0.5;
            updatedHero.friction = 1;
            updatedHero.isFacingLeft = false;
            this.$emit('heroMoved', updatedHero);
        },
        jump() {
            const updatedHero = { ...this.hero };
            if (!updatedHero.isJumping) {
                updatedHero.askToJump = true;
                updatedHero.ay += 0.5;
            }
            this.$emit('heroMoved', updatedHero);
        },
    },

    // eslint-disable-next-line vue/order-in-components
    mounted() {
        // ACCELERATION
        window.addEventListener('keydown', (event) => {
            if (event.key === 'ArrowRight') {
                this.moveRight();
            }
            if (event.key === 'ArrowLeft') {
                this.moveLeft();
            }
            if (event.key === 'ArrowUp') {
                this.jump();
            }
            if (event.key === 'ArrowDown') {
                // eslint-disable-next-line no-console
                console.log('TO BE IMPLEMENTED');
            }
        });

        // DECELERATION
        window.addEventListener('keyup', (event) => {
            if (event.key === 'ArrowRight' || event.key === 'ArrowLeft') {
                const updatedHero = { ...this.hero };
                updatedHero.ax = 0;
                updatedHero.friction = 0.985;
                this.$emit('heroMoved', updatedHero);
            }
        });
    },


};
</script>

<style scoped></style>
