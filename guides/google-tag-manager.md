# Google Tag Manager
> Google Tag Manager integration into a future ffc service requires contacting the **TBD** team. They will create the necessary Google Tag Manager containers needed to be setup for the new ffc service for Non-Production and Production. The developer will need to create a google account using their DEFRA email address. The developer can create a Google account without using Gmail here - https://accounts.google.com/SignUpWithoutGmail.

> Once the **TBD** team have setup up the relevant containers, they will give the developer a GTM code, e.g. GTM-M5YK7JL. They will provide a code for Non-Production and Production environments (containers). They may give the full javascipt code snippets simlair to the following:

-     <!-- Google Tag Manager -->
      <script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
      new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
      j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
      'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
      })(window,document,'script','dataLayer','<INSERT GTM CODE HERE>');</script>
      <!-- End Google Tag Manager -->

-     <!-- Google Tag Manager (noscript) -->
      <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=<INSERT GTM CODE HERE>" 
      height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
      <!-- End Google Tag Manager (noscript) -->

> The developer only needs to replace the GTM code which the **TBD** team provide in the following files for the new ffc service:
- app/views/tagManager/bodyContent.njk
- app/views/tagManager/headContent.njk
- docker-compose.yaml
- helm/ffc-demo-web/values.yaml

> Once the GTM code has been replaced the developer can load and start interaction with the website. Then the developer needs to login Google Tag Manager with their DEFRA email (created in previous steps). The interactions can then be viewed in Google Analytics and Data Studio to verify everything is working correctly.

See [DEFRA documentation](https://github.com/DEFRA/analytics-standards) for more information on Digital Analytics Standards.

