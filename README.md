# -
1. 智能设备设置页面：调用DeviceSettingActivity
2. 点击设置页面>好友授权管理：调用SmartHardwareActivity>进入onCreate>调用initUI>调用GridView.setAdapter(FriendListAdapter)。
3. SmartHardwareActivity的onCreate执行完成后，进入onStart方法。
4. 点击“授权成员”>添加>调用startDeviceShareFriendChooser，这个方法会进入SelectMemberActivity中。
5. 点击“删除成员”>进入startDeviceDeleteFriendChooserActivity，这个方法会进入DeviceDeleteFriendChooserActivity中。
6. 点击“完成”>调用onClick方法的R.id.ivTitleBtnRightText。
7. startDeviceShareFriendChooser中：
maxSelected=20-mFriendList.size();
这里限制了总数不超过20，需要修改。注意的是，mFriendList是授权好友列表，添加“全部好友”时，需要把mFriendList信息传过去。
###################################
FriendListAdapter类中的getCount方法逻辑：
mFriendList小于20个时，Item的总个数=好友数+1；1是留给“添加”按钮的
mFriendList大于等于20时，Item的总个数=好友数
###################################
FriendListAdapter类中的getView方法逻辑：
convertView缓存
只有mFriendList小于20&&当前view是最后一个view时，才显示“添加”按钮
其他情况下显示好友头像。
8. 如果现在添加好友个数的上限由20个变成200个
修改maxSelectd=200-mFriendList.size();

由于现在gridView还是只显示20个，当好友个数<20个时,返回列表项数目=好友个数+1；当好友个数>=20时，返回列表项数目=好友个数。相当于“添加”按妞
消失了。这里是好友个数>=200时，“添加”按钮消失，所以需要修改成：
mFriendList.size()<200...

getView的逻辑需要更正：
当好友个数>=200时，“添加”按钮消失；
当好友个数<200时，“添加”按钮存在；

SmartDeviceWebViewPlugin
SmartHardwareActivity
DeviceSettingActivity
