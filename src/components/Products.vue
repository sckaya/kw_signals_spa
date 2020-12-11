<template>
   <div class="container">
    <table class="table table-responsive w-100 d-block d-md-table table-sm table-dark text-center">
      <thead>
        <tr>
          <th scope="col">alarmNum</th>
          <th scope="col">eventCodeDesc</th>
          <th scope="col">pointDesc</th>
          <th scope="col">signalCode</th>
          <th scope="col">xmit</th>
          <th scope="col">siteDate</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in users"> 
          <th scope="row">{{user.alarmNum}}</th>
          <td>{{user.eventCodeDesc}}</td>
          <td>{{user.pointDesc}}</td>
          <td>{{user.signalCode}}</td>
          <td>{{user.xmit}}</td>
          <td>{{ user.siteDate | formatDate}}</td>
        </tr>
      </tbody>    
    </table> 
    <p v-if="message!='finished'">{{message}}</p>
    <div v-if="firsttable!='true'" class="btn-group" role="group">
            <input class="w-25" v-model="pageNum" type="number" required>
            <button type="button" class="btn btn-secondary" @click="getsignals()" ref="btnchngpage">Jump to page #</button>
    </div>
  </div> 
</template>
<script>
  import axios from 'axios';
  import moment from 'moment';
  export default {
    name: 'Users',
    data() {
      return {
        users: null,
        message: "",
        firsttable: "true",
        loading: "no"
      };
    },
    computed: {
        pageNum: {
            get() {
                return this.$store.state.pagenum;
            },
            set(value) {
                if(this.loading == "finished")
                {
                    //this.$refs.btnchngpage.disabled = false
                    this.$store.commit('updatePageNum', value);
                    //this.$refs.btnchngpage.innerText = "Jump to page "+this.$store.state.pagenum+" of "+this.$store.state.numpages
                }
            }
        },
        pageCount:{
            get() {
                return this.$store.state.numpages;
            }

        }
    },
    methods: {
        getsignals(){

            if(Number(this.$store.state.pagenum) < 1){
                this.message += " Cannot load next page. Number entered is blank or less than 1."
                return
            }
            if(this.$store.state.numpages !='-1' && Number(this.$store.state.pagenum) >= Number(this.$store.state.numpages))
            {
                this.message += " Cannot load next page. Number entered is greater than total page numbers: "+this.$store.state.numpages+"."
                return
            }
            this.message += " Loading page "+this.$store.state.pagenum+"."
            this.loading = "started"
            axios({
                    url: 'https://grasperapi.azurewebsites.net/api/v1/Signals?'+'Page='+this.$store.state.pagenum+'&Limit=10',
                    headers: {
                                Authorization: 'Bearer ' + this.$store.state.token,
                    },
                    method: 'GET'
            })
            .then(res => {
            this.loading = "finished"
            console.log(res.data.numPages)
            if(this.firsttable == "true")
            {
                this.$store.commit('updatePageCount', res.data.numPages);
            }
            this.message = "Current page "+this.$store.state.pagenum+" of "+this.$store.state.numpages+".";
            this.users = res.data.items;
            this.firsttable = "false"
            }).catch(err => {
                this.message += " "+err
                this.loading = "finished"
            })
        }
    },
    created: function() {
        let self=this;
        this.getsignals()
    }
  }
</script>