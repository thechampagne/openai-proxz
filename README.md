![Static Badge](https://img.shields.io/badge/zig-0.13.0-%23F7A41D?logo=zig&logoColor=%23F7A41D)
![Static Badge](https://img.shields.io/badge/License-MIT-blue)

# ProxZ

A well documented OpenAI API library for the Zig programming language!

An easy to use interface familiar to those that have used the `openai-python` package.

## Features

- An easy to use interface familiar to those that have used the `openai-python` package.
- Built-in retry logic
- Environment variable support for API keys, Org. IDs, and Project IDs
- Integration with the most popular OpenAI endpoints

## Installation

To install `proxz`, run

```bash
 zig fetch --save "git+https://github.com/lukeharwood11/openai-proxz#3fd51f2247929c4d161e5a1ed53f2b8aef104261"
```

And add the following to your `build.zig`

```zig
const proxz = b.dependency("proxz", .{
    .target = target,
    .optimize = optimize,
});

exe.root_module.addImport("proxz", proxz.module("proxz"));
```

## Usage

### Client Configuration

```zig
const proxz = @import("proxz");
const OpenAI = proxz.OpenAI;
```

```zig
// make sure you have an OPENAI_API_KEY environment variable set!
var openai = try OpenAI.init(allocator, .{});
defer openai.deinit();
```

### Chat Completions

```zig
const ChatMessage = proxz.ChatMessage;

var response = try openai.chat.completions.create(.{
    .model = "gpt-4o",
    .messages = &[_]ChatMessage{
        .{
            .role = "user",
            .content = "Hello, world!",
        },
    },
});
// This will free all the memory allocated for the response
defer response.deinit();
const completions = response.data;
std.log.debug("{s}", .{completions.choices[0].message.content});
```

## Contributions

Contributions are welcome and encouraged! Submit an issue for any bugs/feature requests and open a PR if you tackled one of them!

## Building Docs

```bash
zig build-lib -femit-docs proxz/proxz.zig
```
