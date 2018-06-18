---
title: accepts_nested_attributes_for
tags:
  - ruby
url: 432.html
id: 432
categories:
  - 后台开发
date: 2017-04-14 11:33:54
---

从 Rails 2.3 开始在 models 中提供了 accepts\_nested\_attributes_for 命令让 SQL 关联嵌套表查询变得简单。

### 不使用 accepts\_nested\_attributes_for

    //models
    class Product < ActiveRecord::Base
      has_one :detail
    end
    class Detail < ActiveRecord::Base
      belongs_to :product
    end
    
    //view
    <% form_for :product do |f| %>
      <%= f.text_field :title %>
      <% fields_for :detail do |detail| %>
        <%= detail.text_field :manufacturer %>
      <% end %>
    <% end %>
    
    //controller
    class ProductsController < ApplicationController
      def create
        @product = Product.new(params[:product])
        @detail = Detail.new(params[:detail])
    
        Product.transaction do
          @product.save!
          @detail.product = @product
          @detail.save
        end
      end
    end
    

上例中，Product 和 Detail 模型是一对一的关系，我們若想建立一個产品並同时建立一個新的细节，需要將 product 对象和 detail 对象， 通过 @detail.product = @product 进行关联。 但是在控制器中做这样的操作明显过于复杂，产品模型应该只负责创建产品。

### 使用 accepts\_nested\_attributes_for

    class Product < ActiveRecord::Base
      has_one :detail
      accepts_nested_attributes_for :detail
    end
    
    <% form_for :product do |f| %>
      <%= f.text_field :title %>
      <% f.fields_for :detail do |detail| %>
        <%= detail.text_field :manufacturer %>
      <% end %>
    <% end %>
    
    class ProductsController < ApplicationController
      def create
        @product = Product.new(params[:product])
        @product.save
      end
    end
    

在 product 模型中添加了 accepts\_nested\_attributes_for，这样 product 模型仅负责自己的创建，是不是简单了呢？

### 一对多关系

同时我们也可以在一对多的模型中使用 accepts\_nested\_attributes_for 来简化关联对象的创建

    class Project < ActiveRecord::Base
      has_many :tasks
      accepts_nested_attributes_for :tasks
    end
    
    class Task < ActiveRecord::Base
      belongs_to :project
    end
    
    <% form_for @project do |f| %>
      <%= f.text_field :name %>
      <% f.fields_for :tasks do |tasks_form| %>
        <%= tasks_form.text_field :name %>
      <% end %>
    <% end %>
    

### 参数配置

    accepts_nested_attributes_for :pics, :allow_destroy => true, :reject_if => :all_blank