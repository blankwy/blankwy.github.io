---
abbrlink: 
categories:
- [技术分享,开源]
date: '2025-01-27T20:00:41.100487+08:00'
tags:
- python
- manim
- 动画
- 频率分布直方图
- 样本估计总体
- 视频
- 开源
- 数学
- 学习
title: 频率分布直方图manim动画源码
updated: '2025-01-27T20:06:15.473+08:00'
---
b站视频动画manim代码开源

```python
from manim import *
import numpy as np

class main(Scene):
    def construct(self):
        chart = BarChart(
            values=[0.010,0.015,0.035,0.030,0.010],
            y_range=[0,0.050,0.010],
            bar_width=1,
            x_axis_config={"include_numbers": False,"include_tip":True},
            y_axis_config={"include_numbers": False,"include_tip":True},
  
        ).to_edge(RIGHT)

        ylabel=Text("频率/组距",font_size=20).next_to(chart.get_y_axis(),RIGHT+UP)
        xlabel=Text("x",font_size=20).next_to(chart.get_x_axis(),RIGHT+DOWN)
        self.add(ylabel,xlabel)
  
        dot_positions = [0.5,1.5, 2.5,3.5, 4.5]
        middle_dots=[Dot(chart.coords_to_point(position,0,0)) for position in dot_positions]

        c_bar_lbls = chart.get_bar_labels(
            color=WHITE, label_constructor=MathTex, font_size=36
        )
  

        self.add(chart,c_bar_lbls)


        #取中
        dot_positions = [5, 15, 25, 35, 45]
        middle_dots = [Dot(chart.coords_to_point(position/10, 0, 0)) for position in dot_positions]
        label_md = [Text(str(point_num),font="Sans Serif",font_size=20).next_to(md, DOWN) for md, point_num in zip(middle_dots, dot_positions)]
        self.play(FadeIn(*middle_dots),FadeIn(*label_md)) #取每组中点
        self.wait()
        mdlist=[]
        mdTransform=[]
        for i,md_input in enumerate(dot_positions):
            if i==0:

                mdtext=MathTex(f"m_{i+1}={md_input}",color=RED).to_edge(UL)
            else:
                mdtext=MathTex(f"m_{i+1}={md_input}",color=RED).next_to(mdlist[i-1],DOWN)
            mdlist.append(mdtext)
            mdTransform.append(FadeTransform(label_md[i],mdlist[i]))
        valuelist=[]
        valueTransform=[]
        for i1,value in enumerate(chart.values):#再取它们的频率
            valuetext=MathTex(f"f_{i1+1}={value}")
            if i1==0:
                valuetext=MathTex(f"f_{i1+1}={value}",r" \times 10",color=YELLOW).next_to(mdlist[4],DOWN,aligned_edge=LEFT)
            else:
                valuetext=MathTex(f"f_{i1+1}={value}",r" \times 10",color=YELLOW).next_to(valuelist[i1-1],DOWN,aligned_edge=LEFT)
  
            valuelist.append(valuetext)
            valueTransform.append(FadeTransform(c_bar_lbls[i1],valuelist[i1]))
        labelGroup=AnimationGroup(mdTransform,valueTransform)
        self.play(labelGroup)
        self.wait()
        #众数
        hightmost=Circumscribe(chart.bars[2],Rectangle)
        self.play(hightmost,Wiggle(valuelist[2]))#频率最高的组
        self.play(Circumscribe(mdlist[2],Rectangle))#的中点即为众数
        self.wait()


  

        #平均数
        average_formula=MathTex(r"\overline{x} = \sum_{n}^{i=1} ",r"{m_{i}}",r" f_{i} ",r" = {m_1} *f_1+{m_2} *f_2+\dots +{m_n} *f_n").to_edge(LEFT+UP)
        average_formula[1].set_color(RED)
        average_formula[2].set_color(YELLOW)#中点分别与其频率相乘再求和即为平均数
        self.play(AnimationGroup((FadeTransform(mdlist[i0],average_formula) for i0,mm in enumerate(mdlist)),(FadeTransform(valuelist[i01],average_formula) for i01,mm1 in enumerate(valuelist))))
        self.wait()
        self.play(FadeOut(average_formula,*middle_dots,*label_md))
        self.wait()

        #百分位数
        #以第40百分位数为例
        #将各组频率依次累加
        sumFrequence1=MathTex(f"f_1+f_2=0.25").to_edge(LEFT+UP)
        sumFrequence2=MathTex(f"f_1+f_2+f_3=0.60").next_to(sumFrequence1,DOWN)
        self.play(FadeIn(sumFrequence1),FadeIn(sumFrequence2))
        #用面积和代表频率频率，40%在0.25到0.60之间，故其对应的数据在第三组内，设其为b
        fortynumdot=Dot(chart.coords_to_point(2.4,0,0),color=RED)
        fdotlabel=Text("b",font_size=25,color=RED).next_to(fortynumdot,DOWN)
        self.play(FadeIn(fortynumdot,fdotlabel))
        self.wait()
        squarepart=Polygon(chart.coords_to_point(2,0,0),chart.coords_to_point(2.4,0,0),chart.coords_to_point(2.4,0.035,0),chart.coords_to_point(2,0.035,0),color=RED_B).align_to(chart.bars[2],LEFT+UP)
        self.play(FadeIn(squarepart))
        self.wait()#由该点与原矩形左侧部分围成矩形
        sb=MathTex(r"S_{b} = (b-20)\times 0.035").next_to(sumFrequence2,DOWN,aligned_edge=LEFT)
        self.play(FadeIn(sb))
        #计算其面积（也就是频率）
        sformula=MathTex(r"\frac{S_{b} + f_{1} +f_{2}}{S_{all}} = P").next_to(sb,DOWN)
        self.play(FadeIn(sformula))
        #其占总面积的比值即为你要求的百分比
        #总面积等于总频率即为1
        sformula1=MathTex(r"\frac{S_{b} + f_{1} +f_{2}}{1} = P").next_to(sb,DOWN)
        self.play(FadeTransform(sformula,sformula1))
        #本题要求的是四十百分位，故P为0.4
        sformula2=MathTex(r"\frac{S_{b} + f_{1} +f_{2}}{1} = 0.4").next_to(sb,DOWN)
        self.play(FadeTransform(sformula1,sformula2))
        #最后解出b≈24.28
        bfinal=MathTex(r"b\approx 24.28").next_to(sformula2,DOWN)
        self.play(FadeIn(bfinal))
        #即第40百分位数约为24.28
        self.play(Circumscribe(bfinal,Rectangle))
  

        #中位数
        #同理，第50百分位数即为中位数
        mediandot=Dot(chart.coords_to_point(2.6,0,0),color=GREEN)
        mdotlabel=Text("m",font_size=25,color=GREEN).next_to(mediandot,DOWN)
        self.play(FadeIn(mediandot,mdotlabel))
        self.wait()
        squarepart1=Polygon(chart.coords_to_point(2,0,0),chart.coords_to_point(2.6,0,0),chart.coords_to_point(2.6,0.035,0),chart.coords_to_point(2,0.035,0),color=GREEN).align_to(chart.bars[2],LEFT+UP)
        sm=MathTex(r"S_{m} = (m-20)\times 0.035").next_to(bfinal,DOWN,aligned_edge=LEFT)
        self.play(FadeIn(squarepart1),FadeIn(sm))
        self.wait()
        sformula3=MathTex(r"\frac{S_{m} + f_{1} +f_{2}}{1} = 0.5").next_to(sm,DOWN)
        self.play(FadeIn(sformula3))
        #最后解出m≈27.14
        mfinal=MathTex(r"m\approx 27.14").next_to(sformula3,DOWN)
        self.play(FadeIn(mfinal))
        #即中位数约为27.14
        self.play(Circumscribe(mfinal,Rectangle))
        self.wait()
        #搞定


  

  


```
