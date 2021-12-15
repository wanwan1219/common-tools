# 控件开发
## 一级控件
一级控件及根据一级控件定义控件
```
// 父类
ecui.ui.Control
```
[具体默认参数和方法](http://dev.oa.bitauto.com/fe-doc/ecui-doc/control/Control.html)

**扩展**
```
// 页面代码
<div ui="type:NS.VarifySave;valid:0">保存</div>
// js代码
// 自定义控件继承于一级控件
NS.ui.VarifySave = ecui.inherits(
    ecui.ui.Control,
    function (el, options) {
        ecui.ui.Control.call(this, el, options);
        this._sValid = options.valid;
    },
    {
        onclick: function () {
            // doing 。。。
        }
    }
);
```
## 二级控件
**二级基础控件**

单选
```
// 父类  
ecui.ui.Radio        // 单选
```
[参数及方法](http://dev.oa.bitauto.com/fe-doc/ecui-doc/control/Radio.html)

文本输入
```
ecui.ui.Text
```
[参数及方法](http://dev.oa.bitauto.com/fe-doc/ecui-doc/control/Text.html)

复选
```
ecui.ui.Checkbox
```
[参数及方法](http://dev.oa.bitauto.com/fe-doc/ecui-doc/control/Checkbox.html)

基础下拉
```
ecui.ui.Select
```
[参数及方法](http://dev.oa.bitauto.com/fe-doc/ecui-doc/control/Select.html)

时间
```
ecui.ui.Time
```
[参数及方法](http://dev.oa.bitauto.com/fe-doc/ecui-doc/control/Time.html)

文件上传
```
ecui.ui.Upload
```
[参数及方法](http://dev.oa.bitauto.com/fe-doc/ecui-doc/control/Upload.html)

输入数字
```
ecui.ui.Number
```
[参数及方法](http://dev.oa.bitauto.com/fe-doc/ecui-doc/control/Number.html)

多级联动下拉框控件
```
ecui.ui.MultilevelSelect
```
[参数及方法](http://dev.oa.bitauto.com/fe-doc/ecui-doc/control/MultilevelSelect-Select.html)

标签控件
```
ecui.ui.Label
```
[参数及方法](http://dev.oa.bitauto.com/fe-doc/ecui-doc/control/Label.html)

日期
```
ecui.ui.Calendar        
```
[参数及方法](http://dev.oa.bitauto.com/fe-doc/ecui-doc/control/Calendar.html)

按钮
```
ecui.ui.Button
```
[参数及方法](http://dev.oa.bitauto.com/fe-doc/ecui-doc/control/Button.html)

当前时间（时钟控件）
```
ecui.ui.Clock
// 配置参数可参考源码src/base/clock.js
```

弹窗
```
ecui.ui.Dialog
```
[参数及方法](http://dev.oa.bitauto.com/fe-doc/ecui-doc/control/Dialog.html)

图片
```
ecui.ui.Image
```
[参数及方法](http://dev.oa.bitauto.com/fe-doc/ecui-doc/control/Image.html)

表格
```
ecui.ui.InlineTable     // 默认
ecui.ui.LockedTable     // 锁定列
```
[参数及方法](http://dev.oa.bitauto.com/fe-doc/ecui-doc/control/Table.html)

多层菜单
```
ecui.ui.PopupMenu
```
[参数及方法]()

选项卡
```
ecui.ui.Tab
```
[参数及方法](http://dev.oa.bitauto.com/fe-doc/ecui-doc/control/Tab.html)


**根据具体2级控件进行拓展**
```
// 页面代码
<div ui="type:label;for:radio">
    <input ui="type:NS.RejectRadio;inputName:certificateAuditReason;"
        type="radio" name="certificateAuditStatus" value="2" > <span class="radio-text">驳回</span>
</div>
// 自定义控件继承于二级控件
NS.ui.RejectRadio = ecui.inherits(
    ecui.ui.Radio,
    function (el, options) {
        ecui.ui.Radio.call(this, el, options);
        this._sInputName = options.inputName;
    },
    {
        onclick: function () {
        }
    }
);
```

# 表单使用
页面
```
<form name="orderListForm">
    <input name="pageNum" class="ui-hide" />
    <input name="pageSize" class="ui-hide" />
    <div
        ui="ext-data:searchItems*@#searchItemSection"
        class="search-item-section"
    ></div>
</form>
```
js
```
// 获取表单元素及其内容
document.forms.orderListForm
// 获取表单里边name属性对应的值
let temps = {}
ecui.esr.parseObject(document.forms.orderListForm, temps);
// 给表单填充默认值
ecui.ui.BTableListRoute.prototype.resetFormValue(document.forms.orderListForm);
ecui.esr.fillForm(document.forms.orderListForm, temps);
```

# 数据变化通知视图更新
页面
```
// 关联数据和视图
<div
    ui="ext-data:searchItems*@#searchItemSection"
    class="search-item-section"
></div>
// 渲染数据的模板
<!-- target: searchItemSection -->
<!-- for: ${searchItems} as ${item} -->
<!-- if: ${item.isShow} -->
<!-- use: selectView(item=${item}) -->
<!-- var: isShowClearBtn = true -->
<!-- /if -->
<!-- /for -->
<!-- if: ${isShowClearBtn} -->
<!-- <span ui="type:NS.ClearBtn;" class="clear-btn">清空</span> -->
<!-- /if -->
```
js
```
const searchItems = []
ecui.esr.setData('searchItems', searchItems);
```
**参数说明**
* searchItems 数据
* searchItemSection 渲染数据对应的模版名称

# 常用方法
**获取当前表单中的控件的控件名称**
```
this.getName()
```
**获取当前表单中的控件的控件的值**
```
this.getValue()
```
**刷新对应子路由**
```
ecui.esr.callRoute('routeName', true);
```
**dom元素操作**
```
var indexView = ecui.$(indexnameID);   // 查  框架自带 也可用原生
ecui.dom.addClass(indexView, 'ui-hide');   // 添加class
ecui.dom.removeClass(indexView, 'ui-hide'); // 移除class
// 创建元素
var closeEl = ecui.dom.create('DIV', {
    innerHTML: '<span class="close-icon"></span>',
    className: 'attachment'
});
el.appendChild(closeEl);
// 如果创建dom是一个控件 为该则需要初始化该控件
ecui.$fastCreate(
    this.CheckboxItem,   // 对应的控件
    _tempCheckbox,       // 所要绑定控件的元素
    this,                // 作用实例
    {
        name: __lists[i].name,
        primary: 'ui-checkbox',  // 控件的使用的UI样式
        value: true,
        checked: __lists[i].isChecked,
        subject:'menu-item-checkbox'
    }
);
```
**数据共享**
共享在模块内，同一模块下的多个页面都能够访问
```
NS.data.monthresultArray = []
```
共享在一个路由下，只有在该路由下的内容控件才能够访问
```
context.columItems = []
```
空函数
```
ecui.util.blank
```
项目打包
```
// 进入存放项目的目录下
cd works
// 执行
ECUI/build.sh 项目名称
```
页面跳转
```
// html  DENY_CACHE进入页面刷新页面
href="#/serving/detail~orderNumber=${item.orderNumber}~DENY_CACHE"
// js
ecui.esr.redirect(
    '/uploadmatter/uploadmatter~orderNumber=' +_orderNumber 
);
```
动态渲染样式
```
// 模板中
<!-- var:  isJS = ${index} % 2 === 0 ? true : false -->
<!-- var: colorStrTr = ${isJS} ? 'background: #FFFFFF;' : 'background: #FCFDFF;' -->
<span style="${colorStrTr}">样式</span>
```
条件渲染
```
<!-- if: ${EnableCreate.editEnable} -->
<span>1</span>
<!-- else -->
<span>2</span>
<!-- /if -->
```
模版定义及使用
```
// 定义
<!-- target: createBtn -->
// 使用  statusLists为模板中要使用的数据 需要传递进去
<!-- use: createBtn(html=true,statusLists=${statusLists}) -->
```
从表单取值
```
let tempFormData = {};
ecui.esr.parseObject(document.forms.projectEditForm,tempFormData,false)
```
表单提交校验
```
let tempData = {};
const validate = ecui.esr.parseObject(document.forms.ModelPicturesUploadTargetUpload,tempData,true);
// 输入框校验
regexp:.+;name:test;
// 下拉必填
required:true;name:test;
```
清空表单
```
ecui.esr.fillForm(document.forms.materialsForm, {
    audienceId: '',
    auditStatus: '',
    search: '',
    status: ''
});
```
弹窗
```
// 打开
ecui.dialog('delAllocation', {});
// 关闭
var __Control = this.getMain();
yiche.findControl(__Control, ecui.ui.Dialog).hide();
```
挂值到上下文
```
// 存
ecui.esr.setData('isClick', {
    materialIdList: Ids
});
// 取
ecui.esr.getData('isClick')
```
列表文本提示
```
// 通用模版
<!-- target: dynamicTipView -->
<span class="word-clamp-2" ui="ext-dynamictip:top">
    ${text | default}
    <span class="tip-view default-tip-view ui-simple-tips" style="z-index: 11;line-height:20px;">${text | default}</span>
</span>
// 列表中使用
<!-- use: dynamicTipView(text=${item.mediaName}) -->
```
下拉联动 动态修改下来内容
```
let CarPayment = ecui.get('ModelPicturesCXMCColor');
CarPayment.removeAll(true);
let targetArrCarPayment = regionInfo.map(function (item) {
    return '<div ui="value:' + item.color + '" title="'+ item.colorName +'">' + item.colorName + '</div>';
 });
var tmpEl = ecui.dom.create('div');
tmpEl.innerHTML = targetArrCarPayment.join('');
CarPayment.add(ecui.dom.children(tmpEl));
```
处理第一次没被初始化的控件
```
ecui.cacheAtShow();
```