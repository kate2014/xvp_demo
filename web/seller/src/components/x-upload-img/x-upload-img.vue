<template>
	<div class="x_upload_img">
		<div class="weui_cells weui_cells_form">
	    <div class="weui_cell">
	      <div class="weui_cell_bd weui_cell_primary">
	        <div class="weui_uploader">
	          <div class="weui_uploader_hd weui_cell">
	            <div class="weui_cell_bd weui_cell_primary" v-if='title'>{{title}}
	            	<slot name="after-title"></slot>
	            	<!-- 举证照片   文字 -->
	            </div>
	            <div class="weui_cell_ft" v-show="max != 101">{{list.length}}/{{max}}</div>
	          </div>
	          <div class="weui_uploader_bd">
	            <ul class="weui_uploader_files">
	              <li v-for="(item,index) in list" class="weui_uploader_file" :style="'background-image:url('+ item +')'" @click="showDel(index)"></li>
	            </ul>
	            <div class="weui_uploader_input_wrp" v-show="list.length < max">
	              <input class="weui_uploader_input" type="file" accept="image/jpg,image/jpeg,image/png,image/gif" @change="change" ref='input'>
	            </div>
	          </div>
	        </div>
	      </div>
	    </div>
	    <canvas ref='canvas' style="display:none;"></canvas>
	    <r-action-sheet v-model="actionFlag" :menus="actions" @on-click-menu="doDel" show-cancel></r-action-sheet>
  	</div>
	</div>
</template>

<script>
import utils from '../../libs/utils.js';
export default {
	props: {
	  title: {
	    type: String,
	    default: ""
	  },
	  list: {
	    type: Array,
	    default() {
	      return []
	    }
	  },
	  max: {
	    type: Number,
	    default: 101
	  }
	},
	mounted () {
    this.$nextTick(() => {
    })
  },
  created () {
  },
  data () {
    return {
      actionFlag:false,
      actions: {
        "delete": '<span class="clr-red">删除</span>'
      },
      deleteId:-1
    }
  },
  computed: {
  	
  },
  watch: {
  	
  },
  methods: {
	  change() {
	    let that = this;
	    utils.loadingShow("上传中");
	    utils.imgToBase64(this.$refs.canvas, this.$refs.input.files[0], function(base64) {
	      that.$emit('upload', base64);
	    });
	  },
	  showDel(index){
	    this.deleteId = index;
	    this.actionFlag = true;
	  },
	  doDel(key){
	    if(this.deleteId == -1){
	      this.$vux.toast.show({
	        text:"请选择要删除的图片",
	        type:'text'
	      });
	      return;
	    }
	    if(key == "delete"){
	        this.list.splice(this.deleteId,1);
	    }
	    this.deleteId = -1;
	  }
  },
  components: {
	"rInput": require('../input/input.vue'),
    "rActionSheet":require('../actionsheet/actionsheet.vue')
    
    
	}
}
</script>

<style lang="less">
@import '../../assets/styles/components/weui/widget/weui_cell/weui_cell_global.less';
@import '../../assets/styles/components/weui/widget/weui_cell/weui_uploader.less';
.x_upload_img .weui_cells_form>.weui_cell {
  padding: 0 0 10px 10px;
}
.x_upload_img .weui_cell_primary .weui_uploader> .weui_cell {
  padding: 10px 10px 10px 0;
}
</style>
