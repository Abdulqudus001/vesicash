<template>
  <div>
    <div class="loader">
      <img class="loader__img" src="@/assets/images/github-black.svg" alt="">
      <div class="error">
        <img class="error__img" src="@/assets/images/block.svg" alt="">
        <h1 class="error__text"></h1>
      </div>
    </div>
    <header class="nav">
      <button class="nav__menu d-block d-md-none" @click="toggleNav">
        <i class="fa fa-bars fa-2x"></i>
      </button>
      <a href="/" class="nav__logo">
        <img src="@/assets/images/github.svg" alt="">
      </a>
      <ul class="nav__items d-block d-md-flex">
        <li class="nav__item search">
          <input type="text" name="search" placeholder="Search or jump to...">
        </li>
        <li class="nav__item">
          <a href="#1">
            Pulls
            <span class="d-md-none d-lg-inline">
              requests
            </span>
          </a>
        </li>
        <li class="nav__item">
          <a href="#2">Issues</a>
        </li>
        <li class="nav__item">
          <a href="#3">Marketplace</a>
        </li>
        <li class="nav__item">
          <a href="#4">Explore</a>
        </li>
      </ul>
      <div class="nav__actions d-flex">
        <div class="badge">
          <i class="fa fa-bell-o"></i>
        </div>

        <div class="dropdown d-none d-md-flex">
          <img src="@/assets/images/plus.svg" alt="" width="14px" height="14px">
          <i class="fa fa-caret-down fa-0.5x"></i>
        </div>

        <div class="dropdown d-none d-md-flex">
          <img class="profile-img" src="https://picsum.photos/30/30/?blur" alt="" width="20px" height="20px">
          <i class="fa fa-caret-down"></i>
        </div>
      </div>
    </header>

    <main class="main">
      <div class="main__header d-none d-md-block">
        <div class="container row">
          <div class="col-col-md-3">
            <div class="avatar">
              <img :src="githubData && githubData.avatarUrl" class="avatar__image">
              <span class="avatar__name">
                Abdulqudus001
              </span>
            </div>
          </div>
          <nav class="tabs col col-md-9">
            <div class="tab__item">
              <img src="@/assets/images/overview.svg" alt="">
              Overview
            </div>
            <div class="tab__item tab__item--active">
              <img src="@/assets/images/repo.svg" alt="">
              Repositories
              <span class="repo__count">
                {{ githubData && githubData.repositories.totalCount }}  
              </span>
            </div>
            <div class="tab__item">
              <img src="@/assets/images/project.svg" alt="">
              Projects
            </div>
            <div class="tab__item">
              <img src="@/assets/images/packages.svg" alt="">
              Packages
            </div>
          </nav>
        </div>
      </div>
      <div class="container">
        <div class="row content">
          <div class="col col-12 col-md-3">
            <div class="user">
              <div class="user__profile">
                <div class="user-img">
                  <img :src="githubData && githubData.avatarUrl">
                  <div class="status" v-html="githubData && githubData.status.emojiHTML"></div>
                </div>
                <div>
                  <h2 class="user__name">
                    {{ githubData && githubData.name }}
                  </h2>
                  <p class="user__account">
                    {{ githubData && githubData.login }}
                  </p>
                </div>
              </div>
              <p class="user__bio">
                {{ githubData && githubData.bio }}
              </p>
            </div>
          </div>
          <div class="col col-12 col-md-9">
            <div class="repo">
              <div class="main__header d-block d-md-none">
                <div class="row">
                  <nav class="tabs col col-12">
                    <div class="tab__item">
                      <img src="@/assets/images/overview.svg" alt="">
                      Overview
                    </div>
                    <div class="tab__item tab__item--active">
                      <img src="@/assets/images/repo.svg" alt="">
                      Repositories
                    </div>
                    <div class="tab__item">
                      <img src="@/assets/images/project.svg" alt="">
                      Projects
                    </div>
                    <div class="tab__item">
                      <img src="@/assets/images/packages.svg" alt="">
                      Packages
                    </div>
                  </nav>
                </div>
              </div>
              <input class="repo__search" type="text" placeholder="Find a repository">
              <div class="repo__overview">
                <span id="repo-count">
                  {{ githubData && githubData.repositories.totalCount }}
                </span>
                results for
                <span>public</span>
                repositories
              </div>
              <ul class="repositories" v-if="githubData">
                <li
                  v-for="repo in githubData.repositories.edges"
                  :key="repo.node.id"
                  class="repository"
                >
                  <div>
                    <a
                      :href="`https://github.com${repo.node.resourcePath}`"
                      target="_blank"
                      rel="noopener noreferrer"
                    >
                      {{ repo.node.name }}
                    </a>
                    <p class="repository__description">
                      {{ repo.node.description }}
                    </p>
                    <div class="repository__details">
                      <div class="language">
                        <div class="language__color" :style="`background-color: ${repo.node.primaryLanguage.color}`"></div>
                        <span>
                          {{ repo.node.primaryLanguage.name }}
                        </span>
                      </div>
                      <div
                        v-if="repo.node.stargazerCount > 0"
                        class="count count--star"
                      >
                        <img src="@/assets/images/star.svg" alt="">
                        <p>
                          {{ repo.node.stargazerCount }}
                        </p>
                      </div>
                      <div
                        v-if="repo.node.forkCount > 0"
                        class="count count--fork"
                      >
                        <img src="@/assets/images/star.svg" alt="">
                        <p>
                          {{ repo.node.forkCount }}
                        </p>
                      </div>
                      <p class="updated">Updated {{ format(new Date(repo.node.pushedAt)) }}</p>
                    </div>
                  </div>
                  <button class="repository__btn">
                    <img src="@/assets/images/star.svg" alt="">
                    Star
                  </button>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  data: () => ({
    username: '',
    token: process.env.VUE_APP_GITHUB_TOKEN,
    githubData: null,
    units: [
      { name: ' second', value: 1000, max: 50, single: 'a second' },
      { name: ' minute', value: 60000, max: 50, single: 'a minute' },
      { name: ' hour', value: 3600000, max: 22, single: 'an hour' },
      { name: ' day', value: 86400000, max: 6, single: 'a day' },
      { name: ' week', value: 604800000, max: 3.5, single: 'a week' },
      { name: ' month', value: 2592000000, max: 11, single: 'a month' },
      { name: ' year', value: 31536000000, max: Infinity, single: 'a year' }
    ],
  }),
  mounted() {
    this.username = prompt('Enter github username');
    this.loadGithubData();

    let debounce;
    window.addEventListener('scroll', () => {
      clearTimeout(debounce);
      debounce = setTimeout(this.handleScroll, 10);
    });
  },
  beforeDestroy() {
    window.removeEventListener('scroll', this.handleScroll)
  },
  methods: {
    loadGithubData() {
      const body = {
        query: `
        query { 
          user(login: "${this.username}") {
            databaseId
            id 
            bio
            avatarUrl
            name
            login
            status {
              emoji
              emojiHTML
            }
            repositories(first: 20 orderBy: { field: PUSHED_AT, direction: DESC }) {
              totalCount
              edges {
                node {
                  id
                  name
                  pushedAt
                  forkCount
                  resourcePath
                  description
                  stargazerCount
                  primaryLanguage {
                    name
                    color
                  }
                }
              }
            }
          }
        }
        `,
      };
      const fetchData = {
        headers: {
          'Content-Type': 'application/json',
          Authorization: `bearer ${this.token}`,
        },
        body: JSON.stringify(body),
        method: 'POST',
      };
      fetch('https://api.github.com/graphql', fetchData)
        .then((res) => res.json())
        .then(({ data: { user } }) => {
          if (user) {
            console.log(user);
            this.githubData = user;
            const loader = document.querySelector('.loader');
            loader.style.display = 'none';
          } else {
            throw({ status: 101, message: 'User does not exist' });
          }
        })
        .catch((err) => {
          document.querySelector('.error').style.display = 'block';
          const errorMessage = document.querySelector('.error__text');
          if (err.status === 101) {
            errorMessage.textContent = err.message;
          } else {
            errorMessage.textContent = 'Something went wrong, please try again';
          }
          const loaderImg = document.querySelector('.loader__img');
          loaderImg.style.display = 'none';
        });
    },
    toggleNav() {
      const nav = document.querySelector('.nav');
      nav.classList.toggle('open');
    },
    handleScroll() {
      const image = document.querySelector('.user__profile img');
      const avatar = document.querySelector('.avatar');
      const mainHeader = document.querySelector('.main__header');
      const { bottom } = image.getBoundingClientRect();
      if (bottom < 5) {
        avatar.classList.add('show');
        mainHeader.style.zIndex = '2';
      } else {
        avatar.classList.remove('show');
        mainHeader.style.zIndex = '0';
      }
    },
    format(date) {
      let diff = Date.now() - date.getTime();

      const future = diff < 0;
      diff = Math.abs(diff);

      if (!future && diff < 10000) return 'just now';
      if (future && diff < 5000) return 'any second';

      const suffix = future ? ' from now' : ' ago';

      for (let i = 0; i < this.units.length; i++) {
        const unit = this.units[i];

        if (diff <= unit.max * unit.value) {
          const t = Math.round(diff / unit.value);
          return t === 1 ? unit.single + suffix : t + unit.name + 's' + suffix;
        }
      }
    }
  }
}
</script>

<style scoped>
@import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css');

.repo__count {
  font-size: .7rem;
  padding: 2px 8px;
  border-radius: 30px;
  display: inline-block;
  margin: 0 5px;
  background: #e1e4e8;
}
</style>

<style src="@/assets/styles/mixins.css" />
<style src="@/assets/styles/main.css" />
