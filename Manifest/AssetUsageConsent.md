# `Asset Usage Consent` Explainer
## Author
* [Diego González-Zúñiga](https://twitter.com/diekus) ([Samsung](https://samsunginter.net))

## Participate
* [Explainer on GitHub](https://github.com/SamsungInternet/Explainers/blob/master/Manifest/AssetUsageConsent.md)

## Introduction
The web app manifest file provides information about a web application in a JSON text file. It contains keys that can be used to describe a web app in order to present it in a similar way to a native app. One of the uses that might come out of this is to create components, storefronts or ways to display this information through the browser or through an app store. The `Asset Usage Consent` is meant to be a new key in the manifest file, that stores a boolean or limited set of values that express the consent of use for brand image and copyright of assets linked in the file, like icons and screenshots.

## Motivating Use Cases
Having an `Asset Usage Content` key in the manifest would allow the developers of a web application to explicitly give consent to the usage of the information present in this file to be used for promotion of the application without the need of going through the creation of a license agreement. 

Currently, some processes that aim to improve discoverability of PWAs require the creation of a license agreement to deal with copyright and brand image of the assets that are referenced in the manifest file. The developer might consent to the usage of the resources in the manifest file, speeding up and easing processes like ingestion into application stores, web components that create install experiences or other similar mechanisms meant to aid discoverability of web applications.

To better understand the concept of the `Asset Usage Consent`, it is not about a license of the application, but a license of the use of the images and resources linked in the web manifest file.

## Non-goals
* This key is not meant to replace any additional agreements or legal consent beyond usage of the assets linked in the manifest file.
* Not meant to describe metadata regarding the software licenses of the described application (as SPDX tries to do).

## Asset Usage Consent key
The `Asset Usage Consent` would be implemented as a key in the manifest file. 
```json
{
    "name": "HackerWeb",
    "short_name": "HackerWeb",
    "start_url": ".",
    "display": "standalone",
    "background_color": "#fff",
    "description": "A simply readable Hacker News app.",
    "icons": [{
        "src": "images/touch/homescreen192.png",
        "sizes": "192x192",
        "type": "image/png"
    }],
    "related_applications": [{
        "platform": "play",
        "url": "https://play.google.com/store/apps/details?id=cheeaun.hackerweb"
    }],
    "asset_usage_consent": true
}

```
## Key Scenarios

### Scenario 1
There’s a web crawler that is gathering information on available PWAs in order to add them into a application repository. The crawler finds the web application and reads in its manifest that there is developer consent to use the provided screenshots, icons and description to create an entry in such repository. The process can continue automatically without having to sign a license agreement regarding the brand use and copyright of these assets.

### Scenario 2
There's a web component from a vendor that that creates “install” experiences based on assets found in the manifest file. The component can display a store-like experience by using the assets linked in the manifest file. 

### Scenario 3
There's an application store that wants to list PWAs and other web applications, therefore it checks the web app manifest file in order to see if it can use the images and screenshots to display it.

## Stakeholder Feedback / Opposition
* No stakeholder feedback until now.

## Alternative approaches
* Using a URL to a license file (e.g. a CC license) instead of a boolean value.

## References & acknowledgements
Many thanks for valuable feedback and advice from:
* [Daniel Appelquist](https://twitter.com/torgo), [Samsung](https://samsunginter.net) 
