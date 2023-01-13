# 如何用 Angular 让图片上传变得简单

> 原文：<https://www.freecodecamp.org/news/how-to-make-image-upload-easy-with-angular-1ed14cb2773b/>

菲利普·耶尔加

# 如何用 Angular 让图片上传变得简单

![1*dxawCwfllIh8ljUcRtwnXg](img/81103bdab1e52462e6322fcf4ce4cabd.png)

这是如何上传图片到亚马逊 S3 教程的第二部分。你可以在这里找到第一部[。在这篇文章中，我们将看看有角的部分。](https://medium.freecodecamp.org/how-to-set-up-simple-image-upload-with-node-and-aws-s3-84e609248792)

你也可以观看我的一步一步上传图片的视频教程。本文底部提供了链接。

### 1.首先创建一个模板

首先，我们希望创建一个可重用的组件，它可以很容易地插入到其他组件中。

让我们从一个简单的 HTML 输入模板开始。不要忘记应用你选择的风格，或者你可以从我的 [GitHub repo](https://gist.github.com/Jerga99/7fe7b1942c6e5bbe4723f2369c760649) 中得到它们。

```
<label class="image-upload-container btn btn-bwm">
  <span>Select Image</span>
  <input #imageInput
         type="file"
         accept="image/*"
         (change)="processFile(imageInput)">
</label>
```

这里重要的是一个**类型的**输入，它被设置为一个**文件。****接受**属性定义了接受输入的文件。 **Image/*** 指定我们可以通过这个输入选择任何类型的图像。 **#imageInput** 是一个输入引用，我们可以通过它访问上传的文件。

当我们选择一个文件时，触发一个 **Change** 事件。所以让我们来看看类代码。

### 2.不要忘记组件代码

```
class ImageSnippet {
  constructor(public src: string, public file: File) {}
}

@Component({
  selector: 'bwm-image-upload',
  templateUrl: 'image-upload.component.html',
  styleUrls: ['image-upload.component.scss']
})
export class ImageUploadComponent {

  selectedFile: ImageSnippet;

  constructor(private imageService: ImageService){}

  processFile(imageInput: any) {
    const file: File = imageInput.files[0];
    const reader = new FileReader();

    reader.addEventListener('load', (event: any) => {

      this.selectedFile = new ImageSnippet(event.target.result, file);

      this.imageService.uploadImage(this.selectedFile.file).subscribe(
        (res) => {

        },
        (err) => {

        })
    });

    reader.readAsDataURL(file);
  }
}
```

让我们来分解这个代码。您可以在**流程文件**中看到，正在获取从**变更**事件发送的图像输入。通过写入 **imageInput.files[0]** ，我们正在访问第一个**文件**。我们需要一个**阅读器**来访问文件的附加属性。通过调用 **readAsDataURL，**我们可以在之前订阅的 **addEventListener** 的回调函数中获得图像的 base64 表示。

在回调函数中，我们创建了一个 **ImageSnippet 的实例。第一个**值是我们稍后将在屏幕上显示的图像的 base64 表示。**第二个**值是一个文件本身，我们将把它发送到服务器上传到亚马逊 S3。

现在，我们只需要提供这个文件并通过服务发送一个请求。

### 3.我们也需要一项服务

如果没有服务，它就不是一个有棱角的应用程序。该服务将负责向我们的节点服务器发送请求。

```
export class ImageService {

  constructor(private http: Http) {}

  public uploadImage(image: File): Observable<Response> {
    const formData = new FormData();

    formData.append('image', image);

    return this.http.post('/api/v1/image-upload', formData);
  }
}
```

正如我在前面的讲座中告诉你的，我们需要发送一个图像作为表单数据的一部分。我们将在一个**图像**的键下追加图像以形成数据(与我们之前在节点中配置的键相同)。最后，我们只需要向服务器发送一个请求，在有效载荷中包含**表单数据**。

现在我们可以庆祝了。就是这样！图片发送上传！

在接下来的几行中，我将提供一些额外的代码来获得更好的用户体验。

### 4.其他 UX 更新

```
class ImageSnippet {
  pending: boolean = false;
  status: string = 'init';

  constructor(public src: string, public file: File) {}
}

@Component({
  selector: 'bwm-image-upload',
  templateUrl: 'image-upload.component.html',
  styleUrls: ['image-upload.component.scss']
})
export class ImageUploadComponent {

  selectedFile: ImageSnippet;

  constructor(private imageService: ImageService){}

  private onSuccess() {
    this.selectedFile.pending = false;
    this.selectedFile.status = 'ok';
  }

  private onError() {
    this.selectedFile.pending = false;
    this.selectedFile.status = 'fail';
    this.selectedFile.src = '';
  }

  processFile(imageInput: any) {
    const file: File = imageInput.files[0];
    const reader = new FileReader();

    reader.addEventListener('load', (event: any) => {

      this.selectedFile = new ImageSnippet(event.target.result, file);

      this.selectedFile.pending = true;
      this.imageService.uploadImage(this.selectedFile.file).subscribe(
        (res) => {
          this.onSuccess();
        },
        (err) => {
          this.onError();
        })
    });

    reader.readAsDataURL(file);
  }
}
```

我们向**图像片段添加了新的属性:待定**和**状态。Pending** 可以是假或真，这取决于当前是否正在上传图像。**状态**是上传过程的结果。可能是 **OK** 或者**失效。**

上传图像后，调用 OnSuccess 和 **onError** ，它们设置图像的状态。

好了，现在让我们来看看更新后的模板文件:

```
<label class="image-upload-container btn btn-bwm">
  <span>Select Image</span>
  <input #imageInput
         type="file"
         accept="image/*"
         (change)="processFile(imageInput)">
</label>

<div *ngIf="selectedFile" class="img-preview-container">

  <div class="img-preview{{selectedFile.status === 'fail' ? '-error' : ''}}"
       [ngStyle]="{'background-image': 'url('+ selectedFile.src + ')'}">
  </div>

  <div *ngIf="selectedFile.pending" class="img-loading-overlay">
    <div class="img-spinning-circle"></div>
  </div>

  <div *ngIf="selectedFile.status === 'ok'" class="alert alert-success"> Image Uploaded Succesfuly!</div>
  <div *ngIf="selectedFile.status === 'fail'" class="alert alert-danger"> Image Upload Failed!</div>
</div>
```

这里我们根据**图像**的**状态**在屏幕上显示我们上传的图像和错误。当图像挂起时，我们还显示一个漂亮的旋转图像来通知用户上传活动。

### 5.添加一些样式

风格不是本教程的重点，所以你可以在这个[链接](https://gist.github.com/Jerga99/7fe7b1942c6e5bbe4723f2369c760649)中找到所有的 SCSS 风格。

任务完成了！:)简单的图片上传应该就这样了。如果有不清楚的地方，请确保您先看了本教程的第一部分。

如果你喜欢这个教程，可以随时在 Udemy**——[完整的 Angular，React &节点指南| Airbnb 风格应用](http://bit.ly/2NeWna4)上查看我的全部课程。**

**已完成项目:** [我的 GitHub 库](https://github.com/Jerga99/bwm-ng)

**视频讲座** : [YouTube 教程](https://www.youtube.com/watch?v=wNqwExw-ECw)

干杯，

菲利普