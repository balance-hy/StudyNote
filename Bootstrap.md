# Bootstrap

## Containers 容器

容器是Bootstrap中最基本的布局元素，在使用我们的默认网格系统时是必需的。容器用于包含、填充和（有时）集中其中的内容。虽然容器可以嵌套，但大多数布局不需要嵌套容器。

Bootstrap有三个不同的容器：

- `.container`, which sets a `max-width` at each responsive breakpoint
- `.container-fluid`, which is `width: 100%` at all breakpoints
- `.container-{breakpoint}`, which is `width: 100%` until the specified breakpoint

**其实就是页面大小不同时，容器大小不一致，具体见下表**

|                    | Extra small <576px | Small ≥576px | Medium ≥768px | Large ≥992px | Extra large ≥1200px |
| ------------------ | ------------------ | ------------ | ------------- | ------------ | ------------------- |
| `.container`       | 100%               | 540px        | 720px         | 960px        | 1140px              |
| `.container-sm`    | 100%               | 540px        | 720px         | 960px        | 1140px              |
| `.container-md`    | 100%               | 100%         | 720px         | 960px        | 1140px              |
| `.container-lg`    | 100%               | 100%         | 100%          | 960px        | 1140px              |
| `.container-xl`    | 100%               | 100%         | 100%          | 100%         | 1140px              |
| `.container-fluid` | 100%               | 100%         | 100%          | 100%         | 100%                |

###  网格系统

Bootstrap的网格系统使用一系列容器、行和列来布局和对齐内容。它是使用flexbox构建的，并且是响应式。下面是一个示例，并深入了解网格是如何组合在一起的。

```html
<div class="container">
  <div class="row">
    <div class="col-sm">
      One of three columns
    </div>
    <div class="col-sm">
      One of three columns
    </div>
    <div class="col-sm">
      One of three columns
    </div>
  </div>
</div>
```

![boot1](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202309241228949.PNG)



上面的示例使用我们预定义的网格类可以在小型、中型、大型和特大型设备上创建三个等宽列。这些列位于父容器的页面中心

问题1：列间有间距吗  行如何控制列间距