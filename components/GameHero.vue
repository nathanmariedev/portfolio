<template>
    <div>
        <h1>GameHero</h1>
        <p>{{ message }}</p>
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
        
    },

    // eslint-disable-next-line vue/order-in-components
    mounted() {
        // ACCELERATION
        
    },

    
};
</script>

<style scoped>

</style>
