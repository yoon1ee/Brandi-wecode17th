<template>
  <div>
    <a-descriptions bordered size="small" class="seller-from" label-width="20%">
      <a-descriptions-item label="상품코드" :span="3">
        SB000000000009045050
        <a-button type="success" size="small" @click="showHistoryModal">수정이력보기</a-button>
      </a-descriptions-item>
      <a-descriptions-item label="판매여부" :span="3">
        <a-radio-group v-model="data.seller_property_id">
          <a-radio :value="1">판매</a-radio>
          <a-radio :value="2">미판매</a-radio>
        </a-radio-group>
      </a-descriptions-item>
      <a-descriptions-item label="진열여부" :span="3">
        <a-radio-group v-model="data.seller_property_id">
          <a-radio :value="1">진열</a-radio>
          <a-radio :value="2">미진열</a-radio>
        </a-radio-group>
      </a-descriptions-item>
      <a-descriptions-item label="카테고리" :span="3">

        <table class="bordered">
          <thead>
          <tr>
            <th>1차 카테고리</th>
            <th>2차 카테고리</th>
            <th></th>
          </tr>
          </thead>
          <tbody>
          <tr>
            <td>
              <a-select style="width: 100%">
                <a-select-option :value="item.value" v-for="item in firstCategory" :key="item.value">{{ item.text }}</a-select-option>
              </a-select>
            </td>
            <td>
              <a-select style="width: 100%">
                <a-select-option :value="item.value" v-for="item in secondCategory" :key="item.value">{{ item.text }}</a-select-option>
              </a-select>
            </td>
            <td><a-button type="normal">-</a-button></td>
          </tr>
          </tbody>
        </table>

      </a-descriptions-item>
      <a-descriptions-item :span="3">
        <template slot="label">상품 정보 고시 <span class="required">*</span></template>
        <a-radio-group v-model="data.gosiType">
          <a-radio :value="1">상품상세 참조</a-radio>
          <a-radio :value="2">직접입력</a-radio>

          <table class="bordered" v-show="data.gosiType == 2">
            <tbody>
            <tr>
              <th width="150">제조사(수입사)</th>
              <td><a-input class="normal-size"/></td>
            </tr>
            <tr>
              <th>제조일자</th>
              <td><a-date-picker/></td>
            </tr>
            <tr>
              <th>원산지</th>
              <td>
                <a-select class="small-size">
                  <a-select-option value="">기타</a-select-option>
                  <a-select-option value="1">중국</a-select-option>
                  <a-select-option value="2">한국</a-select-option>
                  <a-select-option value="3">베트남</a-select-option>
                </a-select>
              </td>
            </tr>
            </tbody>
          </table>
        </a-radio-group>
      </a-descriptions-item>
      <a-descriptions-item :span="3">
        <template slot="label">상품명 <span class="required">*</span></template>
        <a-input placeholder="상품명" class="large-size" v-model="data.brand_name_english" /><br/>
        <info-text label="상품명에는 쌍따옴표(&quot;) 또는 홑따옴표(')를 포함할 수 없습니다."></info-text><br/>
        <info-text label="상품명에는 4byte 이모지를 포함할 수 없습니다."></info-text>
      </a-descriptions-item>
      <a-descriptions-item label="한줄 상품 설명" :span="3">
        <a-input placeholder="한줄 상품 설명" class="large-size" v-model="data.brand_name_english" />
      </a-descriptions-item>
      <a-descriptions-item :span="3">
        <template slot="label">이미지 등록 <span class="required">*</span></template>

        <image-upload v-for="i in [0,1,2,3,4]" :key="i"/>
        <br>
        <info-text label="640 * 720 사이즈 이상 등록 가능하며 확장자는 jpg 만 등록 가능합니다."/>

      </a-descriptions-item>
      <a-descriptions-item :span="3">
        <template slot="label">상세 상품 정보 <span class="required">*</span></template>
        <a-textarea>곧 만들예정</a-textarea>
      </a-descriptions-item>
    </a-descriptions>
    <product-history-modal ref="historyModal"/>
  </div>
</template>

<script>
import ImageUpload from '@/admin/Components/Common/image-upload'
import InfoText from '@/admin/Components/Common/info-text'
import ProductHistoryModal from './product-history-modal'
export default {
  components: {
    ImageUpload,
    InfoText,
    ProductHistoryModal
  },
  data () {
    return {
      data: {
        seller_property_id: 1,
        brand_name_english: '',
        brand_name_korean: '',
        gosiType: 1
      }
    }
  },
  computed: {
    firstCategory () {
      return this.$store.state.const.firstCategory
    },
    secondCategory () {
      return this.$store.state.const.secondCategory
    }
  },
  methods: {
    showHistoryModal () {
      this.$refs.historyModal.show()
    },
    setFormData (value) {

    },
    getFromData () {
      return JSON.stringify(JSON.parse(this.data))
    },
    validate () {
      return true
    }
  }
}
</script>

<style type="scss" scoped>
.normal-size {
  width: 200px;
}
</style>
<style type="scss" scoped>
.normal-size {
  width: 200px;
}
br + .normal-size {
  margin-top: 5px;
}
.manager {
  display: inline-block;
}
.manager-button {
  display: inline-block;
}
.find-post-button {
  margin-left: 5px;
}
hr {
  border: none;
  border-top: 1px solid #CCC;
}

</style>
