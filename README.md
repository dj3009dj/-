# 半公开设备
1. 智能设备设置页面：调用DeviceSettingActivity


2. 继续梳理SmartHardwareActivity：（SmartDeviceWebViewPlugin跳转所致）
a). getDeviceUsersList+mGetDeviceUserObserver===>利用后台获取Get好友列表+给mHandler发送消息，确认是否显示“删除成员”
b). 点击“授权成员”>“添加”>调用startDeviceShareFriendChooser方法
    .利用startActivityForResult进入SelectMemberActivity中。默认好友数<=20。
c). 点击“删除成员”>调用startDeviceDeleteFriendChooserActivity方法
    . 利用startActivityForResult进入DeviceDeleteFriendChooserActivity中
d). onActivityResult+mSendBindListObserver===>利用后台增加Send好友列表+给mHandler发送消息，确认是否显示“删除成员”+手Q发消息给好友
e). onCreate中获取产品、设备信息
    .调用initUI方法。显示设备名称、设备ID、设备头像。
    .调用doLogin方法。显示设备主人、调用getDeviceUsersList方法。
f). FriendListAdapter确定GridView的列表数目
    .getView方法：仅当好友数<20人时，显示“添加”按钮。
    .getCount方法：mFriendList小于20个时，Item的总个数=好友数+1；1是留给“添加”按钮的。mFriendList大于等于20时，Item的总个数=好友数


3. 如果现在添加好友个数的上限由20个变成200个。
   .startDeviceShareFriendChooser中：修改maxSelectd=200-mFriendList.size();
   .由于现在gridView还是只显示20个，当好友个数<20个时,返回列表项数目=好友个数+1；当好友个数>=20时，返回列表项数目=好友个数。相当于“添  加”按钮消失了。这里是好友个数>=200时，“添加”按钮消失，所以需要修改成：mFriendList.size()<200...
   .getView的逻辑需要更正：
       当好友个数>=200时，“添加”按钮消失；
       当好友个数<200时，“添加”按钮存在；


4.研究如何返回上一页按钮(this.finish())
  .SmartHardwareActivity页面左上角显示：“完成”，如何做到的？
     该页面使用的xml是qb_opensdk_smart_hardware_share_setting，这个页面中表示Title的是R.layout.custom_commen_title，
     可以令mLeftBackBtn可见，并且令mLeftBackBtn.setText("返回");
  .如何在SelectMemberActivity中添加悬停搜索框？
    在SelectMemberActivity(添加授权好友界面)中，使用了select_member_layout.xml。其中搜索框在xml中就是selected_and_search_bar这个组件。
    在SelectMemberActivity中,mTopSearchEditText表示搜索框的内容。下面分析搜索框：
       .在键盘中输入内容,如"m"，此时对应————ACTION_DOWN/KEYCODE_S-->ACTION_UP/KEYCODE_S回调OnLeyListener的onKey方法：
          onKey方法主要判断，输入的是不是KEYCODE_DEL键
       .紧接着，回到addTextChangedListener的afterTextChanged方法：
          afterTextChanged方法主要判断：当搜索框内容不为空时，mSearchResultLayout(搜索内容)显示出来。调用fragment搜索startSearch。
       .mSearchResultLayout是搜索后的列表，mListPanel是搜索前的列表。
  .如何去除DeviceDeleteFriendChooserActivity中的checkbox？
    在getView中将holder.checkBox设置隐藏。
  .二维码界面在哪？
    复用手Q中的QRDisplayActivity的qb_qrcode_card.xml
  .底部弹窗界面在哪？
    复用ChatSettingForTroop.java中的showActionSheet方法
    
    
5.梳理DeviceDeleteFriendChooserActivity
  .initParams方法，获取mFriendList好友列表
  .initUI方法，初始化title和listview


7. 准备使用群好友的页面
   .troop_owner_manager2--"群主、管理员"
   .TroopMemberListActivity利用了troop_owner_manager2
   .SearchTextWatcher利用了afterTextChanged方法
   .searchEditText利用了SearchTextWatcher这个监听
   .searchEditText定义：searchEditText = (EditText) mSearchDialog.findViewById(R.id.xxx);
   .mSearchDialog使用了search_layout.xml
   .searchList是搜索的最终结果,是个listview。它使用的适配器：searchList.setAdapter(mSearchResultAdapter)
   .mSearchResultAdapter使用了SearchResultAdapter
   .


6.目前改动的文件
qb_opensdk_smart_hardware_share_setting.xml
SmartHardwareActivity.java
DeviceAllShareFriendActivity.java
device_select_member_character_divided_listview.xml


明天研究FriendChooser
