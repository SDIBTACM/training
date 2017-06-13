# 周报

#### 本周总结
> 写完了 Java 的结课作业，答辩轻松过~~~
> BUT  日语和离散。。。

#### 下周计划
> 还是上周计划的数据结构的“串”、“数组和广义表”和"树和二叉树"


#### 本周学到的一些小东西
###### 不写博客了， 放这里吧
``` java
/**之前想在面板里加背景图片，查了几个API，没有找到合适的。。。
	然后发现其实可以弄一个带背景图的面板:就是继承下JPanel，然后加上背景
	之后的实例化直接用这个就可以*/
public class MyPanel extends JPanel {  
    ImageIcon icon;  
    Image img;  
    public MyPanel() {
        icon=new ImageIcon(getClass().getResource("/img/a.jpg"));  
        img=icon.getImage();  
    }  
    public void paintComponent(Graphics g) {  
        super.paintComponent(g);
        g.drawImage(img, 0, 0,this.getWidth(), this.getHeight(), this);  
    }  
} 


/**想加个验证码，后来发现好麻烦，就模拟了一下   ~.~   哈哈哈*/
int count=0;
JLabel verificationCode = new JLabel("验证码：");
verificationCode.setForeground(Color.YELLOW);
final JTextField vcTextField = new JTextField(4);
String image="D:\\...\\...\\...\\0.jpg";    //验证码的路径
JButton vcImge = new JButton(new ImageIcon(image));
vcImge.addActionListener(new ActionListener() {
	public void actionPerformed(ActionEvent e) {
		if(count==9)
			count=0;
		else
			count++;
		String image="D:\\...\\...\\...\\"+count+".jpg";
		JButton vcImge=(JButton)e.getSource();
		vcImge.setIcon(new ImageIcon(image));
	}
});
final String[] vcStr={"3v23","m3mb","13zx","b1xb","1m1x","b3bn","xbv2","zzbc","czcn","cb22"};

/**验证码的正误判断*/

String t4 = vcTextField.getText();
if("".equals(t4)){
	JOptionPane.showMessageDialog(null, "验证码不能为空!");
}else if(!(t4.equals(vcStr[count]))){
	JOptionPane.showMessageDialog(null, "验证码不正确!");
}
				
```
