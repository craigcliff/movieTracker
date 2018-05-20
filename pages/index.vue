<template >

    <v-card >
      <v-card-title>
      
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>
      
      <v-text-field
        v-model="search"
        append-icon="search"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
</v-card-title>

      <v-data-table
      class="blue darken-3"
                v-bind:headers="headers"
                :items="items"
                :search="search"
                v-bind:pagination.sync="pagination"
                
              
               
                >
    <template slot="items" slot-scope="props">
      <td class = "text-xs-left">{{ props.item.Title }}</td>
      
      <td class="text-xs-left "  >
        <div class = "post-thumbnail" :style="{backgroundImage:'url('+ props.item.Poster + ')'}"> </div>



      </td>
      <td class="text-xs-left">{{ props.item.Plot }}</td>
      <td class="text-xs-left">{{ props.item.Actors }}</td>
      <td class="text-xs-left">{{ props.item.RunTime }}</td>
      <td class="text-xs-left">{{ props.item.Rating }}</td>
      <td class="text-xs-left">{{ props.item.DateAdded }}</td>
    </template>
      <v-alert slot="no-results" :value="true" color="error" icon="warning">
        Your search for "{{ search }}" found no results.
</v-alert>
  </v-data-table>

  </v-card>
 
  
</template>

<script>

import axios from 'axios'


export default {

data(){
return {
pagination : {'sortBy': 'DateAdded', 'descending': true, 'rowsPerPage': -1},
  search: '',
  headers: [
        {
          text:'Title',
          align: 'left',
         
          value:'Title'
        },
    
        { text: 'Poster', value: 'Poster',align: 'left' },
        { text: 'Plot', value: 'Plot',align: 'left' },
        { text: 'Actors', value: 'Actors',align: 'left' },
        { text: 'Run Time', value: 'RunTime',align: 'left' },
        { text: 'IMDB Rating', value: 'Rating',align: 'left' },
            { text: 'Date Added', value: 'DateAdded',align: 'left' }
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
margin-top: 5px;
margin-bottom: 5px;
 background-repeat: no-repeat;
    background-size: contain;

    

}

@media screen and (max-width: 450px){
.post-thumbnail{

height: 150px;
width: 120px;


}


}



.main-section{

  
}

</style>
