<template>
  <div class="root">
    <div>
      <h3>Instagram Ranked Feed Platform</h3>
      <b-tabs content-class="mt-3">

        <div v-for="(tabData, index) in this.tabData" :key="index">
          <b-tab :title=tabData.title @click="changeTab(tabData.tab)">
            <b-container class="ig_container">
              <b-row>
                <b-col cols="12" v-for="(data, index2) in ig_data" :key="index2" class="ig_box" @click="clickBox(data['_source']['permalink'])">
                  <div v-if="!(data['_source']['media_type']).includes('VIDEO')">
                    <img v-bind:src="data['_source']['media_url']" />
                  </div>
                  <div v-else>
                    <video width="50%" controls>
                      <source v-bind:src="data['_source']['media_url']" type="video/mp4">
                      Your browser does not support the video tag.
                    </video>
                  </div>
                  <div class="ig_details">
                    💕 {{data['_source']['like_count']}}
                    🗣️ {{data['_source']['comments_count']}}
                    📅 {{ showDate( data['_source']['timestamp']) }}
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
      ],
      scrolled : false
    }
  },
  methods: {
    async toBottom () {
      //console.log("Triggered")
      //console.log(document.body.scrollHeight);
      //console.log(window.innerHeight + window.scrollY)

      if ((window.innerHeight + window.scrollY) >= document.body.scrollHeight - 150) {

        //console.log("Bottom");

        if(this.scrolled === false) {
          this.scrolled = true;
          var newPage = this.page + 1;

          let ig_data = await this.loadData(this.type, newPage);
          this.ig_data = this.ig_data.concat(ig_data);
          this.page = newPage;

          this.scrolled = false;
        }

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

      //console.log(ig_data);
      return ig_data;
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
    },
    clickBox(permalink){
      window.open(permalink, '_blank');
    },
    showDate(date){
      date = new Date( date ) ;
      return date.getFullYear() + "/" +
              ("00" + (date.getMonth() + 1)).slice(-2) + "/" +
              ("00" + date.getDate()).slice(-2);
    }
  }
}

</script>


<style scoped>
  @import '../assets/main.css';
</style>
