title: C++operator
date: 2016-01-19 10:50:06
tags: C++
---
1.  参考文件
```
platform/base/include/utils/TextOutput.h
```
2.  关于Operator
```
我们只能使用,C++预定义的操作符,主要是依靠Operator的重载,扩大操作符的作用范围.
virtual 为虚拟函数关键字,在有继承关系的类中,子类中可以覆盖父类中的方法.
typedef 是为了提高代码的可读性,为变量,方法起别名.
```
3.  TextOutput.h中的
```
class TextOutput
{
public:
                        TextOutput();
    virtual             ~TextOutput();//析构函数
    virtual status_t    print(const char* txt, size_t len) = 0;//打印函数
    virtual void        moveIndent(int delta) = 0;//缩进函数
    class   Bundle {
      public:
          inline Bundle(TextOutput& to) : mTO(to) { to.pushBundle(); }
          inline ~Bundle() { mTO.popBundle(); }
      private:
          TextOutput&     mTO;
    };
    virtual void        pushBundle() = 0;
    virtual void        popBundle() = 0;
};

TextOutput& endl(TextOutput& to);//换行
TextOutput& indent(TextOutput& to);//缩进
TextOutput& dedent(TextOutput& to);//反缩进
//打印相应数据类型
TextOutput& operator<<(TextOutput& to, const char* str);//打印char
TextOutput& operator<<(TextOutput& to, char);     // writes raw character
TextOutput& operator<<(TextOutput& to, bool);
TextOutput& operator<<(TextOutput& to, int);//打印
TextOutput& operator<<(TextOutput& to, long);
TextOutput& operator<<(TextOutput& to, unsigned int);
TextOutput& operator<<(TextOutput& to, unsigned long);
TextOutput& operator<<(TextOutput& to, long long);
TextOutput& operator<<(TextOutput& to, unsigned long long);
TextOutput& operator<<(TextOutput& to, float);
TextOutput& operator<<(TextOutput& to, double);
TextOutput& operator<<(TextOutput& to, TextOutputManipFunc func);
TextOutput& operator<<(TextOutput& to, const void*);

```
