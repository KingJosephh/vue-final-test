<template>
    <div class="text-end">
        <button class="btn btn-primary" typ="button" @click="openModal(true)">生成modal</button>
    </div>
<table class="table mt-4 container-fluid">
<thead>
<tr>
    <th width="120">分類</th>
    <th>產品名稱</th>
    <th width="120">原價</th>
    <th width="120">售價</th>
    <th width="100">是否啟用</th>
    <th width="200">編輯</th>
</tr>
</thead>
<tbody>
<tr v-for="item in products" :key="item.id">
    <td>{{ item.category }}</td>
    <td>{{ item.title }}</td>
    <td class="text-right">
    {{ item.origin_price }}
    </td>
    <td class="text-right">
    {{ item.price }}
    </td>
    <td>
    <span class="text-success" v-if="item.is_enabled">啟用</span>
    <span class="text-warn" v-else>未啟用</span>
    </td>
    <td>
    <div class="btn-group">
        <button class="btn btn-outline-primary btn-sm"
        @click.prevent="openModal(false)">編輯</button>
        <button class="btn btn-outline-danger btn-sm">刪除</button>
    </div>
    </td>
</tr>
</tbody>
</table>
<ProductModal ref="ProductModal"
:product="tempProduct"
@update-product="updateProduct"></ProductModal>
</template>

<script>
import ProductModal from '../components/productModel.vue'
export default {
  data () {
    return {
      products: [],
      partList: {},
      tempProduct: {
        title: '', // 字符串类型
        category: '', // 字符串类型
        unit: '', // 字符串类型
        origin_price: 0, // 数值类型
        price: 0 // 数值类型
      // 其他属性...
      },
      isNew: true
    }
  },
  components: {
    ProductModal
  },
  methods: {
    getProducts () {
      const api = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/products`
      this.$http.get(api).then((res) => {
        if (res.data.success) {
          this.products = res.data.products
          this.partList = res.data.pagination
        }
      })
    },
    openModal (isNew, item) {
      if (isNew) {
        this.tempProduct = {}
      } else {
        this.tempProduct = { ...item }
      }
      this.isNew = isNew
      console.log(this.tempProduct)
      const productComponent = this.$refs.ProductModal
      productComponent.showModel()
    },
    updateProduct (item) {
      this.tempProduct = item
      const productComponent = this.$refs.ProductModal
      const api = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/product`
      this.$http.post(api, { data: this.tempProduct }).then((response) => {
        console.log(response)
        productComponent.hideModal()
        this.getProducts()
      })
    }
  },
  created () {
    this.getProducts()
  }
}
</script>
