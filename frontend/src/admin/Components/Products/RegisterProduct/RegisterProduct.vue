<template>
  <div>
    <div class="fixed-header">
      <h2>상품 수정
        <a-button type="success" @click="working=true">저장하기</a-button>
        <a-button type="normal" @click="working=false">취소</a-button>
        <a-spin spin v-show="working"/>
      </h2>
      <div class="divide">
        회원 관리 > 상품 관리 > 상품 수정
      </div>
      <div class="form-header">
        <a-tabs type="card" :tabBarStyle="{'padding-bottom': 0, 'margin-bottom': '-1px'}" v-model="tabNo" @change="scrollTab">
          <a-tab-pane :key="0" tab="기본 정보" />
          <a-tab-pane :key="1" tab="옵션 정보"/>
          <a-tab-pane :key="2" tab="판매 정보" />
        </a-tabs>
      </div>
    </div>
    <a-page-header title="기본 정보" key="1" sub-title="기본 정보" class="page-content" v-multi-ref:pageContent>
      <product-basic-info-form></product-basic-info-form>
    </a-page-header>
    <a-page-header title="옵션 정보" key="2" class="page-content" v-multi-ref:pageContent>
      <product-option-info-form></product-option-info-form>
    </a-page-header>
    <a-page-header title="판매 정보" key="3" class="page-content" v-multi-ref:pageContent>
      <product-sale-info-form></product-sale-info-form>
    </a-page-header>

  </div>
</template>

<script>
import ProductBasicInfoForm from './product-basic-info-form'
import ProductSaleInfoForm from './product-sale-info-form'
import ProductOptionInfoForm from './product-option-info-form'
import 'vue-multi-ref'
import Utils from '@/admin/utils/utils'

export default {
  name: 'register-seller',
  data () {
    return {
      tabNo: 0,
      working: false
    }
  },
  components: {
    ProductBasicInfoForm,
    ProductSaleInfoForm,
    ProductOptionInfoForm
  },
  mounted () {
    document.addEventListener('scroll', this.scrollEvent)
  },
  destroyed () {
    document.removeEventListener('scroll', this.scrollEvent)
  },
  methods: {
    scrollEvent (event) {
      const pageContents = this.$refs.pageContent
      const len = pageContents.length
      // 마지막은 끝으로 인식
      if (document.body.scrollHeight - document.body.offsetHeight === document.body.scrollTop) {
        this.tabNo = len - 1
        return
      }
      const elTop = Utils.getOffset(this.$refs.pageContent[0].$el).top
      for (let i = 0; i < len; i++) {
        if (pageContents[i].$el.getBoundingClientRect().top < elTop && pageContents[i].$el.getBoundingClientRect().bottom > elTop) {
          this.tabNo = i
        }
      }
    },
    scrollTab (tabNo) {
      const pageContents = this.$refs.pageContent
      const elTop = Utils.getOffset(this.$refs.pageContent[0].$el).top
      window.scrollTo({ top: Utils.getOffset(pageContents[tabNo].$el).top - elTop, behavior: 'smooth' })
    }
  },
  watch: {
    tabNo (v) {
      console.log('changeV', v)
    }
  }
}
</script>

<style type="scss" scoped>
.fixed-header {
  position: sticky;
  top: 45px;
  background: #FFF;
  z-index: 100;
  margin-top: -25px
}
.form-header {
  margin-top: 5px;
}
.divide {
  background: #F1F1F1;
  padding: 5px 10px;
  margin-left: -20px;
  margin-right: -20px;
  font-size: 12px;
  font-weight: bold;
}
.page-content {
  border: 1px solid #CCC;
  margin-bottom: 1em;
  background: #FFF;
}
h2 {
  padding-top: 10px
}
</style>
<style>
.seller-from .ant-descriptions-item-label {
  width: 190px
}
</style>
