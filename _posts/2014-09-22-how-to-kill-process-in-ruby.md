---
layout: post
title: "查看进程是否存在，并杀死"
category: linux
tags: []
---
{% include JB/setup %}

有时候我们需要定时查看一些进程是否存在，不存在的话，需要重新启动。

---

>直接查看进程是否存在
		#!/usr/bin/env bash

		D=`date -d "seconds" "+%Y_%m_%d_%H_%M_%S" `
		echo $D
		proc_num=`ps ax | grep ruby | grep do_refund_action | wc -l`
		if [ $proc_num -eq 0 ]; then 
			echo 'start up do_refund_action'
			nohup /usr/local/rvm/rubies/ruby-2.1.1/bin/ruby /home/admin/web/pay/trunk/script/rails  runner -e production 'Refund.do_refund_action; Rails.logger.flush' & 
		else
			echo "do_refund_action is running ...."
		fi

>通过查看进程id来判断
		#!/bin/bash

		#根据某个字符串查找匹配到的pid
		function pidof_str
		{
			proc_str=$1
			proc_num=`ps ax | grep $proc_str | grep -v 'grep ' | awk 'BEGIN{s = "" }{ s=$1 " " s }END{print s}' `
			echo $proc_num
		}

		#1:存在, 0:不存在
		function proc_exist
		{
			proc_str=$1
			pid=$( pidof_str $proc_str )
			if [ "X" != "X$pid" ]; then
				echo 1 
			else
				echo 0
			fi
		}

		proc_str='gateway.rb'

		exist=$( proc_exist $proc_str )

		if [ $exist -eq 1 ]; then
			echo "$proc_str is running ..."
			exit 1
		fi

		root_dir=`dirname $0`

		nohup ruby ${root_dir}/gateway.rb ${root_dir}/../cfg/gateway.conf &>> ${root_dir}/gateway.log &
		echo "run $proc_str succ!"
