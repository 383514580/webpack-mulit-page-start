# 基于webpack4的多页模板

- [x] 支持scss
- [x] 支持html中的图片解析
- [x] 支持ES6

## webpack.config.js
```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
    entry: {
        index: './src/js/index.js',
        detail: './src/js/detail.js'
    },
    plugins: [
        new MiniCssExtractPlugin({
            // Options similar to the same options in webpackOptions.output
            // both options are optional
            filename: 'assets/css/[name].css',
            chunkFilename: 'assets/css/[id].css',
        }),
        new HtmlWebpackPlugin({
            chunks: ['index'],
            filename: 'index.html',
            template: path.resolve(__dirname, './src/view/index.html')
        }),
        new HtmlWebpackPlugin({
            chunks: ['detail'],
            filename: 'detail.html',
            template: path.resolve(__dirname, './src/view/detail.html')
        })
    ],
    module: {
        rules: [{
            test: /\.html$/i,
            loader: 'html-loader'
        }, {
            test: /\.m?js$/,
            exclude: /(node_modules|bower_components)/,
            use: {
                loader: 'babel-loader',
                options: {
                    presets: ['@babel/preset-env']
                }
            }
        }, {
            test: /\.s[ac]ss$/i,
            use: [
                process.env.NODE_ENV === 'development' ?
                'style-loader' :
                MiniCssExtractPlugin.loader,
                // Translates CSS into CommonJS
                'css-loader',
                // Compiles Sass to CSS
                'sass-loader',
            ],
        }, {
            test: /\.(png|svg|jpg|gif)$/,
            use: {
                loader: 'file-loader',
                options: {
                    name: 'assets/img/[name].[ext]',
                }
            }
        }, ]
    },

    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: "assets/js/[name].js",
        publicPath: "./"
    }
};
```