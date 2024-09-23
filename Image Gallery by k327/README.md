## About
The Image Gallery by k327 software allows you to create your own image gallery website.
You can try out an example image gallery made using it [here](https://example-image-gallery.k327.eu).
The software is made using Vue.js.

## Features
On the main screen you are greeted with the main panel containing images and their titles below them, a header with a search bar and two optional buttons, one to show the information about your website, and the other one to filter shown images to only show the favorited ones. At the bottom of the screen there's a footer with the number of the page that you're on, the amount of all pages, and buttons to switch between them. 49 images fit on one page.
When you click on a photo on the main panel, a partially transparent black overlay will cover your screen and the image will present itself as big in the middle of it. Below the image there will be the description of it, and above - the title of it as well as the buttons to favorite it as well as close it going to the main screen, and ones to go to the next or the previous image.
When you use the search feature, the images on the main panel will be filtered based on whether either their title or their description contains your search query. When you click the filter by favorited button, only favorited images will be shown. Those two features work well with eachother. If there are no images matching the query, a text saying "No images found" will be shown.

## Setup
First, you need a json file with data about your photos. It requires an array called photos, with each of them having an id, a description, a title and the url to the image itself. 
An example json file meeting those requirements is [this](https://api.slingacademy.com/v1/sample-data/photos?limit=132), however it contains some unnecessary information that you don't need in your json file.
Then, go to src/App.vue and go to the configuration that starts on the 20th line of code. There, assign the url to the json file to jsonUrl. 
After that set siteInformation, which is what will be displayed when you click the i button at the left edge of the header. You can use html code in it. If you set the siteInformation to nothing, that is "", the button will be gone.
The next setting is allowFavoriting, this determines whether favoriting images will be enabled. As an additional fact I can say that the information about which images are favorited is stored locally on the user's device, through localStorage.
You can also change the title of the website in index.html as well as its favicon by replacing it at public/favicon.ico(or changing its location in index.html).
Then, install node_modules(using npm that's npm install), after that build the website(npm run build) and you are ready to put it on your server. An important thing to note is that you must put it in the main directory in order for it to work, so for example having it at gallery.k327.eu is fine but at k327.eu/gallery is not.