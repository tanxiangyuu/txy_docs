# CS61A  

### lab02

Q2: 要注意函数的return

```py
def cake():
...    print('beets')
...    def pie():
...        print('sweets')
...        return 'cake'
...    return pie
```

```PY
chocolate = cake() #首先会调用cake，执行print，然后会将pie返回给chocolate\
# print('beets')
# chocolate = pie()
```

Q3：lambda 匿名函数

```py
lambda x：x * x

def lambda_curry(func):
    return lambda x: func(x)
#要注意这种参数传入情况

a = lambda f, x: -f(x)
```

一定要注意函数的参数传入，在主函数中的参数传入可以用到lambda函数中可以直接使用

返回表达值。不允许出现赋值和控制语句。

Lambda 表达式十分受限：它们仅仅可用于简单的单行函数，求解和返回一个表达式。

Q7：high-order-function

在hof中，一定要注意参数的传递以及return的类型

```py
def cycle(f1, f2, f3):
    def run(n):
        def f(x):
            num_f = n
            while num_f >= 3:
                x = f3(f2(f1(x)))
                num_f -= 3
            if num_f == 1:
                x = f1(x)
            elif num_f == 2:
                x = f2(f1(x))
            return x
        return f
    return run 
"""Returns a function that is itself a higher-order function.

    >>> def add1(x):
    ...     return x + 1
    >>> def times2(x):
    ...     return x * 2
    >>> def add3(x):
    ...     return x + 3
    >>> my_cycle = cycle(add1, times2, add3)
    >>> identity = my_cycle(0)  #传递n
    >>> identity(5)             #传递x
    5
“”“
```

## disc3 recursive递归

### recursive leap of faith

递归调用时，一定要注意传递的参数的变化（伴随着参数增加或者减少），总的来说递归是减少问题的规模来达到简化问题的效果。

### HW02

### the use of recursive

Q4 ：count_coins    

递归使用时，**注意**：递归到最底层时，应该返回的**边界值或者是计算值**，这是递归调用的精髓！！！

```py
def next_largest_coin(coin):
    if coin == 1:
        return 5
    elif coin == 5:
        return 10
    elif coin == 10:
        return 25
    
 def count_coins(total):
    def num_coins(rest_total, kind_coins):
        if rest_total == 0:
            return 1 
        elif rest_total < 0:
            return 0
        elif not kind_coins:
            # next_largest_coin在coin=15时，返回none，使得kind_coins为none     
            return 0         
        else:
            next_coins = next_largest_coin(kind_coins)
            a = num_coins(rest_total-kind_coins, kind_coins)
            b = num_coins(rest_total, next_coins)
            return a + b
    return num_coins(total, 1)
   """
    1. 怎么样才能划定一种情况的结束？？前面的return条件就是在递归结束时应该返回的值，在什么情况返回什么值要弄清楚（分情况讨论！！！！）
    当rest_total = 0 时，说明这一种计算过程递归计算结束，总数加1，return 1.
    当rest_total < 0 时，说明总数不够这一次的kind_coins减，所以return 0.
    not kind_coins是为了防止next_largest_coin返回none值给next_coins
    
   2. 这是从最低端推到最高端，也就是从最小值推向最大值，不断递归可以先计算在往下递归，也可以递归到底在进行计算，对应的是a和b的不同顺序。
    a在前时，先在当前条件下计算后往后面递归；
    	先执行rest_total-kind_coins，先将总数不断减少，也就从1开始加，1+1+1...
    	后执行b，往大数递归，使coins变5，10，25
    b在前时，先往后面递归，递归到最后面时，返回值，并进行a的计算
    	先执行b，从大的开始算。
   """ 
```

## 2. Data

### data abstraction

数据和程序应该是独立的两个部分

数据就可以用list，set，dic等来表示

list：`[1, 2]`   ，`+`表示链接list，`*`表示将list重复

```py
digits = [1, 8, 2]
>>>[2, 7] + digits * 2
[2, 7, 1, 8, 2, 1, 8, 2]		
```



| list | `[1, 2]` |
| :--: | :------: |
|      |          |
|      |          |

getitem(name, position):元素选择函数

void abstraction barriers，尽量避免抽象障碍，能抽象的就抽象，修改方便。

### sequences  序列

#### 2.3.5 Strings

1. 有长度
2. 可以选择元素
3. can be combined：用`+`表示链接 、`*`表示重复
4. membership：`in`匹配的是字符串而不是元素

#### 2.3.6 Trees

```py
>>> def tree(root_label, branches=[]):
        for branch in branches:
            assert is_tree(branch), 'branches must be trees'
        return [root_label] + list(branches)
>>> def label(tree):
        return tree[0]
>>> def branches(tree):
        return tree[1:]

>>> def is_tree(tree):
        if type(tree) != list or len(tree) < 1:
            return False
        for branch in branches(tree):
            if not is_tree(branch):
                return False
        return True
        
```

tree recursive

```py
>>> def fib_tree(n):
        if n == 0 or n == 1:
            return tree(n)
        else:
            left, right = fib_tree(n-2), fib_tree(n-1)
            fib_n = label(left) + label(right)
            return tree(fib_n, [left, right])
>>> fib_tree(5)
[5, [2, [1], [1, [0], [1]]], [3, [1, [0], [1]], [2, [1], [1, [0], [1]]]]]
```

#### lab04

##### Q: max_subseq(n, t) 

```py
def max_subseq(n, t):
    """
    Return the maximum subsequence of length at most t that can be found in the given number n.
    For example, for n = 20125 and t = 3, we have that the subsequences are
        2
        0
        1
        2
        5
        20
        21
        22
        25
        01
        02
        05
        12
        15
        25
        201
        202
        205
        212
        215
        225
        012
        015
        025
        125
    and of these, the maxumum number is 225, so our answer is 225.

    >>> max_subseq(20125, 3)
    225
    >>> max_subseq(20125, 5)
    20125
    >>> max_subseq(20125, 6) # note that 20125 == 020125
    20125
    >>> max_subseq(12345, 3)
    345
    >>> max_subseq(12345, 0) # 0 is of length 0
    0
    >>> max_subseq(12345, 1)
    5
    """
```

~~思路：先将数字转换成列表，在分别组成。可是不会了，嘤嘤~~

题解思路：

There are two key insights for this problem:

- You need to split into the cases where the ones digit is used and the one where it is not. In the case where it is, we want to reduce `t` since we used one of the digits, and in the case where it isn't we do not.
- In the case where we are using the ones digit, you need to put the digit back onto the end, and the way to attach a digit `d` to the end of a number `n` is `10 * n + d`.
- 就是说先把这个数的最后一位数先分解出来，然后分成使用这个数和不使用这个数两种情况。使用这个数时，t要减1（位数被占了一位）；不使用时不用减。然后返回两种情况中大的那一部分。**递归调用**。
- 为什么从最后一位先开始，因为数的顺序没有改变，所以从最后一位开始。
- 正解：

```py
def max_subeq(n, t):
	if t == 0:
        return 0
    elif n < 10:
        return n
    else:
        rest_not_last, last = n // 10, n % 10
        uselast = max_subseq(rest_not_last, t - 1) * 10 + last
        not_uselast = max_subseq(rest_not_last, t)
    return max(uselast, not_uselast)
```

一定要理解uselast那部分的 `max_subseq(rest_not_last, t - 1) * 10 + last`这句是关键。

##### add characters（w1， w2）

字符匹配："sing" is a substring of "ab**s**orb**ing**" and "cat" is a substring of "**c**ontr**a**s**t**".

要求：相同字母出现时，以最左为准：`add_words("coy", "cacophony")` should return       			"acphon", not "caphon" because the first "c" in "coy" corresponds to the first "c" in  			"**c**ac**o**phon**y**".

​			递归解决。

note：涉及到字符串的使用，所以标记一下使用，没那么难，多做！

解题思路：

- 先判断递归到最后一位，字符串为空，应该返回什么，即递归结束条件

- 将w1，和w2从首个字符开始匹配，匹配成功，两个字符同时后退一位，匹配不成功，w1无需后退，w2后退，并将不匹配的字符返回。

  ```py
  def add_chars(w1, w2):
      if w2 == '':
          return ''
      elif w1 != '' and w1[0] == w2[0]:
          return add_chars(w1[1:], w2[1:])
      else:
          return w2[0] + add_chars(w1[:], w2[1:])
  ```

#### dic04

```py
def count_stair_way(n):
    if n == 1:
        return 1
    elif n == 2:
        return 2
     return  count_stair_way(n - 1) + count_stair_way(n - 2) 
```

## cat

1. `min`，`max`函数的key选项使用：**max(arg1, arg2, \*args, \*[, key=func]) -> value**

    key应当传入一个可调用对象，一般传入的是函数。指定key之后，max函数就会根据**key处理后**的元素进行比较。**`lambda x`中 x 是代入的valid_words中的每个元素。**

    ```py
    def autocorrect(user_word, valid_words, diff_function, limit):
        """Returns the element of VALID_WORDS that has the smallest difference
        from USER_WORD. Instead returns USER_WORD if that difference is greater
        than LIMIT.
        """
        # BEGIN PROBLEM 5
        if user_word in valid_words:
            return user_word
    
        possble_str = min(valid_words, key= lambda x: diff_function(x, user_word, limit))
        if diff_function(possble_str, user_word, limit) > limit:
            return user_word
        else:
            return possble_str
    
    ```

    

2. list 的浅复制。

    ```py
    list_two = [[0] * 3] * 3
    print(list_two)
    
    list_two[1][1] = 2
    print(list_two)
    
    >>>[[0, 0, 0], [0, 0, 0], [0, 0, 0]]
    >>>[[0, 2, 0], [0, 2, 0], [0, 2, 0]]
    ```

    原因是浅拷贝，我们以这种方式创建的列表，list_two 里面的三个列表的内存是指向同一块，不管我们修改哪个列表，其他两个列表也会跟着改变。

3. 这段代码要仔细看

    ```py
    def fastest_words(game):
        player_indices = range(len(all_times(game)))  # contains an *index* for each player
        word_indices = range(len(all_words(game)))    # contains an *index* for each word
        # BEGIN PROBLEM 10
        result = []
        for _ in player_indices:
            result.append([])
        # this definition will always sync the lists in the list, as * is a Shallow Copy process
        # result = [[]] * len(all_times(game))
        # print('DEBUG: result =', result, 'len =', len(all_times(game)))
    
        for w in word_indices:
            find_player_time_for_word_w = lambda player: time(game, player, w)
            fatest_player = min(player_indices, key=find_player_time_for_word_w)
            # print('DEUBG: the fatest player for word:', word_at(game,w), 'is:', fatest_player)
            result[fatest_player].append(word_at(game, w))
    
        return result
        # END PROBLEM 10
    ```

    
