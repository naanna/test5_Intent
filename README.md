# Intent
## 自定义WebView验证隐式Intent的使用

本实验通过自定义WebView加载URL来验证隐式Intent
的使用。
实验包含两个应用：

第一个应用：获取URL地址并启动隐式Intent的调用。

第二个应用：自定义WebView来加载URL

1. 新建一个工程用来获取URL地址并启动Intent

    核心代码：
    
    url_intent.xml
    ```xml
    <EditText
        android:id="@+id/editText1"
        android:layout_width="235dp"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:ems="10"
        android:inputType="textPersonName" />
    <Button
        android:id="@+id/button1"
        android:layout_width="112dp"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:text="Button" />
    ```
    webview.xml
    ```xml
        <WebView
        android:id="@+id/webview"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        />
    ```
    url.intent.java
    ```java
    public class url_intent extends AppCompatActivity {
        private WebView webView;
        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.webiew);
            webView=(WebView)findViewById(R.id.webView);
            String url=getIntent().getExtras().getString("url");
            webView.loadUrl(url);
            webView.setWebViewClient(new WebViewClient(){
                @Override
                public boolean shouldOverrideUrlLoading (WebView view,String url){
                    view.loadUrl(url);
                    return true;
                }
            });
            WebSettings settings=webView.getSettings();
            settings.setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK);
        }
    }
    ```
    url_main.java
    ```java
    public class url_main  extends AppCompatActivity {
        private Button button;
        private EditText editText;
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.url_intent);
            button=(Button)findViewById(R.id.button1);
            editText=(EditText)findViewById(R.id.editText1);
            button.setOnClickListener(new View.OnClickListener(){
                public void onClick(View v){
                    Intent intent=new Intent("com.action.webview");
                    String url="http://"+editText.getText().toString();
                    intent.addCategory("com.action.webviewcategory");
                    intent.putExtra("url",url);
                    startActivity(intent);
                }
            });
        }
    }
    ```
    模拟器截图：

    ![1](https://img-blog.csdnimg.cn/20190420221949232.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ3OTEzNA==,size_16,color_FFFFFF,t_70)

    ![2](https://img-blog.csdnimg.cn/20190420222008826.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ3OTEzNA==,size_16,color_FFFFFF,t_70)

1. 新建一个工程使用WebView来加载URL

    MainActivity.java
    ```java
   public class MainActivity extends AppCompatActivity implements View.OnClickListener{

    private Button button2;
    private EditText editText2;
    private String urlHead="http://";
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        button2= (Button) findViewById(R.id.button1);
        editText2= (EditText) findViewById(R.id.editText1);
        button2.setOnClickListener(this);
    }

    @Override
    public void onClick(View view) {
        switch (view.getId()){
            case R.id.button1:
                Intent intent=new Intent(Intent.ACTION_VIEW);
                intent.setData(Uri.parse(urlHead+editText2.getText().toString()));
                Intent choose=Intent.createChooser(intent,"请选择浏览器");
                startActivity(choose);
                break;
            }
        }
    }
    ```
    MyWebView.java
    ```java
    public class MyWebView extends AppCompatActivity {
        private WebView webView;
        private String url;
        
        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.webiew);
            Log.d("123", "onCreate: ");
            webView= (WebView) findViewById(R.id.webView);
            url=getIntent().getData().toString();
            WebSettings webSettings=webView.getSettings();
            webSettings.setJavaScriptEnabled(true);
            webSettings.setSupportZoom(true);

            webView.loadUrl(url);
            webView.setWebViewClient(new WebViewClient(){

                @Override
                public boolean shouldOverrideUrlLoading(WebView view, String url) {
                    webView.loadUrl(url);
                    return true;
                }
            });
        }
    }
    ````
    模拟器截图：

    ![3](https://img-blog.csdnimg.cn/20190424213434326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ3OTEzNA==,size_16,color_FFFFFF,t_70)

    ![4](https://img-blog.csdnimg.cn/20190420222343432.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQ3OTEzNA==,size_16,color_FFFFFF,t_70)