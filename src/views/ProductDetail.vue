<template>
  <div>
    <NavBar />
    <div class="container my-4" v-if="detailProduct.id">
      <div class="row mt-3">
        <div class="col-md-6">
          <!-- GAMBAR UTAMA: SEKARANG IKUTIN mainImage -->
          <img :src="mainImage" class="img-fluid shadow rounded" style="width: 100%; height: 450px; object-fit: contain;" />

          <!-- GALERI GAMBAR KECIL -->
          <div class="d-flex flex-wrap mt-3 gap-2" v-if="detailProduct.images.length > 1">
            <img
              v-for="(img, i) in detailProduct.images"
              :key="i"
              :src="img"
              class="img-thumbnail"
              :class="{ 'border border-success border-3': mainImage === img }" 
              style="width: 80px; height: 80px; object-fit: cover; cursor: pointer;"
              @click="mainImage = img" 
            />
          </div>
        </div>
        <div class="col-md-6">
          <h2 style="color: black">
            <strong>{{ detailProduct.title }}</strong>
          </h2>
          <hr />
          <h4 style="color: black">
            Harga : <strong>Rp. {{ detailProduct.price.toLocaleString('id-ID') }}</strong>
          </h4>
          <hr />
          <h4 style="color: black">
            Stok Barang : <strong>{{ detailProduct.stock }}</strong>
          </h4>
          <form class="mt-4" @submit.prevent="addToCart">
            <div class="form-group">
              <label>Jumlah pemesanan</label>
              <input type="number" v-model="qty" class="form-control" style="width: 120px;" min="1" :max="detailProduct.stock" />
            </div>
            <div class="mt-3">
              <label><strong>Keterangan</strong></label> <br>
              <label>{{ detailProduct.description }}</label>
            </div>
            <hr>
            <!-- <button type="submit" class="btn btn-success">
              <b-icon-cart></b-icon-cart> Pesan
            </button> -->
          </form>
        </div>
      </div>
    </div>
    <div v-else class="text-center mt-5">
      Loading produk...
    </div>
    <FooterView />
  </div>
</template>

<script>
import NavBar from "@/components/NavBar.vue";
import FooterView from "@/components/FooterView.vue";
import axios from "axios";

export default {
  name: "ProductDetail",
  components: {
    NavBar,
    FooterView,
  },
  data() {
    return {
      detailProduct: {},
      mainImage: '', // buat ganti gambar utama pas klik thumbnail
      qty: 1
    };
  },
  watch: {
    // Kalau data udah masuk, set gambar utama = gambar pertama
    detailProduct(val) {
      if(val.images && val.images.length > 0) {
        this.mainImage = val.images[0];
      }
    }
  },
  methods: {
    setProduct(data) {
      this.detailProduct = data;
    },
    addToCart() {
      alert(`Pesanan: ${this.qty} x ${this.detailProduct.title}`);
      // Nanti tinggal push ke cart store/vuex
    }
  },
  mounted() {
    const id = this.$route.params.id; // AMBIL ID DARI URL /product/1
    axios
     .get(`https://dummyjson.com/products/${id}`) // FIX: PAKE /products/{id}
     .then((response) => {
        this.setProduct(response.data);
      })
     .catch((error) => {
        console.log(error);
      });
  },
};
</script>

<style>
</style>