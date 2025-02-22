<template>
  <v-app>
    <v-navigation-drawer
      v-if="$vuetify.breakpoint.smAndDown"
      v-model="drawerOpen"
      color="accent"
      temporary
      fixed
      app
    >
      <v-list nav>
        <div v-for="(group, groupTag) in links" :key="groupTag">
          <v-list-item
            v-if="group.sections == undefined"
            :to="localePath(group.link)"
            nuxt
          >
            <v-list-item-icon>
              <v-icon>{{ group.icon }}</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title v-text="$t('banner.' + groupTag)" />
            </v-list-item-content>
          </v-list-item>
          <v-list-group v-else color="grey darken-4" :prepend-icon="group.icon">
            <template #activator>
              <v-list-item-content>
                <v-list-item-title v-text="$t('banner.' + groupTag)" />
              </v-list-item-content>
            </template>
            <div v-for="(section, textTag) in group.sections" :key="textTag">
              <v-list-item
                v-if="section.type === 'external'"
                :href="section.link"
                target="_blank"
              >
                <v-list-item-content>
                  <v-list-item-title
                    class="ml-2 grey--text text--darken-4"
                    v-text="$t('banner.' + textTag)"
                  />
                </v-list-item-content>
                <v-list-item-icon>
                  <v-icon>mdi-open-in-new</v-icon>
                </v-list-item-icon>
              </v-list-item>
              <v-list-item v-else :to="localePath(section.link)" nuxt>
                <v-list-item-content>
                  <v-list-item-title
                    class="ml-2 grey--text text--darken-4"
                    v-text="$t('banner.' + textTag)"
                  />
                </v-list-item-content>
              </v-list-item>
            </div>
          </v-list-group>
        </div>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar color="primary" app hide-on-scroll>
      <h5
        v-if="$vuetify.breakpoint.mdAndUp"
        class="text-md-h6 ml-5 font-weight-regular"
      >
        <nuxt-link
          class="no-underline underline-on-hover white--text"
          :to="localePath('/')"
        >
          {{ $t('common.woudcFull') }}
        </nuxt-link>
      </h5>
      <template v-if="$vuetify.breakpoint.mdAndUp" #extension>
        <v-menu
          v-for="(group, groupTag) in links"
          :key="groupTag"
          offset-y
          open-on-hover
          open-on-focus
          transition="slide-y-transition"
          close-delay="100"
          open-delay="200"
        >
          <template #activator="{ on, attrs }">
            <v-tabs color="accent" fixed-tabs>
              <v-tab v-if="group.sections" v-bind="attrs" v-on="on">
                <v-icon>{{ group.icon }}</v-icon>
                <span class="pl-1">{{ $t('banner.' + groupTag) }}</span>
                <v-icon>mdi-chevron-down</v-icon>
              </v-tab>
              <v-tab
                v-else
                :to="localePath(group.link)"
                class="accent--text"
                nuxt
              >
                <v-icon color="accent">
                  {{ group.icon }}
                </v-icon>
                <span class="pl-1">{{ $t('banner.' + groupTag) }}</span>
              </v-tab>
            </v-tabs>
          </template>
          <v-list v-if="group.sections" color="accent" nav>
            <div v-for="(section, textTag) in group.sections" :key="textTag">
              <v-list-item
                v-if="section.type === 'external'"
                :href="section.link"
                target="_blank"
              >
                <v-list-item-content>
                  <v-list-item-title
                    class="grey--text text--darken-4"
                    v-text="$t('banner.' + textTag)"
                  />
                </v-list-item-content>
                <v-list-item-icon>
                  <v-icon>mdi-open-in-new</v-icon>
                </v-list-item-icon>
              </v-list-item>
              <v-list-item v-else :to="localePath(section.link)" nuxt>
                <v-list-item-content>
                  <v-list-item-title
                    class="grey--text text--darken-4"
                    v-text="$t('banner.' + textTag)"
                  />
                </v-list-item-content>
              </v-list-item>
            </div>
          </v-list>
        </v-menu>
      </template>
      <v-app-bar-nav-icon
        v-if="$vuetify.breakpoint.smAndDown"
        color="white"
        @click.stop="drawerOpen = !drawerOpen"
      />
      <h5
        v-if="$vuetify.breakpoint.smAndDown"
        class="text-md-h6 ml-5 font-weight-regular"
      >
        <nuxt-link
          class="no-underline underline-on-hover white--text"
          :to="localePath('/')"
        >
          {{ $t('common.woudcFull') }}
        </nuxt-link>
      </h5>
      <v-spacer />
      <nuxt-link
        v-if="$i18n.locale === 'fr'"
        class="white--text"
        :to="switchLocalePath('en')"
      >
        English
      </nuxt-link>
      <nuxt-link
        v-if="$i18n.locale === 'en'"
        class="white--text"
        :to="switchLocalePath('fr')"
      >
        Français
      </nuxt-link>
    </v-app-bar>
    <v-main>
      <nuxt />
    </v-main>
    <v-footer color="primary" class="white--text" app absolute>
      <span>
        &copy; {{ $config.appBuildYYYY }}
        <a
          :href="$config.appHomepage"
          target="_blank"
          rel="noreferrer"
          class="accent--text"
        >
          <span>{{ $config.appName }}</span>
        </a>
        {{ $config.appVersion }}
      </span>
      <v-spacer />
      <nuxt-link
        class="white--text mr-10 no-underline"
        :to="localePath('contact')"
      >
        <v-icon class="mr-1 pb-1" color="white">mdi-email</v-icon>
        <span>{{ $t('banner.contact') }}</span>
      </nuxt-link>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      drawerOpen: false,
      links: {
        data: {
          icon: 'mdi-database',
          link: 'data',
          sections: {
            'data-search': { link: 'data-search' },
            'data-access': { link: 'data-data_access' },
            'data-quality': { link: 'data-data_quality' },
            'data-policy': { link: 'data-data_policy' },
            'data-products': { link: 'data-products' },
            'data-info': { link: 'data-dataset_info' },
            'data-stations': { link: 'data-stations' },
            'data-instruments': { link: 'data-instruments' },
          },
        },
        contributors: {
          icon: 'mdi-account-group',
          link: 'contributors',
          sections: {
            'contributors-guide': {
              type: 'external',
              link: 'https://guide.woudc.org',
            },
            'contributors-registration': { link: 'contributors-registration' },
            'contributors-list': { link: 'contributors' },
            'contributors-submission': { link: 'contributors-submission' },
            'contributors-validation': { link: 'contributors-validation' },
          },
        },
        resources: {
          icon: 'mdi-book-open-variant',
          link: 'resources',
          sections: {
            'resources-sop': { link: 'resources-sop' },
            'resources-groups': { link: 'resources-working_groups' },
            'resources-links': { link: 'resources-links' },
            'resources-software': {
              type: 'external',
              link: 'https://github.com/woudc/woudc/wiki',
            },
          },
        },
        news: {
          icon: 'mdi-newspaper',
          link: 'news',
        },
        about: {
          icon: 'mdi-information',
          link: 'about',
          sections: {
            'about-home': { link: 'about' },
            'about-standards': { link: 'about-standards' },
            'about-formats': { link: 'about-formats' },
            'about-glossary': { link: 'about-glossary' },
            'about-faq': { link: 'about-faq' },
          },
        },
      },
    }
  },
}
</script>

<style>
.underline-on-hover:hover {
  text-decoration: underline;
}
.no-underline {
  text-decoration: none;
}
</style>
