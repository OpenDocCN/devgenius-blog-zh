# 如何检查电子邮件已经存在或不在 Laravel

> 原文：<https://blog.devgenius.io/how-to-check-email-already-exist-or-not-in-laravel-1cfcd9f73d07?source=collection_archive---------3----------------------->

在这个例子中，我将向你展示如何检查电子邮件已经存在或不在 laravel。

很多时候，用户注册重复或现有的电子邮件 id，这是很难维持数据或记录在数据库中，所以我们会检查电子邮件存在与否，如果电子邮件已经注册了相同的电子邮件 id，然后我们会显示错误消息给用户，就像电子邮件已经存在。

这里我已经创建了控制器，并在我的控制器中添加了下面的代码。

```
Use Response;
public function userEmailCheck(Request $request)
{ $data = $request->all(); // This will get all the request data.
    $userCount = User::where('email', $data['email']);
    if ($userCount->count()) {
        return Response::json(array('msg' => 'true'));
    } else {
        return Response::json(array('msg' => 'false'));
    }
}
```

现在为视图创建刀片文件，并在该文件中添加 javascript 验证。

```
<div class="form-group row">
    <label for="email" class="col-md-4 col-form-label text-md-right">{{ __('E-Mail Address') }}</label> <div class="col-md-6">
        <input id="email" type="email" class="form-control @error('email') is-invalid @enderror" name="email" value="{{ old('email') }}" required autocomplete="email"> @error('email')
            <span class="invalid-feedback" role="alert">
                <strong>{{ $message }}</strong>
            </span>
        @enderror
    </div>
</div><script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>  // if required
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.19.2/jquery.validate.min.js" type="text/javascript"></script><script>    
    var email =  $("#email").val();
    $('#registration').validate({
        rules: {            
            email: {
                required: true,
                email: true,
                remote: {
                    url: '{{url('user/checkemail')}}',
                    type: "post",
                    data: {
                        email:$(email).val(),
                        _token:"{{ csrf_token() }}"
                        },
                    dataFilter: function (data) {
                        var json = JSON.parse(data);
                        if (json.msg == "true") {
                            return "\"" + "Email address already in use" + "\"";
                        } else {
                            return 'true';
                        }
                    }
                }
            }
        },
        messages: {            
            email: {
                required: "Email is required!",
                email: "Enter A Valid E-Mail !..",
                remote: "Email address already in use!"
            }
        }
    });
</script>
```

我们的代码在这里完成了。

> [**读取:在**中导入导出 CSV/EXCEL 文件](https://websolutionstuff.com/post/import-export-csv-excel-file-in-laravel)

***注:请不要忘记喜欢、分享、评论和鼓掌。***