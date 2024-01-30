<template>
    <div style="background-color: blue; height: 10px; width: 10px;">
        
    </div>
</template>

<script>
export default {
    props: {
        hero: {
            type: Object,
            required: true,
        },
    },

    methods: {
        moveLeft() {
            const updatedHero = { ...this.hero };
            updatedHero.ax = -0.1;
            this.$emit('heroMoved', updatedHero);
        },
        moveRight() {
            const updatedHero = { ...this.hero };
            updatedHero.ax = 0.1;
            this.$emit('heroMoved', updatedHero);
        },
        jump() {
            const updatedHero = { ...this.hero };
            if (!updatedHero.isJumping) {
                updatedHero.isJumping = true;
                updatedHero.vy += updatedHero.jump;
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
            this.$emit('heroMoved', updatedHero);
        }
      });
    },

    
};
</script>

<style scoped>

</style>
