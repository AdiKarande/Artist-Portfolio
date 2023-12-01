<template>
 
  <footer id="contact" class="page-footer bg-darkest text-white text-center font-weight-light">

    <div class="container py-4">
      <div class="row">
        
        
        <div>
          <h4>Contact</h4>
          <ul class="icons mb-0">
           
            <li class="soundcloud">
              <a :href="SITE.platforms.soundcloud" target="_blank"><Icon class="" name="simple-icons:soundcloud" /></a>
            </li>
            <li class="youtube">
              <a :href="SITE.platforms.youtube" target="_blank"><Icon class="" name="simple-icons:youtube" /></a>
            </li>
            <li class="instagram">
              <a :href="SITE.socials.instagram" target="_blank"><Icon class="" name="simple-icons:instagram" /></a>
            </li>
            <li class="facebook">
              <a :href="SITE.socials.facebook" target="_blank"><Icon class="" name="simple-icons:facebook" /></a>
            </li>
            <li class="twitter">
              <a :href="SITE.socials.twitter" target="_blank"><Icon class="" name="simple-icons:twitter" /></a>
            </li>
          </ul>
          <ul class="contact">
            <li>
              <span class="ml-2">Email: </span><a class="mx-0" href="mailto:connect@auroradigisol.com" target="_blank">connect@auroradigisol.com</a>
            </li>
          </ul>
        </div>
        <div class="col-md-12 text-center mt-4 text-small">
          <p class="mb-0">© 2023 auroa digital solutions pvt. ltd. All rights reserved.</p>
        </div>
      </div>
    </div>
  </footer>
</template>
<script>
export default {
  name: "FooterComponent"
};
</script>