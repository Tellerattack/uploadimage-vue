<template>
  <div class="info">

    <input ref="upload" id="upload-label" @change="fileChange" type="file" class="realfilebt" />
    <!-- <slot/> -->
  </div>
</template>
<script>
import axios from 'axios';

export default {
  name: 'upload',
  props: {
    data: {
      type: Object,
      default() {
        return {};
      },
    },
    onSuccess: {
      type: Function,
      default() {
        return () => {};
      },
    },
    isHide: Boolean,
    onProgress: {
      type: Function,
      default() {
        return () => {};
      },
    },
    beforeUpload: {
      type: Function,
      default() {
        return () => true;
      },
    },
  },

  methods: {
    createFormData(file) {
      const { data } = this;
      const formData = new FormData();
      Object.entries(data).forEach(([key, value]) => {
        formData.append(key, value);
      });
      formData.append('file', file);
      return formData;
    },
    async fileChange(event) {
      const { target: { files } } = event;

      const valid = this.beforeUpload(files);
      if (!valid) {
        return;
      }
      const fileArr = Array.from(files);
      const dataArr = await Promise.all(fileArr.map(this.uploadFile));
      this.onSuccess(dataArr);
    },
    async uploadFile(file) {
      const formData = this.createFormData(file);
      const { data } = await axios({
        method: 'POST',
        url: 'http://up.qiniu.com',
        data: formData,
      });
      return data;
    },
  },
};
</script>
<style lang="scss" scoped>
.uploadBtn {
  position: relative;
  width: 1.3rem;
  height: 1.3rem;
  border-width: 0.02rem;
  border-style: dashed;
  border-color: rgb(223, 223, 223);
  border-radius: 0.06rem;
}
.info {
  width: 100%;
  height: 100%;
}

.upload-label {
  position: relative;
  width: 1.3rem;
  height: 1.3rem;
  border-width: 0.02rem;
  border-style: dashed;
  border-color: rgb(223, 223, 223);
  border-radius: 0.06rem;
}
.realfilebt {
  position: absolute;

  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100%;
  opacity: 0;
  // border: 1px solid red;
}

.avatar {
  position: absolute;
  // top: 50%;
  // bottom: 50%;
  left: 50%;
  color: #999999;
  font-size: 1rem;
  line-height: 1;
  transform: translateX(-50%);
}
.imglist {
  width: 2rem;
  height: 2rem;
  .photo {
    width: 100%;
    height: 100%;
  }
}
</style>
