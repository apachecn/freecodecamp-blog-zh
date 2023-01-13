# JavaScript Try Catch:解释异常处理

> 原文：<https://www.freecodecamp.org/news/error-handling-and-try-catch-throw/>

`try...catch..finally`语句指定了一个代码块，以便在出现错误时尝试响应。`try`语句包含一个或多个`try`块，并以至少一个`catch`和/或一个`finally`子句结束。

### `try...catch`:

```
try {
   throw new Error('my error');
} catch (err) {
  console.error(err.message);
}

// Output: my error
```

### `try...finally`:

```
try {
   throw new Error('my error');
} finally {
  console.error('finally');
}

// Output: finally
```

当不使用`catch`语句时，即使执行了`finally`块中的代码，错误也不会被“捕获”。相反，错误将继续到上层`try`程序块(或主程序块)。

### `try...catch...finally`:

```
try {
   throw new Error('my error');
} catch (err) {
  console.error(err.message);
} finally {
  console.error('finally');
}

// Output:
// my error
// finally
```

**典型用法:**

```
try {
   openFile(file);
   readFile(file)
} catch (err) {
  console.error(err.message);
} finally {
  closeFile(file);
}
```

### 嵌套的`try...catch`:

您还可以:

*   将`try-catch`语句嵌套在`try`块中。

您可以在一个`try`块中嵌套一个`try...catch`语句。例如，向上抛出一个错误:

```
try {
  try {
    throw new Error('my error');
  } catch (err) {
    console.error('inner', err.message);
    throw err;
  } finally {
    console.log('inner finally');
  }
} catch (err) {
  console.error('outer', err.message);
}

// Output: 
// inner my error 
// inner finally 
// outer my error
```