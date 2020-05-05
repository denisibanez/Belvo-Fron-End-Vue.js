<template>
  <div>
    <b-container fluid v-if="loading">
      <b-row style="padding:20%">
        <b-col xl="12">
          <h1>Loading... :)</h1>
          <b-spinner type="grow" label="Spinning"></b-spinner>
          <b-spinner type="grow" label="Spinning"></b-spinner>
          <b-spinner type="grow" label="Spinning"></b-spinner>
          <b-spinner type="grow" label="Spinning"></b-spinner>
        </b-col>
      </b-row>
    </b-container>

    <b-container class="home-wrapper" fluid v-else >
      <b-row class="nav-wrapper justify-content-center">
        <b-navbar type="dark" variant="dark" toggleable="md">
          <b-navbar-toggle target="nav-text-collapse"></b-navbar-toggle>
          <b-navbar-brand class="d-none d-xl-block">
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

      <b-row class="body-wrapper text-left justify-content-start">
        <b-col md="12" xl="10">
          <b-row class="body-top-wrapper">
            <b-col md="12" xl="4" class="mt-2">
              <h5>{{ albums.title.label }}</h5>
            </b-col>

            <b-col md="12" xl="4" class="mt-2">
              <b-button-group size="md">
                <b-button
                  v-for="(btn, idx) in buttons"
                  :key="idx"
                  :class="{ 'active' : btn.state }"
                  @click="checkBtn(btn)"
                  variant="primary"
                >
                  {{ btn.caption }}
                </b-button>
              </b-button-group>
            </b-col>

            <b-col md="12" xl="4" class="mt-2">
              <b-nav-form>
                <b-input
                  size="md"
                  class="mr-md-2"
                  placeholder="Search..."
                  v-model="search">
                </b-input>
              </b-nav-form>
            </b-col>
          </b-row>

          <b-row class="album-wrapper">
            <b-col
              md="6"
              xl="4"
              class="mt-3 mb-3"
              v-for="(album, key) in searchByParam"
              :key="key">
                <b-card
                  no-body
                  class="overflow-hidden pointer"
                  style="max-width: 540px;"
                  v-b-modal.modalAlbumSelect
                  @click="albumSelect = album">
                  <b-row no-gutters>
                    <b-col md="12" xl="4">
                      <b-card-img
                        :src="album.imImg[2].label"
                        alt="Image"
                        class="img-170 rounded-0">
                      </b-card-img>
                    </b-col>
                    <b-col md="12" xl="8">
                      <b-card-body :title="album.title.label">
                        <b-card-text>
                          <strong>Category: </strong>
                          {{ album.category.attributes.label}}
                        </b-card-text>
                      </b-card-body>
                    </b-col>
                  </b-row>
                </b-card>
            </b-col>
          </b-row>
          <b-alert show variant="dark" v-if="!searchByParam.length">
            Sorry, not found album. :(
          </b-alert>
        </b-col>

        <b-col md="12" xl="2">
          <b-card-group deck>
            <b-card header="Categories">
              <b-list-group>
                <b-list-group-item v-for="(item, key) in categories" :key="key">
                  {{ item }}
                </b-list-group-item>
              </b-list-group>

              <p class="card-text mt-2">
                {{ fakeText }}
              </p>
            </b-card>
          </b-card-group>
        </b-col>
      </b-row>

      <b-row class="footer-wrapper mt-5">
        <b-col xl="12">
          <b-row class="d-flex justify-content-xl-start">
            <b-col xl="12">
              <p>Copyright © 2020 <a href="https://www.linkedin.com/in/denis-ibanez/">Denis Ibanez</a>. Todos os direitos reservados.</p>
            </b-col>
          </b-row>
        </b-col>
      </b-row>

      <div v-if="albumSelect"> 
        <b-modal id="modalAlbumSelect" :title="albumSelect.title.label" hide-footer>
          <b-row>
            <b-col xl="4">
              <img 
                :src="albumSelect.imImg[2].label"
                :alt="albumSelect.title.label"
                class="primary">
            </b-col>

            <b-col xl="8">
              <h5>{{ albumSelect.title.label }}</h5>
              <h6>{{ albumSelect.imRelease.attributes.label }} </h6>
              <p> Favorite
                <b-icon-heart
                  v-if="!fav"
                  @click="setFav(true)">
                </b-icon-heart>
                <b-icon-heart-fill 
                  v-else
                  @click="setFav(false)">
                </b-icon-heart-fill>
              </p>
              <p>
                <strong>
                  {{ albumSelect.imPrice.label }}
                </strong>
              </p>

              <p style="font-size:12px">
                {{ albumSelect.rights.label }}
              </p>

              <div class="action-content">
                <a 
                  class="btn-primary btn"
                  :href="albumSelect.link.attributes.href">
                    Preview  
                </a>
              </div>
            </b-col>
          </b-row>

          <b-row>
            <b-col>
              <div class="mt-3">
                <h5>Description</h5>
                <p>{{ fakeText }}</p>
              </div>
            </b-col>
          </b-row>
          
        </b-modal>
      </div>
    </b-container>
  </div>
</template>

<script>
import AlbumService from '@/services/album-service'

export default {
  name: 'Home',
  data () {
    return {
      loading: true,
      fav: false,
      search: null,
      navs: [ 
        { active:false, value: 'Best Album' },
        { active:false, value: 'Library' },
        { active:false, value: 'For You' },
        { active:false, value: 'Radio' },
        { active:false, value: 'Browse' }
      ],
      buttons: [
        { key: 'old', caption: 'New Music', state: true },
        { key: 'new', caption: 'Old Music', state: false }
      ],
      albums: [],
      categories: [],
      albumSelect: null,
      fakeText: 'Quis magna Lorem anim amet ipsum do mollit sit cillum voluptate ex nulla tempor. Laborum consequat non elit enim exercitation cillum aliqua consequat id aliqua. Esse ex consectetur mollit voluptate est in duis laboris ad sit ipsum anim Lorem.'
    }
  },

  computed: {
    searchByParam() {
      if(this.albums.entry && this.search) {
        let items = []
        this.albums.entry.filter(item => {
          const _self = this
          let contains = item.title.label.toLowerCase().includes(_self.search.toLowerCase())
          if(contains) items.push(item)
        })  
        return items
      } else {
        return this.albums.entry
      }
    }
  },

  created() {
    this.albumService = new AlbumService()
    this.getAlbums()
  },

  methods: {
    getAlbums() {
      const param = '100/json'
      this.albumService.getAlbums(param).then((response) => {
        let data = JSON.stringify(response.data.feed)
        data = data.replace(/im:image/g, 'imImg')
        data = data.replace(/im:releaseDate/g, 'imRelease')
        data = data.replace(/im:price/g, 'imPrice')
        
        this.albums = JSON.parse(data)
        this.loading = false
      }).catch((error) => {
        console.log(error)
      }).finally(() => {
        this.sortByNew()
        this.mapCategorys(this.albums)
      })
    },

    mapCategorys(items) {
      items.entry.map((item) => {
        this.categories.push(item.category.attributes.label)
      })

      const setUnic = new Set(this.categories)
      this.categories = [...setUnic]
    },

    checkBtn(btn) {
      this.buttons.map((item) => {
        item.state = (item.caption === 'btn.caption') ? true : false
      })

      if(btn.key == 'old') this.sortByOld()
      else this.sortByNew()
    },

    sortByOld() {
      this.albums.entry = this.albums.entry.sort((a, b) => new Date(b.imRelease.label) - new Date(a.imRelease.label))
    },

    sortByNew() {
      this.albums.entry = this.albums.entry.sort((a, b) => new Date(a.imRelease.label) - new Date(b.imRelease.label))
    },

    setFav(param) {
      this.fav = param
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

  .body-wrapper {
    padding: 30px 15px;

    .body-top-wrapper {
      margin: 15px 0;
      h5 {
        text-align: left;
      }
    }

    .card {
      text-align: left;

      &.pointer {
        cursor: pointer;
      }

      .card-body {
        padding: 1rem;

        .img-170 {
          max-width: 170px;
        }

        .card-title {
          font-size: 12px;
          font-weight: bold;
        }

        .card-text {
          font-size: 10px;
          margin: 0;
        }

        .card-link {
          font-size: 14px;
        }
      }

      .card-header {
        background-color: rgb(50,50,50);
        color: #ffffff;
      }

      .list-group {
        .list-group-item {
          font-size:12px;
          padding: 5px;
          cursor: pointer;

          &:hover {
            background-color: rgb(50,50,50);
            color: #ffffff;
          }
        }
      }
    }
  }

  .footer-wrapper{
    background-color: rgb(50,50,50);
    padding: 15px;

    p {
      color:#f5f5f7;
      opacity: .56;
      text-align: left;
      font-size: 12px;
      margin: 0;
    }

    a {
      color:#ffffff;
    }
  }
}

#modalAlbumSelect {
  img {
    &.primary {
      border-radius: 5px;
      width: 100%;
    }
  }

  .btn-primary {
    background-color: #ffffff;
    border: 1px solid rgb(50,50,50);
    color:rgb(50,50,50);

    &.active, &:hover, &:focus {
      background-color:rgb(50,50,50);
      border: 1px solid rgb(50,50,50);
      color:#ffffff;
    }
  }
}
@media(max-width:575px) {
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
</style>
