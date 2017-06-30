---
layout: post
title: "用python语言实现redis客户端"
date:  2017-06-30 10:23:13
description: python
permalink: post/zhan
disqus:
   id: zhan
categories:
- python redis 
---

为了学习python，找了一个小功能当试手。这是一个用python实现redis客户端的功能，目前只支持set,get方法。

<pre><code>
import socket
import re

class Mredis:
	s = 0
	def __init__(self):
		self.s = socket.socket()
		self.s.connect(("127.0.0.1", 6379))

	def set(self, *arg):
		arg = ("SET",) + arg
		res = self._deal_before(*arg)
		self.s.send(res)
		ret = self.s.recv(1024)
		after_res = self._deal_after(ret)
		return after_res

	def get(self, *arg):
		arg = ("GET",) + arg
		res = self._deal_before(*arg)
		self.s.send(res)
		ret = self.s.recv(1024)
		return self._deal_after(ret)

	def _deal_before(self, *arg):
		crlf  = "\r\n"
		count = len(arg)
		stt   = "*" + str(count) + crlf
		for item in arg:
			leng = len(str(item))
			stt += "$" + str(leng) + crlf
			stt += str(item) + crlf
		return stt

	def _deal_after(self, args):
		if args[0] == "+":
			return "OK"
		elif args[0] == "-":
			return 0
		elif args[0] == ":":
			pass
		elif args[0] == "$":
			str_split = re.split("\r\n", args)
			return str_split[1]
		else:
			return 0


	def __del__(self):
		self.s.close()

r = Mredis()
print r.set('zhan', '{"a":"b","c":2}')
print r.get('zhan')
</code></pre>

