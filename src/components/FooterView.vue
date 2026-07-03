<template>
  <div class="container">
    <div class="footer-container">
        <div class="footer-1">
          <a><h2 class="fufu">BashoFashion</h2></a>
          <!-- <br> -->
          <p><b>ONLINE SHOPPING</b></p>
            <h6>
              Men<br><br>
              Women <br><br>
              Kids <br><br>
              Merline Exclusive<br><br>
            </h6>
        </div>
  
        <div class="footer-2">
          <p><b>USEFUL LINKS</b></p>
          <h6>
            Contact Us<br><br>
            FAQ<br><br>
            T&C<br><br>
            Blog<br><br>
            Privacy Policy<br><br>
          </h6>
        </div>
  
        <div class="footer-3">
          <p><b>100% Original</b> guarantee</p>
            <h6>
              for all products at merlinfashion.com
            </h6>
          <p><b>Return within 30days</b> of</p>
            <h6>
              receiving you order
            </h6>
          <p><b>Get free delivery</b> for every</p>
          <br>
        </div>
      </div>
      <p class="copy-right"><a>BashoFashion</a>&copy;2025</p>
  </div>
</template>

<script>
export default {
  name: "FooterView",
};
</script>

<style scoped>
.footer-container{
  align-content: center;
  display:grid;
  grid-template-columns: 1fr 1fr 1fr ;
  background-color: black;
  object-fit: cover;
  margin-top: 10px;
}
.footer-container .footer-1, .footer-2, .footer-3, h5,h4,h2,h3,h6,p{
  color:white;
}

.footer-container .footer-1 h2,p,h5,h6{
  padding-left:50px;
  padding-top:10px  
}

.footer-container .footer-2,.footer-3{
  padding-top:60px;
}
.copy-right{
  background-color: rgb(28, 162, 184);
  font-family: 'Sacramento', cursive;
  text-align: center;
  color:white; 
  font-size:18px;
  padding-top: 5px;
  padding-bottom: 10px;
}
.copy-right a{
  font-family: 'Condiment', cursive;
  color:white; 
}
.fufu {
  font-family: 'Condiment', cursive;
}
</style>
