# Webpack是什么?

Webpack是一种前端资源构建工具，一个静态模块打包器(module bundler)
在Webpack看来，前端的所有资源文件都会作为模块处理
它将根据模块的依赖关系进行静态分析，打包生成对应的静态资源(bundle)

# Webpack五个核心概念

## 1.Entry

入口 (Entry) 指示 Webpack 以哪个文件为入口起点开始打包，分析构建内部依赖图

## 2.Output

输出 (Output) 指示 Webpack 打包后的资源 bundles 输出到哪里去，以及如何命名

## 3.Loader

Loader 让 Webpack 能够去处理那些非 JavaScript 文件（Webpack自身只理解JavaScript/JSON）

## 4.Plugins

插件(Plugins)可以用于执行范围更广的任务，插件的范围包括。从打包优化和压缩，一直到重新定义环境中的变量等

## 5.Mode

模式(Mode)提示 Webpack 使用相应模式的配置
![](_v_images/20200512230106457_28770.png =594x)

# Webpack初体验

npm i webpack webpack-cli -g
npm i webpack webpack-cli -D

开发环境：webpack ./src/index.js -o ./build/built.js --mode=development

生产环境：webpack ./src/index.js -o ./build/built.js --mode=production

1.webpack能处理js/json资源，不能处理css/img等其他资源
2.生产环境和开发环境将ES6模块化编译成浏览器能识别的模块化
3.生产环境比开发环境多一个压缩JS代码的过程


# Webpack开发环境配置

npm i css-loader style-loader less-loader less html-webpack-plugin html-loader url-loader file-loader -D
```
/* eslint-disable linebreak-style */

const HtmlWebpackPlugin = require('html-webpack-plugin');
const { resolve } = require('path');

module.exports = {
  // webpack配置

  // 入口配置
  entry: resolve(__dirname, 'src', 'js', 'index.js'),

  // 输出配置
  output: {
    // 输出文件名
    filename: 'js/built.js',
    // 输出路径
    path: resolve(__dirname, 'build'),
  },

  // loader配置
  module: {
    rules: [
      // 详细loader配置

      // CSS文件打包配置
      // 1.匹配CSS文件
      {
        test: /\.css$/,
        // 2.使用loader进行处理，先下载相关loader再使用
        use: [
          // use数组中loader执行顺序与下标相反
          // style-loader将模块样式内容插入到head的style标签
          'style-loader',
          // css-loader将css文件解析为commonjs模块，模块内容为样式字符串
          'css-loader',
        ],
      },

      // less文件打包配置
      {
        test: /\.less$/,
        use: ['style-loader', 'css-loader', 'less-loader'],
      },

      // 图片打包配置
      {
        test: /\.(jpg|png|gif)$/,
        // url-loader依赖于file-loader
        loader: 'url-loader',
        options: {
          // 图片小于8kb，base64处理
          // 优点：减少请求数量
          // 缺点：图片体积会更大
          limit: 8 * 1024,
          // 问题：url-loader默认使用ES6模块化解析，而html-loader引入图片是commonjs
          // 解析时会出问题：[object Module]
          // 解决：关闭url-loader的ES6模块化，使用commonjs模块化解析
          esModule: false,
          // 图片重命名
          // 取图片hash值前10位，取文件的原扩展名
          name: '[hash:10].[ext]',
          outputPath: 'img',
        },
      },
      {
        test: /\.html$/,
        // 处理html文件中的img图片（负责引入img，从而被url-loader解析处理）
        loader: 'html-loader',
      },

      // 打包其他资源
      {
        // 排除html/css/js资源
        exclude: /\.(css|js|html|less)$/,
        loader: 'file-loader',
        options: {
          name: '[hash:10].[ext]',
          outputPath: 'media',
        },
      },
    ],
  },

  // plugins配置
  plugins: [
    // 详细plugins配置,plugins先下载引入，再使用

    // html文件打包配置
    // 功能：默认会创建一个空的html，自动引入打包输出的所有资源
    // 需求：需要有结构的html文件
    new HtmlWebpackPlugin({
      // 复制html文件，并自动将打包资源引入
      template: resolve(__dirname, 'src', 'index.html'),
    }),
  ],

  // mode配置
  mode: 'development', // or production

  // 开发服务器devServer，用来自动化
  // 特点:只会在内存中编译打包，不会有任何输出
  // npx webpack-dev-server
  devServer: {
    // 项目构建路径
    contentBase: resolve(__dirname, 'bulid'),
    // 启动gzip压缩
    compress: true,
    // 端口号
    port: 3000,
    // 自动打开浏览器
    open: true,
  },
};

```
# Webpack生产环境配置

## 提取CSS成单独文件

`use:['style-loader', 'css-loader']`
会将样式资源插入到新建style标签中造成闪屏

npm i mini-css-extract-plugin -D

```
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const {resolve} = require('path');

module.exports = {
	entry: resolve(__dirname, 'src', 'js', 'index.js'),
	output: {
		filename: 'js/built.js',
		path: resolve(__dirname, 'build')
	},
	module: {
		rules: [
			{
				test: /\.css$/,
				use: [MiniCssExtractPlugin.loader, 'css-loader']
			}
		]
	},
	plugins: [
		new HtmlWebpackPlugin({
			template: resolve(__dirname, 'src', 'index.html')
		}),
		new MiniCssExtractPlugin({
			filename: 'css/built.css'
		})
	],
	mode: 'development'
}
```
## CSS兼容性处理

npm i postcss-loader postcss-preset-env -D

```
// 设置node.js环境变量，不写默认是生产环境
process.env.NODE_ENV = 'development';
```
```
module: {
		rules: [
			{
				test: /\.css$/,
				use: [MiniCssExtractPlugin.loader, 'css-loader', {
					loader: 'postcss-loader',
					options: {
						ident: 'postcss',
						plugins: () => {
							require('postcss-preset-env')()
						}
					}
				}]
			}
		]
	},
```

修改package.json
```
"browserslist": {
"development": [
"last 1 chrome version",
"last 1 firefox version",
"last 1 safari version"
],
"production": [
">0.2%",
"not dead",
"not op_mini all"
]
}
```

## CSS文件压缩

npm i optimize-css-assets-webpack-plugin -D

`const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin')`

```
	plugins: [
		new HtmlWebpackPlugin({
			template: resolve(__dirname, 'src', 'index.html')
		}),
		new MiniCssExtractPlugin({
			filename: 'css/built.css'
		}),
		new OptimizeCssAssetsWebpackPlugin()
	],
```

## CSS文件打包总结

```
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin');
const {resolve} = require('path');

module.exports = {
	entry: resolve(__dirname, 'src', 'js', 'index.js'),
	output: {
		filename: 'js/built.js',
		path: resolve(__dirname, 'build')
	},
	module: {
		rules: [
			{
				test: /\.css$/,
				use: [MiniCssExtractPlugin.loader, 'css-loader', {
					loader: 'postcss-loader',
					options: {
						ident: 'postcss',
						plugins: () => {
							require('postcss-preset-env')()
						}
					}
				}]
			}
		]
	},
	plugins: [
		new HtmlWebpackPlugin({
			template: resolve(__dirname, 'src', 'index.html')
		}),
		new MiniCssExtractPlugin({
			filename: 'css/built.css'
		}),
		new OptimizeCssAssetsWebpackPlugin()
	],
	mode: 'development'
}
```
## ESlint语法检查

npm i eslint-loader eslint eslint-config-airbnb eslint-plugin-import -D

```
module: {
    rules: [
      /* 语法检查： eslint-loader eslint
      注意：只检查自己写的源代码，第三方的库是不用检查的
      设置检查规则：
      package.json 中 eslintConfig 中设置
      */
      {
        test: /\.js$/,
        exclude: /node_modules/,
        enforce: 'pre',
        loader: 'eslint-loader',
        options: {
          // 自动修复 eslint 的错误
          fix: true
        }
      }
    ]
  },
```

修改package.json
```
  "eslintConfig": {
    "extends": "airbnb-base",
    "env": {
    "browser": true
    }
  }
```

## JS兼容性处理

npm i babel-loader @babel/core @babel/preset-env @babel/polyfill core-js -D

```
module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
        options: {
          // 预设：指示babel做怎样的兼容性处理
          presets: [
            [
              '@babel/preset-env',
              {
              // 按需加载
                useBuiltIns: 'usage',
                // 指定 core-js 版本
                corejs: {
                  version: 3,
                },
                // 指定兼容性做到哪个版本浏览器
                targets: {
                  chrome: '60',
                  firefox: '60',
                  ie: '9',
                  safari: '10',
                  edge: '17',
                },
              },
            ],
          ],
        },
      },
    ],
  },
```

## HTML文件压缩

```
plugins: [
    new HtmlWebpackPlugin({
      template: resolve(__dirname, 'src', 'index.html'),
      minify: {
        collapseWhitespace: true,
        removeComments: true,
      },
    }),
  ],
```
## 生产环境总结

npm i mini-css-extract-plugin postcss-loader postcss-preset-env optimize-css-assets-webpack-plugin eslint-loader eslint eslint-config-airbnb-base eslint-plugin-import babel-loader @babel/core @babel/preset-env @babel/polyfill core-js -D

```
/* eslint-disable linebreak-style */
/* eslint-disable global-require */
const { resolve } = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin');

// 复用loader
const commonCssLoader = [
  MiniCssExtractPlugin.loader,
  'css-loader',
  // css兼容还需要在package.json中配置browserlist
  // "browserslist": {
  //   "development": [
  //   "last 1 chrome version",
  //   "last 1 firefox version",
  //   "last 1 safari version"
  //   ],
  //   "production": [
  //   ">0.2%",
  //   "not dead",
  //   "not op_mini all"
  //   ]
  // }
  {
    loader: 'postcss-loader',
    options: {
      ident: 'postcss',
      plugins: () => [
        require('postcss-preset-env')(),
      ],
    },
  },
];

module.exports = {
  entry: resolve(__dirname, 'src', 'js', 'index.js'),
  output: {
    filename: 'js/built.js',
    path: resolve(__dirname, 'build'),
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [...commonCssLoader],
      },
      {
        test: /\.less$/,
        use: [...commonCssLoader, 'less-loader'],
      },
      // package.json中配置eslint
      // "eslintConfig": {
      //   "extends": "airbnb-base",
      //   "env": {
      //     "browser": true
      //   }
      // }
      {
        test: /\.js$/,
        exclude: /node_modules/,
        enforce: 'pre',
        loader: 'eslint-loader',
        options: {
          fix: true,
        },
      },

      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
        options: {
          presets: [
            [
              '@babel/preset-ev',
              {
                useBuiltIns: 'usage',
                corejs: { version: 3 },
                targets: {
                  chrome: '60',
                  firefox: '60',
                  ie: '9',
                  safari: '10',
                  edge: '17',
                },
              },
            ],
          ],
        },
      },
      {
        test: /\.(jpg|png|gif)$/,
        loader: 'url-loader',
        options: {
          limit: 8 * 1024,
          name: '[hash:10].[ext]',
          esModule: false,
          outputPath: 'img',
        },
      },
      {
        test: /\.html$/,
        loader: 'html-loader',
      },
      {
        exclude: /\.(html|js|css|jpg|gif|png)$/,
        loader: 'file-loader',
        options: {
          outputPath: 'media',
        },
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: resolve(__dirname, 'src', 'index.html'),
      minify: {
        collapseWhitespace: true,
        removeComments: true,
      },
    }),
    new MiniCssExtractPlugin({
      filename: 'css/built.css',
    }),
    new OptimizeCssAssetsWebpackPlugin(),
  ],
  mode: 'production',
};
```

# Webpack性能优化

开发环境性能优化:
    优化打包构建速度，优化代码调试

生产环境性能优化：
    优化打包构建速度，优化代码运行性能

## HMR

hot module replacement 模块热替换
作用：一个模块发生变化，只会重新打包这一个模块，而不是打包所有模块，极大提升构建速度

CSS文件：可以使用HMR功能，因为style-loader内部实现了

JS文件：默认不能使用HMR功能，需要修改JS代码，添加支持HMR更新的代码
注意：HMR功能对JS的处理，只能处理非入口JS文件的其他文件
```
if (module.hot) {
  module.hot.accept('./print.js', () => {
    print()
  })
}
```

HTML文件：默认不会使用HMR功能，同时会导致问题：HTML文件不能热更新了
解决：修改entry入口，将html文件引入

## source-map

一种提供源代码到构建后代码的映射技术
如果构建后代码出错了，通过映射可以追踪到源代码错误

开发环境
`devtool: 'eval-source-map'`

生产环境
`devtool: 'source-map'`

## oneOf

`oneOf: [loader1, loader2, loader3]`
oneOf中loader只会匹配一个
注意oneOf中同类型文件不能有两个loader

## 缓存

让代码运行缓存更好使用

1.babel缓存
    在babel-loader的options中加
    `cacheDirectory: true`

2.文件缓存
    在文件名中加入hash值`built.[hash:10].js`
    三种hash:
        1.hash:每次webpack构建时都会生成一个唯一的hash，如果重新打包会导致所有缓存失效
        2.chunkhash:根据chunk生成hash值，如果打包来源于同一个chunk，那么hash值一样
        3.contenthash:根据文件内容生成hash值，不同文件hash值不一样

## tree shaking

去除无用代码

前提：1.必须使用ES6模块化 2.开启production环境
作用：减少代码体积

在package.json中配置
    `"sideEffects": false` // 所有代码都没有副作用，都可以进行tree shaking
    问题：可能会把css之类的文件干掉
    `"sideEffects": ["*.css"]`

## code split

1.entry多入口

```
entry: {
  // 多入口：有一个入口，输出就有一个bundle
  index: '.src/js/index.js',
  test: '.src/js/test.js',
}

output: {
  filename: 'js/[name].[contenthash:10].js',
  path: resolve(__dirname, 'build')
}
```

2.optimization
    可以将node_modules中代码单独打包成一个chunk最终输出
    自动分析多入口chunk中，有没有公共的文件，如果有会打包成一个单独的chunk

```
optimization: {
  splitChunks: {
    chunks: 'all'
  }
},

mode: 'production'

```

3.通过import动态导入语法单独打包成一个chunk
```
import(/* webpackChunkName: 'test' */ './test')
  .then(() => {})
  .catch(() => {})
```

## 懒加载和预加载

懒加载：当文件需要使用时才加载
```
import(/* webpackChunkName: 'test' */ './test')
  .then(() => {})
  .catch(() => {})
```

正常加载可以认为是并行加载（同一时间加载多个文件）
预加载：等其他资源加载完毕浏览器空闲，偷偷加载资源
```
import(/* webpackChunkName: 'test', webpackPrefetch: true */ './test')
  .then(() => {})
  .catch(() => {})
```

## PWA

```
const { resolve } = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const WorkboxWebpackPlugin = require('workbox-webpack-plugin');

/*
  PWA: 渐进式网络开发应用程序(离线可访问)
    workbox --> workbox-webpack-plugin
*/

// 定义nodejs环境变量：决定使用browserslist的哪个环境
process.env.NODE_ENV = 'production';

// 复用loader
const commonCssLoader = [
  MiniCssExtractPlugin.loader,
  'css-loader',
  {
    // 还需要在package.json中定义browserslist
    loader: 'postcss-loader',
    options: {
      ident: 'postcss',
      plugins: () => [require('postcss-preset-env')()]
    }
  }
];

module.exports = {
  entry: './src/js/index.js',
  output: {
    filename: 'js/built.[contenthash:10].js',
    path: resolve(__dirname, 'build')
  },
  module: {
    rules: [
      {
        // 在package.json中eslintConfig --> airbnb
        test: /\.js$/,
        exclude: /node_modules/,
        // 优先执行
        enforce: 'pre',
        loader: 'eslint-loader',
        options: {
          fix: true
        }
      },
      {
        // 以下loader只会匹配一个
        // 注意：不能有两个配置处理同一种类型文件
        oneOf: [
          {
            test: /\.css$/,
            use: [...commonCssLoader]
          },
          {
            test: /\.less$/,
            use: [...commonCssLoader, 'less-loader']
          },
          /*
            正常来讲，一个文件只能被一个loader处理。
            当一个文件要被多个loader处理，那么一定要指定loader执行的先后顺序：
              先执行eslint 在执行babel
          */
          {
            test: /\.js$/,
            exclude: /node_modules/,
            loader: 'babel-loader',
            options: {
              presets: [
                [
                  '@babel/preset-env',
                  {
                    useBuiltIns: 'usage',
                    corejs: { version: 3 },
                    targets: {
                      chrome: '60',
                      firefox: '50'
                    }
                  }
                ]
              ],
              // 开启babel缓存
              // 第二次构建时，会读取之前的缓存
              cacheDirectory: true
            }
          },
          {
            test: /\.(jpg|png|gif)/,
            loader: 'url-loader',
            options: {
              limit: 8 * 1024,
              name: '[hash:10].[ext]',
              outputPath: 'imgs',
              esModule: false
            }
          },
          {
            test: /\.html$/,
            loader: 'html-loader'
          },
          {
            exclude: /\.(js|css|less|html|jpg|png|gif)/,
            loader: 'file-loader',
            options: {
              outputPath: 'media'
            }
          }
        ]
      }
    ]
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: 'css/built.[contenthash:10].css'
    }),
    new OptimizeCssAssetsWebpackPlugin(),
    new HtmlWebpackPlugin({
      template: './src/index.html',
      minify: {
        collapseWhitespace: true,
        removeComments: true
      }
    }),
    new WorkboxWebpackPlugin.GenerateSW({
      /*
        1. 帮助serviceworker快速启动
        2. 删除旧的 serviceworker

        生成一个 serviceworker 配置文件~
      */
      clientsClaim: true,
      skipWaiting: true
    })
  ],
  mode: 'production',
  devtool: 'source-map'
};

```

## 多进程打包

npm i  thread-loader -D

开启多进程打包。多用于babel
进程启动大概为600ms,进程通信也有开销
只有工作消耗时间比较长，才需要多进程打包
```
{
  loader: 'thread-loader',
  options: {
    workers: 2 //进程2个
  }
}
```

## externals

拒绝npm包被打包进来

```
externals: {
  jquery: 'jQuery' // 后面是包名
}
```

## dll

webpack.dll.js
```
/*
  使用dll技术，对某些库（第三方库：jquery、react、vue...）进行单独打包
    当你运行 webpack 时，默认查找 webpack.config.js 配置文件
    需求：需要运行 webpack.dll.js 文件
      --> webpack --config webpack.dll.js
*/

const { resolve } = require('path');
const webpack = require('webpack');

module.exports = {
  entry: {
    // 最终打包生成的[name] --> jquery
    // ['jquery'] --> 要打包的库是jquery
    jquery: ['jquery'],
  },
  output: {
    filename: '[name].js',
    path: resolve(__dirname, 'dll'),
    library: '[name]_[hash]' // 打包的库里面向外暴露出去的内容叫什么名字
  },
  plugins: [
    // 打包生成一个 manifest.json --> 提供和jquery映射关系
    new webpack.DllPlugin({
      name: '[name]_[hash]', // 映射库的暴露的内容名称
      path: resolve(__dirname, 'dll/manifest.json') // 输出文件路径
    })
  ],
  mode: 'production'
};

```

webpack.config.js

```
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack');
const AddAssetHtmlWebpackPlugin = require('add-asset-html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'built.js',
    path: resolve(__dirname, 'build')
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    }),
    // 告诉webpack哪些库不参与打包，同时使用时的名称也得变~
    new webpack.DllReferencePlugin({
      manifest: resolve(__dirname, 'dll/manifest.json')
    }),
    // 将某个文件打包输出去，并在html中自动引入该资源
    new AddAssetHtmlWebpackPlugin({
      filepath: resolve(__dirname, 'dll/jquery.js')
    })
  ],
  mode: 'production'
};

```

## 总结

### 开发环境性能优化

优化打包构建速度
    HMR
优化代码调试
    source-map

### 生产环境性能优化

优化打包构建速度
    oneOf
    babel缓存
    多进程打包
优化代码运行的性能
    文件缓存（hash-chunkhash-contenthash）
    tree shaking
    code split
    JS懒加载/预加载
    PWA
    externals
    dll
    
# Webpack详细配置

## entry详细配置

```
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

/*
  entry: 入口起点
    1. string --> './src/index.js'
      单入口
      打包形成一个chunk。 输出一个bundle文件。
      此时chunk的名称默认是 main
    2. array  --> ['./src/index.js', './src/add.js']
      多入口
      所有入口文件最终只会形成一个chunk, 输出出去只有一个bundle文件。
        --> 只有在HMR功能中让html热更新生效~
    3. object
      多入口
      有几个入口文件就形成几个chunk，输出几个bundle文件
      此时chunk的名称是 key

      --> 特殊用法
        {
          // 所有入口文件最终只会形成一个chunk, 输出出去只有一个bundle文件。
          index: ['./src/index.js', './src/count.js'], 
          // 形成一个chunk，输出一个bundle文件。
          add: './src/add.js'
        }
*/

module.exports = {
  entry: {
    index: ['./src/index.js', './src/count.js'], 
    add: './src/add.js'
  },
  output: {
    filename: '[name].js',
    path: resolve(__dirname, 'build')
  },
  plugins: [new HtmlWebpackPlugin()],
  mode: 'development'
};

```

## output详细配置

```
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    // 文件名称（指定名称+目录）
    filename: 'js/[name].js',
    // 输出文件目录（将来所有资源输出的公共目录）
    path: resolve(__dirname, 'build'),
    // 所有资源引入公共路径前缀 --> 'imgs/a.jpg' --> '/imgs/a.jpg'
    publicPath: '/',
    chunkFilename: 'js/[name]_chunk.js', // 非入口chunk的名称
    // library: '[name]', // 整个库向外暴露的变量名
    // libraryTarget: 'window' // 变量名添加到哪个上 browser
    // libraryTarget: 'global' // 变量名添加到哪个上 node
    // libraryTarget: 'commonjs'
  },
  plugins: [new HtmlWebpackPlugin()],
  mode: 'development'
};

```

## module详细配置

```
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'js/[name].js',
    path: resolve(__dirname, 'build')
  },
  module: {
    rules: [
      // loader的配置
      {
        test: /\.css$/,
        // 多个loader用use
        use: ['style-loader', 'css-loader']
      },
      {
        test: /\.js$/,
        // 排除node_modules下的js文件
        exclude: /node_modules/,
        // 只检查 src 下的js文件
        include: resolve(__dirname, 'src'),
        // 优先执行
        enforce: 'pre',
        // 延后执行
        // enforce: 'post',
        // 单个loader用loader
        loader: 'eslint-loader',
        options: {}
      },
      {
        // 以下配置只会生效一个
        oneOf: []
      }
    ]
  },
  plugins: [new HtmlWebpackPlugin()],
  mode: 'development'
};

```

## resolve详细配置

```
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/js/index.js',
  output: {
    filename: 'js/[name].js',
    path: resolve(__dirname, 'build')
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  },
  plugins: [new HtmlWebpackPlugin()],
  mode: 'development',
  // 解析模块的规则
  resolve: {
    // 配置解析模块路径别名: 优点简写路径 缺点路径没有提示
    alias: {
      $css: resolve(__dirname, 'src/css')
    },
    // 配置省略文件路径的后缀名
    extensions: ['.js', '.json', '.jsx', '.css'],
    // 告诉 webpack 解析模块是去找哪个目录
    modules: [resolve(__dirname, '../../node_modules'), 'node_modules']
  }
};
```

## devServer详细配置

```
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/js/index.js',
  output: {
    filename: 'js/[name].js',
    path: resolve(__dirname, 'build')
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  },
  plugins: [new HtmlWebpackPlugin()],
  mode: 'development',
  resolve: {
    alias: {
      $css: resolve(__dirname, 'src/css')
    },
    extensions: ['.js', '.json', '.jsx', '.css'],
    modules: [resolve(__dirname, '../../node_modules'), 'node_modules']
  },
  devServer: {
    // 运行代码的目录
    contentBase: resolve(__dirname, 'build'),
    // 监视 contentBase 目录下的所有文件，一旦文件变化就会 reload
    watchContentBase: true,
    watchOptions: {
      // 忽略文件
      ignored: /node_modules/
    },
    // 启动gzip压缩
    compress: true,
    // 端口号
    port: 5000,
    // 域名
    host: 'localhost',
    // 自动打开浏览器
    open: true,
    // 开启HMR功能
    hot: true,
    // 不要显示启动服务器日志信息
    clientLogLevel: 'none',
    // 除了一些基本启动信息以外，其他内容都不要显示
    quiet: true,
    // 如果出错了，不要全屏提示~
    overlay: false,
    // 服务器代理 --> 解决开发环境跨域问题
    proxy: {
      // 一旦devServer(5000)服务器接受到 /api/xxx 的请求，就会把请求转发到另外一个服务器(3000)
      '/api': {
        target: 'http://localhost:3000',
        // 发送请求时，请求路径重写：将 /api/xxx --> /xxx （去掉/api）
        pathRewrite: {
          '^/api': ''
        }
      }
    }
  }
};

```

## optimization详细配置

```
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const TerserWebpackPlugin = require('terser-webpack-plugin')

module.exports = {
  entry: './src/js/index.js',
  output: {
    filename: 'js/[name].[contenthash:10].js',
    path: resolve(__dirname, 'build'),
    chunkFilename: 'js/[name].[contenthash:10]_chunk.js'
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }
    ]
  },
  plugins: [new HtmlWebpackPlugin()],
  mode: 'production',
  resolve: {
    alias: {
      $css: resolve(__dirname, 'src/css')
    },
    extensions: ['.js', '.json', '.jsx', '.css'],
    modules: [resolve(__dirname, '../../node_modules'), 'node_modules']
  },
  optimization: {
    splitChunks: {
      chunks: 'all'
      // 默认值，可以不写~
      /* minSize: 30 * 1024, // 分割的chunk最小为30kb
      maxSiza: 0, // 最大没有限制
      minChunks: 1, // 要提取的chunk最少被引用1次
      maxAsyncRequests: 5, // 按需加载时并行加载的文件的最大数量
      maxInitialRequests: 3, // 入口js文件最大并行请求数量
      automaticNameDelimiter: '~', // 名称连接符
      name: true, // 可以使用命名规则
      cacheGroups: {
        // 分割chunk的组
        // node_modules文件会被打包到 vendors 组的chunk中。--> vendors~xxx.js
        // 满足上面的公共规则，如：大小超过30kb，至少被引用一次。
        vendors: {
          test: /[\\/]node_modules[\\/]/,
          // 优先级
          priority: -10
        },
        default: {
          // 要提取的chunk最少被引用2次
          minChunks: 2,
          // 优先级
          priority: -20,
          // 如果当前要打包的模块，和之前已经被提取的模块是同一个，就会复用，而不是重新打包模块
          reuseExistingChunk: true
        } 
      }*/
    },
    // 将当前模块的记录其他模块的hash单独打包为一个文件 runtime
    // 解决：修改a文件导致b文件的contenthash变化
    runtimeChunk: {
      name: entrypoint => `runtime-${entrypoint.name}`
    },
    minimizer: [
      // 配置生产环境的压缩方案：js和css
      new TerserWebpackPlugin({
        // 开启缓存
        cache: true,
        // 开启多进程打包
        parallel: true,
        // 启动source-map
        sourceMap: true
      })
    ]
  }
};

```