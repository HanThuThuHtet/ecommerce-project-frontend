<template>
    <nav aria-label="Page navigation example">
        <ul class="pagination">

          <li v-for="link in links" class="page-item">
            <a class="page-link" 
                @click.prevent="paginateTo(link.url)"
                :class="{'active':link.active, 'disabled':!link.url}">
                    {{ link.label }}
            </a>
          </li>

        </ul>
      </nav>
</template>

<script>
export default{
    props: {
        links: {
            type: Array
        }
    },
    methods: {
        paginateTo(url){
            this.$emit('fetchLink',url);
        },
        changeLinks(){
            this.links[0].label = "Prev";
            this.links[this.links.length-1].label = "Next";
        }
    },
    mounted(){
        this.changeLinks()
    },
    updated(){
        this.changeLinks()
    }
}
</script>

<style scoped>
</style>