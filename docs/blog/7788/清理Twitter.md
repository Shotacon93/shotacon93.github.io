> 常年搁置的Twitter账号总是被拿去做广告, 转发各种垃圾推文. 辣么如何在找回之后清理呢.

> 使用方法很简单, 不过不是全自动哦.

- Step 1, 清理推文
    1. 进到主页https://twitter.com/yourAccount
    2. 在头像的右侧选择Tweets标签(一般默认就是这里), 然后一直下拉到底部, 因为清理的只是当前显示的.
    3. 右键检查或者F12进入控制台, 选择Console, 粘贴并执行下面的脚本, 等待一会就OK了.

    ```
    $(".js-actionDelete").each(function(){$(this).click()$(".delete-action").get(0).click()})
    ```

- Step 2, 清理转推
    1. 刷新页面. 剩下的这些没有删除提示的就是转推了.
    2. 同样一直下拉到底部, 粘贴脚本, 执行, 等一会就可以了
    ```
     $(".js-actionRetweet").each(function(){$(this).click()})
    ```

- Step 3
    1. 点击头像右侧的Following, 一直下拉到底部, 粘贴并执行脚本, 等一会就全部UnFollow了.
    2. 不要在当前页面重复执行哦. 点一次是取消, 再点一次可就是又关注了.
    ```
     $(".following .js-follow-btn").click()
    ```
