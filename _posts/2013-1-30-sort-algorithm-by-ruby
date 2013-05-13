---
layout: post
title: "用ruby实现常见的排序算法"
category: ruby
tags: []
---
{% include JB/setup %}

有许多经典的排序算法，经常不用都快忘了，于是便用ruby实现了一下，加强记忆

---

>冒泡排序
    
    class Array
      def bubble_sort
        1.upto self.size - 1 do |i|
          1.upto self.size - i do |j|
            self[j], self[j - 1] = self[j - 1], self[j] if self[j] > self[j - 1]
          end
        end
      end
      self
    end

    1.9.3p327:> a = [1, 3, 53, 64, 5, 7, 98, 34]
    => [1, 3, 53, 64, 5, 7, 98, 34] 
    1.9.3p327:> a.bubble_sort
    => [98, 64, 53, 34, 7, 5, 3, 1] 
