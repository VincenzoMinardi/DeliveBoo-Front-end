<script>
export default {};
</script>
<template>
  <div class="footer">
    <div class="container">
      <h2>DeliveBoo</h2>
      <p class="subtitle">Tutto ciò che desideri a portata di click!</p>
      <p class="socialNet">Seguici su:</p>
      <div class="social-icons">
        <a href="#"><i class="bi bi-facebook"></i></a>
        <a href="#"><i class="bi bi-instagram"></i></a>
        <a href="#"><i class="bi bi-twitter"></i></a>
      </div>
      <p class="socialNet">Portaci in tasca:</p>
      <div class="appStore">
        <div class="icons-store">
          <a class="apple" href="#"><i class="bi bi-apple"></i> AppleStore</a>
        </div>
        <div class="icons-store">
          <a class="google" href="#"
            ><i class="bi bi-google-play"></i> GooglePlay</a
          >
        </div>
      </div>
      <div class="footerLinks">
        <a href="#">Informazioni</a>
        <a href="#">Note legali</a>
        <a href="#">Contattaci</a>
        <a href="#">Aiuto</a>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.footer {
  font-family: "Ubuntu", sans-serif;
  background-color: #37363c;
  line-height: 1.8rem;

  .container {
    max-width: 900px;
    text-align: center;
    margin: auto;
    padding: 1.5rem;
    color: white;

    h2,
    .subtitle {
      line-height: 0.9rem;
    }
  }

  .subtitle,
  .socialNet {
    font-size: 0.7rem;
    line-height: 0.4rem;
  }

  .appStore {
    display: flex;
    align-items: center;
    justify-content: center;

    .icons-store {
      border: 1px solid white;
      border-radius: 5px;
      padding: 0.2rem 1rem;
      margin-right: 0.5rem;

      .icons-store {
        transition: all 0.3ms ease;
        border: #f1bc6c;
      }
    }
  }

  .social-icons {
    margin-bottom: 0.6rem;
  }
  .social-icons i,
  .social-icons a {
    padding: 0.5rem;
    font-size: 1.5rem;
    color: white;
  }

  .footerLinks a,
  .appStore a {
    padding: 0.3rem;
    text-decoration: none;
    color: white;
  }

  .footerLinks a {
    font-size: 0.7rem;
    margin-top: 0.4rem;
  }

  .footerLinks a:hover,
  .social-icons i:hover {
    color: #f1bc6c;
  }

  .icons-store .apple:hover,
  .icons-store .google:hover,
  .icons-store:hover {
    color: #f1bc6c;
    transform: scale(1.1);
  }
}
</style>
