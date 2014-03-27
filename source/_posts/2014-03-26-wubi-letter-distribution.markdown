---
layout: post
title: "五笔输入法字母频率分布"
date: 2014-03-26 22:30:45 +0800
comments: true
categories: 
---

最近在学五笔的时候，想到了一个问题：“用五笔和拼音打字时按键的频率分别是怎样的？”

网上搜了下，找到了拼音的但没有找到五笔的，于是就写个程序来探究一下。

下面是查到的数据，作为参考[（来源）](http://zhidao.baidu.com/link?url=3iIPaXVJQg19Dw08ElPft4Nj5EZjK15-YgDy6sMvz0onvAGZxs6ntnTYFXclOw2fBU89RsLxyJ4P7NvCi6FJjq)

	统计数据：字母出现频率百分数
	范文：中华人民共和国国家通用语言文字法（含新闻稿及主席令等）
	拼写文字：汉语拼音方案（不加声调符号，词语连写）
	附注：不计标点符号、阿拉伯数字、空格，只计算字母（包括大写、小写）
	字母频率（大写/小写）
	Aa(0.03/8.19)Bb(0.14/1.30)Cc(0.07/0.61)Dd(0.10/2.46)Ee(0.02/6.33)Ff(0.10/1.11)Gg(0.37/9.13)Hh(0.05/5055)Ii(0.03/12.89)Jj(0.10/2.19)Kk(0.00/0.20)Ll(0.02/0.64)Mm(0.02/0.84)Nn(0.02/12.54)Oo(0.02/7.00)Pp(0.00/0.68)Qq(0.03/0.59)Rr(0.02/0.83)Ss(0.05/2.19)Tt(0.07/1.72)Uu(0.03/9.37)Vv(0.00/0.00)Ww(0.08/1.77)Xx(0.17/1.00)Yy(0.03/5.49)Zz(0.19/3.04)
	合计排序：
	I(12.93) N(12.56) G(9.50) U(9.40) A(8.22) O(7.02) E(6.35) H(5.60) Y(5.52) Z(3.20) D(2.57) J(2.30) S(2.24) W(1.86) T(1.80) B(1.43) F(1.22) X(1.16) M(0.86) R(0.84) P(0.68) C(0.68) L(0.66) Q(0.62) K(0.20) V(0.00)
	字母总数（含大小写）5925。

<!-- more -->

开工
---

程序的整体思路很简单：把一篇相当长的文本转换成分别用五笔和拼音全拼输入对应的英文字母，统计其中各字母出现的频率。

Python代码如下：（第一次写Python，边查资料边写的，欢迎批评指正。表示Python挺方便的，就是还不会调试。。）

	# -*- coding: utf-8 -*-
	
	textfilename = 'text.txt'
	wubi = True
	#wubi = False
	
	print 'Wubi stats:' if wubi else 'Pinyin stats:'
	
	dictfilename = 'wubi86.dict.txt' if wubi else 'pinyin_simp.dict.txt'
	import csv
	with open(dictfilename) as csvfile:
	    spamreader = csv.reader(csvfile, delimiter='\t')
	    lookupdict = {}
	    for row in spamreader:
	        if row[0] in lookupdict:
	            #in rare cases, such as '绿'
	            #will have two or more records in dict file
	            #the later code 'lu' will replace 'lv'
	            #ignore later one
	            continue 
	        lookupdict[row[0]] = row[1]
	
	import string
	result = {}
	for letter in string.lowercase:
	    result[letter] = 0
	
	textfile = open(textfilename)
	text = textfile.read()
	text = unicode(text, 'utf-8')
	textwordcount = 0
	for char in text:
	    char = char.encode('utf-8')
	    if char not in lookupdict:
	        continue;
	    textwordcount += 1
	    code = lookupdict[char]
	    for codeletter in code:
	        result[codeletter] += 1
	
	print 'words: ' + str(textwordcount)
	
	total = 0
	for letter in result:
	    total += result[letter]
	total = float(total)
	
	stat = {}
	for letter, count in result.items():
	    stat[letter.upper()] = count / total * 100;
	
	letterDescList = sorted(stat, key = stat.get, reverse = True)
	for letter in letterDescList:
	    print letter + '\t' + '%.3f%%' % (stat[letter])


wubi86.dict.txt和pinyin_simp.dict.txt是提取自小狼毫的字库文件，text.txt是一篇18万字的文本。 完整的文件在我的github上：<https://github.com/renzhn/WubiLetterDistribution>


统计结果
---
####Using Wubi
	Wubi stats:
	words: 182387
	G	6.857%
	F	6.699%
	T	6.476%
	W	6.325%
	D	5.238%
	J	5.014%
	R	4.743%
	H	4.680%
	U	4.631%
	K	4.095%
	Y	4.063%
	N	4.010%
	P	3.545%
	Q	3.530%
	M	3.504%
	B	3.404%
	L	3.313%
	A	3.290%
	C	3.162%
	I	3.005%
	E	2.824%
	S	2.233%
	X	2.067%
	V	1.856%
	O	1.437%
	Z	0.000%

####Using Pinyin
	Pinyin stats:
	words: 182385
	I	13.352%
	N	11.770%
	A	10.203%
	E	8.196%
	U	7.885%
	G	6.829%
	H	6.659%
	O	5.429%
	D	4.017%
	Z	3.440%
	J	2.835%
	S	2.741%
	Y	2.639%
	L	2.215%
	B	1.687%
	X	1.413%
	T	1.371%
	M	1.367%
	R	1.177%
	C	1.100%
	F	0.951%
	Q	0.858%
	W	0.812%
	K	0.698%
	P	0.294%
	V	0.061%

可以看出，五笔的字母频率分布更均匀。表明用五笔打字除了速度快、可以熟悉字形之外还有利于熟悉键盘键位。