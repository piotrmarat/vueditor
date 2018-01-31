<style module>
  .wrap {
    width: 500px;
    position: relative;
    top: 50%;
    background: #fff;
    margin: 0 auto;
    transform: translateY(-60%);
  }
</style>

<style>

  .icon-youtube {
    background: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAMAAACdt4HsAAABWVBMVEUAAABmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmb6CEA5AAAAcnRSTlMAAQIDBAUGBwgJCgsMDQ4PEBITFBUXGBkaGxwdHh8gIiMlJygqKy0vMDE0Njc8PkFCRUdMTVFSVFZXW1xeX2JjZmdoaWxvd3t8fn+GjI+SlJeYnaWmqKqrrbCytLW3vL7Ax8jMzs/R19ne4OLm8ff5+/2EXHfjAAABjElEQVRYw+3XV1PCQBQF4EMgEYxKUUAhRBQVexexoBR7w957jYpI9v8/+ODoMOOkmKtvOa8750sm2b2TAHbs/Es4l+Cp8/obg+GWqBSTZVmSIi3NwYCvQfQILs6hUatJzqxun96VVGaYN+X6sLg82cZV1cUd9vuUc99EZ4VZypP42ZeZ1bzyAOB4tgywFQDos95nqgDghACwEYCn9Nk5IJEAFRgiAQzIUIE1KrCntbRZNgfcay35+IWKGUDzOj7AvWICYDoAULdhBDj0AcC/qw9wRgAQOtYDnMYAIF9pA7wZAOi4IwJAr0IEwF3QgOQj6Q4St6Rn0HpNeguRM7194DICggeknRjYJR0m7xbpOIvrpgaKorV0ZHKknVNnYpEKLFKBMSrQTgVqSX0FwAMFKACYoABBAPy79f4lACBtua82fZ7YJav9nq+R0aVY6d+EqmZePLv/8pty6SQX+/nB7HTX+0PReEd3/8Dw6HgqPT07l80X8vnsfGZmKjU2MtifTMhhvyhw9j+JnT/OB8HFawcOkKrqAAAAAElFTkSuQmCC')
    left center;
  }
</style>

<template>
  <div class="ve-dialog" v-show="showPopup" @click.self="hideDialog">
    <div :class="$style.wrap">
      <div class="ve-dialog-header">Insert youtube<a href="javascript:;" class="ve-close" @click="hideDialog">&times;</a></div>
      <div class="ve-dialog-body">
        <form ref="form">
          <label>Link</label>
          <div class="ve-input-box">
            <input type="text" class="ve-input" v-model="link">
          </div>
        </form>
      </div>
      <div class="ve-dialog-footer">
        <div class="ve-btn-box">
          <button class="ve-btn" @click="hideDialog">{{lang.cancel}}</button>
          <button class="ve-btn" @click="certainHandler">{{lang.ok}}</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import { getLang } from '../config/lang.js'
  import { getConfig } from '../config/'

  export default {
    data () {
      return {
        link: "",
        lang: getLang('picture')
      }
    },
    computed: {
      showPopup: function () {
        return this.$store.state.toolbar.youtube.showPopup
      }
    },
    watch: {
      'showPopup': function (val) {
        if (val) {
          document.body.classList.add('ve-fixed')
        } else {
          document.body.classList.remove('ve-fixed')
        }
      }
    },
    methods: {
      hideDialog () {
        this.$store.dispatch('updatePopupDisplay')
      },
      changeHandler (event) {
      },
      certainHandler (event) {
        let video_id = this.link.split('/').pop()
        this.link = `http://www.youtube.com/embed/${video_id}`
        event.preventDefault()
        event.stopPropagation()
        let obj = this.$refs.file
        let form = this.$refs.form
        let uploadUrl = this.uploadUrl
        this.$store.dispatch('execCommand', {name: 'insertHTML', value: `<iframe title="YouTube video player" class="youtube-player" type="text/html" width="640" height="390" src="${this.link}" frameborder="0" allowFullScreen></iframe>`})
        this.hideDialog()
      }
    }
  }
</script>
