/**
 * introduce : 图片上传展示
 * author    : cp
 * date      : 2020/01/14
 */
<template>
  <div class="Img-Update">
    <div @click="onPickFile"
         v-show="ImgShow1===true"
         class="up-img-content">
      <i slot="default"
         class="el-icon-plus"></i>
    </div>
    <input style="display: none"
           @change="getFile"
           ref="fileInput"
           accept="image/*"
           type="file" />
    <div v-show="ImgShow1===false"
         class="show-img-content">
      <div class="show-img-model">
        <i @click="onPickFile"
           class="el-icon-edit"></i>
        <i @click="delNowFlie"
           class="el-icon-delete"></i>
      </div>
      <img style="max-width:100%;"
           :src="imageUrl" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'ImgUpdate',
  props: {
    reFresh: {
      type: Number,
      default: 0
    }
  },
  components: {
  },
  data () {
    return {
      formData: new FormData(),
      imgs: {},
      imgLen: 0,
      ImgShow1: true,
      imageUrl: ''
    }
  },
  computed: {
  },
  methods: {
    onPickFile () {
      this.$refs.fileInput.click()
    },
    getFile (e) {
      let file = e.currentTarget.files[0]
      let reader = new FileReader()
      reader.readAsDataURL(file)
      reader.onload = () => {
        this.ImgShow1 = false
        this.imageUrl = reader.result
      }
      this.$emit('commitImg', file)
      e.target.value = null
    },
    delNowFlie () {
      this.imageUrl = ''
      this.ImgShow1 = true
    }
  },
  watch: {
    reFresh (newval, oldval) {
      if (newval === 200) {
        this.delNowFlie()
        this.$emit('update:reFresh', 0)
      }
    }
  }
}
</script>

<style lang='less' scoped>
.Img-Update {
  .up-img-content {
    width: 148px;
    height: 148px;
    cursor: pointer;
    text-align: center;
    line-height: 148px;
    border: 1px dashed #c0ccda;
    background-color: #fbfdff;
    border-radius: 5px;
    font-size: 28px;
  }
  .show-img-content {
    width: 148px;
    height: 148px;
    cursor: pointer;
    text-align: center;
    line-height: 148px;
    border-radius: 5px;
    font-size: 28px;
    position: relative;
    .show-img-model {
      position: absolute;
      top: 0;
      right: 0;
      width: 148px;
      height: 148px;
      transition: 0.5s;
      z-index: 999;
      border-radius: 5px;
      i {
        padding: 5px;
        font-size: 18px;
        margin: 5px 20px;
        opacity: 0;
      }
      :hover {
        color: white;
      }
    }
  }
  .show-img-content:hover .show-img-model {
    // background-color: black;
    background: rgba(0, 0, 0, 0.3);
    transition-duration: 1s;
  }
  .show-img-content:hover i {
    opacity: 1;
    transition-duration: 0.5s;
  }
}
</style>
