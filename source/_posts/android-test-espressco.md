---
title: android_test_espressco
date: 2017-11-01 09:46:12
tags: test
---
```
1. Espresso 简介
	这是一个测试框架,xx目的是自动化测试android,你可以以Activity为模块，针对每个模块写出若干测试用例。
```
```
2. Espresso 使用步骤
	1> 在应用app moudle 的build.gradle 导入相关依赖包
		androidTestCompile('com.android.support.test.espresso:espresso-core:3.0.1', {
        		exclude group: 'com.android.support', module: 'support-annotations'
    		})
    		androidTestCompile 'com.android.support.test:runner:1.0.0'
	    	compile 'com.android.support.test.espresso:espresso-idling-resource:3.0.1'
	2> 在app/src/androidtest s书写测试用例
	3> 在使用gradlew 命令运行案例
		./gradlew connectedAndroidTest
	4> 在app /build/reports/flavours/index.html 查看测试用例报告
```
```
3. Espresso 测试用例
@LargeTest
@RunWith(AndroidJUnit4.class)
public class HomeWorkActivityTest extends BaseTest {
    @Rule
    public ActivityTestRule<HomeWorkActivity> mRule = new ActivityTestRule<HomeWorkActivity>(HomeWorkActivity.class){
        @Override
        protected void beforeActivityLaunched() {
            initBaseData();
            super.beforeActivityLaunched();
        }
        @Override
        protected Intent getActivityIntent() {
            Bundle bundle = new Bundle();
            bundle.putString("childId","1045");
            return launchIntent(bundle);
        }
    };
    private SimpleIdlingResource mIdlingResource;

    @Before
    public void registerIdlingResource() {
        mIdlingResource = mRule.getActivity().getIdlingResource();
        // To prove that the test fails, omit this call:
        Espresso.registerIdlingResources(mIdlingResource);
    }

    @Test
    public void homeWorkActivityTestFromHomeSchoolFragment() {
        clickItem();
    }

    private void clickItem() {
        int id = R.id.list;
        ToastUtil.showMessage("homeWorkActivityTest"+id);
        ViewInteraction view = onView(RecyclerViewActions.withRecyclerView(id).atPosition(1));
        if (view != null){
            view.perform(ViewActions.click());
        }
    }

    @After
    public void unregisterIdlingResource() {
        if (mIdlingResource != null) {
            Espresso.unregisterIdlingResources(mIdlingResource);
        }
    }

}	
```
```
3. Espresso 使用说明
	1> 这个框架是以注解的形式完成配置的＠Rule 对应一个模块（activity）＠test 对应一个测试用例
	＠Before 表示在测试用例＠Test 测试之前的准备工作，＠After表示＠Test跑完之后执行的方法
	2> 常用的API的类如下
	Espresso
		onView()获得对应的ViewInterface(对应的View),同时也可以关闭或者开启软键盘，注册回调Idle和反注册回调Idle
	ViewInterface
		perform(ViewAction)返回ViewInterface,可以连续多个ViewAction
	ViewAction
		可以点击　按回退键　长按　左侧滑　右侧滑一系列的手势动作
		也可以是填写文本内容，打开链接
```
