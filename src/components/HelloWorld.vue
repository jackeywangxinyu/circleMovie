<template>
  <div>
    <div class="moviesrc" v-for="(item,index) in movieListdata" :key="index">
      <a :href="item.url">{{item.title}}</a>
    </div>

  </div>
</template>

<script>
const jsdom = require('jsdom');
const { JSDOM } = jsdom;
const config = {
    "pageMaxNum":"50",
    "parseURL":"http://vip.jlsprh.com/index.php?url="
};

export default {
  name: 'HelloWorld',
  data () {
    return {
      movieListdata:[]
    }
  },
  created(){

  },
  mounted(){
    this.load();
  },
  methods:{
    getAjax:function () {
      this.$axios.get('http://list.iqiyi.com/www/1/-------------11-1-1-iqiyi--.html')
        .then(function (response) {
          let temp = this.parseHTML(response.data);
          console.log(temp);
          for (var i = 0; i < temp.length; i++) {
            this.movieListdata.push(temp[i]);
          }
        }.bind(this))
        .catch(function (error) {
          console.log(error);
        });
    },
    load(){
      this.getAjax();
        // .then(d => saveToFile(d));
    },
    parseHTML(html) {
    const dom = new JSDOM(html);
    let aList = dom.window.document.querySelectorAll('div.site-piclist_pic > a');
    aList = Array.from(aList);
    return aList.map((a) => {
        return {
            source: a.href,
            title: a.title,
            url: `${config.parseURL}${a.href}`
        }
    });
}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
  .hello{
    text-align: center;

    p{
      position: relative;
      display: inline-block;
      outline: none;
      text-decoration: none;
      color: #000;
      font-size: 32px;
      padding: 0 10px;
    }
    p:hover::before,p:hover::after{
      content: "";
      display: block;
      position: absolute;
      top: -15%;
      left: -14%;
      width: 120%;
      height: 120%;
      border-style: solid;
      border-width: 4px;
      border-color: #DDD;
    }
    // p:hover::before{
    //   content: "\5B"; left: -20px;
    // }
    p:hover::after{
      top: 0%;
      left: 0%;
      width: 100%;
      height: 100%;
      border-width: 2px;
    }

  }
</style>
