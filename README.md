# task-from-Mobinets-of-UESTC
A Simulation program of new coronavirus spreading based on Matlab

前言：
----
这个文档是介绍一种基于matlab开发的新冠病毒传播的仿真程序，此程序的最初开发者为CSDN的“UESTC 五高考3模拟”用户；
源程序链接如下：https://blog.csdn.net/weixin_43267645/article/details/105253949

此项目解决问题介绍：
----
1、基于前人经验和代码，对程序ui部分进行优化，设置“累计感染人数”变化曲线<br>
<br>
2、基于源程序，模拟采取4种不同防治措施，检测其对新冠病毒传播的抑制效果，4种措施分别为:<br>
1）、佩戴口罩（wear_mask）; 2)、限制感染者移动范围（infected_restricted_move）<br>
3)、限制全部人员移动范围（all_restricted_move）; 4）、隔离感染人员（infected_ioslation）<br>
<br>
3、基于上述不同模拟结果进行分析，并给出防治措施建议<br>

正文：
----
一、代码思路介绍 <br>
首先设置均值向量以及协相关矩阵，并利用 mvnrnd 函数构造二维正态随机分布矩阵，并从中抽取100个作为实验样本<br>
利用 sub2ind 函数得到样本的序列索引，并初始化健康者（healthy）、携带者（carrier）、感染者（infected）三个矩阵<br>
利用randi函数构造第一个携带者；约定当携带者携带病毒达到 4 天时变为感染者<br>
<br>
当第一个感染者出现时，对感染者（5×5）的范围进行判断，判断其中是否有健康者，当有健康者时，对健康者进行一定概率（pro=0.6）的病毒传播<br>
若传播成功，则健康者成为携带者，对健康者矩阵中相应元素进行修改；否则，不进行改变<br>
<br>
再进行人群模拟移动的实现：分别对感染者、健康者、携带者三个种群进行移动程序的实现，基于每个样本当前回合所处位置并利用randi函数随机确定下一回合本样本的出现位置<br>
<br>
最后基于感染者、健康者、携带者的人数和所在位置进行可视化实现，图中红色点代表感染者，蓝色点代表携带者、绿色点代表健康者<br>

二、基于四种不同的防治措施，进行模拟<br>
1、不采取措施（no_measures）<br>
![image](https://github.com/Swite007/task-from-Mobinets-of-UESTC/blob/main/no_measures.gif)<br>
2、佩戴口罩（wear_mask），效果：降低病毒传播概率（pro由初始的 0.6 降为 0.2）<br>
![image](https://github.com/Swite007/task-from-Mobinets-of-UESTC/blob/main/wear_mask.gif)<br>
3、限制感染者移动范围（infected_restricted_move）,效果：减少感染者的移动范围（由 ±7 的移动范围降为 ±2）<br>
![image](https://github.com/Swite007/task-from-Mobinets-of-UESTC/blob/main/infected_restricted_move.gif)<br>
4、限制全部人员的移动范围（all_restricted_move），效果：减少所有人员的移动范围（由 ±7 的移动范围降为 ±2）<br>
![image](https://github.com/Swite007/task-from-Mobinets-of-UESTC/blob/main/all_restricted_move.gif)<br>
5、隔离感染人员（infected_ioslation），效果：限制感染人员的活动范围为（[0,0]~[5,5]）<br>
![image](https://github.com/Swite007/task-from-Mobinets-of-UESTC/blob/main/infected_ioslation.gif)<br>
<br>
三、结果分析：
通过上述模拟结果，我们可以发现<br>
1、佩戴口罩 和 隔离感染人员对新冠病毒传播的抑制有极为明显的作用，感染人数可由未采取措施（no_measures）时的 62人（60天）降到 4人 和 2人<br>
2、限制感染者移动范围和限制全部人员的移动范围对新冠病毒传播的抑制也有较为明显的作用，感染人数可由未采取措施（no_measures）时的 62人（60天）降到 23人 和 19人<br>

四、相关建议以及现实依据：<br>
1、基于以上结果，我们给出的防治建议为<br>
1）、感染人员隔离（优先级：1）<br>
2）、全员佩戴口罩（优先级：2）<br>
3）、非必要不外出、不串门，自觉减少活动范围（优先级：3）<br>
<br>
2、以上建议的现实依据以及优先级理由为<br>
1）结合我国以及世界其他抗疫成功国家的经验，在疫情刚出现时，对感染以及疑似人员进行迅速隔离、对感染区域进行消杀都是第一时间和最直接的措施，此措施执行方便，见效快，故列为第一优先级<br>
2）依据世界各国卫生组织和相关防疫专家结论，佩戴口罩可以有效切断感染者与健康者之间的传播渠道，从而降低感染概率，但由于口罩需要大多数人都佩戴才有效，且在准备不充分的情况下，口罩很有可能供应不足，且口罩消耗量较大，故列为第二优先级<br>
3）依据我国于2020年初的成功抗疫经验，减少人员外出，减少活动范围，对病毒传播的范围的减小，有较大的作用。但由于此类行动波及面较大，且对经济社会有较大的影响，故列为第三优先级<br>
<br>
备注<br>
上述程序中有一个较为明显的 bug 为：程序运行到一定时间时，总人数不足100人，结合原作者的经验和个人分析为：样本移动过程中的位置重叠和更新状态所导致，由于时间较为紧迫且对最终结果影响不明显，未对此bug进行进一步修复。可将此效果视为样本人群中感染人员的死亡或者健康人群的迁移。
