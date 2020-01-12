<template>
<div class="gray-bg">
    <div class="middle-box text-center loginscreen  animated fadeInDown">
        <div>
            <div>
                <h1 class="logo-name">WR</h1>
            </div>
            <h3>后台管理登录界面</h3>
            <form id="form" name="form" method="post" action="/admin/login/checkLogin"  autocomplete="off">
                <div class="form-group">
                    <input name="account" type="text"  class="form-control" placeholder="账号"  autocomplete="off">
                </div>
                <div class="form-group">
                    <input name="password" type="password" class="form-control" placeholder="密码" autocomplete="off">
                </div>
                <div class="form-group login">
                    <span>验证码</span>
                    <input name="code" style="width:110px" type="text" id="code"/>
                    <a> <img class="reloadverify" alt="验证码" onclick="this.src='/defaultKaptcha?d='+new Date()*1" src="/defaultKaptcha" /></a>
                </div>
                <button type="submit" class="btn btn-primary block full-width m-b">登 录</button>
            </form>
        </div>
    </div>
</div>
</template>
<!--ajax异步提交-->
<script>
    $('form').submit(function(){
        var account  = $("input[name='account']").val();
        var password  = $("input[name='password']").val();
        var code = $("#code").val();
        if(!account){
            layer.msg('用户名不能为空！',{time:2000});
            return false;
        }
        if(!password){
            layer.msg('密码不能为空！',{time:2000});
            return false;
        }
        if(!code){
            layer.msg('验证码不能为空！',{time:2000});
            return false;
        }
        var url  = $(this).attr('action');
        var formData = $("#form").serialize();
        $.ajax({
            type: "post",
            url: url,
            dataType: "json",
            data: formData,
            success: function (res) {
                if (res.status) {
                    layer.msg(res.message, {time: 1000}, function () {
                        window.location.href = "/admin/user_list/lists";
                    });
                } else {
                    //刷新验证码
                    $(".reloadverify").click();
                    layer.msg(res.message);
                }
            }
        });
        return false;
    });
</script>

