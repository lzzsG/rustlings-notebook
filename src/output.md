#  Rustlings Notebook

&#8195;&#8195;欢迎来到 Rustlings 系列笔记！这个系列旨在通过一系列练习来帮助您熟悉 Rust 语言。从基本的变量、函数、控制结构到更高级的概念如泛型、特征和生命周期管理，涵盖了常见的编程概念和高级主题，如模块、向量、字符串处理和错误处理，智能指针、多线程处理、宏和更多，旨在全面提升您的系统编程能力，每个模块都设计有针对性的练习。

&#8195;&#8195;特别值得一提的是，本笔记还包括了对 Rust 独有特性的深入探讨（<span style="color: #8F2C1D;">**基于 Rustlings 扩展的110题的版本**</span>），如所有权和借用规则、模式匹配以及对不可变性的严格处理。通过实际代码操作，例如在练习中使用 `unsafe` 关键字操作裸指针、环境变量的应用、条件编译以及与其他语言的接口（FFI），您将学习如何在 Rust 中进行更为复杂和安全的系统级编程。最后的一系列算法练习，涉及到从基本数据结构的操作到复杂算法的实现。每个练习都设计来帮助学生掌握关键的计算机科学概念，提升解决实际问题的能力。

&#8195;&#8195;注意！本 Rustlings 系列笔记是<span style="color: #8F2C1D;">**由 ChatGPT 提供的回答整理而成**</span>。本站作者仅仅是基于 Rustlings 的原版练习及其扩展的包含110题的版本来编排和整理这些内容。旨在为学习 Rust 语言的编程爱好者提供实用的参考指南和解答，本站作者一定程度上也是基于这些由 ChatGPT 提供的材料进行 Rustlings 学习的。作为一个基于人工智能的工具，ChatGPT 尝试以准确和实用的方式解答各种编程问题，并提供代码示例，但它的回答可能存在解答不够完备或细节处理不够精准的情况，并<span style="color: #8F2C1D;">**不一定是标准答案**</span>。因此，用户在学习过程中需结合其他优质权威的学习资源和官方文档，以获得更全面和深入的理解。本系列只做参考理解。

&#8195;&#8195;如果您在使用本系列笔记中遇到任何问题或错误，或有任何改进补充的建议，请在 GitHub 的 issue 区域提出反馈。非常欢迎您的建议和反馈，这不仅可以帮助改进笔记内容，也能增进使用本笔记的学习体验。任何建议和改进意见都将被认真考虑，以提升本笔记的质量和效果。再次感谢您的支持与理解！

&#8195;&#8195;我们还需要提醒所有用户，学习编程的过程中最重要的是<span style="color: #8F2C1D;">**理解和实践**</span>，而不仅仅是找到答案。强烈建议不要直接抄袭答案，因为这样做不利于深入理解 Rust 编程语言的核心概念和实践应用。建议您在尝试解决每个练习时，保证一定的独立思考和编码。

&#8195;&#8195;另外，我们的教程内容相对基础主要面向初学者。对于希望进一步扩展知识和技能的用户，我们建议<span style="color: #8F2C1D;">**学习以下更全面的资源**</span>：

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

> <span style="color: red;">注意！</span>解答或补充内容可能涉及后续题目内容和答案，酌情选择学习。

> <span style="color: red;">注意！</span>得益于 mdbook 与 Rust Playground 的集成，您可以直接在文档中完成并运行练习。但 Playground 环境与 Rustlings 的运行环境可能不同，某些题目的运行结果可能达不到预期效果。

> <span style="color: red;">注意！</span>ChatGPT 也可能会犯错。请考虑检查重要信息。本系列内容仅供参考理解使用，更加具体的学习请参考权威资料。

<br/>

---

<br/>

更多内容欢迎访问我的网站

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

【4/18】 add 111, review and typesetting

【4/17】 add 89-110 finish and "start"

【4/16】 add 72-88

【4/12】 add 59-71 and history

【4/11】 add 49-58 and components

【4/10】 add 23-48

【4/09】 add 1-22 and story

【2024/3/23】 build framework
