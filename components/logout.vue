<template>
    <div >
        <button @click="logout" class="button is-danger">logout</button>
    </div>
</template>

<script>
    export default {
        name: 'logout',
        
        
        methods:{
        
        async logout(){
            await this.$auth.logout()
        }
        
    }
    }
</script>

<style lang="scss" scoped>

</style>