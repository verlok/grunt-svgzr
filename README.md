# grunt-svgzr
> Convert svg to png, and create templates for sass and compass with base64 svg and png.

[![Build Status](https://travis-ci.org/aditollo/grunt-svgzr.svg?branch=master)](https://travis-ci.org/aditollo/grunt-svgzr)

## Getting Started
This plugin requires Grunt `~0.4.5`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-svgzr --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-svgzr');
```

## The "svgzr" task

### Overview
In your project's Gruntfile, add a section named `svgzr` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
	svgzr: {
		dist: {
			options: {
				templateFileSvg: 'templateSvg.mst',
				templateFileFallback: 'templateFallback.mst',
                files: {
					cwdSvg: 'icons/svg/',
					cwdPng: "sprite/fallback/"
				},
				prefix: 'svg-',
				svg: {
					destFile: 'sass/common/_icons.scss'
				},
				png: true,
				fallback : {
					mixinName: 'svg-fallback',
					dir: 'fallback/',
					destFile: 'sass/common/_icons-fallback.scss'
				}
			}
		}
	}
});
```

### Options

#### options.templateFileSvg
Type: `String`
Default value: `'./test/templateSvg.mst'`

The path of the template file used to create the svg scss file. If the path is invalid, the task will use the old json method.
It is written with the [mustache](http://github.com/janl/mustache.js) template syntax.

#### options.templateFileFallback
Type: `String`
Default value: `'./test/templateFallback.mst'`

The path of the template file used to create the png scss file. If the path is invalid, the task will use the old json method.
It is written with the [mustache](http://github.com/janl/mustache.js) template syntax.

#### options.templateFile (DEPRECATED)
Type: `String`
Default value: `'./test/template.json'`

The path of the template file used to create the svg and png scss files. If the path is invalid, The task will use a standard internal template.
This option is deprecated. It'll be deleted in next issues.

#### options.files.cwdSvg
Type: `String`
Default value: `'svg/'`

The path of svg files for for the conversion from svg to png.

#### options.files.cwdPng
Type: `String`
Default value: `'png/'`

The path of png files for the conversion from svg to png and for the scss file with png fallbacks.

#### options.prefix
Type: `String`
Default value: `'svg-'`

A string used as prefix in css classes

#### options.svg
Type: `object`
Default value: `false`

If `false`, the plugin will not proceed with the creation of the scss file with svg classes.

#### options.svg.destFile
Type: `String`
Default value: `'example/sass/common/_icons.scss'`

The output scss file with svg classes.

#### options.png
Type: `object`
Default value: `false`

If `false`, the plugin will not proceed with the conversion from svg to png files. If true, svgzr will use [svg2png](https://www.npmjs.org/package/svg2png) and phantomJS for the conversion.

#### options.fallback
Type: `object`
Default value: `false`

If `false`, the plugin will not proceed with the creation of the scss file with png classes.

#### options.fallback.mixinName
Type: `String`
Default value: `'svg-fallback'`

The name of the mixin that you want to use in the scss file with png classes

#### options.fallback.dir
Type: `String`
Default value: `undefined`

It will be used from compass to create the png sprite file.
If it is `undefined` or `null`, it'll be set to the relative path from `options.fallback.destFile` to `options.files.cwdPng`.

#### options.fallback.destFile
Type: `String`
Default value: `'example/sass/common/_icons-fallback.scss'`

The output scss file with png classes.

### Usage Examples

#### Default Options
By default, svgzr does not create any output.
```js
grunt.initConfig({
	svgzr: {
		dist: {
			options: {}
		}
	}
});
```
is equal to the following
```js
grunt.initConfig({
	svgzr: {
		dist: {
			options: {
				svg: false,
				png: false,
				fallback : false
			}
		}
	}
});
```

#### ONLY SCSS FILE WITH SVG CLASSES

In this example, svgzr wil get all svg files in `cwdSvg` path, encode them in base64, and embed them in a scss file, using the filename and the `prefix` for the class name.
```js
grunt.initConfig({
	svgzr: {
		dist: {
			options: {
				templateFileSvg: 'templateSvg.mst',
				files: {
					cwdSvg: 'icons/svg/',
				},
				prefix: 'svg-',
				svg: {
					destFile: 'sass/common/_icons.scss'
				}
			}
		}
	}
});
```

#### ONLY SCSS FILE WITH PNG CLASSES

In this example, svgzr wil get all png files in `cwdPng` path, and create a scss file that use the filename and the `prefix` for the class name.
This file will be used from compass to create a sprite file and png fallbacks.

```js
grunt.initConfig({
	svgzr: {
		dist: {
			options: {
			    templateFileFallback: 'templateFallback.mst',
				files: {
					cwdPng: "sprite/fallback/"
				},
				prefix: 'svg-',
				fallback : {
					mixinName: 'svg-fallback',
					dir: 'fallback/',
					destFile: 'sass/common/_icons-fallback.scss'
				}
			}
		}
	}
});
```

#### ONLY SVG TO PNG CONVERSION

In this example, svgzr wil get all svg files in `cwdSvg` path, convert them in png, and store png files in `cwdPng`.
```js
grunt.initConfig({
	svgzr: {
		dist: {
			options: {
				files: {
					cwdSvg: 'icons/svg/',
					cwdPng: "sprite/fallback/"
				},
				png: true
			}
		}
	}
});
```

## Release History

0.2.0 Adopted the mustache template method. The old json method is now deprecated. It'll be deleted in next issues.

0.1.5 Removed im and gm; minor fixes.

0.1.4	Fix templates.

0.1.3	Solved problem with only svg option active.

0.1.2	minor fixes; added some tests

0.1.1	resolved async problem

0.1.0	intial release
