# 如何用 PHP 和 PostgreSQL 构建应用程序

> 原文：<https://blog.devgenius.io/how-to-build-an-application-with-php-and-postgresql-1a5d2b590ea7?source=collection_archive---------2----------------------->

![](img/4cd27fdb167e42319d4f8ecd909b9c79.png)

内森·达席尔瓦在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在本文中，我们将使用 PostgreSQL 作为数据库来构建一个 CRUD PHP 应用程序。CRUD 代表创建、读取、更新和删除。CRUD 操作是数据库的基本数据操作。插入查询用于在数据库中插入数据。选择查询用于从数据库中选择数据。更新查询用于更新数据库中的数据，删除查询用于从数据库中删除数据。

## **先决条件**

本文假设你已经在你的 PC 或者 MacBook 上安装了 PHP，你也安装了 PostgreSQL，并且你知道 PHP 的基础知识，至少你构建了一个 PHP 应用程序，你知道 SQL 的基础知识，并且你应该已经构建了一个使用 SQL 数据库(*例如 Mysql)的 PHP 应用程序。* 要跟随并理解这篇文章，需要 PHP 的基本知识，使用 SQL 和 PostgreSQL，您还应该在您的 PC 或 MacBook 上设置 PHP 和 PostgreSQL，最后，您应该知道如何根据您的操作系统使用 CMD 或终端与 PostgreSQL 交互。

## **创建一个将用于 CRUD 操作的数据库。**

要开始用 PHP 和 PostgreSQL 构建我们的 CRUD 应用程序，我们必须先用 PostgreSQL 创建一个数据库。从终端或 CMD 登录到 PostgreSQL 并运行以下命令

```
create database users_db
```

这里，这个命令在 Postgres 中创建了一个名为 users_db 的数据库。您也可以从用户界面创建数据库，这取决于您如何在 PC 或 MacBook 上安装 Postgres。

## 创建一个数据库表来存储用户信息。

既然我们已经创建了数据库，现在我们可以在数据库中创建表了。因此，我们将在这里创建一个用户表。为了创建该表，我们将在终端或 CMD 中运行以下命令。

```
CREATE TABLE public."user"
(
    id serial NOT NULL,
    name character varying(250),
    email character varying(250),
    phone bigint,
    address text,
    PRIMARY KEY (id)
)
```

这里我们的表叫做 user，它有以下字段 id、name、email、phone 和 address。对于本文，我们的数据库中只有一个表。

## 我们将把 PHP 应用程序连接到数据库

要在 PostgreSQL 中使用 PHP，我们需要连接到数据库，我们将通过创建一个 PHP 文件并在我们的应用程序根目录中将其命名为***connection.php***来做到这一点。在***connection.php***文件里面，我们将会有下面的 PHP 代码。

```
<?php 
   $host = "localhost";
   $port = "5432";
   $dbname = "users_db";
   $user = "postgres";
   $password = "password"; 
   $connection_string = "host={$host} port={$port} dbname={$dbname}   
                              user={$user} password={$password} "; $dbconn = pg_connect($connection_string);     
?>
```

这里，我们使用 PHP 函数 **pg_connect()** 连接到 PostgreSQL。请使用您的详细信息更新$dbname、$user 和$password。

## 构建可重用的 PHP 函数，帮助我们与 PostgreSQL 数据库交互

现在我们已经连接到了数据库，我们必须创建另一个 PHP 文件来处理所有的数据库查询。db_class 将具有以下代码。

```
<?php
     include_once('connection.php');     class Db_Class
    {
        private $table_name = 'user'; function createUser()
        {
             $sql = "INSERT INTO PUBLIC.".$this->table_name."  
             (name,email,mobile_no,address) "."VALUES('".
             $this->cleanData($_POST['name'])."','".
             $this->cleanData($_POST['email'])."','".
             $this->cleanData($_POST['phone'])."','".
             $this->cleanData($_POST['address'])."')"; return pg_affected_rows(pg_query($sql));
        }

        function getUsers()
        {             
            $sql ="select *from public." . $this->table_name . "  
            ORDER BY id DESC"; return pg_query($sql);
        } 

        function getUserById(){    

            $sql ="select *from public." . $this->table_name . "  
            where id='".$this->cleanData($_POST['id'])."'"; return pg_query($sql);
        } 

       function deleteuser()
       {    

            $sql ="delete from public." . $this->table_name . "  
            where id='".$this->cleanData($_POST['id'])."'"; return pg_query($sql);
       } 

       function updateUser($data=array())
       {       

          $sql = "update public.user set name='".
          $this->cleanData($_POST['name'])."',email='".
          $this->cleanData($_POST['email'])."', phone='".
          $this->cleanData($_POST['phone'])."',address='".
          $this->cleanData($_POST['address'])."' where id = '".
          $this->cleanData($_POST['id'])."' "; return pg_affected_rows(pg_query($sql));        
       }
       function cleanData($val)
       {
         return pg_escape_string($val);
       }
?>
```

这个文件包含可重用的 PHP 代码，这将有助于我们与 PostgreSQL 进行交互。

## 处理 PostgreSQL 中数据的读取和删除

PHP 中的索引页面是我们在浏览器上打开网站时访问的第一个页面。对于本文，我们创建一个简单的索引页面。创建一个 index.php 文件并添加下面的 PHP 代码。

```
<?php 
     include('header.php');
     $users = $obj->getUsers();
     $sn=1; if(isset($_POST['update']))
     {
        $user = $obj->getUserById();
        $_SESSION['user'] = pg_fetch_object($user);
        header('location:edit.php');
     } if(isset($_POST['delete']))
     {
         $ret_val = $obj->deleteuser(); if($ret_val==1)
           {
              echo "<script language='javascript'>";
              echo "alert('Record Deleted Successfully')
           {
            window.location.reload();
      }"; echo "</script>";
  }
}
?> <div class="container-fluid bg-3 text-center">

     <h3>CRUD Example Using PHP OOPS And PostgreSQL</h3> <a href="insert.php" class="btn btn-primary pull-right"              
     style='margin-top:-30px'> <span class="glyphicon glyphicon-
     plus-sign"></span>Add Record</a> <br>

  <div class="panel panel-primary">
        <div class="panel-heading">CRUD Example Using PHP OOPS And PostgreSQL</div>

            <div class="panel-body">
            <table class="table table-bordered table-striped">
    <thead>
      <tr class="active">
            <th>Sr. No.</th>  
            <th >Name</th>       
            <th>Email</th>
            <th>Mobile Number</th>
            <th>Address</th>
            <th>Action</th>
      </tr>
    </thead>
    <tbody>
    <?php while($user = pg_fetch_object($users)): ?>   
      <tr align="left">
        <td ><?=$sn++?></td>
        <td><?= $user->name ?></td>
        <td><?= $user->email ?></td>
        <td><?= $user->phone ?></td>
        <td><?= $user->address?></td>
        <td>
            <form method="post">
                <input type="submit" class="btn btn-success" name= "update" value="Update">   
                <input type="submit" onClick="return confirm('Please confirm deletion');" class="btn btn-danger" name= "delete" value="Delete">
                <input type="hidden" value="<?=$user->id?>" name="id">
            </form>
        </td>
      </tr>
    <?php endwhile; ?>   
    </tbody>
  </table>
</div>

</div>
</div>  
<?php include('footer.php');?>
```

这个**index.php**文件包含 HTML 代码，我们也可以从这个**index.php**文件中删除和更新一个用户记录。接下来，我们需要创建 header.php 的**，这是应用程序的所有头部将加载的地方。创建一个 header.php 文件并添加下面的 PHP 代码。**

```
<?php 
    session_start();
    require('db_class.php');
    $obj = new Db_Class();
?>
<!DOCTYPE html>
<html lang="en">
<head>
  <title>PHP PostgreSQL OOPS CRUD Example</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">

  <style>
    /* Remove the navbar's default margin-bottom and rounded borders */ 
    .navbar {
      margin-bottom: 0;
      border-radius: 0;
    }

    /* Add a gray background color and some padding to the footer */
    footer {
      background-color: #f2f2f2;
      padding: 25px;
    }
  </style>
</head>
<body>

<nav class="navbar navbar-inverse">
  <div class="container-fluid">
    <div class="navbar-header">
      <button type="button" class="navbar-toggle" data-toggle="collapse" data-target="#myNavbar">
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>                        
      </button>
      <a class="navbar-brand" href="#">PHP4EVER</a>
    </div>
    <div class="collapse navbar-collapse" id="myNavbar">
      <ul class="nav navbar-nav">
        <li class="active"><a href="#">Home</a></li>        
      </ul>       
    </div>
  </div>
</nav>
```

**我们还将创建一个**footer.php**，它将包含应用程序页脚的代码。创建一个**footer.php**文件并添加下面的 PHP 代码。**

```
<footer class="container-fluid text-center">
  <p>PHPFOREVER © <?=date('Y')?></p>
</footer>
</body>
</html>
```

## **处理向数据库表中插入数据**

**创建一个**insert.php**文件并添加下面的 PHP 代码。**

```
<?php 
include('header.php');
if(isset($_POST['submit']) and !empty($_POST['submit'])){
$ret_val = $obj->createUser();
if($ret_val==1){
    echo '<script type="text/javascript">'; 
    echo 'alert("Record Saved Successfully");'; 
    echo 'window.location.href = "index.php";';
    echo '</script>';
}
}
?>
<div class="container-fluid bg-3 text-center">    
  <h3>CRUD Example Using PHP OOPS And PostgreSQL</h3>
  <a href="index.php" class="btn btn-primary pull-right" style='margin-top:-30px'><span class="glyphicon glyphicon-eye-open"></span> View Records</a>
  <br>

  <div class="panel panel-primary">
        <div class="panel-heading">CRUD Example Using PHP OOPS And PostgreSQL</div>
            <form class="form-horizontal" method="post">
            <div class="panel-body">

             <div class="form-group">
               <label class="control-label col-sm-2">Name:<span style='color:red'>*</span></label>
               <div class="col-sm-5">
                  <input class="form-control" type="text" name="name" required>
               </div>
            </div>
             <div class="form-group">
               <label class="control-label col-sm-2">Email:<span style='color:red'>*</span></label>
               <div class="col-sm-5">
                  <input class="form-control" type="email" name="email" required>
               </div>
            </div>

             <div class="form-group">
               <label class="control-label col-sm-2">Mobile Number:<span style='color:red'>*</span></label>
               <div class="col-sm-5">
                  <input class="form-control" type="number" name="phone" required>
               </div>
            </div>
            <div class="form-group">
               <label class="control-label col-sm-2">Address:<span style='color:red'>*</span></label>
               <div class="col-sm-5">
                  <textarea rows="5" cols="5" class="form-control" name="address" required></textarea>
               </div>
            </div>
             <div class="form-group">
               <label class="control-label col-sm-2">  </label>
               <div class="col-sm-5">
                  <input type="submit" class="btn btn-primary" name="submit"  value="Submit">
               </div>
            </div> 
        </div>      
</form>
</div>
</div>  
 <?php include('footer.php');?>
```

****insert.php**文件用于向 PostgreSQL 数据库插入数据，它包含 HTML 代码和 PHP 代码，用于向 PostgreSQL 数据库提交数据。**

## **处理数据库中数据的编辑和更新。**

**创建一个**edit.php**文件并添加下面的 PHP 代码。**

```
<?php 
include('header.php');
$user = $_SESSION['user'];
if(isset($_POST['update']) and !empty($_POST['update'])){
$ret_val = $obj->updateUser();
if($ret_val==1){
    echo '<script type="text/javascript">'; 
    echo 'alert("Record Updated Successfully");'; 
    echo 'window.location.href = "index.php";';
    echo '</script>';
}
}
?>
<div class="container-fluid bg-3 text-center">    
  <h3>CRUD Example Using PHP OOPS And PostgreSQL</h3>
  <a href="index.php" class="btn btn-primary pull-right" style='margin-top:-30px'><span class="glyphicon glyphicon-step-backward"></span>Back</a>
  <br>  
  <div class="panel panel-primary">
        <div class="panel-heading">CRUD Example Using PHP OOPS And PostgreSQL</div>
            <form class="form-horizontal" method="post">
            <div class="panel-body">             
             <div class="form-group">
               <label class="control-label col-sm-2">Name:<span style='color:red'>*</span></label>
               <div class="col-sm-5">
                  <input class="form-control" value= "<?=$user->name?>"type="text" name="name" required>
               </div>
            </div>
             <div class="form-group">
               <label class="control-label col-sm-2">Email:<span style='color:red'>*</span></label>
               <div class="col-sm-5">
                  <input class="form-control" value= "<?=$user->email?>"type="email" name="email" required>
               </div>
            </div>            
             <div class="form-group">
               <label class="control-label col-sm-2">Mobile Number:<span style='color:red'>*</span></label>
               <div class="col-sm-5">
                  <input class="form-control" value= "<?=$user->mobile_no?>"type="number" name="mobileno" required>
               </div>
            </div>
            <div class="form-group">
               <label class="control-label col-sm-2">Address:<span style='color:red'>*</span></label>
               <div class="col-sm-5">
                  <textarea rows="5" cols="5" class="form-control" name="address" required><?=$user->address?></textarea>
               </div>
               <input type="hidden" value="<?=$user->id?>" name="id">
            </div>
             <div class="form-group">
               <label class="control-label col-sm-2">  </label>
               <div class="col-sm-5">
                 <input type="submit" class="btn btn-success" name="update" value="Update">                    
                </div>
            </div> 
        </div>      
</form>
</div>
</div>  
 <?php include('footer.php');?>
```

**edit.php 文件包含 HTML 代码和 PHP 代码，我们将使用 HTML 表单来更新数据库中的用户记录。**

## **结论**

**我们已经到了这篇文章的结尾，我希望你理解了如何用 PHP 和 PostgreSQL 构建一个应用程序的基础。要了解更多关于如何使用 PHP 和 PostgreSQL 构建应用程序的信息，我建议您搜索更多关于 PHP 和 PostgreSQL 的教程，其中一些可以在 Youtube 上找到。**