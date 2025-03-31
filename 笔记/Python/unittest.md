### unittest

#### 基础使用

```py
# 1. 导包 unittest
import unittest


# 2. 定义测试类, 只要继承 unittest.TestCase 类, 就是 测试类
class TestDemo(unittest.TestCase):
    # 3. 书写测试方法, 方法中的代码就是真正用例代码, 方法名必须以 test 开头
    def test_method1(self):
        print('测试方法一')

    def test_method2(self):
        print('测试方法二')


# 4. 执行
# 4.1 在类名或者方法名后边右键运行
# 4.1.1 在类名后边, 执行类中的所有的测试方法
# 4.1.2 在方法名后边, 只执行当前的测试方法

# 4.1 在主程序使用使用 unittest.main()  来执行,
if __name__ == '__main__':
    unittest.main()
```

#### 套件对象.addTest(测试类名('测试方法名'))

```py
# testcase1
# 1. 导包 unittest
import unittest


# 2. 定义测试类, 只要继承 unittest.TestCase 类, 就是 测试类
class TestDemo1(unittest.TestCase):
    # 3. 书写测试方法, 方法中的代码就是真正用例代码, 方法名必须以 test 开头
    def test_method1(self):
        print('测试方法1-1')

    def test_method2(self):
        print('测试方法1-2')


```

```py
# testcase2
# 1. 导包 unittest
import unittest


# 2. 定义测试类, 只要继承 unittest.TestCase 类, 就是 测试类
class TestDemo2(unittest.TestCase):
    # 3. 书写测试方法, 方法中的代码就是真正用例代码, 方法名必须以 test 开头
    def test_method1(self):
        print('测试方法2-1')

    def test_method2(self):
        print('测试方法2-2')


```

`套件添加测试类里面的方法`

```py
# 1. 导包  unittest
import unittest
from testcase1 import TestDemo1
from testcase2 import TestDemo2

# 2. 实例化套件对象 unittest.TestSuite()
suite = unittest.TestSuite()

# 3. 添加用例方法
# 3.1 套件对象.addTest(测试类名('测试方法名'))  # 建议复制
suite.addTest(TestDemo1('test_method1'))
suite.addTest(TestDemo1('test_method2'))
suite.addTest(TestDemo2('test_method1'))
suite.addTest(TestDemo2('test_method2'))
# 4. 实例化 执行对象 unittest.TextTestRunner()
runner = unittest.TextTestRunner()
# 5. 执行对象执行 套件对象 执行对象.run(套件对象)
runner.run(suite)
```

`套件添加测试类`

```py
# 1. 导包  unittest
import unittest
from hm_02_testcase1 import TestDemo1
from hm_02_testcase2 import TestDemo2

# 2. 实例化套件对象 unittest.TestSuite()
suite = unittest.TestSuite()

# 3. 添加用例方法
# 3.2 添加整个测试类
# 套件对象.addTest(unittest.makeSuite(测试类名))  # 在不同的 Python 版本中,可能没有提示
suite.addTest(unittest.makeSuite(TestDemo1))
suite.addTest(unittest.makeSuite(TestDemo2))
# 4. 实例化 执行对象 unittest.TextTestRunner()
runner = unittest.TextTestRunner()
# 5. 执行对象执行 套件对象 执行对象.run(套件对象)
runner.run(suite)

```

#### 实例化加载对象并加载用例,得到套件对象

```py
import unittest

# 实例化加载对象并加载用例,得到套件对象
# suite = unittest.TestLoader().discover('用例所在的目录', '用例代码文件名*.py')
suite = unittest.TestLoader().discover('.', 'testcase*.py')
# 实例化执行对象并执行
# runner = unittest.TextTestRunner()
# runner.run(suite)

unittest.TextTestRunner().run(suite)

```
