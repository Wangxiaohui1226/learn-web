vite和webpack打包性能优化

vite

使用代码分割可以减少初始加载时间，通过将代码分割成多个小的块来按需加载。Vite 默认支持基于路由的代码分割，但你也可以手动进行代码分割：

```
javascriptCopy Code// 使用动态导入进行代码分割
import('./largeModule').then(module => {
  // 使用模块
});
```

### 3. 预构建依赖

使用 `esbuild` 对依赖进行预构建可以显著提高首次加载速度。这可以通过配置 Vite 来实现：

```
javascriptCopy Code// vite.config.js
export default defineConfig({
  optimizeDeps: {
    entries: ['src/main.js'], // 指定入口文件，可以根据需要调整
    include: ['some-large-lib'], // 指定需要预构建的依赖包
    exclude: ['some-excluded-lib'] // 排除不需要预构建的依赖包
  }
});
```

### 4. 使用缓存

确保你的构建过程中利用了缓存。Vite 利用了浏览器的缓存机制，但你也可以通过配置来进一步优化：

```
javascriptCopy Code// vite.config.js
export default defineConfig({
  server: {
    fs: {
      strict: true // 使用严格模式，启用缓存控制头部，例如 Cache-Control 和 Last-Modified。
    }
  }
});
```

### 5. 减少插件的使用和配置开销

尽量减少不必要的插件使用，并确保每个插件都进行了适当的配置。过多的插件和复杂配置可能会增加构建时间。例如，只使用必要的插件如 `vite-plugin-eslint`、`vite-plugin-vue` 等。

### 6. 分析和优化包大小

使用工具如 `rollup-plugin-visualizer` 来分析你的包大小，找出哪些模块过大，并尝试优化这些模块。例如，移除未使用的代码、使用更小的库版本等。

```
javascriptCopy Code// vite.config.js
import visualizer from 'rollup-plugin-visualizer';
import { defineConfig } from 'vite';

export default defineConfig({
  plugins: [visualizer({ open: true, filename: './dist/stats.html' })] // 分析包大小并生成报告
});
```



webpack

### 1. 使用`production`模式

确保在生产环境下使用`production`模式。这会自动启用许多优化，如代码压缩和分割。

```
javascriptCopy Code// webpack.config.js
module.exports = {
  mode: 'production',
};
```

### 2. 代码拆分（Code Splitting）

使用Webpack的代码拆分功能，可以将代码分割成多个包，按需加载。这可以显著减少初始加载时间。

```
javascriptCopy Code// 使用动态import语法
import(/* webpackChunkName: "moduleA" */ './moduleA').then((moduleA) => {
  // 使用模块A
});
```

### 3. Tree Shaking

确保启用了Tree Shaking，它可以帮助移除JavaScript中未引用的代码。

```
javascriptCopy Code// webpack.config.js
module.exports = {
  mode: 'production',
  optimization: {
    usedExports: true,
  },
};
```

### 4. 缓存利用（Caching）

使用缓存来加快构建速度。可以配置`cache-loader`或`babel-loader`的缓存选项。

```
javascriptCopy Code// webpack.config.js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        use: ['cache-loader', 'babel-loader'],
        include: path.resolve('src'), // 只对src目录下的文件进行缓存
      },
    ],
  },
};
```

### 5. 缩小搜索范围（Limiting the Scope of Plugins and Loaders）

通过在`module.rules`中指定`include`和`exclude`，可以限制Webpack处理的文件范围，只对必要的文件应用loader。

```
javascriptCopy Code// webpack.config.js
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        use: 'babel-loader',
        include: path.resolve('src'), // 只处理src目录下的js文件
        exclude: /node_modules/, // 排除node_modules目录
      },
    ],
  },
};
```