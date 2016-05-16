##webpack常用配置

'''javascript
var webpack            = require('webpack');
var CommonsChunkPlugin = webpack.optimize.CommonsChunkPlugin;
var ExtractTextPlugin  = require('extract-text-webpack-plugin');
 
//自定义"魔力"变量
var definePlugin = new webpack.DefinePlugin({
    __DEV__: JSON.stringify(JSON.parse(process.env.BUILD_DEV || 'false')),
    __PRERELEASE__: JSON.stringify(JSON.parse(process.env.BUILD_PRERELEASE || 'false'))
});
 
module.exports = {
    //上下文
    context: __dirname + '/src',
    //配置入口
    entry: {
        a: './view/index/index.js',
        b: './view/index/b.js',
        vender: ['./view/index/c.js', './view/index/d.js']
    },
    //配置输出
    output: {
        path: __dirname + '/build/',
        filename: '[name].js?[hash]',
        publicPath: '/assets/',
        sourceMapFilename: '[file].map'
    },
    devtool: '#source-map',
    //模块
    module: {
        loaders: [
            {
                //处理javascript
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel'
            }, {
                test: /\.css$/,
                loader: ExtractTextPlugin.extract(
                    "style-loader",
                    "css-loader?sourceMap"
                )
            }, {
                test: /\.less$/,
                loader: ExtractTextPlugin.extract(
                    "style-loader",
                    "css-loader!less-loader"
                )
            }, {
                test: /\.(png|jpg)$/,
                loader: 'url-loader?limit=1024'
            }, {
                //处理vue
                test: /\.vue$/,
                loader: 'vue-loader'
            },
            {
                test: /\.woff(\?v=\d+\.\d+\.\d+)?$/,
                loader: 'url?limit=10000&minetype=application/font-woff'
            },
            {
                test: /\.woff2(\?v=\d+\.\d+\.\d+)?$/,
                loader: 'url?limit=10&minetype=application/font-woff'
            },
            {
                test: /\.ttf(\?v=\d+\.\d+\.\d+)?$/,
                loader: 'url?limit=10&minetype=application/octet-stream'
            },
            {
                test: /\.eot(\?v=\d+\.\d+\.\d+)?$/,
                loader: 'file'
            },
            {
                test: /\.svg(\?v=\d+\.\d+\.\d+)?$/,
                loader: 'url?limit=10&minetype=image/svg+xml'
            }
        ]
 
    },
    plugins: [
        //公用模块
        new CommonsChunkPlugin('common.js', ['a', 'b']),
        //设置抽出css文件名
        new ExtractTextPlugin("css/[name].css?[hash]-[chunkhash]-[contenthash]-[name]", {
            disable: false,
            allChunks: true
        }),
        //定义全局变量
        definePlugin,
        //设置此处，则在JS中不用类似require('./base')引入基础模块， 只要直接使用Base变量即可
        //此处通常可用做，对常用组件，库的提前设置
        new webpack.ProvidePlugin({
            Moment: 'moment', //直接从node_modules中获取
            Base: '../../base/index.js' //从文件中获取
        })
    ],
    //添加了此项，则表明从外部引入，内部不会打包合并进去
    externals: {
        jquery: 'window.jQuery',
        react: 'window.React',
        //...
    }
};
'''