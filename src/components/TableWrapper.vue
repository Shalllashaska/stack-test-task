<template>
    <div class="tableWrapper">
        <TableItem 
         v-for="comp in companies"
         v-bind:comp="comp"
         v-bind:key="comp.id"
        @deleteItem="deleteItemW"
        />
    </div>
</template>

<script>

import TableItem from '@/components/TableItem.vue'
export default {
    name: "TableWrapper",
    props: ['companies'],
    methods:{
        deleteItemW(id){
            this.$emit('deletItem', id);
        }
    },  
    components: {
        TableItem,
    }
}

</script>

<style scoped>
    .tableWrapper{
        width: inherit;
        height: 340px;
    }
</style>