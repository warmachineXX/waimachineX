# warmachineX


## 交大之光
代码到实物课程_交大之光小组

## 1、团队介绍：
团队名称为交大之光，由陈海潮，杨雄，闫坤，袁舒蕾四个成员组成。团队创建于2020年，3月1日。该团队坚持虚心若渴，求知若愚的团风，以着创新未来，挑战自我的口号凝聚为一股力量。四名创始人都为2019级大一新生，倚着敢拼敢闯的精神，用大胆的想象勾勒未来蓝图。

## 2、人员分工：

职位	     姓名	       github id          工作

组长	    陈海潮      warmachineXX      模型建立

组员       杨雄         313902          代码编写

组员       闫坤	        A7kun           演讲展示

组员      袁舒蕾	      August1zz        产品设计

## 3、设计思考：
  我们小组设计的产品是一款帮助矫正坐姿的椅子，主要面向青少年，使得小朋友在成长时可以最大程度避免驼背的困扰。思路如下：通过在椅坐上放置压力传感器控制整个系统的开合，避免在无人时装置误触发，通过椅背上的红外测距装置，测出坐在椅子上的人背部的倾斜角度，当超过一定角度后，会触发振动电机发出轻微震动进行提醒。我们希望可以通过这样的产品减少驼背的困扰。思考：1.如果应用于学校，价格是否太高？2.学生如果总是驼背，是否会对一直提醒感到厌烦？

## 4、模型图纸展示：

![image1](模型展示1.png)
![image2](模型展示2.png)
![image3](模型展示3.png)
![image4](模型展示4.png)
   
## 5、代码展示：

### 一，步进电机
功能：利用HX711模块读取压力值，之后通过步进电机实现压力的反馈
![image1](HX711.png)
HX711接线图如图所示。
hx711读数为0.000（最大量程5Kg，小数点厚=后三位有效数字），步进电机按照煤千分位走2步（步进电机1.8°，无细分）的设计进行（百分位20步，十分位200步，个位2000步），附上代码：代码临时改写还有很多问题，应该增加一个中断判断每次的压力量，不应每次使x进行复位，以后有时间会继续修改：
#include <motor.h>
#include <HX711.h>
float Weight = 0;
void setup()
{   Serial.begin(9600);
  Init_motor();
  Init_Hx711();
  Serial.print("Welcome to use!\n");
  Get_Maopi();
  delay(3000);  }
void loop()
{  delay(100);
  int x = 0;
  Weight = Get_Weight();  //计算放在传感器上的重物重量
  Serial.print(float(Weight/1000),3); //串口显示重量
  Serial.print(" kg\n");  //显示单位
  Serial.print("\n");   //显示单位
  delay(200);        //延时1s
  x = abs(Weight);
  int x_1 = x/100;//x/100 *1 circle
  PUT_N_ForwardCircle(x_1);
  int x_2 = (x%100)/100;  //x%100/10  1/10;
  PUT_N_ForwardCircle(x_2);
  int x_3 = (((x%100)/10)%10)*2;
  PUT_N_Up_Step(x_3);
  delay(1000);
  PUT_N_BackCircle((x_1)+(x_2));
  PUT_N_Down_Step(x_3);   }
### 二，红外测距及红外发出装置
Arduino 读红外测距传感器GP2D12 实例，仅供大家参考！
器材：Arduino 开发板，GP2D12，1602 字符液晶，连接线若干。
GP2D12 是日本SHARP 公司生产的红外距离传感器，价格便宜，测距效果还不错，主要用于模型或机器人制作。
技术规格如下：
探测距离：10-80cm
工作电压：4-5.5V
标准电流消耗：33-50 mA
输出量：模拟量输出，输出电压和探测距离成比例
实验原理图
