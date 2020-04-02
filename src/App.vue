<template>
  <div id="app mdui-container">

    <!-- Header -->
    <div class="header mdui-color-blue mdui-col-xs-12 mdui-toolbar">
      <div>homepage</div>
      <div class="mdui-toolbar-spacer"></div>
    </div>

    <!-- Search bar -->
    <div
      class="searchbar mdui-toolbar mdui-color-blue mdui-col-xs-12"
      v-bind:class="{searchbar_active: isSearch}"
    >
      <!-- <input
          v-on:click="toSearch"
          v-bind:class="{searchbar_input_active: isSearch}"
          class="mdui-textfield-input searchbar_input"
          type="text"
          placeholder="搜索"
      /> -->

      <!-- Search engine icon & list -->
      <div
        class="search_switch"
        v-bind:class="{search_switch_active: isSearch}"
        mdui-menu="{target: '#search_engine'}"
      >
        <img src="./assets/img/search/Google-icon.svg" v-bind:class="{iconHidden: this.searchEngine!='google'}"
             width="20px" height="36px"/>
        <img src="./assets/img/search/Baidu-icon.svg" v-bind:class="{iconHidden: this.searchEngine!='baidu'}"
             width="20px" height="36px"/>
        <img src="./assets/img/search/Bing-icon.svg" v-bind:class="{iconHidden: this.searchEngine!='bing'}" width="20px"
             height="36px"/>
      </div>
      <ul class="mdui-menu" id="search_engine">
        <li class="mdui-menu-item" v-on:click="useGoogle">
          <a href="javascript:;" class="mdui-ripple">Google</a>
        </li>
        <li class="mdui-menu-item" v-on:click="useBing">
          <a href="javascript:;" class="mdui-ripple">Bing</a>
        </li>
        <li class="mdui-menu-item" v-on:click="useBaidu">
          <a href="javascript:;" class="mdui-ripple">Baidu</a>
        </li>
      </ul>

      <!-- Google -->
      <form
        action="https://www.google.com/search"
        v-bind:class="{searchbar_form_active: isSearch}"
        v-if="searchEngine == 'google'"
        class="searchbar_form"
      >
        <input
          v-on:click="toSearch"
          v-bind:class="{searchbar_input_active: isSearch}"
          class="mdui-textfield-input searchbar_input"
          type="text"
          name="q"
          autocomplete="off"
          placeholder="Search"
        >
      </form>

      <!-- Bing -->
      <form
        action="https://www.bing.com/search"
        v-bind:class="{searchbar_form_active: isSearch}"
        v-if="searchEngine == 'bing'"
        class="searchbar_form"
      >
        <input
          v-on:click="toSearch"
          v-bind:class="{searchbar_input_active: isSearch}"
          class="mdui-textfield-input searchbar_input"
          type="text"
          name="q"
          autocomplete="off"
          placeholder="Search"
        >
      </form>

      <!-- Baidu -->
      <form
        action="https://www.baidu.com/s"
        v-bind:class="{searchbar_form_active: isSearch}"
        v-if="searchEngine == 'baidu'"
        class="searchbar_form"
      >
        <input
          v-on:click="toSearch"
          v-bind:class="{searchbar_input_active: isSearch}"
          class="mdui-textfield-input searchbar_input"
          type="text"
          name="wd"
          autocomplete="off"
          placeholder="Search"
        >
      </form>

      <!-- Search button -->
      <div class="search-button">
        <img src="./assets/img/search/search.svg" width="36px" height="36px"/>
      </div>
    </div>

    <!-- Overlay -->
    <div class="overlay" v-on:click="closeSearch" v-bind:class="{overlay_active: isSearch}"></div>

    <div class="card-wrap">
      <!-- MDUI card -->
      <div
        class="mdui-card mdui-col-xs-10 mdui-col-offset-xs-1 mdui-shadow-0 sites-card ">
        <!-- Card title -->
        <div class="mdui-card-header card-header">
          网站
          <div v-on:click="editSites" class="edit">{{editOrComplete}}</div>
        </div>

        <div class="mdui-divider"></div>

        <!-- Sites list -->
        <ul class="mdui-list">
          <li v-for="(site, index) in sites" :key="index" class="site">
            <button
              class="remove"
              v-bind:class="{remove_active: canRemove}"
              v-on:click="removeSite(index)"
            >
              <i class="mdui-icon material-icons">close</i>
            </button>
            <a v-bind:href="site.url" target="_blank">
              <div>
                <img v-bind:src="site.icon" width="55px" height="55px" v-bind:class="{remove_active: canRemove}"/>
              </div>
              <div class="site-title">{{ site.title}}</div>
            </a>
          </li>
          <li class="site" v-on:click="showAddpanel">
            <i class="mdui-icon material-icons add-site-icon" v-bind:class="{iconHidden: hasAddPanel}">add</i>
            <i class="mdui-icon material-icons add-site-icon" v-bind:class="{iconHidden: !hasAddPanel}">close</i>
            <div class="site-title">添加</div>
          </li>
        </ul>

        <!-- New site panel -->
        <div
          class="new_site mdui-color-blue-grey-50"
          v-bind:class="{new_site_active: hasAddPanel}"
        >
          <div class="new-site-info mdui-textfield mdui-textfield-floating-label">
            <label class="mdui-textfield-label">网站地址</label>
            <input class="mdui-textfield-input" type="url" v-model="newSiteurl"/>
          </div>
          <div class="new-site-info mdui-textfield mdui-textfield-floating-label">
            <label class="mdui-textfield-label">网站名称</label>
            <input class="mdui-textfield-input" type="text" v-model="newSitetitle"/>
          </div>
          <div class="new-site-info mdui-textfield mdui-textfield-floating-label">
            <label class="mdui-textfield-label">网站图标地址</label>
            <input class="mdui-textfield-input" type="text" v-model="newSiteicon"/>
          </div>
          <button class="addtodo_button mdui-btn mdui-btn-raised mdui-color-theme-accent mdui-ripple"
                  v-on:click="addSite">添加
          </button>
        </div>

      </div>

    </div>

    <!-- Footer -->
    <div class="footer mdui-card mdui-col-xs-12">
      <ul class="social-buttons">
        <li><a href="https://github.com/ghostsf/homepage" target="_blank"><img src="./assets/img/footer/github.svg" alt="" width="24px" height="24px;"></a></li>
        <li><a href="https://ghostsf.com/" target="_blank"><img src="./assets/img/footer/about.svg" alt="" width="24px" height="24px;"></a></li>
      </ul>

      <p class="copyright">
        © 2020 <a href="https://ghostsf.com" target="_blank">ghostsf</a>
      </p>
    </div>

  </div>
</template>

<script>
  import Store from './store'

  // init first visit
  if (!Store.getCookie('visited')) {
    if (Store.fetch_site().length == 0) {
      Store.save_site([
        {
          "url": "https://github.com/",
          "title": "Github",
          "icon": require('./assets/img/icons/github.svg')
        }, {
          "url": "https://store.steampowered.com",
          "title": "Steam",
          "icon": require('./assets/img/icons/steam.svg')
        }, {
          "url": "https://www.wikipedia.org/",
          "title": "Wikipedia",
          "icon": require('./assets/img/icons/wikipedia.svg')
        }, {
          "url": "https://www.reddit.com/",
          "title": "Reddit",
          "icon": require('./assets/img/icons/reddit.svg')
        }, {
          "url": "https://www.flickr.com/",
          "title": "Flickr",
          "icon": require('./assets/img/icons/flickr.svg')
        }, {
          "url": "https://500px.com/",
          "title": "500px",
          "icon": require('./assets/img/icons/500.svg')
        }, {
          "url": "http://digg.com/",
          "title": "Digg",
          "icon": require('./assets/img/icons/digg.svg')
        }, {
          "url": "https://tumblr.com/",
          "title": "Tumblr",
          "icon": require('./assets/img/icons/tumblr.svg')
        }, {
          "url": "https://facebook.com/",
          "title": "Facebook",
          "icon": require('./assets/img/icons/facebook.svg')
        }, {
          "url": "https://twitter.com/",
          "title": "Twitter",
          "icon": require('./assets/img/icons/twitter.svg')
        }, {
          "url": "https://zhihu.com/",
          "title": "知乎",
          "icon": require('./assets/img/icons/zhihu.svg')
        }, {
          "url": "https://www.dropbox.com/",
          "title": "Dropbox",
          "icon": require('./assets/img/icons/dropbox.svg')
        }, {
          "url": "https://www.behance.net/",
          "title": "Behance",
          "icon": require('./assets/img/icons/behance.svg')
        }, {
          "url": "https://plus.google.com/",
          "title": "Google+",
          "icon": require('./assets/img/icons/google_plus.svg')
        }, {
          "url": "https://quora.com/",
          "title": "Quora",
          "icon": require('./assets/img/icons/quora.svg')
        }, {
          "url": "https://www.pinterest.com/",
          "title": "Pinterest",
          "icon": require('./assets/img/icons/pinterest.svg')
        }, {
          "url": "https://getpocket.com/",
          "title": "Pocket",
          "icon": require('./assets/img/icons/pocket.svg')
        }, {
          "url": "https://dribbble.com/",
          "title": "Dribbble",
          "icon": require('./assets/img/icons/dribbble.svg')
        }, {
          "url": "http://stackoverflow.com/",
          "title": "Stack Overflow",
          "icon": require('./assets/img/icons/stackoverflow.svg')
        }, {
          "url": "https://www.amazon.com/",
          "title": "Amazon",
          "icon": require('./assets/img/icons/amazon.svg')
        }, {
          "url": "https://taobao.com/",
          "title": "淘宝",
          "icon": require('./assets/img/icons/taobao.svg')
        }, {
          "url": "http://weibo.com/",
          "title": "微博",
          "icon": require('./assets/img/icons/weibo.svg')
        }, {
          "url": "http://coolapk.com/",
          "title": "酷安",
          "icon": require('./assets/img/icons/coolapk.svg')
        }, {
          "url": "http://douban.com/",
          "title": "豆瓣",
          "icon": require('./assets/img/icons/douban.svg')
        }
      ])
    }

    Store.setCookie("se", "google", 365);
    Store.setCookie("visited", "true", 365);
    if (!Store.getCookie("cityid")) {
      Store.setCookie("cityid", 101010100, 365);
    }
  }

  //  console.log(Store.fetch_site().length)

  export default {
    data() {
      return {
        editOrComplete: '编辑',
        searchEngine: Store.getCookie('se'),
        newSiteurl: '',
        newSitetitle: '',
        newSiteicon: '',
        canRemove: false,
        hasAddPanel: false,
        isSearch: false,
        sites: Store.fetch_site(),
      }
    },
    watch: {
      sites: {
        handler: function (sites) {
          Store.save_site(sites)
        },
        deep: true
      }
    },
    methods: {
      showAddpanel: function () {
        this.hasAddPanel = !this.hasAddPanel;
      },
      addSite: function () {
        this.sites.push({
          url: this.newSiteurl,
          title: this.newSitetitle,
          icon: (this.newSiteicon ? this.newSiteicon : require('./assets/img/icons/default.svg'))
        })
        this.newSiteurl = ''
        this.newSitetitle = ''
        this.newSiteicon = ''
      },
      removeSite: function (index) {
        this.sites.splice(index, 1);
      },
      editSites: function () {
        this.canRemove = !this.canRemove;
        if (this.canRemove == true) this.editOrComplete = '完成';
        if (this.canRemove == false) this.editOrComplete = '编辑';
      },
      toSearch: function () {
        if (this.isSearch == false) {
          this.isSearch = true
        }
      },
      closeSearch: function () {
        if (this.isSearch == true) {
          this.isSearch = false
        }
      },
      useGoogle: function () {
        this.searchEngine = 'google';
        Store.setCookie("se", "google", 30);
      },
      useBing: function () {
        this.searchEngine = 'bing';
        Store.setCookie("se", "bing", 30);
      },
      useBaidu: function () {
        this.searchEngine = 'baidu';
        Store.setCookie("se", "baidu", 30);
      }
    }
  }

</script>

<style>
  #app {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
