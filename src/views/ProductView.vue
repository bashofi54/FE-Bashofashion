<template>
  <div>
    <NavBar />
    <div class="container">
      <div>
        <h2 class="main-head-of-products">
          Everyone Has A Style Statment<br />
          <span class="colored-word-bigger-size">FIND YOURS HERE!</span>
        </h2>
        <br />
        <div class="bg-images">
          <a href=""><img src="../assets/images/14.jpg" alt="" /></a>
        </div>
        <p class="msg-ps">
          <span class="colored-word">Click</span> on the product to add in your
          shopping list. <br />Go below and check
          <span class="colored-word">YOUR CART</span> for details.
        </p>
        <hr style="background-color: rgb(28, 162, 184); height: 2px" />
      </div>

      <!-- FITUR BARU: LIST 9 CATEGORY -->
      <div class="mb-3">
        <h5 class="mb-2">Filter Kategori:</h5>
        <div class="d-flex flex-wrap gap-2">
          <button
            v-for="cat in categories"
            :key="cat.slug"
            class="btn btn-sm"
            :class="
              activeCategory === cat.slug
                ? 'btn-info text-white'
                : 'btn-outline-info'
            "
            @click="getProductsByCategory(cat.slug)"
          >
            {{ cat.slug.replace("-", " ") }}
          </button>
        </div>
      </div>

      <div class="input-group mb-3">
        <input
          v-model="search"
          type="text"
          class="form-control"
          placeholder="Cari Produk"
          aria-label="Search"
          aria-describedby="basic-addon1"
          @keyup="searchProduct"
        />
        <span class="input-group-text" id="basic-addon1"
          ><b-icon-search></b-icon-search
        ></span>
      </div>
      <div class="row mb-4">
        <div
          class="col-6 col-md-3 mb-4 d-flex align-items-stretch"
          v-for="product in products"
          :key="product.id"
        >
          <CardProduct :product="product" />
        </div>
        <div v-if="products.length === 0" class="col-12 text-center mt-4">
          <p>Produk tidak ditemukan...</p>
        </div>
      </div>
    </div>
    <FooterView />
  </div>
</template>

<script>
import NavBar from "@/components/NavBar.vue";
import CardProduct from "@/components/CardProduct.vue";
import FooterView from "@/components/FooterView.vue";
import axios from "axios";

export default {
  name: "ProductView",
  components: { NavBar, CardProduct, FooterView },
  data() {
    return {
      products: [],
      search: "",
      categories: [],
      activeCategory: "all",
      allowedCats: [
        "beauty",
        "mens-shirts",
        "mens-shoes",
        "mens-watches",
        "womens-bags",
        "womens-dresses",
        "womens-jewellery",
        "womens-shoes",
        "womens-watches",
      ],
      defaultCats: ["beauty", "mens-shirts", "womens-bags", "womens-dresses"],
    };
  },
  methods: {
    setProduct(data) {
      const unique = [...new Map(data.map((item) => [item.id, item])).values()];
      this.products = unique;
    },

    getProductsByCategory(slug) {
      const self = this;
      self.activeCategory = slug;
      self.search = "";

      // KUNCI: Kalau klik All, jangan panggil API /category/all
      if (slug === "all") {
        self.loadMultipleCategories(self.defaultCats); // Panggil yg 4 category
        return;
      }

      // Kalau category biasa, baru panggil API
      axios
        .get(`https://dummyjson.com/products/category/${slug}?limit=100`)
        .then(function (response) {
          self.setProduct(response.data.products);
        })
        .catch(function () {
          self.products = [];
        });
    },

    loadMultipleCategories(slugArray) {
      const self = this;
      self.activeCategory = "all";
      self.search = "";

      const requests = slugArray.map(function (slug) {
        return axios.get(
          `https://dummyjson.com/products/category/${slug}?limit=100`,
        );
      });

      Promise.all(requests)
        .then(function (responses) {
          let allProducts = [];
          responses.forEach(function (res) {
            allProducts = allProducts.concat(res.data.products);
          });
          self.setProduct(allProducts);
        })
        .catch(function () {
          self.products = [];
        });
    },

    searchProduct() {
      const self = this;
      const q = self.search.trim();

      if (q === "") {
        if (self.activeCategory === "all") {
          self.loadMultipleCategories(self.defaultCats);
        } else {
          self.getProductsByCategory(self.activeCategory);
        }
        return;
      }

      axios
        .get(`https://dummyjson.com/products/search?q=${q}&limit=100`)
        .then(function (response) {
          let result = response.data.products;
          if (self.activeCategory === "all") {
            result = result.filter(function (p) {
              return self.defaultCats.includes(p.category);
            });
          } else {
            result = result.filter(function (p) {
              return p.category === self.activeCategory;
            });
          }
          self.setProduct(result);
        })
        .catch(function () {
          self.products = [];
        });
    },

    loadCategories() {
      const self = this;
      axios
        .get("https://dummyjson.com/products/categories")
        .then(function (res) {
          self.categories = [
            { slug: "all", name: "All" },
            ...res.data.filter(function (cat) {
              return self.allowedCats.includes(cat.slug);
            }),
          ];
        });
    },
  },
  mounted() {
    this.loadCategories();
    this.loadMultipleCategories(this.defaultCats);
  },
};
</script>

<style scoped>
.msg-ps {
  color: black;
  text-align: center;
  padding-bottom: 30px;
}
.bg-images {
  text-align: center;
  border-radius: 50px;
}
.bg-images img {
  width: 800px;
  object-fit: cover;
}
.bg-images {
  background-color: #f3f3f3;
}
.main-head-of-products {
  color: black;
  text-align: center;
}
.colored-word {
  color: #f84258;
}
.colored-word-bigger-size {
  color: #f84258;
  font-size: 35px;
}
/* STYLE BARU BUAT CATEGORY */
.d-flex.gap-2 {
  gap: 0.5rem;
}
.btn-outline-info:hover {
  color: #fff;
}
</style>
