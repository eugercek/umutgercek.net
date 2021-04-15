---
title: "1 Advanced C Programming - errno"
lastmod: 2021-02-04T19:37:25+03:00
tags: ["C", "linux"]
categories: ["tlpi"]
draft: true
---

`errno` is a integer global variable declared in `<errno.h>`.

-   It's default value is 0.
-   When a error happened, Library function or system call sets `errno` a non-zero value.
-   We can look `errno` to identify what was the error.


## What is system call {#what-is-system-call}

>
>
> No one can use anything in OS.<br>
> Only kernel allowed to interact with hardware.<br>
> If anything happened it could happend thanks to kernel.<br>

Beside being funny, I tried to summarize what's kernel's job.
System call is kernel's interfaces, APIs.


### For example. {#for-example-dot}

When a program allocated memory, it actually asks kernel to allocate memory.

When we say process creted new process ...<br>
I think it's obvious.


### Wrapper Functions {#wrapper-functions}

Generally system calls wrapped around a function for friendlier interface.

| Function    | System call |
|-------------|-------------|
| fopen       | open        |
| printf      | write       |
| malloc,free | brk         |


## Exception check {#exception-check}

In C you need to check nearly every sytem call.<br />
File operations, memory allocations.<br />
You will get really interesting erros if you won't.<br />

```C
 FILE *ftpr = fopen("foo.txt", "r");
 if (fptr == NULL)
   printf("I hate writing error messages");
```

With `errno` you can have better interface for deaing with errors


## perror() {#perror}

Defined in `<stdio.h>`.
Does something like this `function_error_map[errno]`.

```C
 FILE *ftpr = fopen("foo.txt", "r");
 if (errno != 0)
   perror("open");
```

open: No such file or directory


## Explore open error messages {#explore-open-error-messages}

We can use `strerror()` which defined in `<string.h>`.
It returns `char *`.
133 is generaly maximum error number.

```C
 for (int i = 0; i < 10; i++) {
   printf("%d\t%s\n", i, strerror(i));
 }
```

| No | Message                   |
|----|---------------------------|
| 0  | Success                   |
| 1  | Operation not permitted   |
| 2  | No such file or directory |
| 3  | No such process           |
| 4  | Interrupted system call   |
| 5  | Input/output error        |
| 6  | No such device or address |
| 7  | Argument list too long    |
| 8  | Exec format error         |
| 9  | Bad file descriptor       |
