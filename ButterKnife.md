# ButterKnife

### GitHub

https://github.com/JakeWharton/butterknife

### 1. 介绍

ButterKnife是一个专注于Android系统的View注入框架,以前总是要写很多findViewById来找到View对象，有了ButterKnife可以很轻松的省去这些步骤。

ButterKnife的优势：
 1、强大的View绑定和Click事件处理功能，简化代码，提升开发效率
 2、方便的处理Adapter里的ViewHolder绑定问题
 3、运行时不会影响APP效率，使用配置方便
 4、代码清晰，可读性强

### 2. 添加依赖

```groovy
android {
  ...
  // Butterknife requires Java 8.
  compileOptions {
    sourceCompatibility JavaVersion.VERSION_1_8
    targetCompatibility JavaVersion.VERSION_1_8
  }
}

dependencies {
  implementation 'com.jakewharton:butterknife:10.2.1'
  annotationProcessor 'com.jakewharton:butterknife-compiler:10.2.1'
}
```

If you are using Kotlin, replace `annotationProcessor` with `kapt`.

如果你使用kotlin，将`annotationProcessor` 代替 `kapt`

Snapshots of the development version are available in [Sonatype's `snapshots` repository](https://oss.sonatype.org/content/repositories/snapshots/).

### 3. 如何使用

- 插件：Android ButterKnife Zelezny
- 使用：获取控件、事件点击处理

### Activity使用：

```kotlin
    public class MainActivity extends AppCompatActivity {
    
        //获取控件
        @BindView(R.id.name)
        EditText name;
    
        @BindView(R.id.btn)
        Button btn;
        @BindView(R.id.txt)
        TextView txt;
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            //绑定处理
            ButterKnife.bind(this);
        }
    
        //按钮点击事件处理
        @OnClick(R.id.btn)
        public void onViewClicked() {
   
            if (TextUtils.isEmpty(name.getText().toString().trim())){
                return;
            }
    
            if (name.getText().toString().trim().length() < 6){
                return;
            }
    
            txt.setText(name.getText());
        }
    }
```

多个按钮点击事件处理方式如下：

```java
@OnClick({R.id.imgv_base_title_back_id})
public void onViewClicked(View view) {
    switch (view.getId()) {
        case R.id.imgv_base_title_back_id:
            finish();
            break;
    }
}
```



### Fragment使用：

```java
    public class BlankFragment extends Fragment {
    
    
        @BindView(R.id.txt)
        TextView txt;
        @BindView(R.id.btn)
        Button btn;
    
    
        Unbinder unbinder;
    
        @Override
        public View onCreateView(LayoutInflater inflater, ViewGroup container,
                                 Bundle savedInstanceState) {
            // Inflate the layout for this fragment
            View inflate = inflater.inflate(R.layout.fragment_blank, container, false);
            unbinder = ButterKnife.bind(this, inflate);
            return inflate;
        }
    
        @Override
        public void onDestroyView() {
            super.onDestroyView();
            unbinder.unbind();
        }
    
        @OnClick(R.id.btn)
        public void onViewClicked() {
    
        }
    }
```