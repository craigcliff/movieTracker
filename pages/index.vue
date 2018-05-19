<template>
  <v-layout column justify-center align-center>

      <v-data-table
                v-bind:headers="headers"
                :items="items"
                hide-actions
                class="elevation-1"
                >
    <template slot="items" slot-scope="props">
      <td>{{ props.item.Title }}</td>
      <td class="text-xs-right">{{ props.item.DateAdded }}</td>
      <td class="text-xs-right "  >
        <div class = "post-thumbnail" :style="{backgroundImage:'url('+ props.item.Poster + ')'}"> </div>



      </td>
      <td class="text-xs-right">{{ props.item.Plot }}</td>
      <td class="text-xs-right">{{ props.item.Actors }}</td>
      <td class="text-xs-right">{{ props.item.RunTime }}</td>
      <td class="text-xs-right">{{ props.item.Rating }}</td>
    </template>
  </v-data-table>
   
  </v-layout>
</template>

<script>

import axios from 'axios'


export default {

data(){
return {
  headers: [
        {
          text:'Title',
          align: 'left',
          sortable: false,
          value:'title'
        },
        { text: 'Date Added', value: 'date' },
        { text: 'Poster', value: 'poster',align: 'left' },
        { text: 'Plot', value: 'plot',align: 'left' },
        { text: 'Actors', value: 'actors',align: 'left' },
        { text: 'Run Time', value: 'runTime' },
        { text: 'Rating', value: 'rating' }
      ],

 items: []

}

},


  components: {
    
  },

  mounted() {
    this.getData();

  },

  methods: {
    getData: function(){


      axios.get('https://moviewatcher-3d46f.firebaseio.com/server.json')
      .then( response => {

        console.log(response.data);
         const postsArray = []
            for (const key in response.data){
                postsArray.push({ ...response.data[key], id:key})

            }
        console.log(postsArray[1]);
        this.items = postsArray;
        

      }).catch(function (error) {
    console.log(error);
  });
      


console.log(this.items);
    }


  }
}
</script>


<style scoped>

.post-thumbnail{

height: 200px;
width: 150px;
margin: 10px;
 background-repeat: no-repeat;
    background-size: contain;

}

</style>
