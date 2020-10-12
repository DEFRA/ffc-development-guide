# Google Tag Manager
> Google Tag Manager integration into a future ffc service requires contacting the <**TBD**> team. They will create the necessary Google Tag Manager containers needed to be setup for the new ffc service for Non-Production and Production. The developer will need to create a google account using their DEFRA email address. The developer can create a Google account without using Gmail here - https://accounts.google.com/SignUpWithoutGmail.

> Once the <**TBD**> team have setup up the relevant containers, they will give the developer a GTM code, e.g. GTM-M5YK7JL. They will provide a code for Non-Production and Production environments (containers). They may give the full javascipt code snippets similar to the following:

-     <!-- Google Tag Manager -->
      <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
      new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
      j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
      'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
      })(window,document,'script','dataLayer','GTM-M5YK7JL');</script>
      <!-- End Google Tag Manager -->

-     <!-- Google Tag Manager (noscript) -->
      <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-M5YK7JL" 
      height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
      <!-- End Google Tag Manager (noscript) -->

> The developer only needs to take to GTM code provided by the <**TBD**> team and replace in the following files for the new ffc service:
- helm/<**helm chart name**>/values.yaml (see example for [ffc-demo-web](https://github.com/DEFRA/ffc-demo-web/blob/master/helm/ffc-demo-web/values.yaml))
   * in section - **container:**
      * variable - **googleTagManagerKey:**
- docker-compose.yaml (see example for [ffc-demo-web](https://github.com/DEFRA/ffc-demo-web/blob/master/docker-compose.yaml))
   * in section **services:** -> <**service name**>: -> **environment:**
   * variable - **GOOGLE_TAG_MANAGER_KEY**

> If javascript code snippets are provided by the <**TBD**> team. Then extract the GTM code from them and replace in the above files.

> Once the GTM code has been replaced the developer can load and start interaction with the website. Then the developer needs to login Google Tag Manager with their DEFRA email (created in previous steps). The interactions can then be viewed in Google Analytics and Data Studio to verify everything is working correctly.

See [DEFRA documentation](https://github.com/DEFRA/analytics-standards) for more information on Digital Analytics Standards.

