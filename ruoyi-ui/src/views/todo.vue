<!DOCTYPE html>
<html lang="en">
<template>
<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>TODOLIST</title>
  <link rel="stylesheet"
    href="https://at.alicdn.com/t/font_3367451_ccpjp65hhf9.css?spm=a313x.7781069.1998910419.51&file=font_3367451_ccpjp65hhf9.css" />
</head>

<body>
  <div class="container">
    <!-- header -->
    <div class="header">
      <div class="header-left">
        <h1>我的一天</h1>
        <div class="myday">十二月 30日  星期五</div>
      </div>
      <div class="header-right">今天</div>
    </div>

    <!-- 添加任务 -->
    <div class="new-task">
      <span class="iconfont icon-plus"></span>
      <input type="text" placeholder="添加任务" />
    </div>

    <!-- todo-list -->
    <div class="todo-list"></div>

    <!-- done-list -->
    <div class="done-list"></div>

    <!-- todo-list模板 -->
    <div class="template">
      <div class="todo-item">
        <span class="iconfont icon-circle"></span>
        <div class="todo-content">
          <div class="todo-text">下班取快递</div>
          <div class="todo-type">任务</div>
        </div>
        <span class="iconfont icon-xingxing1"></span>
      </div>
    </div>
  </div>
</body>
</template>

<style>
    * {
        margin: 0;
        padding: 0;
    }

    /* 定义滚动条样式 */
    ::-webkit-scrollbar {
        width: 6px;
        height: 6px;
    }

    /*定义滚动条轨道 内阴影+圆角*/
    /* ::-webkit-scrollbar-track {
        box-shadow: inset 0 0 0px rgba(240, 240, 240, 0.5);
        border-radius: 10px;
        background-color: #272d44;
    } */

    /*定义滑块 内阴影+圆角*/
    /* ::-webkit-scrollbar-thumb {
        border-radius: 10px;
        box-shadow: inset 0 0 0px rgba(240, 240, 240, 0.5);
        background-color: rgba(240, 240, 240, 0.5);
    }  */


    .container {
        width: 800px;
        height: 700px;
        margin: 100px auto;
        /* background-color: pink; */
        overflow: hidden;
        border: 1px solid rgb(223, 223, 22);
        border-radius: 5%;
        overflow-y: scroll;
    }

    /* header */
    .header {
        /* width: auto; */
        height: 100px;
        margin: 10px 15px;
        /* background-color: skyblue; */
    }

    /* header-left */
    .header .header-left {
        float: left;
    }

    .myday {
        margin-top: 5px;
    }

    /* header-right  */
    .header .header-right {
        margin-top: 20px;
        float: right;
    }

    /* 模板 */
    .template {
        display: none;
    }

    /* new-task */
    .new-task {
        margin: 0 15px;
        padding-bottom: 10px;
        border-bottom: 1px solid #f0f0f0;
    }

    .new-task .icon-plus {
        font-size: 30px;
        color: #a5a5a5;
        vertical-align: middle;
    }

    /* input */
    .new-task input {
        width: 670px;
        height: 30px;
        color: #a5a5a5;
        outline: none;
        border: 0;
        padding-left: 11px;
    }

    input:placeholder-shown {
        font-size: 1em;
    }

    /* 代办 and 已做 todo-item*/
    .todo-list .todo-item,
    .done-list .todo-item {
        margin: 0 15px;
        padding: 10px 0;
        border-bottom: 1px solid #f7f7f7;
    }

    /* 小圆圈 和 星号 */
    .todo-list .todo-item span:first-child,
    .todo-list .todo-item span:last-child,
    .done-list .todo-item span:first-child,
    .done-list .todo-item span:last-child {
        display: block;
        /* height: 100%; */
        /* line-height: 100%; */
        font-size: 20px;
    }

    /* todo-list 小圆圈 */
    .todo-list .todo-item span:first-child {
        float: left;
        color: rgb(212, 55, 55);
    }

    /* todo-list 星号 */
    .todo-list .todo-item span:last-child {
        float: right;
        color: rgb(212, 55, 55);
    }

    /* todo-content */
    .todo-list .todo-item .todo-content,
    .done-list .todo-item .todo-content {
        display: inline-block;
        margin-left: 25px;
    }

    /* .todo-list and .done-list */
    /* todo-content > todo-text and todo-type */
    /* todo-text */
    .todo-list .todo-content .todo-text,
    .done-list .todo-content .todo-text {
        font-size: 17px;
        color: #696969;
    }

    /* todo-type */
    .todo-list .todo-content .todo-type,
    .done-list .todo-content .todo-type {
        font-size: 12px;
        color: #696b71;
        /* transform-origin: 0 0; */
        /* transform: scale(0.8); */
    }

    /* done-list */
    /* done-list 圆圈 */
    .done-list .todo-item span:first-child {
        float: left;
        color: rgb(49, 151, 191);
    }

    /* done-list 星号 */
    .done-list .todo-item span:last-child {
        float: right;
        color: rgb(49, 151, 191);
    }

    /* .todo-text 加删除线 */
    .done-list .todo-content .todo-text {
        text-decoration: line-through double #333;
    }

</style>







<script>
window.addEventListener('load', function () {
    var newTaskInput = this.document.querySelector('.new-task input');
    var todoList = this.document.querySelector('.todo-list');
    var doneList = this.document.querySelector('.done-list');
    // 获取模板
    var todoItem = this.document.querySelector('.todo-item');
    var date = null;

    // 添加任务
    newTaskInput.addEventListener('keyup', function (e) {
        if (e.keyCode == 13) {
            // 克隆任务条款
            var todo_item = todoItem.cloneNode(true);
            // 更改任务条款里的文本
            todo_item.children[1].children[0].innerHTML = newTaskInput.value;


            // 实现 刷新页面后不会消失
            // // 获取 时间戳
            // date = Date.now();
            // // 存储数据 (存储任务条款)
            // localStorage.setItem(date,todo_item);

            // 把更改 value 后的 新 任务条款添加至最前面(第一个前面)
            todoList.insertBefore(todo_item, todoList.children[0]);
            // 获取circle圆圈
            var circle = todoList.children[0].querySelector('.icon-circle');
            // 获取 小星星
            var star = todoList.children[0].querySelector('.icon-xingxing1');

            // circle圆圈 和 小星星 设置 行高和高 为父元素的高 (padding+content) 最后需要减去 padding
            circle.style.lineHeight = circle.parentNode.clientHeight - 20 + 'px';
            circle.style.height = circle.parentNode.clientHeight - 20 + 'px';
            star.style.lineHeight = star.parentNode.clientHeight - 20 + 'px';
            star.style.height = star.parentNode.clientHeight - 20 + 'px';

            // 为circle注册点击事件 删除所在item
            circle.addEventListener('click', function () {
                if (circle.className == 'iconfont icon-circle') {
                    // 如果在 todo-list 则加入 done-list
                    // 更改circle字体图标
                    circle.className = 'iconfont icon-check-circle';
                    // 把更改后的 item 添加至 done-list 从 todo-list 中移除
                    doneList.insertBefore(this.parentNode, doneList.children[0]);
                } else {
                    // 如果在 done-list 则加入 todo-list
                    // 更改circle字体图标
                    circle.className = 'iconfont icon-circle';
                    // 把更改后的 item 添加至 todo-list 从 done-lsit 中移除
                    todoList.insertBefore(this.parentNode, todoList.children[0]);
                }
            })
            // 为 star 绑定点击事件 标为重要 或 不重要
            star.addEventListener('click', function () {
                if (star.className == 'iconfont icon-xingxing1') {
                    // 如果是 空心 则改为实心
                    star.className = 'iconfont icon-xingxing';
                } else {
                    // 如果是 实心 则改为空心
                    star.className = 'iconfont icon-xingxing1';
                }
            })
            // 表单 value 置空
            newTaskInput.value = '';
        }
    })
})

</script>

</html>
