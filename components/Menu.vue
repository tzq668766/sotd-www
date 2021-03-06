<template>
<div class="component-Menu" :class="'-' + color">
  <div class="nameplate">
    <nuxt-link class="logo-link -icon" :to="{ name: 'home' }" @click.native="trackMenu('logo')">
      <SvgIconLogo :fill="color" :width="45" :height="45" />
    </nuxt-link>
    <nuxt-link class="logo-link -wordmark" :to="{ name: 'home' }" @click.native="trackMenu('logo')">
      <SvgLogotype :fill="color" :width="120" :height="26" />
    </nuxt-link>
  </div>
  <ul class="nav-list" role="navigation">
    <li class="nav-item -home">
      <nuxt-link class="nav-link" :class="'-' + color" :to="{ name: 'home' }" @click.native="trackMenu('home')" exact>Home</nuxt-link>
    </li>
    <li class="nav-item">
      <nuxt-link class="nav-link" :class="'-' + color" :to="{ name: 'dapps' }" @click.native="trackMenu('dapp-list')">All ÐApps</nuxt-link>
    </li>
    <li class="nav-item">
      <nuxt-link class="nav-link" :class="'-' + color" :to="{ name: 'rankings' }" @click.native="trackMenu('rankings')">Rankings</nuxt-link>
    </li>
    <!-- <li class="nav-item">
      <nuxt-link class="nav-link" :class="'-' + color" :to="{ name: 'collections' }" @click.native="trackMenu('collections')">Collections</nuxt-link>
    </li> -->
    <li class="nav-item -stats">
      <nuxt-link class="nav-link" :class="'-' + color" :to="{ name: 'stats' }" @click.native="trackMenu('stats')" exact>Stats</nuxt-link>
    </li>
    <template v-if="this.$route.name != 'dapps' && this.$route.name != 'dapps-tab' && this.$route.name != 'dapps-tags' && this.$route.name != 'dapps-tab-tags'">
      <media :query="{maxWidth: 975}">
        <li class="nav-item">
          <nuxt-link class="nav-link -search" :class="'-' + color" :to="{ name: 'dapps' }" @click.native="trackMenu('dapps')"><SvgIconMagnifier :theme="color"/></nuxt-link>
        </li>
      </media>
    </template>
    <!-- <li class="nav-item -newsletter" :class="'-' + color" @click="scrollToMailingList('subscribe')">
      <SvgIconMail class="nav-icon -newsletter" :fill="color" :width="18" :height="18" /> 
      <span class="nav-link -newsletter" :class="'-' + color" >Stay in the loop</span>
    </li> -->
  </ul>
  <template v-if="this.$route.name != 'dapps' && this.$route.name != 'dapps-tab' && this.$route.name != 'dapps-tags' && this.$route.name != 'dapps-tab-tags'">
    <media :query="{minWidth: 975}">
      <ul class="nav-list-search" :class="search.length ? 'is-searching' : ''">
        <li class="nav-item -search">
          <GlobalSearch
            :color="color"
            />
        </li>
      </ul>
    </media>
  </template>
  <ul class="nav-list-submit">
    <li class="nav-item -submit">
      <nuxt-link @click.native="trackMenu('dapps-new')" :to="{ name: 'dapps-new' }" class="nav-link -submit" :class="$route.name === 'home' ? 'is-home' : ''">Submit a ÐApp</nuxt-link>
    </li>
  </ul>
</div>
</template>

<script>
import Media from 'vue-media'
import { mapGetters } from 'vuex'
import { trackMenu } from '~/helpers/mixpanel'
import GlobalSearch from './GlobalSearch'
import SvgIconLogo from './SvgIconLogo'
import SvgIconMail from './SvgIconMail'
import SvgIconMagnifier from './SvgIconMagnifier'
import SvgLogotype from './SvgLogotype'

export default {
  data () {
    return {
      list: [],
      sourcePath: this.$route.path
    }
  },
  components: {
    GlobalSearch,
    Media,
    SvgIconLogo,
    SvgIconMail,
    SvgIconMagnifier,
    SvgLogotype
  },
  computed: {
    ...mapGetters({
      search: 'search',
      siteSection: 'siteSection',
      statDappCount: 'statDappCount',
      myList: 'list/items'
    })
  },
  methods: {
    scrollToMailingList (targetMenuItem) {
      document.getElementById('component-SecondaryCtaMailingList')
              .scrollIntoView({ behavior: 'smooth' })
      this.trackMenu(targetMenuItem)
    },
    trackMenu (targetMenuItem) {
      const action = trackMenu(this.sourcePath, targetMenuItem)
      this.$mixpanel.track(action.name, action.data)
    }
  },
  props: {
    color: {
      default: 'black'
    }
  }
}
</script>

<style lang="scss" scoped>
@import '~assets/css/settings';

.component-Menu {
  display: flex;
  align-items: center;
  padding: 18px 10px 16px 10px;
  margin: -10px -10px 0px;
  @include tweakpoint('min-width', 640px) {
    padding-left: 22px;
    padding-right: 22px;
    margin-left: -22px;
    margin-right: -22px;
  }
  &.-white {
    background: rgba($color--black, .2)
  }
}

.list-count {
  position: absolute;
  top: -10px;
  right: -8px;
  background: rgba($color--black, .2);
  display: inline-block;
  font-weight: 600;
  padding: 2px 4px;
  border-radius: 8px;
  font-size: .7rem;
}

.logo-link {
  &.-icon {
    width: 45px;
    height: 45px;
    @include tweakpoint('min-width', 834px) {
      display: none;
    }
  }
  &.-wordmark {
    display: none;
    @include tweakpoint('min-width', 834px) {
      display: block;
      height: 26px;
    }
  }
}

.nameplate {
  display: flex;
  align-items: center;
}

.nav-icon {
  position: relative;
  top: 1px;
}

.nav-item {
  margin-left: 20px;
  display: flex;
  align-items: center;
  text-align: center;
  cursor: pointer;
  &.-newsletter, &.-submit, &.-my-list {
    display: none;
    @include tweakpoint('min-width', 700px) {
      display: flex;
    }
  }
  &.-submit {
    margin-left: auto;
  }
  &.-search {
    cursor: text;
    display: none;
    margin-right: 20px;
    @include tweakpoint('min-width', 600px) {
      display: flex;
    }   
  }
}

.nav-link {
  text-decoration: none;
  display: flex;
  align-items: center;
  border: 1px solid transparent;
  border-left: 0;
  border-right: 0;
  position: relative;
  &.nuxt-link-active, &.nuxt-link-exact-active {
    &.-white {
      border-bottom: 1px solid $color--white;
    }
    &.-black {
      border-bottom: 1px solid $color--black;
    }
    &.-search {
      border-color: transparent;
    }
  }
  &.-newsletter {
    display: none;
    @include tweakpoint('min-width', 1050px) {
      display: inline-block;
      padding-left: 10px;
    }
  }
  &.-submit {
    border: 1px solid $color--black;
    padding: 5px;
    @include tweakpoint('min-width', 840px) {
      padding: 7px 10px;
    }
    &.is-home {
      border-color: rgba($color--white, .7);
    }
  }
}

.nav-list {
  display: flex;
  align-items: center;
}

.nav-list-search {
  &.is-searching {
    flex-grow: 1;
  }
}

.nav-list-submit {
  margin-left: auto;
}

.tagline {
  display: none;
  @include tweakpoint('min-width', 1100px) {
    display: inline-block;
    padding-left: 10px;
  }
}
</style>
