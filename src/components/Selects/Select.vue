<template>
    <select class="form-control" @change="handleChange">
        <option v-for="option in options" :key="option.title">{{option.title}}</option>
    </select>
</template>

<script>
export default {
    name:'Select',
    props:{
        options:Array,
    },
    methods:{
        handleChange(event){
            const index = this.options.findIndex(a=>a.title == event.target.value)
            this.options[index].func()
        }
    },
    created(){
        console.log(this.options)
    }
}
</script>