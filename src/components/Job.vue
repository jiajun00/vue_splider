<template>
  <div class="job">
    <div class="page-header">
      <div class="page-search">
        <mt-search placeholder="输入职位" v-blur="{methods: getjobs}" @keyup.enter.native="getjobs" v-model="keywords">
        </mt-search>
      </div>
      <div class="caozuo">
        <div class="select-area-div">
          <select v-model="area">
            <option value="昆明">昆明</option>
          </select>
        </div>
        <div class="select-type-div" >
          <select v-model="type" @change="getjobs">
            <option v-for="ns in typeoption" :value="ns.value">{{ns.text}}</option>
          </select>
        </div>
        <div class="select-pageno-div">
          <select v-model="pageno" id="pageno" @change="getjobs" disabled="disabled">
              <option v-for="n in 5" :value="n">第{{n}}页</option>
          </select>
        </div>
        <div class="clear"></div>
      </div>
    </div>
    
    <div class="zhangwei"></div>
    <div class="joblists">
      <a class="joblist" v-for="joblist in joblists" :href='joblist.url' target="_blank">
        <h3>{{joblist.positionName}}</h3>
        <div class="salary">{{joblist.salary}}</div>
        <div class="company">{{joblist.companyFullName}}</div>
        <div class="area">{{joblist.city}}</div>
        <div class="time">{{joblist.createTime}}</div>
      </a>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'job',
  activated() {
    this.$emit('title', '招聘信息');
  },
  mounted() {
    this.getjobs()
  },
  methods: {
    getjobs: function() {
      var self = this
      let url='http://splider.qqxio.cn/splider/job/index?type='+this.type
      url = url+'&pageno='+this.pageno
      if(this.keywords.length>0){
        url = url+'&keywords='+this.keywords
      }
      
      axios.get(url)
        .then(function(res){
          console.log(res.data.code)
          self.joblists = res.data.data
          if(res.data.usepageno == 'use'){
            document.getElementById("pageno").removeAttribute("disabled")
          }else{
            document.getElementById("pageno").setAttribute("disabled", "disabled")
          }
        })
        .catch(function (error) {
          console.log(error)
        })
    }
  },
  directives: {
    blur: {
      update:function(el,binding){
        var value = binding.value; // 传递给指令的值
        let oInput = el.querySelector('input')
        oInput.onblur = function(){
          value.methods.call(this);
        }
      }
      
    }
    
  },
  data () {
    return {
      keywords:'',
      pageno:'1',
      area:'昆明',
      type:'51job',
      typeoption:[
        {text:'前程无忧',value:'51job'},
        {text:'智联招聘',value:'zhilian'},
        {text:'拉钩网',value:'lagou'},
        {text:'猎聘网',value:'liepin'},
        {text:'Boss直聘',value:'boss'},
      ],
      joblists:{},
      msg: 'test'
    }
  }
}
</script>

<style lang="scss" scoped>
$page-header-height:102px;
  .job{
    width:100%;
    position:relative;
    background-color: #eee;
    .clear{clear:both;}
    .page-header{
      width:100%;
      position:fixed;
      z-index:2;
      height:$page-header-height;
      max-width: 750px;
      .page-search{
        height: 52px;
        .mint-search{
          height: 52px;
        }
      }
      .caozuo{
        position:relative;
        z-index:999;
        width:98%;
        margin:0 auto;
        border:1px solid #eee;
        background-color: #fff;
        .select-area-div,.select-pageno-div,.select-type-div{
          width:30%;
          select{
            margin:5px auto;
            width:92%;
            display: block;
            border: 1px solid #e5e5e5;
            height: 38px !important;
            line-height: 38px;
          }
        }
        .select-area-div{
          float:left;
          border-right: 1px solid #eee;
        }
        .select-type-div{
          float:left;
          width:39%;
        }
        .select-pageno-div{
          float:right;
          border-left: 1px solid #eee;
          
        }
      }
    }
    .zhangwei{
      height:$page-header-height;
    }
    .joblists{
      padding:0px 20px 15px 13px; 
      .joblist{
        display:block;
        position:relative;
        width:95%;
        height:100px;
        margin:13px auto 0px auto;
        padding:12px;
        background-color: #fff;
        h3{
          display:block;
          float:left;
          color:#269ffa;
        }
        .salary{
          color:red;
          position:absolute;
          top:38px;
          right: 25px;
          font-size: 17px;
          font-weight:bold;
        }
        .company{
          position:absolute;
          font-size: 16px;
          color:#878787;
          top: 65px;
          left:15px;
        }
        .area{
          position:absolute;
          bottom: 5px;
          left:25px;
          font-size:14px;
          color:#999;
        }
        .time{
          position:absolute;
          bottom: 20px;
          right: 15px;
          font-size:13px;
          color:#ddd;
        }
      }
    }
  }

</style>
