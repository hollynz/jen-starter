# Jen Starter
[![Lighthouse score: 100/100](https://lighthouse-badge.appspot.com/?score=100)](https://developers.google.com/speed/pagespeed/insights/?url=https%3A%2F%2Finspiring-sammet-8f8ba9.netlify.com&tab=desktop)

> A starter project for the [Jen static website generator](). The primary goal of the project is to facilitate rapid development of static websites.

This project includes:
* Sass preprocessing
* Nunjucks/HTML for templating
* CSS concatenation and minification
* Javascript concatenation and uglification
* Multi-browser live browser reload
* Master-detail pattern
* A modular, reusable design

## Getting Started
### 1. Clone this repository
```
git clone https://github.com/appyay/jen-starter.git
cd jen-starter
```
Or [download as a ZIP](https://github.com/appyay/jen-starter/archive/master.zip).

### 2. Install packages
```
npm install
```

### 3. Run the development server
```
npm run dev
```
The website will be viewable at http://localhost:3000. On save of the project, output will be compiled and built to the "public" directory and the website will be reloaded.

## How to use
### Project structure
Jen assumes the following project structure:
````
|--src // required
    |--data // required, but folder and contents will be auto-generated if you supply a dataUrl
        |--db.json // required
    |--templates //required
        |--pages //required
            |--home //required. This is the homepage folder. Other pages can be added in the same manner
                |--index.html //required
|--gulpfile.js // required
````

### Creating templates
[Nunjucks](https://mozilla.github.io/nunjucks/) is used for compiling template files to HTML.

Templates are stored in "src/templates". To create a template, create a file in the templates directory with the ".html" file extension. 

### Pages
Pages are added as sub-directories of the "pages" directory with an index.html file.

### Includes
To pull in another template, use the following syntax:

```
{% include "folder/file.html" %}
```
This is used to include the layout templates in each of the pages:
````
{% include "layout/_header.html" %}
````

### Partials
Partial files begin with an underscore and encapulate reusable components of a page. The home folder (home page) contains two examples of partials: _landing.html and _reviews.html.

Partial files are defined with Nunjucks blocks:
````
{% block greeting %} 
<h2>Hi</h2>
{% endblock %}
````
The partial can then be used in your page like so:
````
{% block hello %}{% endblock %}
````

### Frontmatter
The page object provides metadata about the page. It should be set at the top of every page HTML file:
````
{% set page = { 
name: 'home', // page name
title: 'Home' // title of page, used in layout/header/_header.html
} %}
````
More properties can be added to this object as needed. 

### Detail pages
The master-detail pattern (i.e. a list page and a detail page), is demonstrated in the features page:
````
...
        |--pages
        ...
            |--features // page folder
                |--detail //
                    |--index.html // this is your detail page
                |--index.html //this is your list (master) page
...
````
So, if a feature item has an ID of "abc123", the detail page would be accessible at:
````
http://localhost:3000/features/abc123
````

### Data
#### Loading data
Dummy data of reviews, features and documentation is already included in the project. Data can be loaded in three ways:
1. Manually add a db.json file to the data folder.
2. Load remote data by running ````npm load --dataUrl 'http://example.com/api/whatever'````
3. Load remote data and build the project by running ````npm build````

#### Database
The database data for the application is located at data/db.json. This data can be repopulated every time the project builds, so you can have dynamic data if used in combination with static hosting services, webhooks and a headless CMS.

Data in this file should be in the following format (using "features" as an example):
````
{
    "features": {
        "items":  [
            {
                "id": "abc123",
                ...
            }
        ]
    }
}
````

### Assets
#### Sass
Sass files are stored in the ````src/assets/scss/```` directory and in the root of page directories. The root Sass file is ````src/assets/scss/index.scss````.
Sass files kept at a component level (in the templates directory) are automatically compiled and are imported into the main Sass file:
````
@import '../../bin/generated/components';
````
But note that you won't have control over source order, so component level styles should be written so that they are encapsulated. If source order is imported to you, you should create Sass files in the src/assets/scss directory instead and import them manually into the main Sas file.


#### Javascript
Javascript files can be added to the ````src/assets/js```` folder and will be concatenated into one file and uglified.

#### Images
Images can be added to the ````src/assets/images```` folder. This is an example of how to access an image:
````
<img src="{{ page.path | path }}my-image.png" alt="My image"/>
````