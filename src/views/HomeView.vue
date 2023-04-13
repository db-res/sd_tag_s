<template>
  <div id="box">
    <div  class="container">
      <h1 class="f38 bold">sd tag生成</h1>
      <div class="mg-t-20 pd-10 fdc aic mg-b-10" style="width: 1200px;border: 1px solid #eeeeee;box-shadow: 0px 0px 10px 5px #eeeeee;border-radius: 4px;">
        <div class="flex aic fw W100 mg-b-20" style="flex-wrap: wrap;">
          <div class="dslb pd-r-20 pd-l-20 pd-t-10 pd-b-10 pointer" :class="[item.checked?'selectedTag':'']" v-for="item in tagList" :key="item.value" @click="selectedTag" :data-value="item.value">
            {{ item.name }}
          </div>
        </div>
        <div class="fdc ais W100 mg-b-10" >
          <div class="flex aic W100">
            <span class="w100 txalE mg-r-10">提示语</span>
            <div class="flex1 pd-6 flex aic fwp" style="border:1px solid #eeeeee;border-radius: 4px;flex:1;min-height: 40px;">
              <div class="flex aic" v-for="(item,index) in selectedTagList" :key="item.value">
                <div class="" v-for="(i,j) in (Math.abs((item.weighted||0) + (item.reduction||0)))" :key="j">
                  <span v-if="((item.weighted||0) + (item.reduction||0)) > 0">(</span>
                  <span v-if="((item.weighted||0) + (item.reduction||0)) < 0">{</span>
                </div>
                <span>{{ item.english }}</span>
                <div class="" v-for="(i,j) in (Math.abs((item.weighted||0) + (item.reduction||0)))" :key="j">
                  <span v-if="((item.weighted||0) + (item.reduction||0)) > 0">)</span>
                  <span v-if="((item.weighted||0) + (item.reduction||0)) < 0">}</span>
                </div>
                <span>{{ index+1 == selectedTagList.length ? '':',' }}</span>
              </div>
            </div>
            <div class="pd-10 bor-4 t5 mg-l-10 pointer" style="background-color: aquamarine;" @click="copy(1)">复制</div>
            <div class="pd-10 bor-4 t5 mg-l-10 pointer" style="background-color: aquamarine;" @click="cleanAll">清空</div>
          </div>
          <div class="flex mg-t-10 aic W100">
            <span class="w100 txalE mg-r-10">反向提示语</span>
            <div class="flex1 pd-6 flex aic fwp" style="border:1px solid #eeeeee;border-radius: 4px;flex:1;min-height: 40px;"></div>
            <div class="pd-10 bor-4 t5 mg-l-10 pointer" style="background-color: aquamarine;" @click="copy(2)">复制</div>
          </div>
        </div>
        <div class="W100 pd-r-20 pd-l-20 pd-t-20 pd-b-20 flex aic" style="border: 1px solid #eee;border-radius: 0px;">
          <div class="w90">移动/权重：</div>
          <draggable 
            v-model="selectedTagList" 
            group="people" 
            @start="drag=true" 
            @end="drag=false" 
            item-key="item"
            class="flex fwp flex1" style="flex: 1;">
            <template #item="{element}">
              <div class="flex aic mg-r-10 mg-t-6" style="border: 1px solid #eeeeee;">
                <div class="pd-10 pointer" style="background-color: aqua;color: #fff;" @click="weighted(element)">+</div>
                <div class="pd-10 pointer" style="">{{element.chinese}}</div>
                <div class="pd-10 pointer" style="background-color: brown;color: #fff;" @click="reduction(element)">-</div>
              </div>
            </template>
          </draggable>

        </div>
        <div class="W100 mg-t-10 mg-b-10 pd-20" style="border: 1px solid #eee;border-radius: 0px;">
          <div class="flex aic" style="flex-wrap: wrap;">
            <div class="pd-10 pointer mg-r-10 mg-b-10" :class="[item.checked?'selected':'']" style="border: 1px solid #eeeeee;border-radius: 4px;" v-for="item in selectedTagContent" :key="item.english" @click="toggleTag(item)">
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
import draggable from 'vuedraggable';
export default {
  setup() {
    
  },
  components: {
    draggable,
  },
  data(){
    return {
      tag:null,
      selectTag:'',
      tagList:[
        // {
        //   name:'常用',
        //   value: 'commonly'
        // }, 
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
      selectedTagContent:[],// 提示词组
      selectedTagList:[], //已选择的提示词组
      drag: false,
    }
  },  
  watch:{
    selectTag:function (n,o) {
      this.selectedTagContent = this.tag[n]
      this.tagList.map(item=>{
        if(item.value == n){
          item.checked = true
        }else{
          item.checked = false
        }
      })
    }
  },
  created(){
    this.selectTag = 'Style'
    this.tag = JSON.parse(JSON.stringify(tag))
  },
  methods:{
    weighted(data){
      this.selectedTagList.map(item=>{
        if(item.english == data.english){
          item.weighted = item.weighted? item.weighted + 1 : 1 
        }
      })
    },  
    reduction(data){
      this.selectedTagList.map(item=>{
        if(item.english == data.english){
          item.reduction = item.reduction? item.reduction - 1 : -1 
        }
      })
    },  
    copy(){
      // 复制 已选择提示词组
      let text = this.selectedTagList.map(item=>{return item.english})
      if (navigator.clipboard) {
          navigator.clipboard.writeText(text.join(','));
      }else{
        alert('复制失败，clipboard不可用，建议更换浏览器使用')
      }
    },  
    selectedTag(params) {
      // 切换 提示词组
      let {value} = params.target.dataset  
      
      this.selectTag = value
    },
    toggleTag(tag) {
      // 选择提示词，若在selectedTagList中已存在则进行反选
      let enList = this.selectedTagList.map(item=>{return item.english})
      if (enList.includes(tag.english)) {
        this.selectedTagList = this.selectedTagList.filter((t) => t.chinese !== tag.chinese);
        this.selectedTagContent.map((item)=>{
          if(item.english == tag.english){
            item.checked = false
          }
        })
      } else {
        this.selectedTagList.push(tag);
        this.selectedTagContent.map((item)=>{
          if(item.english == tag.english){
            item.checked = true
          }
        })
      }
      // console.log(this.selectedTagList);
    },
    cleanAll(){
      this.selectedTagList=[];
      this.selectedTagContent.map((item)=>{
        item.checked = false
      })
      // this.$forceUpdate()
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
.selectedTag {
  background-color: #f0cd58;
  color: white;
  border-radius: 4px;
  box-shadow: 0 0 10px 5px #f0cd5850;
}
</style>
