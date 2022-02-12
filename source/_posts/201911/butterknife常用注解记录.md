title: butter knife 常用注解记录
date: '2019-11-14 10:26:02'
updated: '2021-01-05 16:52:11'
tags: [butterknife, Android]
permalink: /articles/2019/11/14/1573698361728.html
cover: https://tmx.fishpi.cn/img/bird-6946682_1920.jpg
categories: 
- 菜鸡修炼手册

---
![中国汉服127.jpg](https://tmx.fishpi.cn/img/bird-6946682_1920.jpg)

`OnTextChanged`老是记不住，抄的作业，找地方存着而已

```java
public class MainActivity extends AppCompatActivity {
 
    //ButterKnife可以在activity，fragment , adapter中使用
 
    @Nullable //为了防止异常没有id
    @BindView(R.id.tv_text)
    TextView tv_text;
    @BindView(R.id.yzs)
    TextView yzs;
    @BindView(R.id.wzs)
    TextView wzs;
    @BindViews({R.id.b1 , R.id.b2 , R.id.b3}) //绑定viewID
    List<Button> buttons;
    @BindString(R.string.app_name) //绑定资源ID
    String appname;
    @BindView(value = R.id.et_edittext)
    EditText edt;
    @BindView(R.id.list)
    ListView list;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 
        ButterKnife.bind(this); //注释使用
        tv_text.setText("BindView");
        for (int i= 0 ; i < buttons.size() ; i++) {
            int ii = i + i;
            buttons.get(i).setText("bt" + ii);
        }
 
        String [] items = {"1" , "2" , "3" , "4"};
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this , R.layout.support_simple_spinner_dropdown_item , items);
        list.setAdapter(adapter);
    }
 
    @OnClick(R.id.b1) //绑定监听
    public void bt1click(){
        Toast.makeText(this, "这是点击按钮1", Toast.LENGTH_SHORT).show();
    }
 
    @OnLongClick(R.id.b2) //长按监听
    public boolean bt2longclick(Button bt2){
        bt2.setText("这是按钮2的长按事件");
        return true;
    }
 
    @OnTouch(R.id.b3) //触摸事件
    public boolean bt3ontouch(View view , MotionEvent event) {
        Toast.makeText(this, "触摸事件", Toast.LENGTH_SHORT).show();
        return true;
    }
 
    //监听edittext的变化
    @OnTextChanged(value = R.id.et_edittext , callback = OnTextChanged.Callback.BEFORE_TEXT_CHANGED)
    void beforeTextChanged(CharSequence s , int start , int count , int after){
        Log.i("mydate" , s.toString() + " -- " + start + " -- " + count + " -- " + after);
    }
 
    @OnTextChanged(value = R.id.et_edittext , callback = OnTextChanged.Callback.TEXT_CHANGED)
    void onTextChanged(CharSequence s , int start , int before , int count) {
        //已输入的字数
        yzs.setText(s.length() + "");
        //剩余字数
        int z = 50 - s.length();
        wzs.setText(z + "");
 
    }
 
    @OnTextChanged(value = R.id.et_edittext , callback = OnTextChanged.Callback.AFTER_TEXT_CHANGED)
    void afterTextChanged(Editable s){
        Log.i("mydate" , s.toString());
    }
 
    //item点击监听
    @OnItemClick(R.id.list)
    void onItemClick(int position) {
        Toast.makeText(this, position + "", Toast.LENGTH_SHORT).show();
    }
 
}
```

版权声明：遵循 [CC 4.0 BY-SA](http://creativecommons.org/licenses/by-sa/4.0/) 版权协议，转载请附上原文出处链接和本声明。
原文地址：[https://blog.csdn.net/qq_38261174/article/details/79821093](https://blog.csdn.net/qq_38261174/article/details/79821093)

