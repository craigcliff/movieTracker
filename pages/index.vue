<template>

    <v-card>
      <v-data-table
                v-bind:headers="headers"
                :items="items"
                hide-actions
                class="elevation-1"
                >
    <template slot="items" slot-scope="props">
      <td class = "titleTd">{{ props.item.Title }}</td>
      
      <td class="text-xs-left "  >
        <div class = "post-thumbnail" :style="{backgroundImage:'url('+ props.item.Poster + ')'}"> </div>



      </td>
      <td class="text-xs-left">{{ props.item.Plot }}</td>
      <td class="text-xs-left">{{ props.item.Actors }}</td>
      <td class="text-xs-left">{{ props.item.RunTime }}</td>
      <td class="text-xs-left">{{ props.item.Rating }}</td>
      <td class="text-xs-left">{{ props.item.DateAdded }}</td>
    </template>
  </v-data-table>

  </v-card>
 
  
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
    
        { text: 'Poster', value: 'poster',align: 'left' },
        { text: 'Plot', value: 'plot',align: 'left' },
        { text: 'Actors', value: 'actors',align: 'left' },
        { text: 'Run Time', value: 'runTime',align: 'left' },
        { text: 'IMDB Rating', value: 'rating',align: 'left' },
            { text: 'Date Added', value: 'date',align: 'left' }
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
        this.items = postsArray.reverse();
        

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
margin-top: 5px;
margin-bottom: 5px;
 background-repeat: no-repeat;
    background-size: contain;

    

}

@media screen and (max-width: 450px){
.post-thumbnail{

height: 100px;
width: 100px;


}

.titleTd{

  margin: 0 !important;
  padding: 0 !important;
  font-size:12px;


}
}

.main-section{

  
}

</style>
