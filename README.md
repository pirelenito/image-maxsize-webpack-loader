[![Build Status](https://travis-ci.org/shekhei/image-maxsize-webpack-loader.svg?branch=master)](https://travis-ci.org/shekhei/image-maxsize-webpack-loader)
[![npm version](https://badge.fury.io/js/image-maxsize-webpack-loader.svg)](https://www.npmjs.com/package/image-maxsize-webpack-loader)

# Webpack module for max size (width/height)
This loader will resize images to fit maximum width / height dimensions while retaining their aspect ratio.

## Changelog
* v0.1.0 - Merged code from @danielberndt that added ImageMagick support and various code cleanup
* v0.2.0 - Merged code from @chromakode allowing default parameters
* v1.0.0 - Upgrade to webpack >= 2 < 4 and adding tests



## Basic Usage

## Version 1 and above(with webpack2/3)
```
npm install image-maxsize-webpack-loader gm
```

In your webpack config file:

```js
// Example webpack.config.js. I recommend using the url and image loaders.
module.exports = {
    module: {
        rules: [
            // The max-width and max-height parameters here are optional. They set the default max dimensions for all images using this loader.
            { 
                test: /\.(png|svg|jpe?g)(\?.*)?$/,
                loaders: [
                    {
                        loader: "url-loader",
                        options: {
                            limit: 800
                        }
                    },
                    "image-loader",
                    {
                        loader: "image-maxsize-webpack-loader",
                        options: {
                            "max-width": 800,    // sets max-width for gm/imagemagick scaling, in pixels
                            "max-height": 600,   // sets max-height for gm/imagemagick scaling, in pixels
                            "useImageMagick": false // defaults to false, this controls the usage of imagemagick or graphicsmagick, when false, graphicsmagick is used
                        }
                    }
                ]
            }
        ]
    }
};
```

In addition to the default values, you can also set ***max-width*** and ***max-height*** inline

```css

@media screen and (max-width: 800px) {
    #abc {
        background: url('./abc.jpg?max-width=800&max-height=600');
    }
}
@media screen and (max-width: 480px) {
    #abc {
        background: url('./abc.jpg?max-width=480');
        /* The max-width and max-height parameters are both optional, if not provided the current height/width of the image will be used. */
    }
}
/* The above will shrink the file to fit into a 800x600 file, while retaining its aspect ratio. */
```


## Before version 1
```
npm install image-maxsize-webpack-loader
```

In your webpack config file:

```js
// Example webpack.config.js. I recommend using the url and image loaders.
module.exports = {
    module: {
        loaders: [
            // The max-width and max-height parameters here are optional. They set the default max dimensions for all images using this loader.
            { test: /\.(png|svg|jpe?g)(\?.*)?$/, loader: "url?limit=800!image!image-maxsize?max-width=800&max-height=600"}
        ]
    }
};
```

You can specify the maximum size for individual images using the `max-width` and `max-height` query parameters:

```css
@media screen and (max-width: 800px) {
    #abc {
        background: url('./abc.jpg?max-width=800&max-height=600');
    }
}
@media screen and (max-width: 480px) {
    #abc {
        background: url('./abc.jpg?max-width=480');
        /* The max-width and max-height parameters are both optional, if not provided the current height/width of the image will be used. */
    }
}
/* The above will shrink the file to fit into a 800x600 file, while retaining its aspect ratio. */
```

## Requirements
GraphicsMagick _or_ ImageMagick

If you prefer to use ImageMagick add `?useImageMagick=true` to the loader.

### Windows Users

I truly recommend installing chocolatey, then you can just run:

```
choco install graphicsmagick
```

Credits
-------
I would have to say, thanks to people who wrote the gm bindings, that made it much easier!
Thanks @danielberndt for cleaning up the code and adding the imagemagick options!
