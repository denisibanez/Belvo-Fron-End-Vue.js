<template>
  <b-container fluid class="home-wrapper">
    <b-row class="nav-wrapper justify-content-center">
      <b-navbar type="dark" variant="dark" toggleable="sm">
        <b-navbar-toggle target="nav-text-collapse"></b-navbar-toggle>
        <b-navbar-brand class="d-none d-md-block">
          <img src="https://www.apple.com/ac/globalnav/5/pt_BR/images/globalnav/apple/image_large.svg" alt="logo">
        </b-navbar-brand>

        <b-collapse id="nav-text-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-item
              :active="nav.active" 
              v-for="(nav, key) in navs"
              :key="key">
                {{ nav.value }}
            </b-nav-item>
          </b-navbar-nav>
        </b-collapse>
      </b-navbar>
    </b-row> 
    
    <b-row>
      <b-carousel
        id="carousel-no-animation"
        style="text-shadow: 0px 0px 2px #000"
        no-animation
        indicators
        img-width="1920"
        img-height="280"
      >
        <b-carousel-slide
          caption="Michael Jackson"
          :img-src="require('../assets/slider1.png')"
        ></b-carousel-slide>
        <b-carousel-slide
          caption="Drake"
          :img-src="require('../assets/slider2.png')"
        ></b-carousel-slide>
        <b-carousel-slide
          caption="Billie Eilish"
          :img-src="require('../assets/slider3.png')"
        ></b-carousel-slide>
      </b-carousel>
    </b-row>  

    <b-row class="body-wwrapper">
      <b-col col md="10">
        <b-breadcrumb
          :items="breadcrumb">
        </b-breadcrumb>
      </b-col>
      <b-col col md="2">

      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      navs: [ 
        { active:true, value: 'Best Album' },
        { active:false, value: 'Library' },
        { active:false, value: 'For You' },
        { active:false, value: 'Radio' },
        { active:false, value: 'Browse' }
      ],
      breadcrumb: [
        { text: 'Home', href: '#' },
        { text: 'Best Albums', href: '#' }
      ]
    }
  }
}
</script>

<style  lang="scss">
.home-wrapper {
  .nav-wrapper {
    height: 45px;
    background: #343a40;

    .navbar {
      padding: 0;
      .navbar-brand{
        padding: 0;

        a {
          padding: 0;
        }
      }

      .navbar-toggler {
        border: 0;
      }
    
      a {
        color: #f5f5f7;
        font-size: 14px;
        padding: 0rem 3rem;

        &:hover {
          color: #f5f5f7;
        }

        &.active {
          color: #f5f5f7;
          opacity: .56;
        }
      }
    }
  }

  @media(max-width:768px) {
    .carousel-caption {
      display: none;
    }

    .navbar {
      width: 100%;
        .navbar-collapse {
          background: #343a40;
          width: 100%;
          z-index: 9999;
        }

        .navbar-nav {
          padding-top: 15px;
          
          .nav-item {
            padding: 15px;
            border-top: 1px solid #cccccc;
          }
        }
    }
  }
}
</style>
