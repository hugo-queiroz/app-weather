<template>
  <router-view />
</template>

<style lang="scss">
/* VARIABLES */
$mobile: 480px;
$desktop: 1024px;
/* MIXINS RESPONSIVO */
@mixin desktop {
  @media only screen and (max-width: $desktop) {
    @content;
  }
}
@mixin mobile {
  @media only screen and (max-width: $mobile) {
    @content;
  }
}
h1,
h2,
h3,
h4,
h5,
h6,
p,
a,
figure {
  margin: 0;
  padding: 0;
}
a {
  text-decoration: none;
}
body {
  height: 100vh;
  font-family: 'Poppins', sans-serif;
}
#app,
main {
  height: 100%;
}
main,
#modal {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  header {
    text-align: center;
    line-height: 100%;
    h1 {
      font-weight: 200;
      text-transform: uppercase;
      font-size: 2.3rem;
      letter-spacing: 0.15rem;
      @include mobile {
        font-size: 1.8rem;
      }
    }
    h4 {
      font-weight: 100;
      font-size: 1.45rem;
      letter-spacing: 0.06rem;
      @include mobile {
        font-size: 1.2rem;
      }
    }
  }
}
/* CENTER CONTENT */
.wrapper {
  figure {
    margin: 2.6rem 0 3.2rem 0;
    img {
      width: 7.2rem;
      @include mobile {
        width: 6rem;
      }
    }
  }
}
/* FOOTER */
footer {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  text-align: center;
  gap: 2rem 0;
  width: 30%;
  @include desktop {
    width: 80%;
  }
  @include mobile {
    width: 100%;
  }
}
</style>
