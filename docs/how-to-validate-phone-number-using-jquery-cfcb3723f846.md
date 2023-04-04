# 如何使用 jQuery 验证电话号码

> 原文：<https://blog.devgenius.io/how-to-validate-phone-number-using-jquery-cfcb3723f846?source=collection_archive---------2----------------------->

在本文中，我们将看到如何使用 jquery 来验证电话号码。我们将学习用 jQuery 在 PHP 中验证电话号码的不同方法。

此外，我们将看到一个 PHP 中 10 位数字电话号码验证的例子，以及使用 regex 或正则表达式进行手机号码验证的例子。

因此，让我们看看使用 jquery 验证电话号码，jquery 中使用正则表达式的 10 位手机号码验证，使用模式在 PHP 中验证电话号码。

**示例 1:使用正则表达式验证 10 位数的电话号码**

正则表达式验证为用户提供了根据用户要求以不同格式输入数字的灵活性。

```
<html>
<head>
    <title>Allow Only 10 Digit Mobile Number Using Regex - websolutionstuff</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
</head>
<body>
<form>
    <h1>Allow Only 10 Digit Mobile Number Using Regex - websolutionstuff</h1>
   <div class="col-md-6">
    <label>Phone Number: </label>
    <input class="form-control" type="text" name="phone Number" placeholder="Phone Number" minlength="10" maxlength="10" required>
    <br>
    <label>Phone Number: </label>
  	<input type="text" class="form-control" name="phone Number" placeholder="Phone Number" pattern="[1-9]{1}[0-9]{9}"  required ><br>
    <button type="submit" class="btn btn-success">Submit</button>
  </div>
</form>
</body>
</html>
```

**读也:**[Laravel 9 where column 查询示例](https://websolutionstuff.com/post/laravel-9-wherecolumn-query-example)

**示例 2:匹配 10 位数电话号码的正则表达式**

在本例中，我们将使用带有正则表达式的 jquery 来验证 10 位数的电话号码。

```
<html>
    <body>
        Phone Number: <input type='text' id='phone_number'/>
        <span id="validation_status"></span>
    </body>
</html>
<script>
$(document).ready(function() {
    $('#phone_number').blur(function(e) {
        if (validatePhone('phone_number')) {
            $('#validation_status').html('Valid');
            $('#validation_status').css('color', 'green');
        }
        else {
            $('#validation_status').html('Invalid');
            $('#validation_status').css('color', 'red');
        }
    });
});

function validatePhone(phone_number) {
    var a = document.getElementById(phone_number).value;
    var filter = /^(\+\d{1,2}\s)?\(?\d{3}\)?[\s.-]\d{3}[\s.-]\d{4}$/;    
    if (filter.test(a)) {
        return true;
    }
    else {
        return false;
    }
}
</script>
```

**另请参阅:** [**如何使用 JQuery**](https://websolutionstuff.com/post/how-to-validate-password-and-confirm-password-using-jquery) 验证密码和确认密码

如果您不想匹配非美国号码，请使用。

```
^(\+0?1\s)?\(?\d{3}\)?[\s.-]\d{3}[\s.-]\d{4}$
```

匹配以下字符串示例。

```
123-456-7890
(123) 456-7890
123 456 7890
123.456.7890
+91 (123) 456-7890
```

**你可能也会喜欢:**

*   **读也:** [**Laravel 9 AJAX CRUD 示例**](https://websolutionstuff.com/post/laravel-9-ajax-crud-example)
*   **阅读也:** [**如何在 React JS 中验证表单**](https://websolutionstuff.com/post/how-to-validate-form-in-react-js)

如果这篇文章有帮助，请点击拍手👏下面的按钮。