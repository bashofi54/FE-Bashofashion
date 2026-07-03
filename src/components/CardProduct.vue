<template>
  <div class="h-100"> <!-- 1. Bungkus paling luar kasih h-100 -->
    <div class="card shadow h-100 d-flex flex-column"> <!-- 2. Card = flex column + tinggi 100% -->
      <img
        :src="product.thumbnail"
        class="card-img-top"
        style="height: 200px; object-fit: cover; border-radius: 15px 15px 0 0;" 
        alt="..."
      />
      <div class="card-body d-flex flex-column flex-grow-1"> <!-- 3. Body juga flex column -->
        <h5 class="card-title">
          {{ product.title }}
        </h5>
        <p class="card-text mb-2">Harga : Rp. {{ product.price.toLocaleString('id-ID') }}</p>
        
        <router-link
          class="btn btn-link p-0 text-info mt-auto" 
          :to="'/product/' + product.id"
        >
          <b-icon-info-circle-fill></b-icon-info-circle-fill> Detail Product
        </router-link>

        <!-- <div class="button mt-2"> 5. Tombol di bawah link -->
          <!-- <button
            type="button"
            class="btn btn-primary w-100"
            @click="$bvModal.show('modal-' + product.id)"
          >
            <b-icon-cart></b-icon-cart> Pesan
          </button> -->
          <!-- ... modal tetep sama ... -->
        <!-- </div> -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardProduct",
  props: ["product"],
  data() {
    return {
      activeSlide: 0,
      qty: 1
    };
  },
  methods: {
    addToCartModal() {
      alert(`Pesanan: ${this.qty} x ${this.product.title}`);
      this.$bvModal.hide('modal-' + this.product.id);
    }
  }
};
</script>

<style scoped>
.card-title {
  color: black;
  font-size: 0.95rem;
  font-weight: 600;
  
  /* 6. KUNCI: Paksa judul max 2 baris. Lebih dari itu = ... */
  display: -webkit-box;
  line-clamp: 2;
  -webkit-line-clamp: 2; 
  -webkit-box-orient: vertical; 
  overflow: hidden;
  text-overflow: ellipsis;
  min-height: 2.8em; /* = 2 baris */
}

.card-text {
  color: black;
  font-weight: bold;
}

.button {
  text-align: center;
}

/* 7. KUNCI: Buat card oval kayak di ss kamu */
.card {
  border-radius: 120px 120px 20px 20px; /* Atas bulat, bawah agak kotak */
  overflow: hidden; 
}
</style>