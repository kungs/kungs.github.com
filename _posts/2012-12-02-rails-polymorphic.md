---
layout: post
title: "在Rails使用polymorphic"
category: rails
tags: []
---
{% include JB/setup %}
现在有个场景：两个model：Album 和 Book，这两个model都需要添加一个图片。有两种方法：
>建立直接关联关系
首先，添加一个Imgae model，做一下设置：
	class Album < ActiveRecord::Base
	  has_one :image, :dependent => :destroy
	end
	class Book < ActiveRecord::Base
	  has_one :image, :dependent => :destroy
	end
	class Image < ActiveRecord::Base
	  belongs_to :album
	  belongs_to :book
	end
同时，表albums添加字段：image_id, 表books添加字段：image_id。  
搞定，完全符合要求。  
过了一天，客户说，Person加个头像吧，好吧，只能把上边的步骤重复一遍，不觉的烦么？  
>建立polymorphic关联关系
	class Album < AcitveRecord::Base
	  has_one :image, :as => imagable, :dependent => :destroy
	end
	class Book < ActiveRecord::Base
	  has_one :image, :as => imagable, :dependent => :destroy
	end
	class Image < ActiveRecord::Base
	  belongs_to :imagable, :polymorphic => true
	end
此时，imgaes表中需加两个属性： imagable_id, imagable_type 。  
这样的话，
	@book = Book.new(params[:book])
	@bool.build_image(params[:image]
	@book.save
	@book.image
或者：
	@book = Book.new(params[:book])
	@image = Image.create(params[:image]
	@image.imagbale = @image
	@book.save
