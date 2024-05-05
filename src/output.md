# Rustlings Notebook

---

## <span style="color: #8F2C1D;">告知</span>

感谢您访问和使用本资源。这里分享的是我个人的学习记录，包括我在学习过程中遇到的问题及我所采用的解答方式。这些内容是参考费曼学习法的模式，通过记录和文档化我的学习过程形成的笔记和教程。请注意，这些材料是为了辅助理解和掌握知识点，它们并非官方认可的答案或教学资源，并且可能包含错误。鼓励您在使用这些资源时保持批判性思维，自行解决问题并形成独立的见解。如果您正在进行相关课程学习，请尽量先独立完成作业或任务。使用本资源默认不会破坏您的独立学习过程。同时，为尊重教育者的意愿并减少潜在冲突，请避免在相关课程社区或团体中传播这些内容。

- 独立思考和实践至关重要。我们强烈建议不直接抄袭答案，以便深入理解 Rust 的核心概念和实践应用。

> 删掉了原版介绍。

<br/>

---

内容相对基础主要面向初学者。对于希望进一步扩展知识和技能的用户，我们<span style="color: #8F2C1D;">**建议学习以下更全面的资源**</span>：

1. [**Rust 程序设计语言（The Rust Programming Language）**](https://doc.rust-lang.org/book/) - 官方书籍，系统全面地介绍了 Rust 的基础和高级特性。适合作为学习 Rust 的主要教材，无论是新手还是有经验的开发者都能从中获益。

2. [**通过例子学 Rust（Rust by Example）**](https://doc.rust-lang.org/rust-by-example/) - 提供大量实际代码示例，直观展示 Rust 的语法和功能。这个网站非常适合通过实例学习和快速理解 Rust 的具体应用。

3. [**Rust 标准库文档（Rust std）**](https://doc.rust-lang.org/std/) - 深入解析 Rust 标准库中的各种函数和模块，是进行高效编程和深入理解 Rust 不可或缺的资源。

4. [**Rust 圣经**](https://course.rs/) - 一本全面的中文 Rust 学习资源，涵盖从入门到高级的内容，非常适合中文用户系统学习 Rust。

5. [**Rust死灵书（The Rustonomicon）**](https://doc.rust-lang.org/nomicon/) - 深入探讨 Rust 中使用不安全代码的高级主题，如生命周期、并发性和内存布局等，适合已有 Rust 基础并希望挑战更深层次理解的开发者。

6. [**Rust 参考手册（The Rust Reference）**](https://doc.rust-lang.org/reference/) - 提供 Rust 语言的详细规范描述，包括语法、语义和其他高级语言特性，是编写准确和高效 Rust 代码的权威指南。

7. [**Rust 版本指南（The Rust Edition Guide）**](https://doc.rust-lang.org/edition-guide/) - 介绍 Rust 语言的版本更新和迁移策略，帮助开发者理解每个版本的新特性和改进，确保代码库的现代化和高效性。

8. [**Exercism**](https://exercism.io/tracks/rust) - 提供一个免费且互动的学习平台，您可以在这里完成各种 Rust 练习并获得社区导师的反馈和指导。

9. [**Rust Cookbook**](https://rust-lang-nursery.github.io/rust-cookbook/) - 这本在线书籍集中介绍如何利用 Rust 标准库来解决常见的编程任务，非常适合寻找具体编程问题解决方案的开发者。

10. [**Rust 宏小册（The Little Book of Rust Macros）**](https://danielkeep.github.io/tlborm/book/) - 专门介绍 Rust 宏编程的深入指南，帮助开发者理解和应用 Rust 强大的元编程能力。

11. [**Rust 和 WebAssembly**](https://rustwasm.github.io/docs/book/) - 官方书籍，详细讲解如何将 Rust 代码与 WebAssembly 结合使用，适合希望在 Web 浏览器中运行 Rust 应用的开发者。

12. [**嵌入式 Rust 之书（The Embedded Rust Book）**](https://docs.rust-embedded.org/book/) - 针对希望在嵌入式系统中使用 Rust 的开发者，这本书详细介绍了在无操作系统环境下使用 Rust 的技术和策略。

13. [**Hello，算法**](https://www.hello-algo.com/chapter_hello_algo/) -
本书旨在通过清晰易懂的动画图解和可运行的代码示例，使读者理解算法和数据结构的核心概念，并能够通过编程来实现它们。

> <span style="color: red;">注意！</span>解答或补充内容可能涉及后续题目内容和答案，酌情选择学习。

> <span style="color: red;">注意！</span>得益于 mdbook 与 Rust Playground 的集成，您可以直接在文档中完成并运行练习。但 Playground 环境与 Rustlings 的运行环境可能不同，某些题目的运行结果可能达不到预期效果。

> <span style="color: red;">注意！</span>ChatGPT 也可能会犯错。请考虑检查重要信息。本系列内容仅供参考理解使用，更加具体的学习请参考权威资料。

<br/>

---

<br/>

更多内容欢迎访问：

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

【4/19】 reviewed and fixed, release version 1

【4/18】 add 111, review and typesetting

【4/17】 add 89-110 finish and "start"

【4/16】 add 72-88

【4/12】 add 59-71 and history

【4/11】 add 49-58 and components

【4/10】 add 23-48

【4/09】 add 1-22 and story

【2024/3/23】 build framework
