<script setup>
definePageMeta({ layout: "site" });
</script>
<template>
  <section id="releases" class="bg-darkest">
    <div class="container text-secondary text-center pt-3 pb-4 px-3">
      <h3 class="mt-5 text-capitalize text-white">{{ release.title }}</h3>
      <h4 class="font-weight-light mb-0">{{ release.artists }}</h4>
      <div id="reproductor" class="me-5 ms-5 px-5" data-aos="fade-in" data-aos-easing="ease-in-sine">
        <div class="text-center py-3">
         
        </div>
      </div>
      <div id="track-info" class="row mt-4 mb-4 pt-3 bg-dark">
        <div class="col-12 col-lg-5 info text-center mb-3 px-3">
          <img class="img-fx img-fluid release-color-covers" :src="`/images/releases/${release.year}/${release.cover}.jpg`" :alt="`${release.artists} - ${release.title}`">
        </div>
        <div class="track-info-tags col-12 col-lg-7 meta text-justify px-3">
          <div class="tags">
           
            <h5 v-if="release.date" class="mb-0"><span class="meta-heading mb-0 text-white">Release date:</span></h5>
            <h6 class="mb-2"><span class="tag">{{ dateFormat(release.date) }}</span></h6>
            
            
            
            <div id="platforms" class="mt-2 mb-1 d-flex flex-wrap justify-content-left">
              <div v-for="(value, index) of obj" :key="index" class="me-2 mb-2" :title="value.store_title" data-aos="fade-up" data-aos-easing="ease-in-sine">
                <a :class="`icons-fx text-white platform-icons ${value.id}_card rounded-circle`" :href="value.stores" target="_blank">
                  <Icon v-if="value.id !== 'anghami'" class="fa-fw" :name="value.icon" />
                  <span v-else><span class="fab fa-anghami fa-fw" aria-hidden="true">
                    <img src="/images/anghami-logo.svg"></span></span><span class="visually-hidden">{{ value.store_title }}
                  </span>
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div style="border-top: 2px solid #bbbbbb; width: 30%;" />
      <div id="more_tracks" class="pt-4 text-start">
        <div id="more_releases" class="pb-4">
          <h4 class="m-0 text-white">More <a class="tag" :href="`/releases/${type(release.type).type_page}`">{{ type(release.type).release_type }}</a> by Advait</h4>
        </div>
        <div class="row">
          <div v-for="(item, index) of moreTracks" :key="index" class="col-6 col-lg-3" data-aos="fade-in" data-aos-easing="ease-in-sine">
            <div class="item">
              <div class="cover">
                <NuxtLink :to="`/releases/${item.year}${releaseType(item.link)}/${item.release}`">
                  <div class="overflow-hidden">
                    <img id="covers" class="img-fx img-fluid release-color-covers scale-on-hover" :src="`/images/releases/${item.year}/${item.cover}.jpg`" :alt="`${item.artists} - ${item.title}`">
                  </div>
                  <h5 class="mb-0" style="font-size: 1.25rem;"><small><p class="mb-0 mt-2">{{ item.title }}</p></small></h5>
                </NuxtLink>
                <small><p class="mb-0">{{ item.artists }}</p></small>
                <small><p class="mb-4">{{ dateFormat(item.date) }}</p></small>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
<script>
export default {
  name: "ReleasePage",
  data () {
    return {
      paramYear: this.$route.params.years,
      paramRelease: this.$route.params.release,
      obj: []
    };
  },
  computed: {
    release () {
      return tracks.filter((elem) => {
        return elem.release == this.paramRelease;
      })[0];
    },
    moreTracks () {
      return this.isBootleg().slice(0, 8);
    }
  },
  async mounted () {
    this.obj = await this.platforms();
  },
  created () {
    let description;
    const url = `${SITE.url}/releases/${this.paramYear}/${this.paramRelease}`;
    const imageUrl = `${SITE.url}/images/releases/${this.release.year}/${this.release.cover}.jpg`;
    const title = `${this.release.artists} - ${this.release.title}`;
    this.release.description != null ? description = this.release.description : description = this.release.title;
    useHead({
      title: `${this.release.title} | ${SITE.name}`,
      meta: [
        { name: "keywords", content: `release, ${this.release.title}, ${this.release.genre}, ${this.release.year}, play, stream, download, fanlink, music, EDM` },
        { name: "description", content: description },
        // Protocolo Open Graph
        { property: "og:url", content: url },
        { property: "og:type", content: "website" },
        { property: "og:title", content: title },
        { property: "og:site_name", content: SITE.name },
        { property: "og:image", content: imageUrl },
        { property: "og:image:width", content: "500" },
        { property: "og:image:height", content: "500" },
        { property: "og:image:alt", content: title },
        { property: "og:description", content: description },
        // Twitter Card
        { name: "twitter:card", content: "summary" },
        { name: "twitter:image", content: imageUrl },
        { name: "twitter:title", content: title },
        { name: "twitter:description", content: description },
        { name: "twitter:site", content: `@${SITE.twitter}` }
      ],
      script: [
        // Schema.org
        { type: "application/ld+json", children: this.SEO(url, imageUrl) }
      ],
      link: [
        { rel: "canonical", href: url }
      ]
    });
  },
  methods: {
    SEO (url, imageUrl) {
      const schemaOrg = {
        "@context": "http://schema.org",
        "@type": "MusicRecording",
        name: this.release.title,
        url: url,
        image: imageUrl,
        genre: this.release.genre,
        duration: this.release.duration,
        datePublished: new Date(this.release.date),
        byArtist: [{
          "@type": "MusicGroup",
          "name": this.release.artists
        }]
      };
      return JSON.stringify(schemaOrg);
    },
    type (type) {
      if (type == "Releases") {
        return {release_type: "releases", type_page: ""};
      } else {
        return {release_type: "bootlegs", type_page: "all-bootlegs"};
      }
    },
    isBootleg () {
      const type = "-bootlegs";
      if (!this.paramYear.includes(type)) {
        return tracks.filter((elem) => {
          return elem.link !== type && elem.release !== this.paramRelease;
        }).sort((a, b) => new Date(b.date) - new Date(a.date));
      } else {
        return tracks.filter((elem) => {
          return elem.link === type && elem.release !== this.paramRelease;
        }).sort((a, b) => new Date(b.date) - new Date(a.date));
      }
    },
   
  }
};
</script>