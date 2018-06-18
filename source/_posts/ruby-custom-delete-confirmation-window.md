---
title: Ruby自定义删除确认窗口
tags:
  - ruby
  - rails
url: 180.html
id: 180
categories:
  - 后台开发
date: 2016-12-07 12:11:50
---

haml代码：

= link_to post, class:'trash', remote: true, method: :delete, data: {confirm: "你确定要删除么？"} do
  %i.fa.fa-trash

点击会默认弹出浏览器默认的 Confirm 弹窗，但是样式不能够自定义，所以要改成弹出自己的弹窗，最主要的代码如下：

//修改 rails 默认动作
$.rails.allowAction = function(link){
  var message = link.attr('data-confirm');
  if(!message) { return true; }
  Confirm(message,link); //自定义的处理方法
  return false;
}
//这个方法移除data-confirm属性，rails就无法再去触发默认弹窗，然后模拟点击链接
$.rails.confirmed = function(link){
  link.removeAttr('data-confirm');
  link.trigger('click.rails');
}

Confirm的定义，使用的是Qtip2的Modal：

function dialogue(content,link){
  $('<div />').qtip({
    content: {
      text: content
    },
    position: {
      my: 'top center',
      at: 'top center',
      target: $(window)
    },
    show: {
      ready: true,
      modal: {
        on: true,
        blur: false
      }
    },
    hide: false,
    style: 'dialogue',
    events: {
      render: function(event,api){
        $('button.windowConfirm\_\_btn\_ok', api.elements.content).click(function(){
          $.rails.confirmed(link);
          api.hide();
        });
        $('button.windowConfirm\_\_btn\_cancel', api.elements.content).click(function(){
          api.hide();
        });
      },
      hide: function(event,api){
        api.destroy();
      }
    }
  })
}

window.Confirm = function(content,link){
  var message = $('<div class="windowConfirm__content">'+content+'</div>'),
      ok = $('<button class="windowConfirm\_\_btn\_ok">确定</button>'),
      cancel = $('<button class="windowConfirm\_\_btn\_cancel">取消</button>');
  dialogue(message.add(ok).add(cancel),link);
}