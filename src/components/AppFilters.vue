<template>
<aside class="app-filters">
        <section class="toggle-group">
          <button 
          v-for="filter in filters" :key="filter"
          class="button" :class="{'button--primary': activeFilters === filter}"
          @click="shiftFilter(filter)"
          >{{filter}}</button>
          
        </section>
      </aside>

</template>

<script lang="ts">
  import {defineComponent, Prop, PropType} from 'vue';
  import AppTodoItem from "./AppTodoItem.vue";
  import {Todo} from "@/types/Todo"
  
  interface State {
    filters: string[]
  }

  export default defineComponent({
    props:{
      activeFilters: {
        type: String,
        required: true,
      }
    },
    data():State{
      return(
        {filters: ['All', 'Active', 'Done']}
      )
    },
    methods:{
      shiftFilter(filter:string){
        this.$emit('shiftFilter', filter)
      }
    },
    emits:{
      shiftFilter: (filter:string)=>filter
    }
  })
</script>