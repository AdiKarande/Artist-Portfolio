<script setup>
definePageMeta({ layout: "site" });
</script>
<template>
  <section id="dark" class="cabecera text-white py-5">
    <div class="container pt-4 cabecera-adjust" style="overflow-x: hidden;">
      <div class="row align-items-center">
        <div class="col-12 col-md-6 text-start d-none d-md-block" data-aos="fade-right" data-aos-duration="1000" data-aos-easing="ease-in-sine">
          <h1 class="display-4 text-uppercase">{{ SITE.name }}</h1>
          <h4 class="my-3 font-weight-light">{{ SITE.description }}</h4>
          <h5 class="mb-0 font-weight-light"><span>Contact: </span><a class="text-white" :href="`mailto:${SITE.email}`" target="_blank">{{ SITE.email }}</a></h5>
        </div>
        <div class="col-12 col-md-12 text-center d-block d-md-none mt-4 mb-4" data-aos="zoom-in" data-aos-duration="1000" data-aos-easing="ease-in-sine">
          <h1 class="display-4 text-uppercase mb-0">{{ SITE.name }}</h1>
          <h4 class="mt-2 mb-3 font-weight-light">{{ SITE.description }}</h4>
          <small><p class="mb-0"><span>Contact: </span><a class="text-white" :href="`mailto:${SITE.email}`" target="_blank">{{ SITE.email }}</a></p></small>
        </div>
        <div class="col-12 col-md-6 row text-end px-0 align-items-center" data-aos="fade-left" data-aos-duration="1000" data-aos-easing="ease-in-sine">
          <div class="col-12 col-md-7 d-none d-md-block pe-0 text-secondary">
            <h5>Latest Release:</h5>
            <NuxtLink :to="`releases/${lastTrack.year}${releaseType(lastTrack.link)}/${lastTrack.release}`" class="text-white latest-release-title" :class="{ underline : hover }">
              <h3 class="font-weight-light">{{ lastTrack.title }}</h3>
            </NuxtLink>
            <div><h5 class="font-weight-light">by {{ lastTrack.artists }}</h5></div>
            <small><p class="mb-0 mt-1">{{ dateFormat(lastTrack.date) }}</p></small>
          </div>
          <NuxtLink :to="`releases/${lastTrack.year}${releaseType(lastTrack.link)}/${lastTrack.release}`" class="col-12 col-md-5 d-none d-md-block p-0 latest-release-cover" @mouseover="hover=true" @mouseout="hover=false">
            <div class="me-0 text-start px-2">
              <img id="covers" class="mx-auto d-flex w-100" :src="`images/releases/${lastTrack.year}/${lastTrack.cover}.jpg`" alt="" style="max-width:230px;">
            </div>
          </NuxtLink>
        </div>
      </div>
    </div>
  </section>
  <section id="latest releases" class="bg-darkest">
    <div class="container text-secondary py-5 text-center" style="overflow-x: hidden;">
      <h3 class="mt-3 text-uppercase text-white">Recent Arts</h3>
      <h5 class="font-weight-light">Check out my latest releases</h5>
      <div class="row my-4 text-start">
        <!--
        <div class="col-6 col-lg-3" data-aos="fade-in" data-aos-easing="ease-in-sine">
          <div class="item">
            <div class="cover">
              <div class="upcoming-container">
                <img class="img-fluid scale-on-hover upcoming" src="/images/releases/2023/obsession.jpg">
                <div class="centered">
                  <h4>Upcoming</h4>
                  <small><h5>September 8, 2023</h5></small>
                </div>
              </div>
              <h5 class="mb-0"><small><p class="mb-0 mt-2">Balganesh</p></small></h5>
              <small><p class="mb-4">Advait</p></small>
            </div>
          </div>
        </div>-->
        <div v-for="(tracks, index) of indexTracks" :key="index" class="col-6 col-lg-3" data-aos="fade-in" data-aos-easing="ease-in-sine">
          <div class="item">
            <div class="cover">
              <NuxtLink :to="`/releases/${tracks.year}${releaseType(tracks.link)}/${tracks.release}`">
                <div class="overflow-hidden">
                  <img id="covers" class="img-fx img-fluid release-color-covers scale-on-hover" :src="`/images/releases/${tracks.year}/${tracks.cover}.jpg`" :alt="`${ tracks.artists } - ${ tracks.title }`">
                </div>
                <h5 class="mb-0" style="font-size: 1.25rem;"><small><p class="mb-0 mt-2">{{ tracks.title }}</p></small></h5>
              </NuxtLink>
              <small><p class="mb-0">{{ tracks.artists }}</p></small>
              <small><p class="mb-4">{{ dateFormat(tracks.date) }}</p></small>
            </div>
          </div>
        </div>
      </div>
      <NuxtLink class="btn btn-outline-releases px-4 py-1" to="/releases/" data-aos="flip-left">See all releases</NuxtLink>
    </div>
  </section>
  
  <section id="about" class="bg-black text-secondary text-center py-5">
    <div class="container" style="overflow-x: hidden;">
      <h3 class="mt-3 text-uppercase text-white">About</h3>
      <div id="about-desc" class="row, text-left">
        Advait, a 10-year-old artist from Solapur, Maharashtra, began his artistic journey in 2019. He started by creating vibrant and colorful drawings, exploring different styles and techniques.
        Advait has gained recognition and signed collaborations with several art collectives and galleries, including "Artistic Expressions Collective," "Colorful Creations Gallery," "Doodlemania Studios," "Sketch Haven," "Imagination Artworks," "Creative Canvas Co-op," "Artistic Vibes Syndicate," "Whimsical Wonders Gallery," "Expressive Artisans Guild," and "Illustration Junction." Keep an eye out for Advait's upcoming series, promising even more creativity and innovation in the world of drawings!
        <div class=" text-center"><img id="covers" class="img-fluid shadow" src="/images/bayza-about.jpg" data-aos="fade-left" data-aos-easing="ease-in-sine"></div>
      </div>
    </div>
  </section>
</template>
<script>
export default {
  name: "IndexPage",
  data() {
    return {
      hover: false
    };
  },
  computed: {
    lastTrack () {
      return tracks.sort((a, b) => new Date(b.date) - new Date(a.date))[0];
    },
    indexTracks () {
      return tracks.sort((a, b) => new Date(b.date) - new Date(a.date)).slice(0, 12);
    }
  },
  created () {
    const url = SITE.url;
    const cover = `${SITE.url}/${SITE.cover}`;
    const logo = `${SITE.url}/${SITE.logo}`;
    const title = SITE.name;
    const description = SITE.meta_description;
    useHead({
      title: title,
      meta: [
        { name: "keywords", content: "bayza, dj, producer, panama, music, EDM, dance, musician, artist, ahmed" },
        { name: "description", content: description },
        // Protocolo Open Graph
        { property: "og:url", content: url },
        { property: "og:type", content: "website" },
        { property: "og:title", content: title },
        { property: "og:site_name", content: SITE.name },
        { property: "og:image", content: cover },
        { property: "og:image:width", content: "300" },
        { property: "og:image:height", content: "300" },
        { property: "og:image:alt", content: title },
        { property: "og:description", content: description },
        // Twitter Card
        { name: "twitter:card", content: "summary" },
        { name: "twitter:image", content: logo },
        { name: "twitter:title", content: title },
        { name: "twitter:description", content: description },
        { name: "twitter:site", content: `@${SITE.twitter}` }
      ],
      script: [
        { type: "application/ld+json", children: this.SEO }
      ],
      link: [
        { rel: "canonical", href: url }
      ]
    });
  },
  methods: {
    SEO() {
      const schemaOrg = {
        "@context": "http://schema.org",
        "@graph": [
          {
            "@type": "WebSite",
            name: SITE.name,
            url: SITE.url,
            image: `${SITE.url}/${SITE.logo}`
          },
          {
            "@type": "Organization",
            name: SITE.name,
            url: SITE.url,
            logo: `${SITE.url}/${SITE.logo}`,
            image: `${SITE.url}/${SITE.cover}`,
            description: SITE.meta_description
          },
          {
            "@type": ["Person", "MusicGroup"],
            "@id": SITE.socials.musicbrainz,
            name: SITE.name,
            alternateName: SITE.person.fullname,
            url: SITE.url,
            image: `${SITE.url}/${SITE.cover}`,
            description: SITE.meta_description,
            birthDate: SITE.person.birthdate,
            birthPlace:
            {
              "@type": "AdministrativeArea",
              "@id": SITE.person.province.id,
              name: SITE.person.province.name,
              containedIn:
                {
                  "@type": "Country",
                  "@id": SITE.person.country.id,
                  name: SITE.person.country.name
                }
            },
            sameAs: [
              SITE.socials.youtube,
              SITE.socials.soundcloud,
              SITE.socials.facebook,
              SITE.socials.twitter,
              SITE.socials.instagram
            ]
          }
        ]
      };
      return JSON.stringify(schemaOrg);
    }
  },
};
</script>