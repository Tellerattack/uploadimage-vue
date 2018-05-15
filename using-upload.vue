

 <template>
  <div class="photo-wrapper">
    <img :src="identity" class="photo" v-if="formData.identity">
    <label for="file" class="upload-label" :class="{label_hide:formData.identity}">
      <div class="avatar">+</div>

      <uploadimg class="upload" :data="uploadForm" :beforeUpload="beforeUpload" :onSuccess="([data]) => uploadSuccess(data, 'identity')"></uploadimg>

    </label>
  </div>
</template>

<script>
import uploadimg from '../../components/upload/uploadimg';
export default{
  data(){
    return{}
  },
 computed: {

    uploadForm() {
      return {
        token: this.uptoken,
      };
    },

},
methods:{
beforeUpload(files) {
      const is5M = 5 * 1000 * 1000; // 5M 大小限制
      if (!files.length) {
        return false;
      }
      // files 不是 Array 的实例
      for (let i = 0; i < files.length; i += 1) {
        const { size } = files[i];
        const validSize = size <= is5M;
        if (!validSize) {
          Toast(`图片大小不能超过${is5M / 1000 / 1000}M`);
          return false;
        }
      }
      return true;
    },
     uploadSuccess(data, type) {
      console.info('uploadSuccess,', data, type);
      const { base_url, path } = data;
      this[type] = `${base_url}${path}-w750`;
      this.formData[type] = path;
    },
},

</script>



