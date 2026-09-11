---
title: "I built my own app debugger because I got tired of not knowing what my app was doing"
date: "2025-02-14"
tags: ["Tools", "Rust", "Debugging", "Systems"]
author: "Janarthanan S"
---

Every debugger I've used has the same problem: it tells you what the developer of the debugger thought you'd want to know. Not what you actually want to know. I got tired of it and built my own.

## The frustration

I was debugging a race condition in a multi-threaded Rust service. The standard tools — `gdb`, `lldb`, `tokio-console` — gave me pieces of the picture. None of them gave me the complete picture: which thread touched which memory, in what order, with what stack trace at each point. I wanted a tool that would let me ask arbitrary questions about runtime behaviour.

## What I built

A Rust CLI tool that attaches to a running process (Linux only, for now) using `ptrace` and reads DWARF debug information from the ELF binary. It can:

- Trace every function call and return, with arguments and return values
- Log memory reads and writes to specified address ranges
- Record thread interleaving at the instruction level
- Produce a timeline view of concurrent execution that you can replay

```bash
$ blt-debug --pid 1234 --trace-fn "my_crate::*" --watch-mem 0x7fff...
[0.000ms] Thread 1: my_crate::handler called (req_id=42)
[0.012ms] Thread 2: my_crate::db_write called (key="user:99")
[0.013ms] Thread 1: READ 0x7fff1234 → 0 (stale)
[0.014ms] Thread 2: WRITE 0x7fff1234 ← 1
[0.015ms] Thread 1: my_crate::handler → Err(Conflict)
```

That timeline is what found my race condition in 20 minutes. The same bug took three days with conventional tools.

## What I learned about DWARF

DWARF is a surprisingly complete debug format. The information needed to reconstruct the full call stack, local variable values, and type information is all in the binary — you just need to read it. The Rust `gimli` crate makes this tractable. The hard part is interpreting the location expressions that tell you where in memory a variable lives at any given instruction pointer — they're essentially a small stack-based bytecode VM.

## What's next

I want to add support for attaching to Flutter apps on Android — reading the Dart VM's debug protocol rather than DWARF. That would make this useful for a much wider class of debugging problems. The tool is in use for my own work. I'll open-source it when it's not embarrassing.
