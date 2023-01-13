# 如何从 Blob 响应中检索文件名

> 原文：<https://www.freecodecamp.org/news/angular-how-to-retrieve-filename-from-a-blob-response/>

如果您是 Angular 的新手，您可能想知道如何从 API 响应中检索文件名。

假设您有一个向远程 API 发出 POST 请求并接收包含文件的`Blob`的方法:

```
public downloadExcel(data): void {
  const url: string = '[api endpoint here ]';
  this.http.post(url, data.body, { responseType: 'blob' })
    .subscribe((response: Blob) => saveAs(response, data.fileName + '.xlsx'));
}
```

根据 MDN，`Blob`对象只包含大小和类型，所以你需要另一种方法来获得实际的文件名。

但是由于`data`是作为参数传递给你的函数的，所以它也可能包含来自服务器的有效载荷。将它记录到控制台，并查看包括哪些信息。

## 读取响应标头

另一个可能的选择是读取 HTTP 响应头本身。

因为您正在从 API 获取数据，所以很可能您正在使用`httpClient`来发出请求。API 响应通常会在头部包含有用的信息。

需要注意的一点是`X-Token`条目。但是请记住，不是所有的头都可以从客户端访问，所以需要从服务器端设置`access-control-expose-headers`。

如果`X-Token`被暴露，您可以使用 HTTP 方法`{ observe: 'response' }`获得完整的响应，然后将`X-Token`登录到控制台:

```
http
  .get<any>('url', { observe: 'response' })
  .subscribe(resp => {
    console.log(resp.headers.get('X-Token'));
  });
```

Source: [Stack Overflow](https://stackoverflow.com/questions/48184107/read-response-headers-from-api-response-angular-5-typescript)

一般来说，阅读[响应头](https://developer.mozilla.org/en-US/docs/Web/API/Response/headers)也是值得的。