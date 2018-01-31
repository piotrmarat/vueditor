<style>
  .icon-asset {
    background: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEAAAABACAMAAACdt4HsAAAASFBMVEUAAABmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmZmbrBiVgAAAAF3RSTlMAAwQFCBAWJC4wO3CAiJGdur7Z6fH5/TEQ90UAAACfSURBVFjD7cxLEsIwDATRwRgIhJDw1f1vyopy4diKxt6q9/0AAIe3qAFAgNIs28AwKcDLAsitDogJUAQjUBesQFUwAzXBDlQEAigLDFAUKKAkcEBBIIG1wAIrgQZygQcyoQGQiQXikBVIQG0LiL3A2AvIGDuBYncHHHDAAQcccMABBzTg0wQsCViagGsCzk3AMQG4POn9ccJfYc+1+41fiBsV0vc5ZyEAAAAASUVORK5CYII=')
    no-repeat
    left center;
  }
</style>
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

<template>
  <div class="ve-dialog" v-show="showPopup" @click.self="hideDialog">
    <div :class="$style.wrap">
      <div class="ve-dialog-header">Upload asset<a href="javascript:;" class="ve-close" @click="hideDialog">&times;</a></div>
      <div class="ve-dialog-body">
        <form ref="form">
          <label>Link</label>
          <div class="ve-input-box">
            <input type="text" class="ve-input" v-model="link">
          </div>
          <input type="file" name="image" @change="changeHandler" ref="file">
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
        lang: getLang('picture'),
        uploadUrl: getConfig('uploadUrl')
      }
    },
    computed: {
      showPopup: function () {
        return this.$store.state.toolbar.asset.showPopup
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
        event.preventDefault()
        event.stopPropagation()
        let obj = this.$refs.file
        let form = this.$refs.form
        let uploadUrl = this.uploadUrl
        this.$parent.upload(obj, function (href) {
          this.$store.dispatch('execCommand', {name: 'insertHTML', value: `<a href="${href}" target='_blank'>${this.link}</a>`})
          this.hideDialog()
        }.bind(this))
      }
    }
  }
</script>
