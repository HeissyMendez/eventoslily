<template>
    <div>
        <table class="table table-hover table-border">
  <thead>
    <tr>
      <th scope="col" style="width:15%"></th>
      <th scope="col" style="width:10%"></th>
      <th v-for="(Hora, index) in TableThead" :key="index">{{Hora}}</th>
    </tr>
  </thead>
  <tbody v-for="(channel, index) in EventData.channels" :key="index">
    <tr v-for="(room, indexRoom) in channel.rooms" :key="indexRoom">

      <td v-if="(indexRoom ==0 )" :rowspan="channel.rooms.length">
          {{channel.name}}
      </td>
      <td class="border-start border-end border-dark">
          {{room.name}}
      </td>
      <td v-for="(Hora, indexhora) in TableThead" :key="indexhora">
          <ColumTable :Hora="Hora" :Sesiones="room.sessions"></ColumTable>  
      </td>
    </tr>
  </tbody>
</table>

    </div>
</template>

<script>
import axios from 'axios'
import ColumTable from './column-table.vue'

export default {
    name : 'TableEvent',
    components : {
        ColumTable
    },
    data(){
        return {
            OrganizerSlug : this.$route.params.SlugOrganizer,
            EventSlug : this.$route.params.SlugEvent,
            EventData : {},
            TableThead : [],
        }
    },
    mounted(){
        this.DatosEvento();
    },
    methods : {
        DatosEvento(){
            axios.get('organizer/'+ this.OrganizerSlug + '/events/' + this.EventSlug)
            .then((response) => {
                if(response.status == 200) {
                    this.EventData = response.data;
                    this.HorasEvento();
                }
            })
            .catch((error) => {
                console.log(error);
            })
        },
        HorasEvento(){
            this.EventData.channels.forEach(Channel =>{
                Channel.rooms.forEach(Room =>{
                    Room.sessions.forEach(Session =>{
                        let Hora = Session.start.substr(11,2);
                        if(!this.TableThead.includes(Hora)){
                            this.TableThead.push(Hora);
                        }
                    })
                })
            })
            this.TableThead = this.TableThead.sort((a, b) => a-b);
        }
    }
}
</script>