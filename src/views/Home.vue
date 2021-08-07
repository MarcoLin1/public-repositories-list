<template>
  <div class="container">
    <div class="title">
      Marco's Public Repositories
    </div>
    <div
      class="repo__container"
    >
      <div
        v-for="(repo, index) in slicedData"
        :key="index"
        class="repo__item"
      >
        <div
          class="repo__item__image__wrapper"
        >
          <img
            :src="'https://picsum.photos/300/300?image='+index"
            alt=""
            class="repo__item__image"
          >
        </div>
        <div
          :key="repo.id"
          class="repo__item__content"
        >
          <div class="repo__item__title__wrapper">
            <div class="repo__item__title__tag">
              Repository
            </div>
            <div class="repo__item__title">
              {{ repo.name }}
            </div>
          </div>
          <div class="repo__item__description__wrapper">
            <div class="repo__item__description__tag">
              Description
            </div>
            <div class="repo__item__description">
              {{ repo.description }}
            </div>
          </div>
          <div class="repo__item__language__wrapper">
            <div class="repo__item__language__tag">
              Language
            </div>
            <div class="repo__item__language">
              {{ repo.language }}
            </div>
          </div>
          <div class="repo__item__url__wrapper">
            <div class="repo__item__url__tag">
              URL
            </div>
            <a
              class="repo__item__url__link"
              :href="repo.html_url"
            >
              <div class="repo__item__url">
                Please click me
              </div>
            </a>
          </div>
        </div>
      </div>
      <div
        ref="footer"
        class="footer"
      >
        <div
          id="scroll__trigger"
          ref="infinitescrolltrigger"
        >
          End
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      repos: [],
      currentDataPage: 0,
      slicedData: [],
      names: [],
      options: {
        root: null,
        rootMargins: '0px',
        threshold: 0
      }
    }
  },
  created () {
    this.getRepo()
  },
  mounted () {
    this.scrollTrigger()
  },
  methods: {
    async getRepo () {
      try {
        const { data } = await axios.get('https://api.github.com/users/MarcoLin1/repos')
        data.forEach(item => {
          this.names.push(item.name)
        })
        this.oneRepo()
      } catch (e) {
        console.log(e)
      }
    },
    async oneRepo () {
      try {
        const name = this.names[this.currentDataPage]
        const { data } = await axios.get(`https://api.github.com/repos/MarcoLin1/${name}`)
        this.slicedData.push(data)
        this.currentDataPage += 1
      } catch (e) {
        console.log(e)
      }
    },
    scrollTrigger () {
      const observer = new IntersectionObserver(this.oneRepo, this.options)
      observer.observe(this.$refs.footer)
    }
  }
}
</script>

<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css2?family=Alfa+Slab+One&family=Bree+Serif&family=Domine:wght@400;500;600&display=swap');
%tag__style {
  width: 110px;
  min-width: 110px;
  font-weight: 600;
  padding: 8px;
  border-radius: 10px;
  border: 2px solid gray;
  text-align: center;
  margin-right: 10px;
  font-family: 'Domine', serif;
}
.container {
  height: 100%;
  .title {
    font-weight: 700;
    font-size: 1.5rem;
    font-family: 'Alfa Slab One', cursive;
    width: 100%;
    margin: 0 auto;
    text-align: center;
    padding: 20px 0 20px 0;
  }
}
.repo__container {
  outline: 2px solid ;
  height: 430px;
  max-width: 1000px;
  overflow: auto;
  margin: 0 auto;
  padding: 20px;
  .repo__item {
    margin-bottom: 40px;
    margin-top: 20px;
    box-shadow: 10px 10px 20px #cbced1, 10px 10px 20px white;
    background-color: #ecf0f3;
    border-radius: 10px;
    &__image__wrapper {
      display: flex;
      justify-content: center;
      padding-top: 20px;
    }
    &__image {
      border-radius: 10px;
      margin-right: 20px;
    }
    &__content {
      width: 100%;
      min-width: 200px;
      padding: 20px;
      display: flex;
      flex-direction: column;
    }
  }
  .repo__item__title {
    font-family: 'Bree Serif', serif;
    padding-right: 20px;
    &__wrapper {
      display: flex;
      align-items: center;
      margin-bottom: 10px;
    }
    &__tag {
      @extend %tag__style;
      &:hover {
        background-color: rgb(90, 120, 131);
        color: #fff;
        border-color: rgb(90, 120, 131);
      }
    }
  }
  .repo__item__description {
    font-family: 'Bree Serif', serif;
    word-break: break-word;
    padding-right: 20px;
    &__wrapper {
      display: flex;
      align-items: center;
      margin-bottom: 10px;
    }
    &__tag {
      @extend %tag__style;
      &:hover {
        background-color: rgb(90, 120, 131);
        color: #fff;
        border-color: rgb(90, 120, 131);
      }
    }
  }
  .repo__item__language {
    font-family: 'Bree Serif', serif;
    &__wrapper {
      display: flex;
      align-items: center;
      margin-bottom: 10px;
    }
    &__tag {
      @extend %tag__style;
      &:hover {
        background-color: rgb(90, 120, 131);
        color: #fff;
        border-color: rgb(90, 120, 131);
      }
    }
  }
  .repo__item__url {
    font-family: 'Bree Serif', serif;
    color: rgb(90, 120, 131);
    &:hover {
      background-color: rgb(90, 120, 131);
      color: #fff;
      border-radius: 10px;
      width: 100%;
      padding: 8px;
      text-align: center;
    }
    &__wrapper {
      display: flex;
      align-items: center;
      margin-bottom: 10px;
    }
    &__tag {
      @extend %tag__style;
      &:hover {
        background-color: rgb(90, 120, 131);
        color: #fff;
        border-color: rgb(90, 120, 131);
      }
    }
    &__link {
      text-decoration: none;
    }
  }
  .footer {
    display: flex;
    justify-content: center;
  }
  #scroll__trigger {
    text-align: center;
    width: 110px;
    min-width: 110px;
    font-weight: 600;
    padding: 5px;
    border-radius: 10px;
    background-color: rgb(90, 120, 131);
    color: #fff;
  }
}

@media screen and (min-width: 756px) {
  .repo__container {
    .repo__item {
      display: flex;
      height: 400px;
    }
    .repo__item__image__wrapper {
      padding: 20px;
    }
    .repo__item__image {
      margin: 0;
    }
    .repo__item__content {
      justify-content: space-around;
    }
  }
}
</style>
