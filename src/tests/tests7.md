# Exercise 98

- Name: ```tests7```
- Path: ```exercises/tests/tests7.rs```
#### Hint: 

The command to set up an environment variable is "rustc-env=VAR=VALUE".


---



```rust,editable
// tests7.rs
//
// When building packages, some dependencies can neither be imported in
// `Cargo.toml` nor be directly linked; some preprocesses varies from code
// generation to set-up package-specific configurations.
//
// Cargo does not aim to replace other build tools, but it does integrate
// with them with custom build scripts called `build.rs`. This file is
// usually placed in the root of the project, while in this case the same
// directory of this exercise.
//
// It can be used to:
//
// - Building a bundled C library.
// - Finding a C library on the host system.
// - Generating a Rust module from a specification.
// - Performing any platform-specific configuration needed for the crate.
//
// When setting up configurations, we can `println!` in the build script
// to tell Cargo to follow some instructions. The generic format is:
//
//     println!("cargo:{}", your_command_in_string);
//
// Please see the official Cargo book about build scripts for more
// information:
// https://doc.rust-lang.org/cargo/reference/build-scripts.html
//
// In this exercise, we look for an environment variable and expect it to
// fall in a range. You can look into the testcase to find out the details.
//
// You should NOT modify this file. Modify `build.rs` in the same directory
// to pass this exercise.
//
// Execute `rustlings hint tests7` or use the `hint` watch subcommand for a
// hint.

// I AM NOT DONE

fn main() {}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_success() {
        let timestamp = std::time::SystemTime::now()
            .duration_since(std::time::UNIX_EPOCH)
            .unwrap()
            .as_secs();
        let s = std::env::var("TEST_FOO").unwrap();
        let e: u64 = s.parse().unwrap();
        assert!(timestamp >= e && timestamp < e + 10);
    }
}
```

```rust,editable
//! This is the build script for both tests7 and tests8.
//!
//! You should modify this file to make both exercises pass.

fn main() {
    // In tests7, we should set up an environment variable
    // called `TEST_FOO`. Print in the standard output to let
    // Cargo do it.
    let timestamp = std::time::SystemTime::now()
        .duration_since(std::time::UNIX_EPOCH)
        .unwrap()
        .as_secs(); // What's the use of this timestamp here?
    let your_command = format!(
        "Your command here with {}, please checkout exercises/tests/build.rs",
        timestamp
    );
    println!("cargo:{}", your_command);

    // In tests8, we should enable "pass" feature to make the
    // testcase return early. Fill in the command to tell
    // Cargo about that.
    let your_command = "Your command here, please checkout exercises/tests/build.rs";
    println!("cargo:{}", your_command);
}
```

---

### 本题内容：

本练习目的是让学生熟悉和实践使用 Cargo 的自定义构建脚本（`build.rs`）来设置和管理环境变量。此功能特别适用于需要预处理步骤的项目，例如代码生成、查找系统库、编译捆绑的 C 库或执行平台特定的配置。

### 相关知识点：

1. **Cargo构建脚本（`build.rs`）**：
   - `build.rs` 是一个特殊的脚本，可以在构建过程中执行，并允许开发者编写 Rust 代码来完成构建任务。
   - 这个脚本在编译其他源文件之前运行，可以用来生成代码、编译静态文件、链接到系统库等。

2. **设置环境变量**：
   - 在 `build.rs` 中，可以使用 `println!("cargo:rustc-env=VAR=VALUE");` 来设置编译时环境变量。
   - 这些环境变量可以在程序的运行时通过 `std::env::var` 访问。

3. **环境变量的检查**：
   - 在测试中，可以通过环境变量来传递参数或配置，测试脚本将验证这些变量是否设置正确。

### 解题方法：

1. **创建或修改 `build.rs`**：
   - 在项目根目录下创建或修改 `build.rs` 文件。
   - 使用 Rust 代码来设置必要的环境变量。

2. **实现环境变量的设置**：
   - 在 `build.rs` 中，使用 `println!` 宏输出特定格式的字符串来指示 Cargo 设置环境变量。

3. **测试验证**：
   - 运行测试以确保环境变量正确设置并被程序适当使用。

### 代码示例：

```rust
// build.rs
fn main() {
    // 获取当前 UNIX 时间戳
    let timestamp = std::time::SystemTime::now()
        .duration_since(std::time::UNIX_EPOCH)
        .expect("Time went backwards")
        .as_secs();

    // 设置环境变量`TEST_FOO`，值为当前时间戳
    // println!("cargo:rustc-env=TEST_FOO={}", timestamp);
    let command_for_tests7 = format!("rustc-env=TEST_FOO={}", timestamp);
    println!("cargo:{}", command_for_tests7);
}
```

这段代码会在编译时获取当前时间戳，并将其设置为环境变量`TEST_FOO`。在测试脚本中，这个变量被检索并用来验证程序是否在接受的时间范围内运行，从而确保环境变量的正确设置和使用。


---

## 扩展知识点与解答：

### 扩展知识点：

1. **Cargo构建脚本的进阶使用**：
   - **代码生成**：利用构建脚本来自动生成 Rust 模块或配置文件，例如根据模板或外部数据生成 Rust 代码。
   - **外部依赖处理**：构建脚本可以用来检测系统上的库或工具的存在，配置如何链接它们，或者甚至自动下载和构建那些不在 Cargo 仓库中的依赖。

2. **环境变量的动态管理**：
   - **条件编译**：使用环境变量作为条件编译的标志，根据不同的环境变量值来决定编译不同的代码块。
   - **配置管理**：在构建阶段将配置写入环境变量，使得应用启动后能够根据编译时的设置更加灵活地运行。

3. **安全性考量**：
   - 在使用外部数据生成代码或配置时，确保检查和验证这些数据的安全性和正确性，避免注入攻击或数据破坏。

### 扩展解题方法：

1. **高级环境变量设置**：
   - 使用环境变量来控制功能的开关或行为的修改，比如日志级别、功能特性开关等。
   - 示例代码：
     ```rust
     println!("cargo:rustc-env=LOG_LEVEL=debug");
     ```

2. **集成外部工具**：
   - 构建脚本可以调用外部命令行工具来进行复杂的预处理任务，如调用脚本语言处理文本数据或调用系统命令进行文件操作。
   - 示例代码：
     ```rust
     use std::process::Command;
     
     fn main() {
         let output = Command::new("echo")
             .arg("Hello from build script!")
             .output()
             .expect("Failed to execute command");
     
         println!("cargo:rustc-env=BUILD_OUTPUT={}", String::from_utf8_lossy(&output.stdout));
     }
     ```

3. **生成代码的实践**：
   - 使用模板引擎如 `handlebars` 或 `tera` 在构建时从模板生成 Rust 代码或配置文件。
   - 示例代码：
     ```rust
     // 假设使用 handlebars 模板
     let template = "pub const MESSAGE: &str = \"{{message}}\";";
     let mut handlebars = handlebars::Handlebars::new();
     handlebars.register_template_string("template", template).unwrap();
     
     let data = serde_json::json!({ "message": "Hello, world!" });
     let rendered = handlebars.render("template", &data).unwrap();
     
     std::fs::write("src/generated.rs", rendered).expect("Unable to write file");
     ```

## 关于 `build.rs` 

在 Rust 项目中，`build.rs` 是一个特殊的脚本，通常用于在编译时执行自定义的构建任务。这个脚本有许多用途，从处理复杂的编译时依赖到代码生成，再到调用外部构建系统。以下是一些深入了解 `build.rs` 及其应用的方面：

### 1. **基本概念和用途**
`build.rs` 脚本是一个独立的 Rust 程序，它在 Cargo 处理其他编译任务之前运行。这使得它非常适合进行预编译配置和资源生成，如：
- 生成编译前必需的代码或配置文件。
- 编译时计算某些值，然后将这些值传递给主构建过程。
- 自动绑定到 C 库，生成 Rust 可用的绑定。
- 动态决定哪些功能或代码模块将被包括在编译中。

### 2. **如何使用 `build.rs`**
要在项目中使用 `build.rs`，你需要在 Cargo.toml 文件中声明一个构建脚本路径：
```toml
[package]
# ...
build = "build.rs"
```
此文件应位于项目的根目录下（或指定的其他位置）。在 `build.rs` 中，你可以编写任意 Rust 代码，以及输出特定的 Cargo 指令以影响构建过程。

### 3. **与 Cargo 的交互**
`build.rs` 可以通过打印特定格式的字符串到标准输出与 Cargo 交互，以提供构建指令，例如：
- **添加环境变量**：
  ```rust
  println!("cargo:rustc-env=NAME=VALUE");
  ```
- **链接外部库**：
  ```rust
  println!("cargo:rustc-link-lib=static=foo");
  ```
- **向编译器传递标志**：
  ```rust
  println!("cargo:rustc-flags=-l dylib=chilkat");
  ```
- **重新运行构建脚本的触发条件**：
  ```rust
  println!("cargo:rerun-if-changed=path/to/file");
  ```

### 4. **使用外部工具和库**
`build.rs` 不仅限于 Rust 代码，你还可以调用外部工具生成文件或数据，然后将这些文件包含到你的包中。例如，你可以使用系统命令、外部程序或调用网络服务。

### 5. **错误处理**
在 `build.rs` 中的错误处理非常重要，因为任何错误都会停止构建过程。确保适当地处理可能的错误情况，并通过 `Result` 类型适当地传播错误。

### 6. **性能考量**
虽然 `build.rs` 强大，但它也可能延长构建时间，特别是当执行复杂操作或网络请求时。适当管理其复杂性和运行时间，避免不必要的构建延迟。

通过深入了解和合理利用 `build.rs`，你可以显著增强 Rust 项目的灵活性和功能性。
