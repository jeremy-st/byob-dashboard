ℹ️ Disclaimer: This project is community-supported and is maintained by members of the Okta team for developers and other IT professionals. BYOB-dashboard is not an official Okta product and does not qualify for any Okta support. Okta makes no warranties regarding this project. Anyone who chooses to use this project must ensure that their implementation meets any applicable legal obligations including any Okta terms and conditions.

ℹ️ It is recommended that you collaborate with your preferred Okta Solution Provider [(link)](https://www.okta.com/partners/meet-our-partners/?field_partner_type_tid=8101&field_solutions_target_id=6061) to implement and adapt this app code sample within your existing portal. This app features frontend and backend components and like any web app hosted and running on your side, you should perform a code review, as well as security and scalability tests. 


# BYOB Dashboard
**B**ring **Y**our **O**wn **B**rand...to the Okta Chicklet Page

## Introduction
![alt text](images/byob-demo.gif)

If you've ever considered building your own User HomePage (aka the "Chicklet Page") – in order to have 100% control of the branding – and want some sample code to get started, you've found the right repo!

This project is built in Vue.js and uses
* [Vuetify 2.x](https://vuetifyjs.com/en/) Material Design Component Framework
* [Vuedraggable](https://github.com/SortableJS/Vue.Draggable) Vue drag-and-drop component based on Sortable.js
* [Okta Vue.js SDK](https://github.com/okta/okta-oidc-js/tree/master/packages/okta-vue) 
* [Okta Sign-in Widget 3.x](https://github.com/okta/okta-signin-widget)


## Okta Org Setup
This application is represented by an OpenID Connect application in Okta, so we need to configure one:
1. In your **Developer Console**, navigate to the **Applications** menu, click **Add Application** and select **Single-Page App**
2. Click **Next**, then enter an Application **Name**. Then:
   * Set Base URIs to `http://localhost:8080/` (it should already be set by default)
   * Set `http://localhost:8080/oauth/callback` as the *Redirect URI* 
   * Leave the default setting, Group assignments = **Everyone**
   * Select **Authorization Code** and deselect the default **Implicit** checkbox
3. Click **Done** to redirect back to the *General* tab of your application.
4. Make note of the **Client ID**, as it will be needed environment configuration. 
5. Make sure that **Use PKCE (for public clients)** (underneath the Client Id) is selected
6. Navigate to **Api** > **Trusted Origins** and add `http://localhost:8080` as a type = **CORS** entry.

## Local Installation
1. Clone this repository and `cd` into the main directory
2. Run `npm install`
3. Run `npm install @vue/cli -g`
4. Create env file `.env.development.local` in the root directory (There is an existing `.env` file. Do not touch that file, add this new file in addition to it). Edit it in with the values below:
```
VUE_APP_CLIENT_ID={{Your Client ID from the "Okta Org Setup" setup}}
VUE_APP_ISSUER=https://{{Your Okta Org Url}}/oauth2/default
VUE_APP_API=https://oted1jcxcb.execute-api.us-east-2.amazonaws.com/stage/dashboard
```
5. The following command compiles and hot-reloads for development environment
`npm run serve`
6. Open your browser to `http://localhost:8080` and login

## Compile and minify for production
```
npm run build
```

# Alternative Login Flow
To see an example of logging in via redirect to the Okta hosted Signin page *(instead of the Signin Widget)*, add `loginRedirect` to the `.config.js` file.
The resulting file should look like this:
```
export default {
    oidc: {
        client_id: '{{Your Client ID from the "Okta Org Setup" setup}}',
        issuer: 'https://{{Your Okta Org Url}}/oauth2/default',
        redirect_uri: '/oauth/callback',
        scope: 'openid profile email',
    },
    loginRedirect: true
}
```

# Overriding the Icons uploaded into Okta
Sometimes it is useful to override the App icons that were uploaded into Okta with higher resolution images. To do this, edit the `/src/assets/icons.js` file with a list of objects containing the appId and the associated logo URL. 


# SPA APIs
This sample provides sample implementation user management (profile/password update) using Lambda and Amazon API Gateway. Navigate to the  [api folder](/api) to see more.
