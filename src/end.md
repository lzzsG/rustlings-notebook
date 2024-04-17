# end

After 110 challenges, you finally get the Ancient Artifact. After opening the treasure chest, you freeze because  inside is a piece of RUST and it's not even ancient！

早早构思好了结局哈哈哈😄

---

> ### 本题内容：
>
> （简要描述每个练习的目标和它希望学生学习的内容。这有助于学生理解练习的目的。注意：一些要求在注释中表明了。）
>
> ### 相关知识点：
>
> （提取列出本题涉及的知识点并简要讲解）
>
> ### 解题方法：
>
> - **步骤描述**：（简要描述解决练习的步骤。）
> - **代码示例**：（提供一到多个代码示例，展示如何实现这些解题步骤。务必使代码清晰、简洁，并包含必要的注释。）

---

> ### 故事设定：
>
> 在一个远古的魔法世界中，存在一个由古老力量守护的神秘森林。这个森林藏着许多秘密，包括隐藏的宝藏、强大的魔法、神奇的生物，以及那些被时间遗忘的古老遗迹。勇敢的冒险者们进入森林，希望发现这些秘密，但他们必须面对各种挑战和试炼。故事的走向依据冒险者的选择和遭遇的事件而变化。
>
> ### 角色：
>
> - **冒险者**：故事的主角，可以是魔法师、勇士、探险家等。
> - **神秘生物**：如精灵、龙、树妖等，根据故事的不同阶段提供帮助或阻碍。
> - **古老的守护者**：守护着森林秘密的古老存在。
>
> ### 故事模板：
>
> 1. **开始冒险**：冒险者进入森林，遭遇第一个选择或挑战。
>    - 例：遇到分叉路，选择左边还是右边？
> 2. **发现与遭遇**：根据选择，冒险者可能发现有用的物品、遭遇神秘生物或进入奇妙地点。
>    - 例：发现一本古老的魔法书，或遭遇一个愿意提供帮助的精灵。
> 3. **挑战与试炼**：冒险者需要通过一系列的挑战或试炼，这些可能包括解谜、战斗或道德选择。
>    - 例：解开古老遗迹的谜题，或与恶龙战斗。
> 4. **结局**：根据冒险者如何应对这些挑战和试炼，故事有多种可能的结局。
>    - 例：找到宝藏并成为英雄，或由于错误的选择而遗失在森林深处。

---

> <span style="color: red;">注意！</span>回答非标准答案，用于学习参考而不是考满分。

> <span style="color: red;">注意！</span>解答或补充内容可能涉及后续题目内容和答案，酌情选择阅读。

> <span style="color: red;">注意！</span>playground 环境不同于 rustlings 一些题目的运行会达不到测试效果

> <span style="color: red;">注意！</span>chatGPT 可能产生幻觉，请考虑核实重要信息。本系列内容仅供参考理解使用，更加具体的学习请参考权威资料。（要不跑跑试试？）

先放一下HTML

---
<span style="color: red;">注意！</span>

<!-- 卡片 -->

<div class="card">
    <h2>Card Title</h2>
    <p>This is some text within a card component.</p>
</div>
<div class="card card1">
    <h2>Card Title</h2>
    <p>This is some text within a card component.</p>
</div>

<!-- 位于卡片之下的独立内容 -->
<div class="content-below-card">
    <p>This content is outside the card, and thus not surrounded by the card's border.</p>
    <!-- 在这里可以放置更多的HTML或Markdown转换的HTML内容 -->
</div>
<style>
    .card  {
        border: 1px solid #ccc;
        padding: 20px;
        margin: 20px 0;
    }

    @media (prefers-color-scheme: dark) {
        .card {
            border-color: #555;
        }
    }
</style>

<button class="simple-button">点击我</button>
<button class="simple-button">👍<span class="like-counter">0/999</span></button>

<style>
.simple-button {
    border: 1px solid #d1d1d1;
    padding: 5px 10px;
    cursor: pointer;
    transition: background-color 0.3s;
 font-size: 16px;
}

.simple-button:hover {
    background-color: #999999;
}
</style>
<div class="alert alert-info"><svg t="1712852929473" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="2727" width="14" height="14"><path d="M512 1024c-281.6 0-512-230.4-512-512s230.4-512 512-512 512 230.4 512 512S793.6 1024 512 1024zM512 64C262.4 64 64 262.4 64 512s198.4 448 448 448 448-198.4 448-448S761.6 64 512 64z" fill="#ffffff" p-id="2728"></path><path d="M512 800c-19.2 0-32-12.8-32-32L480 422.4c0-19.2 12.8-32 32-32s32 12.8 32 32L544 768C544 787.2 531.2 800 512 800z" fill="#ffffff" p-id="2729"></path><path d="M512 288 512 288C492.8 288 480 275.2 480 256l0 0c0-19.2 12.8-32 32-32l0 0c19.2 0 32 12.8 32 32l0 0C544 275.2 531.2 288 512 288z" fill="#ffffff" p-id="2730"></path></svg> 这是一条信息。</div>
<div class="alert alert-error"><svg t="1712853078809" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="4111" id="mx_n_1712853078809" width="14" height="14"><path d="M512 992C246.912 992 32 777.088 32 512 32 246.912 246.912 32 512 32c265.088 0 480 214.912 480 480 0 265.088-214.912 480-480 480z m0-64c229.76 0 416-186.24 416-416S741.76 96 512 96 96 282.24 96 512s186.24 416 416 416z" fill="#ffffff" p-id="4112"></path><path d="M572.512 512l161.696 161.664-60.544 60.544L512 572.48l-161.664 161.696-60.544-60.544L451.52 512 288 348.512 348.512 288 512 451.488 675.488 288 736 348.512 572.512 512z" fill="#ffffff" p-id="4113"></path></svg> 这是一个错误。</div>
<div class="alert alert-warning"><svg t="1712853156615" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="6131" width="14" height="14"><path d="M484 428v224c0 15.5 12.5 28 28 28s28-12.5 28-28V428c0-15.5-12.5-28-28-28s-28 12.5-28 28z" p-id="6132" fill="#ffffff"></path><path d="M512 764m-28 0a28 28 0 1 0 56 0 28 28 0 1 0-56 0Z" p-id="6133" fill="#ffffff"></path><path d="M952.6 825.1l-393.2-681c-10.5-18.2-29-27.4-47.4-27.4s-36.9 9.1-47.4 27.4l-393.2 681c-21.1 36.5 5.3 82.1 47.4 82.1h786.4c42.1 0 68.5-45.6 47.4-82.1z m-832.7 28L512 174l393.2 677.2-785.3 1.9z" p-id="6134" fill="#ffffff"></path></svg> 这是一条警告。</div>
<div class="alert alert-success"><svg t="1712853177040" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="7179" width="14" height="14"><path d="M512 1024C229.248 1024 0 794.752 0 512S229.248 0 512 0s512 229.248 512 512-229.248 512-512 512z m0-938.666667C276.352 85.333333 85.333333 276.352 85.333333 512s191.018667 426.666667 426.666667 426.666667 426.666667-191.018667 426.666667-426.666667S747.648 85.333333 512 85.333333z m-10.368 625.365334a42.624 42.624 0 0 1-60.330667 0 41.130667 41.130667 0 0 1-7.381333-11.136L275.413333 536.32a42.666667 42.666667 0 1 1 61.056-59.605333l137.216 141.269333 262.016-262.016a42.666667 42.666667 0 0 1 60.330667 60.330667l-294.4 294.4z" fill="#ffffff" p-id="7180"></path></svg> 这是一个成功。</div>

<style>
.alert {
    padding: 15px 20px;
    margin: 10px 0;
    color: #fff;
}

.alert-info {
    background-color: #305D8D;
}

.alert-error {
    background-color: #8D3D30;
}

.alert-warning {
    background-color: #8D7F30;

}

.alert-success {
    background-color: #4B903D;
}
</style>

<ul class="custom-list">
    <li class="completed">已完成的任务</li>
    <li class="pending">未完成的任务</li>
    <li class="completed">已完成的任务</li>
    <li class="pending">未完成的任务</li>
</ul>

<style>
.custom-list li {
    margin-bottom: 10px;
    list-style-type: none; /* 移除默认的列表项目符号 */
    padding-left: 20px;
    position: relative;
}

.custom-list .completed:before {
    content: "✔"; /* 已完成的任务前使用对号 */
    position: absolute;
    left: 0;
    color: #4CAF50; /* 自定义颜色 */
}

.custom-list .pending:before {
    content: "✘"; /* 未完成的任务前使用叉号 */
    position: absolute;
    left: 0;
    color: #FF5722; /* 自定义颜色 */
}
</style>

<div class="accordion">
    <div class="accordion-item">
        <button class="simple-button accordion-button">折叠面板 1</button>
        <div class="accordion-content">
            <p>这是折叠面板 1 的内容。</p>
        </div>
    </div>
    <!-- 添加更多折叠面板项 -->
</div>

<style>

.accordion-content {
    display: none;
    padding: 20px;
    border: 1px solid #ccc;
}

.accordion-content p {
    margin: 0;
}
</style>

<script>
document.querySelectorAll('.accordion-button').forEach(button => {
    button.addEventListener('click', () => {
        const accordionContent = button.nextElementSibling;
        accordionContent.style.display = accordionContent.style.display === 'block' ? 'none' : 'block';
    });
});
</script>
<span class="tag">标签一</span>
<span class="tag tag-primary">标签二</span>

<style>
.tag {
    display: inline-block;
    background-color: #e0e0e0;
    color: #333;
    padding: 0px 5px;
    font-size: 12px;
}

.tag-primary {
    background-color: #222222;
    color: #ffffff;
}
</style>

<style>
    .iframe-wrapper {
        position: relative;
        width: 100%;
        height: 600px;
    }

    iframe {
        width: 100%;
        height: 100%;
        border: 3px solid #333;
        border-radius: 20px;
    }
    
    .iframe-overlay-quarter-t {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 40%;
        /* 只覆盖iframe的1/4高度 */
        background-color: rgba(0, 0, 0, 0.05);
        /* 灰度背景 */
        color: white;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 20px;
        z-index: 15;
        /* 高于iframe内容的层级 */
}
</style>

<div class="iframe-wrapper">
    <iframe src="https://lzzs.fun/" frameborder="0" allowfullscreen></iframe>
    <div class="iframe-overlay-quarter-t">
        <a href="https://lzzs.fun" target="_blank">点击此处打开</a>
    </div> <!-- 新的遮罩层放在iframe底部1/4区域 -->
</div>

</div>

---

# 更新记录







【4/12】 add 59-71 and history

【4/11】 add 49-58 and components

【4/10】 add 23-48

【4/09】 add 1-22 and story

【2024/3/23】 完成框架
