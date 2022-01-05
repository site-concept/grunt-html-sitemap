# grunt-html-sitemap

> Create HTML sitemaps from a directory of static files.

Useful for sites that maintain a `<ul>` sitemap on its own page that should be updated on each build or deploy.

__Note:__ This task works but needs a lot of work. Pull requests are very welcome.

## Getting Started
This plugin requires Grunt `~0.4.5`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

### Installation

```shell
npm install grunt-html-sitemap --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-html-sitemap');
```

Or install [load-npm-tasks](https://github.com/sindresorhus/load-grunt-tasks) so you don't have to worry about that anymore.

## The "html_sitemap" task

This task traverses a directory of static files and creates an HTML sitemap from its contents. When run, this task will build a map of all static HTML files in a directory and use their paths, title tags, and some additional options (detailed below) to create an HTML sitemap.

This task generates HTML only, it __does not create XML sitemaps__.

### Overview
In your project's Gruntfile, add a section named `html_sitemap` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
  html_sitemap: {
    options: {
      // Task-specific options go here.
    },
    your_target: {
      // Target-specific file lists and/or options go here.
    },
  },
});
```

### Options

#### options.siteBase
Type: `String`
Default value: Homepage in package.json

A string to be appended to the `href` on each anchor tag. Defaults to creating all links as relative to site root. For example, if you specify `http://example.com` as your base then the task will append that URL to the paths it finds.

#### options.separator
Type: `String`, `Boolean`
Default value: `false`

A string to separate page names from site names in title tags. Many sites use a "Page name | Company Name" format in their title tags. This task uses each page's title tag to generate anchor text for each list item in the sitemap. When this option is set the first part of the title tag before the separator will be used as anchor text.

#### options.searchPath
Type: `String`
Default value: `''`

A path prefix to strip from generated URLs. Because you may have a project set up with a `src` and `dist` folders, the URLs generated by the task will contain the base path where your files live (something that's on the roadmap to be fixed). Setting the `searchPath` option will strip that path from the generated URLs.

#### options.template
Type: `String`, `Boolean`
Default value: `false`

By default the task generates an HTML `<ul>` element in a file by itself (useful if you plan to inlcude the file elsewhere with another plugin). By providing the template option with the path to a template file you want the markup inserted into you can create full sitemap.html pages with the look and feel of your website dynamically. To use a template, provide this option with the path to the template file. Then, inside the template, add a `<%= sitemap %>` template string where you want the `<ul>` to be placed.

#### options.descriptions
Type: `Boolean`
Default value: `false`

Whether to include descriptions along with the anchor text. By default the descriptions will come from the page's `<meta />` description but can be overridden using the alternate description comment format shown below. Setting this to true will result in the following markup being generated:

```html
<ul>
  <li><a href="/example">Example page</a>
    <p>This comes from the meta description by default</p>
  </li>
  ...
</ul>
```

#### Alternate description text
If you choose to display descriptions of pages below the link generated in the sitemap you have the option to change the text that is shown from the default (what's contained in your meta description tag) to any other string by using the following special HTML comment format:

```html
<!-- sitemap:description This text is different than what's in my meta tag -->
```

#### Alternate anchor text
In addition to the config options set in your Gruntfile, you have the ability to set custom anchor text by adding specially formatted HTML comment blocks to your markup. For example, a page with this markup:

```html
<html>
<head>
  <title>My title</title>
  <!-- sitemap:anchor My Different Title -->
</head>
...
</html>
```

Will produce a sitemap listing that looks like this:

```html
<ul>
  ...
  <li><a href="http://example.com">My Different Title</a></li>
</ul>
```

### Usage Examples

#### Default Options
In this example the default options are used to create a sitemap in `dest/sitemap.html` from all HTML files in `src/`.

```js
grunt.initConfig({
  html_sitemap: {
    options: {},
    files: {
      'dest/sitemap.html': ['src/**/*.html']
    }
  }
});
```

#### Custom Options
In this example we set all the options are used to create a sitemap file in `dest/custom_sitemap.html`from all the HTML files in `src/`. The anchor text will use the text that comes before the `-` separator, `src/` will be removed from the URL paths, the base of the site will be relative to the site root and not include a URL (the task uses the homepage value in package.json by default), and it uses a custom template to populate the sitemap into.

```js
grunt.initConfig({
  html_sitemap: {
    options: {
      separator: '-',
      searchPath: 'src/',
      siteBase: '/',
      template: 'src/sitemap_template.html'
    },
    files: {
      'dest/custom_sitemap.html': ['src/**/*.html']
    },
  },
});
```

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/). Make sure your editor supports EditorConfig. It'll help maintain styles.

## Release History
_(Nothing yet)_