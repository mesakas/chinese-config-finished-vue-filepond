<template>
  <div class="FileUpload">

    <file-pond
        name="files"
        ref="pond"
        v-bind:allow-multiple="true"
        accepted-file-types="image/jpeg, image/png"
        server="/api/upload/img"
        v-bind:files="myFiles"
        @processfile="handleFilePondProcessfile"
        @removefile="handleFilePondRemovefile"

        :server="{
        url: '/api/upload/img',
        revert: (source, load, error) => {
          let image_name = this.transImgId(source)
          this.$api.fileUpload.delete(image_name).then((res) => {
            // console.log('删除成功')
            load()
          }).catch((err) => {
            // console.log('删除失败')
            load()
          })
        },
    }"
        label-idle="点击上传或将文件拖到此处.."
        label-file-loading="正在加载..."
        labelFileProcessing="正在上传..."
        labelFileProcessingComplete="上传完成!"
        labelFileProcessingAborted="取消上传"
        labelFileProcessingError="上传出现问题"
        labelFileProcessingRevertError="上传出现问题"
        labelFileRemoveError="删除文件出现问题"
        labelTapToCancel="点击取消上传"
        labelTapToRetry="点击重试"
        labelTapToUndo="点击撤销"
        labelButtonRemoveItem="删除"
        labelButtonAbortItemLoad="终止"
        labelButtonRetryItemLoad="重试"
        labelButtonAbortItemProcessing="终止"
        labelButtonUndoItemProcessing="撤销"
        labelButtonRetryItemProcessing="重试"
        labelButtonProcessItem="上传"
        labelFileLoadError="文件加载失败"
    />

  </div>
</template>

<script>
// Import Vue FilePond
import vueFilePond from 'vue-filepond';

// Import FilePond styles
import 'filepond/dist/filepond.min.css';

// Import FilePond plugins
// Please note that you need to install these plugins separately
// Import image preview plugin styles
import 'filepond-plugin-image-preview/dist/filepond-plugin-image-preview.min.css';

// Import image preview and file type validation plugins
import FilePondPluginFileValidateType from 'filepond-plugin-file-validate-type';
import FilePondPluginImagePreview from 'filepond-plugin-image-preview';

// Create component
const FilePond = vueFilePond(FilePondPluginFileValidateType, FilePondPluginImagePreview);

export default {
  name: 'FileUpload',
  data: function () {
    return {
      myFiles: [],
      uploadedFiles: []
    }
  },
  created() {
  },
  mounted() {
    // 关闭角标 如未给作者捐助或在其他位置放置，那么你不应该把这段注释关掉 :D
    // document.querySelector(".filepond--credits").style.display = "none"
  },
  methods: {
    transImgId(source) {
      return source.charAt(0) === "\"" ? source.slice(1, source.length - 1) : source
    },

    handleFilePondProcessfile: function (error, file) {
      this.uploadedFiles.push({filename: file.filename, file_server_name: file.serverId});
      this.$nextTick();
    },
    handleFilePondRemovefile: function (err, file) {
      this.uploadedFiles.map((item, index) => {
        if (item.filename === file.filename) {
          this.uploadedFiles.splice(index, 1)
          this.$nextTick();
        }
      });
    },
  },
  computed: {},
  components: {
    FilePond
  },
  watch: {
    files: {
      handler: function (val) {
        console.log(val)
      },
      deep: true
    }
  }

};
</script>


<style lang="scss">


</style>
