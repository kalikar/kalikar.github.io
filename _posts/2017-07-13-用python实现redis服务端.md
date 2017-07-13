---
layout: post
title: "用python语言实现redis服务端"
date:  2017-07-13 21:14:13
description: python
permalink: post/python_redis_server
disqus:
   id: kalikar
categories:
- python redis server
---
这是一个用python实现redis服务端的功能，目前只支持set,get方法。

<pre><code>import socket
import sys

class Redis_server:
	s = None
	arr = {}

	def __init__(self,host,port):
		try:
			self.s = socket.socket()
		except socket.error:
			print "server error"
			sys.exit(1)
		try:
			self.s.bind((host,port))
			self.s.listen(5)
		except socket.error:
			print "bind error"
			sys.exit(1)

	def __parseJson(self, data):
		type = data[:1]
		state = data.find('\r')
		total = data[1:state]
		if  type == '*':
			count = int(total)
			res = []
			for i in range(count):
				if i == 0:
					data = data[state+2:]
				else:
					length = state+2
					temp_count = int(data[1:state])
					data = data[length+temp_count+2:]
				res.append(self.__parseJson(data))
			return res
		elif type == '$':
			length = state +2
			ret = data[length:length+int(total)]
			return ret
		else:
			return False

	def worker(self):
		while True:
			c, addr = self.s.accept()
			data = c.recv(1024)
			res = self.__parseJson(data)
			send_msg = self.sendMessage(res)
			if send_msg:
				c.send(send_msg)

	def sendMessage(self,data):
		if data == False:
			return False
		if data[0] == 'SET':
			key = data[1]
			val = data[2]
			self.arr[key] = val
			send_msg = "+OK" + "\r\n"
		elif data[0] == 'GET':
			key = data[1]
			send_msg = "$" + str(len(self.arr[key])) + "\r\n" + self.arr[key] + "\r\n"
		else:
			send_msg = "+OK" + "\r\n"

		return send_msg
	def run(self):
		self.worker()	

	def __del__(self):
		self.s.close()

server = Redis_server("127.0.0.1", 6356)
server.run()
</code></pre>

