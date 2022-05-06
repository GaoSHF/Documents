# list #
	list有序集合，可以随时添加和删除其中元素
	插入          insert(index,xxx)
	删除末尾元素   pop()
	删除指定位置   pop(i)
	替换          如:classmates[1] = 'Sarah'
# tuple #
	tuple有序列表，元组，一旦初始化不能修改
	可变的tuple
	>>> t = ('a', 'b', ['A', 'B'])
	>>> t[2][0] = 'X'
	>>> t[2][1] = 'Y'
	>>> t
	('a', 'b', ['X', 'Y'])
	相当于变的只是list中的元素