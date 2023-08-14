### EventListener （事件）

------

[TOC]

##### 01：EventListener（javax.swing.event）

- 由 AWT 组件所激发的各类事件的接口和类

- ```java
  public interface EventListener {
    // 一下接口均实现了此接口
  }
  ```

##### 02：MouseListener：鼠标（按下、释放、单击、进入或离开）事件的监听器接口

```java
// 实现此接口，并且注册到监听器中
public interface MouseListener extends EventListener {
    // 鼠标按键在组件上单击（按下并释放）时调用
    public void mouseClicked(MouseEvent e);
    // 鼠标按键在组件上按下时调用
    public void mousePressed(MouseEvent e);
    // 鼠标按键在组建上松开时调用
    public void mouseReleased(MouseEvent e);
    // 鼠标进入到组件上时调用
    public void mouseEntered(MouseEvent e);
    // 鼠标离开组件时调用
    public void mouseExited(MouseEvent e);
}
```

##### 03：MouseMotionListener：

- 用于接收组件上的鼠标移动事件的监听器接口 		

###### 方法

- void mouseDragged(MouseEvent e)  ：鼠标按键在组件上按下并拖动时调用；	
- void mouseMoved(MouseEvent e)：鼠标光标移动到组件上但无按键按下时调用；               	

##### 04：FocusListener

- 用于接收组件上的键盘焦点事件的监听器接口

###### 方法

- void focusGained(FocusEvent e) ：组件获得键盘焦点时调用      		
- void focusLost(FocusEvent e) ：组件失去键盘焦点时调用   	

##### 05：KeyListener

- 用于接收键盘事件（击键）的侦听器接口

###### 方法	

- void keyPressed(KeyEvent e) ：按下某个键时调用此方法    		
- void keyReleased(KeyEvent e)  ：释放某个键时调用此方法 		
- void keyTyped(KeyEvent e) ：键入某个键时调用此方法

```java
textField = new JTextField(); 
textField.setFont(new Font("黑体", Font.BOLD, 16)); 	
// 匿名内部类，重写其方法
textField.addFocusListener(new FocusListener() {
      @Override
      public void focusGained(FocusEvent arg0） {
        // 获取焦点时执行此方法
        label.setText("文本框获得焦点，正在输入内容");
      }
      @Override
      public void focusLost(FocusEvent arg0) {
        // 失去焦点时执行此方法
        label.setText("文本框失去焦点，内容输入完成");
      }
});
```
