# -
1. 智能设备设置页面：调用DeviceSettingActivity
2. 
继续梳理SmartHardwareActivity：
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


8. 如果现在添加好友个数的上限由20个变成200个。
   .startDeviceShareFriendChooser中：修改maxSelectd=200-mFriendList.size();

由于现在gridView还是只显示20个，当好友个数<20个时,返回列表项数目=好友个数+1；当好友个数>=20时，返回列表项数目=好友个数。相当于“添加”按妞
消失了。这里是好友个数>=200时，“添加”按钮消失，所以需要修改成：
mFriendList.size()<200...

getView的逻辑需要更正：
当好友个数>=200时，“添加”按钮消失；
当好友个数<200时，“添加”按钮存在；

SmartDeviceWebViewPlugin
SmartHardwareActivity
DeviceSettingActivity

研究如何返回上一页按钮
  .SmartHardwareActivity页面左上角显示：“完成”，如何做到的？
该页面使用的xml是qb_opensdk_smart_hardware_share_setting，这个页面中表示Title的是R.layout.custom_commen_title，
可以令mLeftBackBtn可见，并且令mLeftBackBtn.setText("返回");
  .如何在SelectMemberActivity中添加悬停搜索框？
select_member_layout.xml中
  .如何去除DeviceDeleteFriendChooserActivity中的checkbox？
在getView中将holder.checkBox设置隐藏。



梳理DeviceDeleteFriendChooserActivity
  .initParams方法，获取mFriendList好友列表
  .initUI方法，初始化title和listview


