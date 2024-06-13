# 第一部分：基础教程pyfastpaper 快速上手教程（针对Python 0基础的科研人员）

## 0  导入工具包

> Python下载pyfastpaper：``pip install pyfastpaper``

在你要存放图片的文件夹下右键，点击``在终端中打开(T)``（Windows 11系统），出现“黑窗”（术语为DOS界面），在里面输入``jupyter notebook``，按下回车，浏览器弹出，启动``Jupyter``，将该黑窗口最小化（千万别关！），浏览器编代码即可。


```python
from pyfastpaper import *   # 固定写法
%config InlineBackend.figure_format = 'retina'  # 提高分辨率
```


## 第一部分：提供数据来描点——`data_lines`函数

### 1.1 单数据描点
#### 例题1


```python
# 以方括号（列表、numpy数组均可）形式给出数据，并给这组数据起个名字：
data = {
    '旅游人次':[1230, 45789, 2600, 320, 991480, 65780, 
            89990, 70001, 6423, 415000, 340, 102]
}

data_lines(data)
```

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/data1.png?raw=true" style="display:inline; width:70%;"/>
    </div>
</div>    

#### 例题2


```python
# 以方括号（列表、numpy数组均可）形式给出数据，并给这组数据起个名字：
data = {
    '旅游人次': [1230, 45789, 2600, 320, 991480, 
             65780, 89990, 70001, 6423, 415000, 340, 102]
}

# 给横轴添加刻度标签，注意要和data长度一致！
label_x = ['2020-1', '2020-2', '2020-3', '2020-4',  '2020-5', 
           '2020-6',  '2020-7',  '2020-8',  '2020-9',  '2020-10',  
           '2020-11',  '2020-12']

# 自定义xy轴名称：
x_name = '月总旅游人次'
y_name = '月份'

# 保存路径
save_dir = 'outputs/data_sigle.tiff'

# 图片大小
figsize = [7, 4] # 宽 高

# 字号大小
fsize = 13

# x轴刻度旋转角度
xt_rotation = 45

data_lines(data, label_x=label_x, x_name=x_name, y_name=y_name, 
           save_dir=save_dir, fsize=fsize, 
           figsize=figsize, xt_rotation=xt_rotation)
```

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/data_sigle.png?raw=true" style="display:inline; width:90%;"/>
    </div>
</div>      



### 1.2 多数据描点


```python
# 以方括号（列表、numpy数组均可）形式给出数据，并给这组数据起个名字：
data = {
    '音乐1': [1, 2, 3, 3.5, 5, 6, 7, 8], 
    '音乐2': [8, 7, 6, 5, 3.5, 3, 2, 1],
    '音乐3': [1, 1, 5, 5, 6, 6, 5, 0],
    '音乐4': [3.5, 3.5, 3, 3, 2, 2, 1, 0],
}

# 给横轴添加刻度标签，注意要和data长度一致！
label_x = [1, 2, 3, 4,  5,  6,  7,  8]

# 自定义xy轴名称：
x_name = '节拍'
y_name = '音调'

# 保存路径
save_dir = 'outputs/data_multi.tiff'

# 图片大小
figsize = [7, 4] # 宽 高

# 字号大小
fsize = 13

# x轴刻度旋转角度
xt_rotation = 0

# 图例位置和列数
location='best'
ncol=4

data_lines(data, label_x=label_x, x_name=x_name, y_name=y_name, 
           save_dir=save_dir, fsize=fsize, figsize=figsize, 
           xt_rotation=xt_rotation,location=location, ncol=ncol)
```

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/data_multi.png?raw=true" style="display:inline; width:90%;"/>
    </div>
</div>  

## 第二部分：根据函数表达式数值仿真

### 2.1 定义数学符号，写出要画图的表达式


```python
# 定义符号
c_n, c_r, delta, e_n, e_r, p_e, E, k, b, alpha = symbols(
    					'c_n, c_r, delta, e_n, e_r, p_e, E, k, b, alpha')

# 四个表达式
Pir_NW = E*p_e+(k*(alpha*delta*(c_n+e_n*p_e)-
                   (c_r+e_r*p_e))**2)/(8*(k+alpha*delta*(1-alpha*delta))**2)
Pir_BW = E*p_e + ( k*(delta*(c_n+e_n*p_e)-
                      (c_r+e_r*p_e+b))**2 )/( 8*(k+delta-delta**2)**2)
Pir_NS = E*p_e + ((k+2*alpha*delta)*(alpha*delta*(c_n+e_n*p_e)-
                   (c_r+e_r*p_e))**2 )/( 8*(k+alpha*delta*(2-alpha*delta))**2)
Pir_BS = E*p_e + ( (k+2*delta)*(delta*(c_n+e_n*p_e)-
              (c_r+e_r*p_e+b))**2 )/( 8*(k+2*delta-delta**2)**2)

# 工具包内附赠的latex工具mathshow，用于将latex显示为Jupyter内的公式。
mathshow(r'\begin{equation} \pi_r^{NW}=' + latex(Pir_NW) + r'\tag{1} \end{equation}')
mathshow(r'\begin{equation} \pi_r^{BW}=' + latex(Pir_BW) + r'\tag{2} \end{equation}')
mathshow(r'\begin{equation} \pi_r^{NS}=' + latex(Pir_NS) + r'\tag{3} \end{equation}')
mathshow(r'\begin{equation} \pi_r^{BS}=' + latex(Pir_BS) + r'\tag{4} \end{equation}')
```


$\displaystyle \begin{equation} \pi_r^{NW}=E p_{e} + \frac{k \left(\alpha \delta \left(c_{n} + e_{n} p_{e}\right) - c_{r} - e_{r} p_{e}\right)^{2}}{8 \left(\alpha \delta \left(- \alpha \delta + 1\right) + k\right)^{2}}\tag{1} \end{equation}$



$\displaystyle \begin{equation} \pi_r^{BW}=E p_{e} + \frac{k \left(- b - c_{r} + \delta \left(c_{n} + e_{n} p_{e}\right) - e_{r} p_{e}\right)^{2}}{8 \left(- \delta^{2} + \delta + k\right)^{2}}\tag{2} \end{equation}$



$\displaystyle \begin{equation} \pi_r^{NS}=E p_{e} + \frac{\left(2 \alpha \delta + k\right) \left(\alpha \delta \left(c_{n} + e_{n} p_{e}\right) - c_{r} - e_{r} p_{e}\right)^{2}}{8 \left(\alpha \delta \left(- \alpha \delta + 2\right) + k\right)^{2}}\tag{3} \end{equation}$



$\displaystyle \begin{equation} \pi_r^{BS}=E p_{e} + \frac{\left(2 \delta + k\right) \left(- b - c_{r} + \delta \left(c_{n} + e_{n} p_{e}\right) - e_{r} p_{e}\right)^{2}}{8 \left(- \delta^{2} + 2 \delta + k\right)^{2}}\tag{4} \end{equation}$


### 2.2 画单条曲线图——`draw_lines`函数


```python
# 给表达式起个名字，写成字典的形式：
expressions = {r'$\pi_r^{NS}$': Pir_NS}
# 参数赋值：
assigns = {
    c_n: 0.2,
    c_r: 0.1,
    delta: 0.8,
    e_n: 1,
    e_r: 0.6,
    p_e: 0.1,
    E: 2,
    k: 1.1,
    b:0.05
}

# 要分析的参数，及其取值范围：
the_var = alpha
ranges = [0.7, 0.9, 0.02]  # [起始值, 终止值, 间隔]。 

# 横纵轴的名字：
x_name = r'$\rm{(b) \ Parameter \ } \alpha$'  # 可以写LaTex公式，用"$ $"括上。
y_name = r'$\pi_r^{NS}$'

# 图片保存路径、文件名
save_dir = r'outputs\sigle_line.tiff'

# y轴标签旋转角度
yrotation = 0

# 固定写法：
draw_lines(expressions=expressions, assigns=assigns, the_var=the_var, ranges=ranges, 
           x_name=x_name, y_name=y_name, save_dir=save_dir, yrotation=yrotation,
           ylabelpad=14)
```

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/sigle_line.png?raw=true" style="display:inline; width:70%;"/>
    </div>
</div>      

### 2.3 多条曲线图——`draw_lines`函数


```python
# 给表达式起个名字，写成字典的形式：
expressions = {
    r'$\pi_r^{NW}$': Pir_NW, 
    r'$\pi_r^{BW}$': Pir_BW, 
    r'$\pi_r^{NS}$': Pir_NS,
    r'$\pi_r^{BS}$': Pir_BS,
}

# 参数赋值：
assigns = {
    c_n: 0.2,
    c_r: 0.1,
    delta: 0.8,
    e_n: 1,
    e_r: 0.6,
    p_e: 0.1,
    E: 2,
    k: 1.1,
    alpha:0.9
}

# 要分析的参数，及其取值范围：
the_var = b
ranges = [0, 0.08, 0.01]  # [起始值, 终止值, 间隔]。

# 横纵轴的名字：
x_name = r'$\rm{(a) \ Parameter \ } b$'
y_name = r'$\pi_r$'

# 图片保存路径、文件名
save_dir = r'outputs/mutiple_line.tiff'

# 固定写法：
draw_lines(expressions=expressions, assigns=assigns, the_var=the_var, 
           ranges=ranges, x_name=x_name, y_name=y_name, save_dir=save_dir)
```

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/mutiple_line.png?raw=true" style="display:inline; width:70%;"/>
    </div>
</div>      


### 2.4 画单片三维图——`draw_3D`函数


```python
# 给表达式起个名字，写成字典的形式：
expressions = {r'$\pi_r^{NS}$': Pir_NS}

# 参数赋值：
assigns = {
    c_n: 0.2,
    c_r: 0.1,
    delta: 0.8,
    e_n: 1,
    e_r: 0.6,
    p_e: 0.1,
    E: 2,
    k: 1.1
}

# 要分析的参数，及其取值范围：
the_var_x = alpha
start_end_x = [0.7, 0.8]  # [起始值, 终止值]。
the_var_y = b
start_end_y = [0, 0.08]  # [起始值, 终止值]。

# xyz轴的名字：
x_name = r'$\alpha$' 
y_name = r'$b$'  
z_name = r'$\pi_r^{NS}$'

# 图片保存路径、文件名
save_dir = r'outputs\sigle_3d.tiff'

# 固定写法：
draw_3D(expressions=expressions, assigns=assigns, the_var_x=the_var_x, 
        start_end_x=start_end_x, the_var_y=the_var_y, 
        start_end_y=start_end_y, x_name=x_name, y_name=y_name, 
        z_name=z_name, save_dir=save_dir, zrotation=0, colors=['#707070']) 

# 说明：由于Jupyter窗口大小不够，显示不全，但导出的`sigle_3d.tiff`文件是全的，可以放心用！
```

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/sigle_3d.png?raw=true" style="display:inline; width:70%;"/>
    </div>
</div>      

### 2.5 画多片三维图——`draw_3D`函数


```python
# 给表达式起个名字，写成字典的形式：
expressions = {
    r'$\pi_r^{NW}$': Pir_NW, 
    r'$\pi_r^{BW}$': Pir_BW, 
    r'$\pi_r^{NS}$': Pir_NS,
    r'$\pi_r^{BS}$': Pir_BS,
}

# 参数赋值：
assigns = {
    c_n: 0.2,
    c_r: 0.1,
    delta: 0.8,
    e_n: 1,
    e_r: 0.6,
    p_e: 0.1,
    E: 2,
    k: 1.1
}

# 要分析的参数，及其取值范围：
the_var_x = alpha
start_end_x = [0.7, 0.8]  # [起始值, 终止值]。
the_var_y = b
start_end_y = [0, 0.08]  # [起始值, 终止值]。

# xyz轴的名字：
x_name = r'$\alpha$' 
y_name = r'$b$'
z_name = r'$\pi_r$'
# 图片保存路径、文件名
save_dir = r'outputs\muti_3d.tiff'

# 固定写法：
draw_3D(expressions=expressions, assigns=assigns, the_var_x=the_var_x, 
        start_end_x=start_end_x, the_var_y=the_var_y, 
        start_end_y=start_end_y, x_name=x_name, y_name=y_name, z_name=z_name, 
        save_dir=save_dir, color_alpha=0.8, precision=1000, zrotation=0, ncol=4)
```

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/muti_3d.png?raw=true" style="display:inline; width:70%;"/>
    </div>
</div>      



### 2.6 两个参数影响下，最大函数值所在区域图——`draw_max_area`函数


```python
# 给表达式起个名字，写成字典的形式：
expressions = {
    r'$\pi_r^{NW}$': Pir_NW, 
    r'$\pi_r^{BW}$': Pir_BW, 
    r'$\pi_r^{NS}$': Pir_NS,
    r'$\pi_r^{BS}$': Pir_BS,
}

# 参数赋值：
assigns = {
    c_n: 0.2,
    c_r: 0.1,
    delta: 0.8,
    e_n: 1,
    e_r: 0.6,
    p_e: 0.1,
    E: 2,
    k: 1.1
}

# 要分析的参数，及其取值范围：
the_var_x = alpha
start_end_x = [0.7, 0.8]  # [起始值, 终止值]。
the_var_y = b
start_end_y = [0, 0.08]  # [起始值, 终止值]。

# xyz轴的名字：
x_name = '$\\alpha$ \n $\\rm{(b) \\ With \\ blockchain}$' 
y_name = r'$b$'  

# 图片保存路径、文件名
save_dir = r'outputs\max_area.tiff' 

# 其它细节（可选，默认值即可）
texts = [r'$\rm{NW}$', r'$\rm{BW}$', r'$\rm{NS}$', r'$\rm{BS}$']  # 和表达式expressions对应处的函数值若最大，则标记的文本

# 固定写法：
draw_max_area(expressions=expressions, assigns=assigns, the_var_x=the_var_x, start_end_x=start_end_x, the_var_y=the_var_y, 
                  start_end_y=start_end_y, x_name=x_name, y_name=y_name, texts=texts, save_dir=save_dir, yrotation=0)
```

    区域 0: 中心坐标 = [0.7671627089581086, 0.0634762952000335]
    区域 1: 中心坐标 = [0.74544927 0.03695231]
    区域 2: 中心坐标 = [0.7224541825812423, 0.07136930428315474]

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/max_area.png?raw=true" style="display:inline; width:70%;"/>
    </div>
</div>      



### 2.7 两个参数影响下，画详细大小关系区域图——`draw_detail_area`函数
#### 2.7.1 例题1


```python
# 给表达式起个名字，写成字典的形式：
expressions = {
    r'$\pi_r^{NW}$': Pir_NW, 
    r'$\pi_r^{BW}$': Pir_BW, 
    r'$\pi_r^{NS}$': Pir_NS,
    r'$\pi_r^{BS}$': Pir_BS,
}

# 参数赋值：
assigns = {
    c_n: 0.2,
    c_r: 0.1,
    delta: 0.8,
    e_n: 1,
    e_r: 0.6,
    p_e: 0.1,
    E: 2,
    k: 1.1
}

# 要分析的参数，及其取值范围：
the_var_x = alpha
start_end_x = [0.7, 0.8]  # [起始值, 终止值]。
the_var_y = b
start_end_y = [0, 0.08]  # [起始值, 终止值]。

# xyz轴的名字：
x_name = '$\\alpha$ \n $\\rm{(b) \\ With \\ blockchain}$' 
y_name = r'$b$'  

# 图片保存路径、文件名
save_dir = r'outputs\detail_area1.tiff' 

# 固定写法：
draw_detail_area(expressions=expressions, assigns=assigns, the_var_x=the_var_x, 
                 start_end_x=start_end_x, the_var_y=the_var_y, 
                 start_end_y=start_end_y, x_name=x_name, y_name=y_name, 
                 save_dir=save_dir, yrotation=0)
```

```latex
区域 $\pi_r^{BW} > \pi_r^{BS} > \pi_r^{NS} > \pi_r^{NW}$: 中心坐标 = [0.71801738 0.03542176]
区域 $\pi_r^{BW} > \pi_r^{BS} > \pi_r^{NW} > \pi_r^{NS}$: 中心坐标 = [0.7522649316484371, 0.050000071576472446]
区域 $\pi_r^{NW} > \pi_r^{NS} > \pi_r^{BW} > \pi_r^{BS}$: 中心坐标 = [0.75467691 0.06691737]
区域 $\pi_r^{NS} > \pi_r^{NW} > \pi_r^{BW} > \pi_r^{BS}$: 中心坐标 = [0.72038526 0.07323454]
```

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/detail_area1.png?raw=true" style="display:inline; width:90%;"/>
    </div>
</div>  


#### 2.7.2 例题2


```python
# 定义符号
alpha, beta, c, x, y = symbols('alpha,beta,c,x,y')

# 三个表达式
exp1 = 2 * alpha * x - beta * y ** 2 + c * x * y
exp2 = 2 * alpha * x + 3 * beta * y + c * x * y
exp3 = alpha * x - 2 * beta * y + c * x * y

# 给表达式起个名字，写成字典的形式：
expressions = {r'$\Pi_1$': exp1,
               r'$\Pi_2$': exp2,
               r'$\Pi_3$': exp3}

# 参数（alpha,beta,c,y）赋值：
assigns = {alpha: 2, beta: 3, c: 1.5}

# 要分析的两个参数，及其绘制的范围：
the_var_x = x
start_end_x = [0, 20]  # [起始值, 终止值]。 参数x绘制范围在[-10,10]区间
the_var_y = y
start_end_y = [-20, 20]  # [起始值, 终止值]。 参数y绘制范围在[-20,20]区间

# 横纵轴的名字：
x_name = r'参数$x$' 
y_name = r'参数$y$'

# 图片保存路径、文件名
save_dir = r'outputs\detail_area2.tiff' 

# 固定写法：
draw_detail_area(expressions=expressions, assigns=assigns, the_var_x=the_var_x, 
                 start_end_x=start_end_x, the_var_y=the_var_y, 
                  start_end_y=start_end_y, x_name=x_name, y_name=y_name, 
                 save_dir=save_dir, yrotation=0, remove_eq=False, dropout=0.01)
```

```latex
区域 $\Pi_3 > \Pi_2 > \Pi_1$: 中心坐标 = [  9.99649702 -11.49728879]
区域 $\Pi_3 > \Pi_1 > \Pi_2$: 中心坐标 = [ 9.60920009 -2.26787271]
区域 $\Pi_1 > \Pi_2 > \Pi_3$: 中心坐标 = [10.27059755 -0.86965807]
区域 $\Pi_2 > \Pi_1 > \Pi_3$: 中心坐标 = [10.25020558  1.88464055]
区域 $\Pi_2 > \Pi_3 > \Pi_1$: 中心坐标 = [ 9.71049007 11.56355666]
```

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/detail_area2.png?raw=true" style="display:inline; width:90%;"/>
    </div>
</div>  

​    


# 高阶教程（需要具备一定Python基础）——工具包各个函数api介绍

相关学习资料：    
[Markdown学习网站](https://zhuanlan.zhihu.com/p/108984311)    
[LaTex学习网站](https://flowus.cn/latex/share/66110e84-b24a-4cd5-b8a7-2ba2afb35a30)

## 1. `data_lines`函数

```Python
data_lines(data, label_x=None, x_name=None, y_name=None, save_dir=None, 	
           location='best', ncol=1, fsize=14, figsize=[5, 4], xt_rotation=0, 
           xrotation=0, yrotation=90, linestyles=None, linewidth=1, markers=m_list, 
           markersize=3.5, colors=c_list, isgrid=False, xpad=3, 
           ypad=3, xlabelpad=3, ylabelpad=3)
```

- location: 图例的方位，可以选填的内容有：

  'best', 'northeast', 'northwest', 'southwest', 'southeast', 'east', 'west', 'south', 'north', 'center'。

  默认值为'best'，表示自动安排至合适的位置。

- ncol: 图例的列数，默认为1列，即竖着排布。

- fsize: 图片中字号的大小，默认值为14。

- figsize: 图片的大小，写成`[宽, 高]`的形式。默认为`[5, 4]`。

- xt_rotation: 横轴刻度标签旋转角度。用于刻度为年份，横着挤不下的情况，可以设成45度，错开排布。默认不旋转，即0度。

- xrotation: 横轴名字标签旋转角度，默认值0，基本不需要动。

- yrotation: 纵轴名字标签旋转角度，默认值90，字是正的。如果y轴的名字较长，不好看，可以设成0，字是竖倒着写的，紧贴y轴，但看的话需要歪脖子，专治脊椎不好使哈。

- linestyles: 一组线的形状，`[]`列表形式去写，在多线图中用于按顺序制定每个线的形状。如实线'-'，点横线'-.'，虚线'--'，点线':'。默认值为None，表示取工具内的线型列表`ls_list`得第一个，即实线'-'。    
  本工具中提供的线型列表如下：    
  ``ls_list = ['-', (0, (1, 10)), (0, (5, 10)),'-.','--', ':', (0, (3, 10, 1, 10, 1, 10)), (5, (10, 3)), (0, (5, 5)), (0, (5, 1))]``    
  关于线型的详细说明：    
  [有关linestyles的具体说明](https://matplotlib.org/stable/gallery/lines_bars_and_markers/linestyles.html#linestyles)

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://matplotlib.org/stable/_images/sphx_glr_linestyles_001.png" style="display:inline; width:90%;"/>
    </div>
</div>

- linewidth: 线粗。字面意思，没啥好说的，默认值为1。
- markers: 一组标记符号，`[]`列表形式去写，在多线图中用于按顺序制定每个线的标记符号。工具内的标记符号列表`m_list`。如果只有一条线，就取`m_list`中的第一个，即圆点'o'。    
  本工具中提供的标记符号列表如下：     
  ``m_list = ['o','s', '*', 'P', 'X', 'D', 'p', 'x', '8', '2', 'H', '+', '|', '<', '>', '^', 'v', ',']``    
  关于标记符号的详细说明：
  [有关marker的具体说明](https://matplotlib.org/stable/api/markers_api.html#module-matplotlib.markers)。

- markersize: 标记符号的大小，默认3.5。
- colors: 一组颜色，`[]`列表形式去写，在多线图中用于按顺序制定每个线的颜色（包含标记符号的颜色）。工具内的颜色列表`c_list`。如果只有一条线，就取`c_list`中的第一个，即蓝色'blue'。    
  本工具中提供的颜色列表如下：     
  ``c_list = ['blue', 'gold', 'green', 'red', 'darkgoldenrod', 'darkcyan', 'indigo',
          'chocolate', 'olivedrab', 'teal', 'navy', 'firebrick','darkgreen', 'purple', 'cadetblue']``    
  关于颜色的详细说明：
  [有关color的具体说明](https://matplotlib.org/stable/gallery/color/named_colors.html#base-colors)

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://matplotlib.org/stable/_images/sphx_glr_named_colors_003.png" style="display:inline; width:90%;"/>
    </div>
</div>

- isgrid: 是否要网格。要就填True，不要就是False，默认不要。
- xpad=3, ypad=3, xlabelpad=3, ylabelpad=3: 分别为横轴刻度值距离横轴的距离、纵轴刻度值距离纵轴的距离、横轴名字标签距离横轴刻度的距离、纵轴名字标签距离纵轴刻度的距离。默认值3,3,3,3。如果挤了不好看了，再微调此参数，一般不用动。

## 2. `draw_lines`函数

```Python
draw_lines(expressions, assigns, the_var, ranges, x_name, y_name, save_dir=None, 
           location='best', ncol=1, fsize=14, figsize=[5, 4], 
           xt_rotation=0, xrotation=0, yrotation=90, linestyles=None, linewidth=1, 
           markers=m_list, markersize=3.5, colors=c_list, 
           isgrid=False, x_lim=None, y_lim=None, xpad=3, ypad=3, xlabelpad=3, 
           ylabelpad=3)
```

大部分参数和`data_lines`函数相同。不再赘述。特别说明的有：    

- x_lim: 横轴显示的范围，以`[起始值,结束值]`的形式去写。默认None，根据数据自动安排。除非不好看再调，一般不动该参数。
- y_lim: 纵轴显示的范围，以`[起始值,结束值]`的形式去写。默认None，根据数据自动安排。除非不好看再调，一般不动该参数。举个例子，“2.2 画单条曲线图”部分不太好看：


```python
expressions = {r'$\pi_r^{NS}$': Pir_NS}

assigns = {c_n: 0.2, c_r: 0.1, delta: 0.8, e_n: 1, e_r: 0.6, p_e: 0.1, E: 2, k: 1.1, b:0.05}

the_var = alpha
ranges = [0.7, 0.9, 0.02] 

x_name = r'$\rm{(b) \ Parameter \ } \alpha$'  
y_name = r'$\pi_r^{NS}$'

draw_lines(expressions=expressions, assigns=assigns, the_var=the_var, ranges=ranges, 
           x_name=x_name, y_name=y_name, yrotation=0, ylabelpad=14)
```

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/sigle_line.png?raw=true" style="display:inline; width:70%;"/>
    </div>
</div>      



可将其改为：


```python
draw_lines(expressions=expressions, assigns=assigns, the_var=the_var, 
           ranges=ranges, x_name=x_name, y_name=y_name, 
           yrotation=0, ylabelpad=14,  y_lim=[0.2, 0.20025])
```

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/sigle_line2.png?raw=true" style="display:inline; width:70%;"/>
    </div>
</div>  

​    


## 3. `draw_3D`函数

```Python
draw_3D(expressions, assigns, the_var_x, start_end_x, the_var_y, start_end_y, 
        x_name, y_name, z_name, save_dir=None, colors=c_list, color_alpha=0.8, 
        linestyles=ls_list, linewidth=0.2, location='best', ncol=1, fsize=14, 
        figsize=[7, 5], precision=1000, xrotation=0, yrotation=0, zrotation=90, 
        isgrid=True, edgecolor=None, density=100, x_lim=None, y_lim=None, 
        z_lim=None, elevation=15, azimuth=45, roll=0, left_margin=0, 
        bottom_margin=0, right_margin=1, top_margin=1, xpad=1, ypad=1, zpad=5, 
        xlabelpad=2, ylabelpad=2, zlabelpad=12)
```

大部分参数和`data_lines`函数相同。不再赘述。特别说明的有： 

- color_alpha: 曲面的透明度。取值范围0到1，浮点数。0表示全透明，1表示完全不透明。默认取0.8。
- precision: 绘画的精细程度。默认取1000，表示画 $1000 \times 1000$ 个点。该值越大，运行速度越慢，太大没必要，根据个人情况权衡。
- zrotation: Z轴名字标签旋转角度，默认值90，字是正的。如果Z轴的名字较长，不好看，可以设成0，字是竖倒着写的，紧贴Z轴，但看的话需要歪脖子，专治脊椎不好使哈。
- edgecolor: 曲面上线框的颜色。若为None，则曲面上不画线。当该参数不为None时，参数`linestyles`，`linewidth`和`density`才起作用。
- density: 曲面上画线的密度，也就是曲面横纵方向各画多少根线。默认100。
- elevation: 仰角 (elevation)。定义了观察者与 xy 平面之间的夹角，也就是观察者与 xy 平面之间的旋转角度。当elevation为正值时，观察者向上倾斜，负值则表示向下倾斜。默认15度。可根据美观与否微调。
- azimuth: 方位角 (azimuth)。定义了观察者绕 z 轴旋转的角度。它决定了观察者在 xy 平面上的位置。azim 的角度范围是 −180 到 180 度，其中正值表示逆时针旋转，负值表示顺时针。默认45度。可根据美观与否微调。
- roll: 滚动角 (roll)。 定义了绕观察者视线方向旋转的角度。它决定了观察者的头部倾斜程。默认0度，不需要动。
- left_margin=0, bottom_margin=0, right_margin=1, top_margin=1: 左、下、右、上的图片留白，默认分别为0,0,1,1。不需要动，除非不好看。

关于视角elevation和azimuth，举个例子：


```python
expressions = {r'$\pi_r^{NW}$': Pir_NW, r'$\pi_r^{BW}$': Pir_BW, r'$\pi_r^{NS}$': Pir_NS, r'$\pi_r^{BS}$': Pir_BS,}
assigns = {c_n: 0.2, c_r: 0.1, delta: 0.8, e_n: 1, e_r: 0.6, p_e: 0.1, E: 2, k: 1.1}

the_var_x = alpha
start_end_x = [0.7, 0.8]  
the_var_y = b
start_end_y = [0, 0.08]  

x_name = r'$\alpha$' 
y_name = r'$b$'
z_name = r'$\pi_r$'

draw_3D(expressions=expressions, assigns=assigns, the_var_x=the_var_x, start_end_x=start_end_x, the_var_y=the_var_y, 
        start_end_y=start_end_y, x_name=x_name, y_name=y_name, z_name=z_name, color_alpha=0.8, precision=1000, zrotation=0, ncol=4)
```

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/muti_3d.png?raw=true" style="display:inline; width:70%;"/>
    </div>
</div>  

```python
draw_3D(expressions=expressions, assigns=assigns, the_var_x=the_var_x, 
        start_end_x=start_end_x, the_var_y=the_var_y, start_end_y=start_end_y, 
        x_name=x_name, y_name=y_name, z_name=z_name, color_alpha=0.8, 
        precision=1000, zrotation=0, ncol=4, elevation=100, azimuth=270)
```

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/multi_3d2.png?raw=true" style="display:inline; width:70%;"/>
    </div>
</div>  

​    

## 4. `draw_max_area`函数

```Python
draw_max_area(expressions, assigns, the_var_x, start_end_x, the_var_y, start_end_y, 
              x_name, y_name, texts=None, save_dir=None, 
              precision=1000, fsize=14, figsize=[5, 4], theme='sci', xrotation=0, 
              yrotation=90, linewidths=0.1, isgrid=False, 
              xpad=3, ypad=3, xlabelpad=3, ylabelpad=5)
```

大部分参数和上面讲过的函数相同。不再赘述。特别说明的有：

- texts: 表达式expressions中的函数值若分别达到最大，则相应区域应分别标记的文本，以`[]`形式写。默认None，按照expressions提供的名字标记。注意：标签个数和expressions中的函数顺序要对应。
- theme: 主题。即各区域的配色方案。一般sci论文要保证黑白打印出来能看清，因此，默认值为'sci'，表示符合学术论文美感的灰度配色方案。若``theme=None``，则表示全白色。    
  用户可以以列表`['颜色名称或16进制颜色值']`的形式提供一套配色方案，16进制颜色值取法可参考下图：        
<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/pic1.png?raw=true" style="display:inline; width:70%;"/>
    </div>
</div>   



例如：`theme=['#ffffff', '#F0F0F0', '#E6E6E6', '#DCDCDC', '#D2D2D2', '#C8C8C8', '#BEBEBE', '#B4B4B4', 
 '#AAAAAA', '#A0A0A0', '#969696', '#919191', '#8C8C8C', '#878787', '#828282', '#7D7D7D', 
 '#787878', '#6E6E6E', '#646464']`。

 工具中（确切地说是Python的Matplotlib）提供了现成的配色方案（例如'viridis', 'plasma', 'inferno', 'magma', 'cividis'等），具体完整方案和效果列示如下：    
 <div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://matplotlib.org/stable/_images/sphx_glr_colormap_reference_001.png" style="display:inline; width:100%;"/>
    </div>
    <div style="flex: 1; text-align: center;">
        <img src="https://matplotlib.org/stable/_images/sphx_glr_colormap_reference_002.png" style="display:inline; width:100%;"/>
    </div>
</div>
<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://matplotlib.org/stable/_images/sphx_glr_colormap_reference_003.png" style="display:inline; width:100%;"/>
    </div>
    <div style="flex: 1; text-align: center;">
        <img src="https://matplotlib.org/stable/_images/sphx_glr_colormap_reference_004.png" style="display:inline; width:100%;"/>
    </div>
</div>
<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://matplotlib.org/stable/_images/sphx_glr_colormap_reference_005.png" style="display:inline; width:100%;"/>
    </div>
    <div style="flex: 1; text-align: center;">
        <img src="https://matplotlib.org/stable/_images/sphx_glr_colormap_reference_006.png" style="display:inline; width:100%;"/>
    </div>
</div>
<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://matplotlib.org/stable/_images/sphx_glr_colormap_reference_007.png" style="display:inline; width:100%;"/>
    </div>
</div>

举几个例子，来看看这些方案的效果如何：


```python
expressions = {
    r'$\pi_r^{NW}$': Pir_NW, 
    r'$\pi_r^{BW}$': Pir_BW, 
    r'$\pi_r^{NS}$': Pir_NS,
    r'$\pi_r^{BS}$': Pir_BS,}

assigns = {c_n: 0.2, c_r: 0.1, delta: 0.8, e_n: 1, e_r: 0.6, p_e: 0.1, E: 2, k: 1.1}

the_var_x = alpha
start_end_x = [0.7, 0.8] 
the_var_y = b
start_end_y = [0, 0.08]  

x_name = '$\\alpha$ \n $\\rm{(b) \\ With \\ blockchain}$' 
y_name = r'$b$'  

texts = [r'$\rm{NW}$', r'$\rm{BW}$', r'$\rm{NS}$', r'$\rm{BS}$']  


draw_max_area(expressions=expressions, assigns=assigns, the_var_x=the_var_x, start_end_x=start_end_x, the_var_y=the_var_y, 
             start_end_y=start_end_y, x_name=x_name, y_name=y_name, texts=texts, yrotation=0)
```

    区域 0: 中心坐标 = [0.7671627089581086, 0.0634762952000335]
    区域 1: 中心坐标 = [0.74544927 0.03695231]
    区域 2: 中心坐标 = [0.7224541825812423, 0.07136930428315474]

 <div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/max_area.png?raw=true" style="display:inline; width:70%;"/>
    </div>
</div>  


```python
draw_max_area(expressions=expressions, assigns=assigns, the_var_x=the_var_x, 
              start_end_x=start_end_x, the_var_y=the_var_y, 
              start_end_y=start_end_y, x_name=x_name, y_name=y_name,
              texts=texts, yrotation=0, theme=None)
```

    区域 0: 中心坐标 = [0.7671627089581086, 0.0634762952000335]
    区域 1: 中心坐标 = [0.74544927 0.03695231]
    区域 2: 中心坐标 = [0.7224541825812423, 0.07136930428315474]

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/maxarea_white.png?raw=true" style="display:inline; width:70%;"/>
    </div>
</div>  


```python
draw_max_area(expressions=expressions, assigns=assigns, the_var_x=the_var_x, 
              start_end_x=start_end_x, the_var_y=the_var_y, 
              start_end_y=start_end_y, x_name=x_name, y_name=y_name, 
              texts=texts, yrotation=0, theme='viridis')
```

    区域 0: 中心坐标 = [0.7671627089581086, 0.0634762952000335]
    区域 1: 中心坐标 = [0.74544927 0.03695231]
    区域 2: 中心坐标 = [0.7224541825812423, 0.07136930428315474]

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/maxarea_viridis.png?raw=true" style="display:inline; width:70%;"/>
    </div>
</div>  


```python
draw_max_area(expressions=expressions, assigns=assigns, the_var_x=the_var_x, 
              start_end_x=start_end_x, the_var_y=the_var_y, 
              start_end_y=start_end_y, x_name=x_name, y_name=y_name, 
              texts=texts, yrotation=0,
              theme= ['red', 'yellow', 'blue', 'green', 'gold', 'orange', 'cyan'])
```

    区域 0: 中心坐标 = [0.7671627089581086, 0.0634762952000335]
    区域 1: 中心坐标 = [0.74544927 0.03695231]
    区域 2: 中心坐标 = [0.7224541825812423, 0.07136930428315474]

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/maxarea_color.png?raw=true" style="display:inline; width:70%;"/>
    </div>
</div>      


## 5. `draw_detail_area`函数

```Python
draw_detail_area(expressions, assigns, the_var_x, start_end_x, the_var_y, start_end_y, 
                 x_name, y_name, save_dir=None, 
                 precision=1000, fsize=14, figsize=[7, 4], theme='sci', xrotation=0, 
                 yrotation=90, linewidths=0.1, 
                 isgrid=False, xpad=3, ypad=3, xlabelpad=3, ylabelpad=5, 
                 prefix=r'$\rm{Region}$', 
                 numbers='roman', reverse=True, remove_eq=True, dropout=0.01)
```

各参数含义同`draw_max_area`函数以及之前讲过的函数。需要特别说明的地方如下：

- prefix: 前缀。可以是``"区域"``也可以是``"$\rm{Region}$"``，默认``"$\rm{Region}$"``，是以LaTeX格式写的新罗马字体的英文，显示为"$\rm{Region}$"。
- numbers: 序号标记风格。有三种可选："roman", "letter" 和"number"，分别表示罗马数字、大写英文字母和阿拉伯数字。默认"roman"。
- reverse: 默认True，表示降序排布，即$A>B>C...$的形式。若为False，则升序排布$A<B<C...$。
- remove_eq: 是否不考虑等式？默认True，不带等式。若为False，则带等式，如$A>B=C>D..$。
- dropout: 点数占比小于多少不考虑？用于排除一些边界部分十分微小的区域。默认0.01。这个参数不要去动。

举几个例子，看看效果：


```python
alpha, beta, c, x, y = symbols('alpha,beta,c,x,y')

exp1 = 2 * alpha * x - beta * y ** 2 + c * x * y
exp2 = 2 * alpha * x + 3 * beta * y + c * x * y
exp3 = alpha * x - 2 * beta * y + c * x * y

expressions = {r'$\Pi_1$': exp1, r'$\Pi_2$': exp2, r'$\Pi_3$': exp3}

assigns = {alpha: 2, beta: 3, c: 1.5}

the_var_x = x
start_end_x = [0, 20]  
the_var_y = y
start_end_y = [-20, 20]  

x_name = r'参数$x$' 
y_name = r'参数$y$'
 
draw_detail_area(expressions=expressions, assigns=assigns, the_var_x=the_var_x, 
                 start_end_x=start_end_x, the_var_y=the_var_y, 
                  start_end_y=start_end_y, x_name=x_name, y_name=y_name, yrotation=0, 
                 remove_eq=False, dropout=0.01)
```

    区域 $\Pi_3 > \Pi_2 > \Pi_1$: 中心坐标 = [  9.99649702 -11.49728879]
    区域 $\Pi_3 > \Pi_1 > \Pi_2$: 中心坐标 = [ 9.60920009 -2.26787271]
    区域 $\Pi_1 > \Pi_2 > \Pi_3$: 中心坐标 = [10.27059755 -0.86965807]
    区域 $\Pi_2 > \Pi_1 > \Pi_3$: 中心坐标 = [10.25020558  1.88464055]
    区域 $\Pi_2 > \Pi_3 > \Pi_1$: 中心坐标 = [ 9.71049007 11.56355666]

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/detail_area2.png?raw=true" style="display:inline; width:90%;"/>
    </div>
</div>  


```python
draw_detail_area(expressions=expressions, assigns=assigns, the_var_x=the_var_x, 
                 start_end_x=start_end_x, the_var_y=the_var_y, 
                 start_end_y=start_end_y, x_name=x_name, y_name=y_name, 
                 yrotation=0, remove_eq=True, dropout=0.01, prefix='区域', 
                 numbers='letter', reverse=False)
```

    区域 $\Pi_1 < \Pi_2 < \Pi_3$: 中心坐标 = [  9.99649702 -11.49728879]
    区域 $\Pi_2 < \Pi_1 < \Pi_3$: 中心坐标 = [ 9.60920009 -2.26787271]
    区域 $\Pi_3 < \Pi_2 < \Pi_1$: 中心坐标 = [10.27059755 -0.86965807]
    区域 $\Pi_3 < \Pi_1 < \Pi_2$: 中心坐标 = [10.25020558  1.88464055]
    区域 $\Pi_1 < \Pi_3 < \Pi_2$: 中心坐标 = [ 9.71049007 11.56355666] 

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/detail2_2.png?raw=true" style="display:inline; width:90%;"/>
    </div>
</div>   


```python
draw_detail_area(expressions=expressions, assigns=assigns, the_var_x=the_var_x, 
                 start_end_x=start_end_x, the_var_y=the_var_y, 
                 start_end_y=start_end_y, x_name=x_name, y_name=y_name, yrotation=0, 
                 remove_eq=True, dropout=0.01, theme='coolwarm', 
                 prefix='区域',  numbers='number', reverse=False)
```

    区域 $\Pi_1 < \Pi_2 < \Pi_3$: 中心坐标 = [  9.99649702 -11.49728879]
    区域 $\Pi_2 < \Pi_1 < \Pi_3$: 中心坐标 = [ 9.60920009 -2.26787271]
    区域 $\Pi_3 < \Pi_2 < \Pi_1$: 中心坐标 = [10.27059755 -0.86965807]
    区域 $\Pi_3 < \Pi_1 < \Pi_2$: 中心坐标 = [10.25020558  1.88464055]
    区域 $\Pi_1 < \Pi_3 < \Pi_2$: 中心坐标 = [ 9.71049007 11.56355666]

<div style="display: flex;">
    <div style="flex: 1; text-align: center;">
        <img src="https://github.com/Jesse-tien/pyfastpaper/blob/6a09e651d801424aceeaeda4e40acd2ead258420/src/detail2_3.png?raw=true" style="display:inline; width:90%;"/>
    </div>
</div>      

