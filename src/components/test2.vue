<template>
<v-container grid-list-md>
     <v-layout row>
    <v-flex v-for="list in lists" :key="list.id">
      <v-toolbar color="white" flat text-center>
          <v-toolbar-title>{{list.name}}</v-toolbar-title>
          <v-spacer></v-spacer>
        </v-toolbar>

     <v-list >
          <draggable v-model="list.cards" :options="{group:'people'}" @start="drag=true" @end="drag=false" >
            <v-layout row>
          <v-flex xs2 v-for="card in list.cards" :key="card.id">
          
            <v-card :color="list.color" width="15em" height="10em" hover>
              <v-card-title primary-title >
                <div class="title">{{card.name}}</div>
              </v-card-title>
              <!-- <v-card-actions>
                <v-btn flat dark>Listen now</v-btn>
              </v-card-actions> -->
            </v-card>

          </v-flex>
            </v-layout>
            </draggable>         
</v-list> 
        <!-- <v-list>
          <v-list-group
            v-for="card in list.cards"
            :key="card.id"
            no-action
          >
            <v-list-tile slot="activator">
              <v-list-tile-content>
                <v-list-tile-title>{{ card.name }}</v-list-tile-title>
              </v-list-tile-content>
            </v-list-tile>
            <v-list-tile v-for="card in list.cards" :key="card.id" @click="">
              <v-list-tile-content>
                <v-list-tile-title>{{ card.name }}</v-list-tile-title>
              </v-list-tile-content>
            </v-list-tile>
          </v-list-group>
        </v-list> -->

    </v-flex>
  </v-layout>
  </v-container>
</template>

<script>
import draggable from 'vuedraggable'
export default {
  components: {
            draggable,
        },
  data () {
    return {
      board:{},
      lists:[],
      cards:[]
    }
  },
  created: function() {
    let that = this;
    this.board.id = '5ae1a0112d6448380e98e341'
    Trello.boards.get(this.board.id, function(res) {
      that.board.name = res.name
      if (res.desc != '') {
        that.board.desc = JSON.parse(res.desc)
      }
    })
    Trello.boards.get(this.board.id + '/lists',{cards: 'open'}, function(res) {
      res.map( l => {
        let list = {}
        list.id = l.id
        list.name = l.name
        list.cards = l.cards
        switch (list.name)
        {
          case '資料/文件/連結':
          list.color = 'blue'
          break
          case '問題面向':
          list.color = 'yellow'
          break
          case '問題細節':
          list.color = 'yellow lighten-3'
          break
          case '解法':
          list.color = 'green'
          break
          case '回應':
          list.color = 'orange'
          break
          case '困難':
          list.color = 'pink'
          break
          case '利害關係人':
          list.color = 'blue darken-4'
          break
          default:
          list.color = 'teal'
          break
        }
        that.lists.push(list)
      })
      console.log(that.lists)
    })
  }
}
</script>

<style>

</style>
