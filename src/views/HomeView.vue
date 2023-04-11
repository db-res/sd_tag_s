<template>
  <div id="box">
    <div  class="container">
      <h1 class="f38 bold">sd tag生成</h1>
      <div class="mg-t-20 pd-10 fdc aic mg-b-10" style="width: 1200px;border: 1px solid #eeeeee;box-shadow: 0px 0px 10px 5px #eeeeee;border-radius: 4px;">
        <div class="flex aic fw W100" style="flex-wrap: wrap;">
          <div class="dslb pd-r-20 pd-l-20 pd-t-10 pd-b-10 pointer" v-for="item in tagList" :key="item.value" @click="selectedTag" :data-value="item.value">
            {{ item.name }}
          </div>
        </div>
        <div class="fdc ais W100 mg-b-10" >
          <div class="flex aic W100">
            <span class="w100 txalE mg-r-10">提示语</span>
            <div class="flex1 pd-6" style="border:1px solid #eeeeee;border-radius: 4px;flex:1;min-height: 30px;">{{ selectedTagList.join(',') }}</div>
          </div>
          <div class="flex mg-t-10 aic W100">
            <span class="w100 txalE mg-r-10">反向提示语</span>
            <div class="flex1 pd-6 h30" style="border:1px solid #eeeeee;border-radius: 4px;flex:1;"></div>
          </div>
        </div>
        <div class="W100 mg-t-10 mg-b-10 pd-20" style="border: 1px solid #eee;border-radius: 0px;">
          <div class="flex aic" style="flex-wrap: wrap;">
            <div class="pd-10 pointer mg-r-10 mg-b-10" :class="[item.checked?'selected':'']" style="border: 1px solid #eeeeee;border-radius: 4px;" v-for="item in selectedTagContent" :key="item.chinese" @click="toggleTag(item)">
              <span>{{ item.chinese }}</span>
            </div>
          </div>
        </div>
        
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive } from "vue";
import tag from '../util/tag'
export default {
  setup() {
    const tags = [];
    const selectedTags = ref([]);


    return {
      tags,
      selectedTags,
    };
  },
  data(){
    return {
      tag:null,
      tagList:[
        {
          name:'常用',
          value: 'commonly'
        }, 
        {
          name:'动作',
          value: 'action'
        }, 
        {
          name:'风格',
          value: 'Style'
        }, 
        {
          name:'服装',
          value: 'Clothing'
        }, 
        {
          name:'环境',
          value: 'Environment'
        }, 
        {
          name:'人物&角色',
          value: 'Roles'
        }, 
        {
          name:'身体',
          value: 'Body'
        }, 
        {
          name:'头发&发饰',
          value: 'Hair'
        }, 
        {
          name:'袜子&腿饰',
          value: 'Socks'
        }, 
        {
          name:'五官&表情',
          value: 'Expressions'
        }, 
        {
          name:'鞋',
          value: 'shoes'
        }, 
        {
          name:'眼睛',
          value: 'Eyes'
        }, 
        {
          name:'装饰',
          value: 'decoration'
        }, 
        {
          name:'Emoji',
          value: 'Emoji'
        }, 
        {
          name:'R18',
          value: 'R18'
        },
      ],
      selectedTagContent:[],
      selectedTagList:[],
    }
  },  
  created(){
    this.tag = JSON.parse(JSON.stringify(tag))
  },
  methods:{
    selectedTag(params) {
      let {value} = params.target.dataset  
      this.selectedTagContent = this.tag[value]
      console.log(this.selectedTagContent);
    },
    toggleTag(tag) {
      if (this.selectedTagList.includes(tag.english)) {
        this.selectedTagList = this.selectedTagList.filter((t) => t !== tag.english);
        this.selectedTagContent.map((item)=>{
          if(item.english == tag.english){
            item.checked = false
          }
        })
      } else {
        this.selectedTagList.push(tag.english);
        this.selectedTagContent.map((item)=>{
          if(item.english == tag.english){
            item.checked = true
          }
        })
      }
      console.log(this.selectedTagList, this.selectedTagContent);
    }
  }
};
</script>

<style scoped lang="scss">
#box{
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    font-family: Arial, sans-serif;
    padding: 10px;
  }
}


.selected {
  background-color: #f06e58;
  color: white;
}
</style>
