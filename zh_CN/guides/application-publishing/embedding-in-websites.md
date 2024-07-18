# 嵌入网站

Dify 应用可以使用 iframe 嵌入到网站中。这允许你将 Dify 应用集成到你的网站、博客或任何其他网页中。

当你在网站中使用 Dify 聊天机器人气泡按钮时，你可以自定义按钮的样式、位置和其他设置。

## 自定义 Dify 聊天机器人气泡按钮

Dify 聊天机器人气泡按钮可以通过以下配置选项进行自定义：

```javascript
window.difyChatbotConfig = {
    // 必填项，由 Dify 自动生成
    token: 'YOUR_TOKEN',
    // 可选项，默认为 false
    isDev: false,
    // 可选项，当 isDev 为 true 时，默认为 '[https://dev.udify.app](https://dev.udify.app)'，否则默认为 '[https://udify.app](https://udify.app)'
    baseUrl: 'YOUR_BASE_URL',
    // 可选项，可以接受除 `id` 以外的任何有效的 HTMLElement 属性，例如 `style`、`className` 等
    containerProps: {},
    // 可选项，是否允许拖动按钮，默认为 `false`
    draggable: false,
    // 可选项，允许拖动按钮的轴，默认为 `both`，可以是 `x`、`y`、`both`
    dragAxis: 'both',
    // 可选项，在 dify 聊天机器人中设置的输入对象
    inputs: {
        // 键是变量名
        // 例如：
        // name: "NAME"
    }

};
```

## 覆盖默认按钮样式

你可以使用 CSS 变量或 `containerProps` 选项来覆盖默认按钮样式。根据 CSS 优先级使用这些方法实现自定义样式。

### 1.修改 CSS 变量

支持以下 CSS 变量进行自定义：

```css
/* 按钮距离底部的距离，默认为 `1rem` */
--dify-chatbot-bubble-button-bottom

/* 按钮距离右侧的距离，默认为 `1rem` */
--dify-chatbot-bubble-button-right

/* 按钮距离左侧的距离，默认为 `unset` */
--dify-chatbot-bubble-button-left

/* 按钮距离顶部的距离，默认为 `unset` */
--dify-chatbot-bubble-button-top

/* 按钮背景颜色，默认为 `#155EEF` */
--dify-chatbot-bubble-button-bg-color

/* 按钮宽度，默认为 `50px` */
--dify-chatbot-bubble-button-width

/* 按钮高度，默认为 `50px` */
--dify-chatbot-bubble-button-height

/* 按钮边框半径，默认为 `25px` */
--dify-chatbot-bubble-button-border-radius

/* 按钮盒阴影，默认为 `rgba(0, 0, 0, 0.2) 0px 4px 8px 0px)` */
--dify-chatbot-bubble-button-box-shadow

/* 按钮悬停变换，默认为 `scale(1.1)` */
--dify-chatbot-bubble-button-hover-transform
```

例如，要将按钮背景颜色更改为 #ABCDEF，请添加以下 CSS：

```css
#dify-chatbot-bubble-button {
    --dify-chatbot-bubble-button-bg-color: #ABCDEF;
}
```

### 2.使用 `containerProps` 选项

使用 `style` 属性设置内联样式：

```javascript
window.difyChatbotConfig = {
    // ... 其他配置
    containerProps: {
        style: {
            backgroundColor: '#ABCDEF',
            width: '60px',
            height: '60px',
            borderRadius: '30px',
        },
        // 对于较小的样式覆盖，也可以使用字符串作为 `style` 属性的值：
        // style: 'background-color: #ABCDEF; width: 60px;',
    },
}
```

使用 `className` 属性应用 CSS 类：

```javascript
window.difyChatbotConfig = {
    // ... 其他配置
    containerProps: {
        className: 'dify-chatbot-bubble-button-custom my-custom-class',
    },
};
```

### 3. 传递 `inputs`

支持四种类型的输入：

1. **`text-input`**：接受任何值。如果输入字符串的长度超过允许的最大长度，将被截断。
2. **`paragraph`**：类似于 `text-input`，接受任何值并在字符串长度超过最大长度时截断。
3. **`number`**：接受数字或数字字符串。如果提供的是字符串，将使用 `Number` 函数将其转换为数字。
4. **`options`**：接受任何值，前提是它匹配预先配置的选项之一。

示例配置：

```javascript
window.difyChatbotConfig = {
    // ... 其他配置
    inputs: {
        name: 'apple',
    },
}
```

注意：使用 embed.js 脚本创建 iframe 时，每个输入值将被处理——使用 GZIP 压缩并以 base64 编码——然后附加到 URL 上。

例如，处理后的输入值 URL 将如下所示：
`http://localhost/chatbot/{token}?name=H4sIAKUlmWYA%2FwWAIQ0AAACDsl7gLuiv2PQEUNAuqQUAAAA%3D`