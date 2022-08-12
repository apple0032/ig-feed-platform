<template>
  <div class="root">
    <div>
      <h3>Instagram Ranked Feed Platform</h3>
      <b-tabs content-class="mt-3">

        <div v-for="(tabData, index) in this.tabData" :key="index">
          <b-tab :title=tabData.title @click="changeTab(tabData.tab)">

            <b-container class="ig_container">
              <b-row>
                <b-col cols="12" v-for="(data, index2) in ig_data" :key="index2" class="ig_box" @click="clickBox()">
                  <div v-if="!(data['_source']['media']).includes('mp4')">
                    <img v-bind:src="data['_source']['media']" />
                  </div>
                  <div v-else>
                    <video width="50%" controls>
                      <source v-bind:src="data['_source']['media']" type="video/mp4">
                      Your browser does not support the video tag.
                    </video>
                  </div>
                  <div class="ig_details">
                    💕 {{data['_source']['like']}}
                    📅 {{data['_source']['date']}}
                  </div>
                </b-col>
              </b-row>
            </b-container>

          </b-tab>
        </div>

      </b-tabs>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: 'Ig-Feed',
  async mounted(){
    window.addEventListener('scroll', this.toBottom);
  },
  async created(){
    let ig_data = await this.loadData(this.type,this.page);
    this.ig_data = ig_data;
  },
  data(){
    return {
      ig_data: null,
      type : 'hot',
      page : 1,
      tabData : [
        {
          title : 'By Popularity',
          tab : 'hot'
        },
        {
          title : 'By Recency',
          tab : 'fresh'
        }
      ]
    }
  },
  methods: {
    async toBottom () {
      //console.log("Triggered")
      if ((window.innerHeight + window.scrollY) >= document.body.scrollHeight + 20) {

        //console.log("Bottom");
        //console.log(this.page);

        var newPage = this.page + 1;
        //console.log(newPage);

        let ig_data = await this.loadData(this.type,newPage);
        this.ig_data = this.ig_data.concat( ig_data );
        this.page = newPage;
      }
    },
    async loadData(type,page){
      let ig_data;
      await axios.get('http://localhost:3000/getData?type='+type+'&page='+page)
              .then(function (response) {
                ig_data = response.data.data;
              })
              .catch(function (error) {
                console.log(error);
              });

      return ig_data;
      //console.log(this.ig_data);
    },
    async clickBox(){
      window.open('https://www.instagram.com/9gag/', '_blank');
    },

    async changeTab(clickType){
      this.page = 1;
      window.addEventListener('scroll', this.toBottom);

      if(clickType == this.type){
        //console.log('same!');
        return;
      } else {
        this.type = clickType;
        this.ig_data = [];
        let ig_data = await this.loadData(clickType,1);
        this.ig_data = ig_data;
      }
    }
  }
}


</script>



<style scoped>
.ig_details{
  border : 1px solid gray;
  border-radius: 20px;
}

.ig_container img{
  width: 50%;
}

.ig_box{
  border : 1px solid #616161;
  border-radius: 10px;
  margin-bottom: 10px;
  padding : 10px;
  cursor: pointer;
}

.ig_box:hover{
  background: rgba(77, 226, 161, 0.43);
  transition: 0.2s all;
}


h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
