---
layout: default
title: Favicon
parent: Styling Modern Web Apps
nav_order: 1
---
# Update the Favicon

A Favicon is an icon that appear on the tab for a page next to the page’s title. Favicons help boost visibility in the user's browser and help strengthen the website's brand identity. Changing the default favicon of a Next.js application is one of the first things you can do when branding your website.

Download the McMaster favicon from the [McMaster University Brand Standards Website Favicon Folder](https://brand-resources.mcmaster.ca/asset-bank/action/browseItems?categoryId=1516&categoryTypeId=2&cachedCriteria=1).

The favicon is set in the `app/layout.tsx` file by adding an `icons` attribute as shown in the following code snippet.

```ts
export const metadata: Metadata = {
    title: 'McMaste Branding',
    description: 'Generated by create next app',
    viewport: 'width=device-width, initial-scale=1',
    icons: {
        icon: '/favicon.ico',
    },
}
```

To change the favicon of your SPA, delete the `public/favicon.ico` file and replace it with the McMaster `favicon.ico` file. Make the sure the name of the downloaded favicon is `favicon.ico`.

Default Favicon          |  McMaster Favicon
:-------------------------:|:-------------------------:
![old-favicon](assets/img/old-favicon.png)  |  ![new-favicon](assets/img/new-favicon.png)

Note that your browser may have already cached the favicon, so may need to clear the browser data to force the browser to load the new icon.
