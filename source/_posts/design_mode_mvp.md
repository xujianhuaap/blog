title: 设计模式-MVP
date: 2015-12-29 17:30:51
tags: design_mode_mvp

---
# 1.MVP设计模式的基本特点
view层负责展示
presenter负责逻辑控制
model负责数据请求,数据本地化

主要是实现了view层和presenter的逻辑分离,
避免GodActivity的出现,有利于逻辑的可扩展行.

# 2.MVP与MVC的区别
在MVC中M与V可以直接进行关联,因此MVC三层之间形成了一个闭环,不利于管理和扩展.

# 3 MVP 演示

```
public interface IController <FD>{

    public Activity getActivity();
    //正向流 View---->Model 一对一
    public void saveData();
    //通过Controller发起网络请求 Controller---> Model

    //反向流 Model--->View 一对多
    public void processData(FD d);//Controller----->View


}

public abstract  class AbstractController<FD> implements IController<FD>{
    private Activity activity;

    private AbstractData data;


    protected AbstractController(AbstractData data, Activity activity) {
        ErrorUtils.checkNotNull(data);
        ErrorUtils.checkNotNull(activity);
        this.data = data;
        this.activity = activity;
        data.init(this);
    }

    @Override
    public Activity getActivity() {
        return activity;
    }

    /***
     * get data from the net
     */
    protected void fetchData() {
        data.fetchData();
    }
}

public abstract  class CertifyInfoController extends AbstractController<CertifyInfo> {

    private CertifyInfoData mCertifyInfoData;

    public CertifyInfoController(Activity mActivity, CertifyInfoData mCertifyInfoData) {
       super(mCertifyInfoData,mActivity);
        this.mCertifyInfoData=mCertifyInfoData;
    }

    @Override
    public void saveData() {
        mCertifyInfoData.saveData();
    }

    public void fetchData(){
        super.fetchData();
    }


}


public interface IData {
    public void saveData();
    public void freshData();
    public void fetchData();

}

public abstract class AbstractData implements IData{
    private IController mController;
    private Activity mActivity;
    private WaitDialog mDialog;
    protected  boolean canFetching=false;
    protected boolean isFirst=true;



    protected boolean canFetch(){
        return isFirst|| canFetching;
    }
    public void init(IController mController) {
        this.mController = mController;
        this.mActivity=mController.getActivity();
    }

    public final class CallBackProxy<T extends AbstractResponse> implements Callback<T>{
        @Override
        public void start() {
            if(mDialog==null){
                ErrorUtils.checkNotNull(mActivity);
                mDialog=WaitDialog.show(mActivity);
            }
        }

        @Override
        public void success(T t, Response response) {
            ErrorUtils.checkNotNull(mController);
            mController.processData(t.getData());
        }

        @Override
        public void failure(HNetError hNetError) {
            maimeng.yodian.app.client.android.network.ErrorUtils.checkError(mActivity,hNetError);
        }

        @Override
        public void end() {
            if(mDialog!=null){
                mDialog.dismiss();
            }
        }

    }


    public final class CallBackProxyStr<String> implements Callback<String>{
        @Override
        public void start() {
            if(mDialog==null){
                ErrorUtils.checkNotNull(mActivity);
                mDialog=WaitDialog.show(mActivity);
            }
        }

        @Override
        public void success(String t, Response response) {
            ErrorUtils.checkNotNull(mController);
            mController.processData(t);
        }

        @Override
        public void failure(HNetError hNetError) {
            maimeng.yodian.app.client.android.network.ErrorUtils.checkError(mActivity,hNetError);
        }

        @Override
        public void end() {
            if(mDialog!=null){
                mDialog.dismiss();
            }
            canFetching=true;
        }

    }
}

public class CertifyInfoData extends AbstractData {
    private AuthService mService;
    public void init(IController mController) {
        super.init(mController);
        mService=Network.getService(AuthService.class);

    }

    @Override
    public void freshData() {

    }

    @Override
    public void fetchData() {
        if(canFetch()){
            mService.getCertifyInfo(new CallBackProxy<CertifyInfoResponse>());
            isFirst=false;
        }


    }

    @Override
    public void saveData() {

    }
}



public class RemainderMainActivity extends AbstractActivity implements View.OnClickListener, PtrHandler {
    private MoneyService service;
    private AcitivityRemainderMainBinding binding;
    private Remainder defaultValue;
    private static final int REQUEST_SHOW_DURING = 0x1001;
    private static final int REQUEST_INFO_CERTIFY = 0x1002;
    private static final int REQUEST_VOUCH_APPLY = 0x1003;
    private boolean isObtainRemainder = false;
    private CertifyInfo certifyInfo;
    private RemainderController remainderController;
    private CertifyInfoController certifyInfoController;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        binding = DataBindingUtil.inflate(getLayoutInflater(), R.layout.acitivity_remainder_main, null, false);
        setContentView(binding.getRoot());

        RemainderInfoData remainderInfoData=new RemainderInfoData();
        remainderController = new RemainderController(this,remainderInfoData) {
            @Override
            public void saveData() {

            }

            @Override
            public void processData(Remainder remainder) {
                //Controller----->View
                binding.setRemainder(remainder);
                isObtainRemainder = true;
            }

        };

        CertifyInfoData certifyInfoData=new CertifyInfoData();
        certifyInfoController = new CertifyInfoController(this,certifyInfoData){

            @Override
            public void saveData() {

            }

            @Override
            public void processData(CertifyInfo certifyInfo) {
                if (certifyInfo != null) {
                    binding.basicName.setText(certifyInfo.getCname());
                }
            }
        };
        binding.btnRemainder.setOnClickListener(this);
        binding.btnRemaindered.setOnClickListener(this);
        binding.btnConfirmInfo.setOnClickListener(this);
        binding.btnDrawMoneyInfo.setOnClickListener(this);

        binding.refreshLayout.setPtrHandler(this);
        StoreHouseHeader header = PullHeadView.create(this).setTextColor(0x0);
        binding.refreshLayout.addPtrUIHandler(header);
        binding.refreshLayout.setHeaderView(header);
        binding.refreshLayout.autoRefresh();

    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()) {
            case android.R.id.home: {
                onBackPressed();
                return true;
            }
        }
        return super.onOptionsItemSelected(item);
    }

    @Override
    protected void initToolBar(Toolbar toolbar) {
        super.initToolBar(toolbar);
        ActionBar actionBar = getSupportActionBar();
        if (actionBar != null) {
            actionBar.setHomeButtonEnabled(true);
            actionBar.setDisplayHomeAsUpEnabled(true);
            actionBar.setHomeAsUpIndicator(R.mipmap.ic_go_back);
        }
    }



    @Override
    public void onClick(View v) {
        if (v == binding.btnRemainder) {
            RemainderInfoActivity.show(this, binding.getRemainder());
        } else if (v == binding.btnRemaindered) {
            startActivity(new Intent(this, WDListHistoryActivity.class).putExtra("mony", binding.getRemainder().getWithdraw()));
        } else if (v == binding.btnConfirmInfo) {
            BasicalInfoConfirmActivity.show(RemainderMainActivity.this,certifyInfo,REQUEST_INFO_CERTIFY);
        } else if (binding.btnDrawMoneyInfo == v) {
            //必须获得Remainder之后binding.getRemainder()才有效
            DrawMoneyInfoConfirmActivity.show(this, binding.getRemainder().getDraw_account());
        }
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == RESULT_OK) {
            if (requestCode == REQUEST_SHOW_DURING) {
                Remainder remainder = data.getParcelableExtra(RemainderInfoActivity.KEY_REMAINDER);
                if (remainder != null) {
                    remainderController.processData(remainder);
                }
            } else if (requestCode == REQUEST_INFO_CERTIFY) {
                certifyInfo= Parcels.unwrap(data.getParcelableExtra("certifyinfo"));
                if(certifyInfo!=null){
                    certifyInfoController.processData(certifyInfo);
                }

            } else if (requestCode == REQUEST_VOUCH_APPLY) {
                //申请完成
                int applyStatus = data.getIntExtra("apply", 0x12);
                if (applyStatus == BindStatus.WAITCONFIRM.getValue()) {
                    AlertDialog.show(this, "", getString(R.string.apply_alertdialog_tip), getString(R.string.btn_name));
                }

            }

        }
    }

    @Override
    protected void onRestart() {
        super.onRestart();
        remainderController.fetchData();
    }

    @Override
    public boolean checkCanDoRefresh(PtrFrameLayout frame, View content, View header) {
        return PtrDefaultHandler.checkContentCanBePulledDown(frame, content, header);
    }

    @Override
    public void onRefreshBegin(PtrFrameLayout frame) {

        remainderController.fetchData();
    }



}
```
