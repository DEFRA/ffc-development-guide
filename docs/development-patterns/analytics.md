# Analytics

[Google Tag Manager](https://developers.google.com/tag-platform/tag-manager) (GTM) is a tag management system that allows you to quickly and easily update tags and code snippets on your website or mobile app, such as those intended for traffic analysis and marketing optimisation. Once the GTM code is added to your site, you can configure tags via the GTM web interface without having to alter your website code.

## Setup Google Tag Manager

A GTM container needed to be setup for a new service for both Non-Production and Production. 

> Note that at time of writing ownership of GTM within Defra is still to be determined. Please contact a Principal Developer for support.

Those requiring access will need create a google account using their Defra email address. Accounts can be created [without using Gmail here](https://accounts.google.com/SignUpWithoutGmail).

Once a container has been created, a GTM codes will be provided.  They may give the full JavaScript code snippets similar to the following:

```html
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
  })(window,document,'script','dataLayer','GTM-M5YK7JL');</script>
<!-- End Google Tag Manager -->

<!-- Google Tag Manager (noscript) -->
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-M5YK7JL" 
height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
<!-- End Google Tag Manager (noscript) -->
```

Once the GTM has been provided, the [GTM guidance](https://support.google.com/tagmanager/answer/6103696) explains how to add the GTM code to your site.
