# guzhi

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

## 项目说明
# 该工程是最简单且基于Air进行配置的前端工程.包括基本的登录、token保存及一个基于HUI的简单demo
# 使用技术: Vue/Vue-router(前端路由管理)/axios(向服务端发送请求)/HUI

文件目录说明
build --- 前端工程webpack构建设置
config --- 前端工程构建参数设置(通过index.js配置项目开发运行端口等及打包相关配置)
src --- 项目源码
    -api --- 服务端接口请求配置
      --moduleName ---模块名
        ---xxxApi --- 页面名称
    -assets --- 项目静态文件,图片等
    -components ---项目自定义组件
    -mock ---mock模拟数据（真实交互环境可以不需要）
    -model --- 模型管理（对应view中需要的配置，form表单，校验等）
      --moduleName  --- 模块名
        ---xxxModel --- 页面对应的模块
    -router --- 项目路由配置
    -style --- 项目样式文件
    -utils --- 项目公共方法文件
    -directives --- 项目自定义指令文件
    -mixins --- 项目混入文件
    -views --- 项目中各功能页
        --moduleName --- 模块名
           ---xxx ---页面
             ---index.vue  --具体的页面元素抽出来
             ---approve.vue
             ***********
    -app.vue --- 项目入口渲染页面
    -main.js --- 项目入口文件(创建整个项目的vue实例，并将router、hui等注入)
index.html --- 项目入口模板
package.json --- 项目的依赖配置、运行配置等
sysconfig.js --- 项目常用接口配置
```

2.命名规范

2.1 目录名称   目录名称一律小驼峰
2.2 文件名称   组件名称 一律大驼峰     其余文件一律小驼峰
2.3 模块名，   页面名称与后台对应

              myWorkbench(我的工作台)
              Cotrun(科创版)
              ProRecord(项目档案)
              InquireStatistics(查询统计)
              DispatchOrg(派出机构)
              SystemManage（系统管理）

2.4 与后台交互的api    文件名    业务+Api    例如 ：bpm/bpmApi.js

​```javascript
import {get, post} from '@/api/bizApi/commonUtil'
const moduleName = 'demo';
const baseUrl = "/iasis/" + moduleName+"/";

//后端请求映射
const urls = {
  "query": baseUrl + 'query',
  "add": baseUrl + 'addUser',
  "edit": baseUrl + 'edit',
  "delete": baseUrl + 'delete',
  "approve": baseUrl + 'delete'
}

//方法区
var businessNameDemoApi = {
  "queryUrl": urls.query,
  add(param) {
    return post(param,urls.add)
  },
  edit(param) {
    return post(param,urls.edit)
  },
  delete(param) {
    return post(param,urls.delete)
  }
}

export default businessNameDemoApi

说明：一些常规的异常等操作框架会处理，无需每个业务都去处理
```

2.5 model   业务名称+Model 例如：/bpm/bpmModel.js

```javascript
var businessNameDemoModel = {
 //配置
   columns: [
    {
      title: "节点id",
      key: "id"
    },{
      title: "节点名称",
      key: "name"
    }],
  queryFrom:[

  ]
}
export default businessNameDemoModel
```

2.6 view层    模块名/业务名/组建          例如   /bpm/bpm

```
main
edit
detail
approve
```

2.7 方法名 -- 小驼峰


3.人员选择组件  - PersonnelSelection.vue
例子：
<personnel-selection :TreeData="treeData" :currPersonnel="currPersonnel" ref="personselection" :isRemote="true" @save-personnel="getperson"></personnel-selection>


  @save-personnel  :绑定方法获取选中人员

  groupClasList: { default: () => null }, // 树结构 中 只查 主任分管主任 处长 的 方法 已经可以了,,需要如此参数groupClasList
  groupNumList: { default: () => [] },
  TreeData: { default: () => [] }, // 数据 ，父组件有传，以父组件为准，没有，直接获取整棵树
  showCheckbox: { default: false }, // 左侧树是否显示勾选框,默认false
  type: { default: '1' }, // 树的类型，1：查看类型树，勾选节点时如果是组织或小组则获取组织或小组下所有池子；2：只能单选池子类型节点；3：同一组织类型节点下只能单选池子类型;4:只能勾选池子且可多选
  currPersonnel: { default: () => [] }, // 父组件设置的当前选中人员,需要有userId,userName
  fixedCurrPersonnel:{default:()=>[]}, // 固定传进来的人员，不能删除。默认不固定
  checkType: { default: 'selection' },
  isRemote: { default: false }, // 是否从中正的接口获取
  code: {default: ''}, //从中正的接口远程查询的时候使用的code
  isPart: { default: false } // 是否获取当前登录用户的部分tree




4. 格式化

4.1   日期格式化：this.formatDate(this.form.date)
4.2   时间格式化：this.formatTime(this.form.date)
4.3   百分比格式化：this.formatPecent(data)，如果反显的日期是直接反显到日期组件里的，那么要写一个计算属性，通过get和set方法，来格式化这个数据后传入日期组件
      例：
      formatAfterDate: {
        set (val) {
          this.submitDataObject.expeMeetTime = val;
        },
        get () {
          return this.formatTime(this.submitDataObject.expeMeetTime);
        }
      }

5. 字典

5.1 字典代码：查看地址：iasis_vue/src/api/public/CommonData.js

5.2 组件---DicSelect
例子：
<dic-select v-model="selectValue1" :multiple="false" :range="range" code="fileStatus"></dic-select>

    disabled: { default: false },
    disabledRadio: { default: false },
    dicType: { default: '1' }, // 组件类型，1-select  2-radio  3-cascader
    code: { default: '' }, // 字典代码
    range: { default: () => [] }, // 控制显示范围，存要显示的cod
    multiple: { default: true }, // 控制select是否多选，select默认多选
    placeholder: { default: '请选择' },
    max: {default: 3}, // 显示最大的多选项，大于的折叠一起，默认3个,
    dropWidth: {default: '100%'},
    transfer:{default:true} // 默认弹出放在body里
    placement:{default:'bottom-start'},
    optionRange:{default:()=>[]} // select输入的需要固定disabled的options

5.3 字典反显-render函数
例子：
{
  title: '举报信来源',
  key: 'reportSource',
  width: 130,
  render: (h, params) => {
    return h(
      'div',
      this.dictionaryApi.getStorageDictionarySubLabelByKey(
        'informationSource',
        params.row.reportSource
      )
    );
  }
}

5.4 用户字段反显 ---getAuthSystemUserByID(id)
例子：
{
  title: '上传人',
  key: 'userId',
  width: 130,
  render: (h, params) => {
    return h(
      'div',
      this.dictionaryApi.getAuthSystemUserByID(
        params.row.userId
      )
    );
  }
}

6. 上传下载弹出框组件---iasis_vue/src/components/upload/UploadMsgBox.vue
例子：
<upload-msg-box ref="upload" @on-success="getUploadFile" :single="false"  ></upload-msg-box>

showBotton: { default: false },  点击打开弹出框按钮，默认不显示
@on-success:  //获取上传文件的方法
single:{default:true}, //显示单个文件，默认为true
readOnly:{default:false},  //是否查看，去掉删除按钮
memoOpen：{default:false},  //是否查看，去掉备注按钮
queryFileList: //初始显示已上传文件，业务传回fileKey, fileName, uploadTime,跟组件原本的根据key请求获取有区别
hasLoading:{default:false}, //是否要上传的时候，显示loading遮罩

下面2个参数配置支持的文件格式，2个都需要配置
accept: {default: uploadApi.accept}, // 默认：'.xls,  .xlsx, .doc, .docx, .pdf,  .rar, .rar4, .zip',
format: {default: () => uploadApi.format}  // 默认：['xls', 'xlsx', 'doc', 'docx', 'pdf', 'rar', 'rar4', 'zip'],


6.1  showBotton为false的时候，上传弹出框通过 this.$refs.upload.open = true; 调用

6.2  单独下载文件调用全局方法：this.downloadFile(fileKey)      fileKey---对应的文件的fileKey
     单独删除文件调用全局方法：handleRemove (file)             file-----对应的文件对象

7.  等2次确认全局方法-----src/utils/index.js

    msg (callback, title = '确认提交', content = '是否确认提交')
    调用方式 ：this.$msg(() => {...之前处理的代码，就是点击确认处理的代码})
    例子1：
    this.$msg(() => {
          r.getOpt("vue").uploadedFileList.splice(0,length,fobj)
          r.getOpt("vue").$hMessage.success(file.fileName + "上传覆盖成功！");
        })

    例子2  :
    @click="$msg(ok)"

8. 打印功能----全局注册 $printPage 方法

例子：
<h-button type="primary" @click.native="$printPage">打印</h-button>



11.行业和区域数据获取接口

  前台获取行业级联效果数据：
  this.dictionaryApi.queryDictTradeSelectList().then(res=>{
    this.selectData=res
  });
  前台获取行业级联效果数据：
  this.dictionaryApi.queryDictAreaSelectList().then(res=>{
    this.selectData=res
  });


12.导出功能   ------全局变量   this.$exportExcel(exportExcel,'项目管理')
例子：
  // 导出按钮
  exportExcel () {
    this.$exportExcel(exportExcel,'项目管理')
  },

13. textarea输入框--- HTextArea.vue  包了一层计数，里面还是用iview的input-type='textarea'
例子：
  <h-form-item label="意见：" prop="iasisNode.checkOpinion" required :validRules="validateTextArea">
    <h-text-area :maxlength="1000" :value="audit_from.iasisNode.checkOpinion">
      <Input ref="input" v-model="audit_from.iasisNode.checkOpinion" type="textarea" />
    </h-text-area>
  </h-form-item>
  ---------------------------------------------------------------------------------------------------
  validateTextArea: [{test: this.$validate.validateTextArea, trigger: 'change'}],

14. 弹框样式
14.1-----例子：<h-msg-box
                v-model="typeAShow"
                :title="config.title" //必须，可以不是动态
                :width="msgWidth" //必须，可以不是动态
                :height="msgHeight" //必须，可以不是动态
                :scrollable="true" //设置了高度就必须
                :canDrag="false" //必须
                :mask-closable="false" //必须
                transfer //必须
                class-name="vertical-center-modal" //必须
                :top="0" //必须

                :footerHide="true" //只有关闭按钮的需要
              >
              <div slot="footer">
                      <h-button type="primary" @click.native="ok">确定</h-button>
                    </div>

15.  table里显示的要点
15.1 日期在table里反显的宽度：100
     时间在table里反显的宽度：130
     4个字的显示宽度：80
     6个字的显示看度：120
     table表格滚动条在弹窗内失效时，自己算好高度，加height
     类似意见这种长字段：Columns添加  宽度，ellipsis:true，  ||   h-table添加  showTitle
     notSetWidth这个属性不要使用

     有ellipsis:true的render函数的render标签不能是块状，可以用span，例：
     {
       title: '邀请参会人员',
       width: 200,
       ellipsis: true,
       key: 'notJoinMeetPeople',
       render: (h, params) => {
         return h(
           'span',{
             attrs:{
               title:this.dictionaryApi.getAuthSystemUserByID(
                 params.row.notJoinMeetPeople
               )
             }
           },
           this.dictionaryApi.getAuthSystemUserByID(
             params.row.notJoinMeetPeople
           )
         );
       }
     },
16. 统一风格样式组件：
16.1 列表组件：HList.vue
      title:{default:''},  //页面标题，不传就是没有头
      hasAdcanced:{default:true},  // 是否需要更多搜索的扩展  用了混合引入advancedQueryMixin.js  参考prom.vue
      hasSearch:{default:true}    //  是否需要搜索模块
16.2 内容组件：HInfo.vue
      title:{default:''}, //页面标题，不传就是没有头
      isCollapse:{default:false} //是否折叠  参考ToDoTask.vue

17. 金格插件：使用混合mixins
    例：
    <template>
    ...
    <JinGeMsgBox v-model="showJinGe" v-if="showJinGe" :file="jingeFile" @success="jingeSave"></JinGeMsgBox>
    </template>
    import iWebOfficeMixin from '@/mixins/iWebOfficeMixin'
    export default {
    ...
    mixins:[iWebOfficeMixin],
    ...
    }

    打开金格的弹框的方法----this.showJinGeBox(params.row, params.index);
    {
      title: '文件名',
      key: 'attaName',
      ellipsis: true,
      render: (h, params) => {
        return h('div', [
          h('a', {
            class: {
              'disableda': !api.format.includes(params.row.attaTabId.split('.')[1])
            },
            on: {
              click: () => {
                let fileType = params.row.attaTabId.split('.')[1];
                if (api.format.includes(fileType)) {
                  return self.showJinGeBox(params.row, params.index);
                } else {
                  return false;
                }
              }
            }
          },
          params.row.attaName)
        ]
        );
      }
    },

    jingeSave (file) {
      this.mesBoxUploadFile.splice(this.currentFileIndex, 1, file);
      this.$emit('on-success', this.mesBoxUploadFile);
    },


18. select组件筛选低版本下输入不能删除解决方案
select定义一个ref="select"，添加一个@click.native="clickSelect"方法
例子：
import {clickSelect} from '@/utils/index'
<Select
    ref="select"
    v-model="selectValue"
    multiple
    filterable
    placeholder="placeholder"
    @click.native="clickSelect(select)"
     :capture="false"  // 注意：设置为false，下拉选则框展开得时候，不需要点击2次
    >
    <Option
    v-for="item in selectData"
    :value="item.value"
    :key="item.value"
    >{{ item.label }}</Option>
    </Select>

clickSelect(ref){
      if (this.filterable) {
        return clickSelect(ref,this)
      };
    }



// author：崔文情

19.  配置金格插件可在线编辑的文件类型在路径为："@/api/public/jinGeApi.js"

    const api = {

      userJinge: window.LOCAL_CONFIG.IS_USER_JINGE, // 是否使用金格，在sysconfig里配置

      maxFileSize: 100 * 1024, // 金格编辑文件保存的时候控制文件大小，单位是kb

      format: ['doc','docx','xxx','xxx'],           <---------------此处为：配置金格插件可在线编辑的文件类型

      httpFileServer: window.LOCAL_CONFIG.API_JINGE, // 服务地址

      saveApi: '/IWebOfficeServer', // 保存文件接口

      httpFilePath: '/common/iweboffice/download/' // url打开文件接口

    };

    export default api;
20. 列表页缓存
<!-- 目的：返回到列表页，1.数据更新 2. 页码不变 3.搜索输入框条件不变 -->
  （1）路由中配置 默认缓存
     {
        path: 'list',
        component: () => import('@/views/myWorkbench/reportManage/reportList'),
        name: '举报信列表 ',
        meta: {
          keepAlive: true // 默认缓存 keep-alive
        }
      },
    （2）组件内keep-alive钩子配置
      activated () {
          <!-- 因为初次渲染这里也会执行，列表就请求2次，所以这里加个状态，再跳转之前更改状态，
          例如，1.跳转到编辑页面，返回后状态会变化，需要刷新2.，跳转到查看页返回页面不需要刷新。
          可根据需求，来决定返回是否刷新列表
                      this.activated = 1;  //编辑刷新
                      this.$router.push({
                        path: '/myworkbench/ReportManage/addrmletter',
                        query: {
                          id: params.row.tipoffId
                        }
                      });
              -->
        if(是否刷新){   
        <!-- 按旧的搜索条件，刷新当前表格 -->
          this.search('page');
        }
      }


## 安裝步驟
# 安裝插件包 --- cnpm instll (推荐) 或者 npm instll
# 安裝h_ui  --- cnpm install h_ui(推荐) 或者 cnpm instll h_ui
# 运行 --- npm run dev
# 模拟后台 --- 打开另一DOS窗口 运行 npm run mock
# 开发 --- localhost:8091

#支持iframe嵌套
#支持mock(登录时需先npm run mock)
#支持menu进行转换



