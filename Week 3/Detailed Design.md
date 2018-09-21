# 详细算法设计        
本组所实现的Markdown Editor是基于Windows的线下编辑器，算法设计思路如下：        
- 输入符合Markdown文法的字符串；               
- 设计算法将字符串解析为html格式；      
- 调用html解析模块进行显示；         

## 基于PDL语言的算法流程设计       
![YuHang](https://github.com/YuHang0121151624/Markdown-Editor-01/blob/master/img/2.jpg)       
根据标题引用和列表模块（Module1）流程图，设计对应程序描述语言如下：		

	PROCEDURE Title is:
		读取行首的 # 个数x
		解析为x级标题格式
	RETURN 解析后的html标题        

	PROCEDURE Quote is:
		读取行首的 > 个数y
		解析为y级引用格式
	RETURN 解析后的html引用        

	PROCEDURE List is:
		读取每个行首的 - 
			IF 行首不是 - THEN 退出
			ELSE 对每一行解析为列表
	RETURN 解析后的html列表        

	CASE 行首是# 
		Title();
	CASE 行首是>
		Quote();
	CASE 行首是-:
		List();
	DEFAULT 其他模块

![YuHang](https://github.com/YuHang0121151624/Markdown-Editor-01/blob/master/img/3.jpg)        
根据段落和代码模块（Module2）流程图，设计对应程序描述语言如下：		

	PROCEDURE Paragraph is:
		解析为段落换行格式
	RETURN 解析后的html段落换行        

	PROCEDURE Code is:
		读取每个行首的空格
			IF 行首不是四个空格 THEN 退出
			ELSE 解析为代码格式
	RETURN 解析后的多行html代码块        

	PROCEDURE CutOff is:
		将该行解析为分割线 
	RETURN 解析后的html分割线        

	CASE 行尾是四个以上空格 
		Paragraph();
	CASE 行首是四个空格
		Code();
	CASE 全行为- || 全行为=
		CutOff();
	DEFAULT 其他模块
![YuHang](https://github.com/YuHang0121151624/Markdown-Editor-01/blob/master/img/4.jpg)       			
根据强调和标记模块（Module3）流程图，设计对应程序描述语言如下：		

	PROCEDURE Emphasize is:
		中间文本解析为强调格式
	RETURN 解析后的html强调格式        

	PROCEDURE Mark is:
		中间文本解析为标记格式
	RETURN 解析后的html标记格式                

	CASE 行首是- && 行尾是-
		Paragraph();
	CASE 行首是< && 行尾是>
		Mark();
	DEFAULT 其他模块
![YuHang](https://github.com/YuHang0121151624/Markdown-Editor-01/blob/master/img/5.jpg)       				
根据链接和图片模块（Module4）流程图，设计对应程序描述语言如下：		

	PROCEDURE Link is:
		读取()中的地址
	RETURN 解析后的html链接格式        

	PROCEDURE Image is:
		读取()中的地址
		IF 相对地址 THEN 找到当前所在目录并读取
		ELSE IF 绝对地址 THEN 直接作为地址读取
		ELSE 退出 
	RETURN 解析后的html图片格式                

	CASE 该行为[]()格式
		Link();
	CASE 该行为![]()格式
		Image();
	DEFAULT 其他模块
![YuHang](https://github.com/YuHang0121151624/Markdown-Editor-01/blob/master/img/6.jpg)         				
根据导出文件模块（Module5）流程图，设计对应程序描述语言如下：		

	PROCEDURE Save is:
		用户输入保存目录
		IF 目录存在 THEN 将文本写入文件
		ELSE 创建新目录并保存
	END                      

# 数据定义         

根据总体设计和对具体流程的分析，初步将不同模块分别定义类：      
- 标题引用和列表模块 Module1.h , Module1.cpp         
- 段落和代码模块 Module2.h , Module2.cpp         
- 强调和标记模块 Module3.h , Module3.cpp         
- 链接和图片模块 Module4.h , Module4.cpp         
- 导出文件模块 Module5.h , Module5.cpp         
- 界面模块 EditorView.h , EditorView.cpp         
- 总模块 Main.cpp       

## 类图
