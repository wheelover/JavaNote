#form标签
<form>是块状标签，<form>标签不能嵌套<form>标签
属性：
method：它的值可以是GET或POST，用来规定如何发送表单信息
action：一个处理此表单信息的程序所在的url，所述表单信息将在表单提交时被发送到指定地址
可以在<form>里交互式表单控件，<!-- action=""则表单信息将提交到当前页面 -->

单行文本输入框<input>内联元素
```
<form action="">
  <input type="text" />
</form>
```
1. 占位文本“placeholder”
输入框名字“name”：为了给昵称输入框加上标签属性，作为<input>的名字，在提交表单数据的时候就不会和其他<input>搞混了
1. 输入框的值“value”，预填写内容
2. 不可修改的输入框“readonly”和“disable”：把输入框变成只读输入框


---------------------------------------------------------------------

多行文本输入框和密码输入框
多行输入<textarea>
属性：row和cols分别表示行数和文本域的可视宽度

--------------------------------------------------------------------



密码输入框：把<input>中标签属性type=“text“，type=”password”

单选框和复选框：
单选框：把控件类型type=“text“改成text=”radio”
因为是单选框，所以拥有相同的name属性
```
<input type="radio" name="gender" value="male" />男
<input type="radio" name="gender" value="female" />女
```

在标签后内容把选项内容写到网页上
使用label增大点击范围
```
<label> <input type="radio" name="gender" value="male" />男 </label>
<label> <input type="radio" name="gender" value="female" />女 </label>
```


复选框：
把type改成checkbox
```
<label> <input type="checkbox" name="interest" value="coding" />编程 </label>
<label> <input type="checkbox" name="interest" value="other" />其他 </label>
```


选项菜单
标签<select>,<option>
```
<select name="career">
  <option value="default">请选择职业</option>
  <option value="staff">公司职员</option>
  <option value="freelancer">自由职业者</option>
  <option value="student">学生</option>
  <option value="other">其他</option>
</select>
```
如果用户选择了学生，那么提交的数据将会是：areer:"student"
多选菜单给<select>标签添加multiple（按Ctrl进行多选）

按钮
<button>

```
<button type="submit">注册</button>
```