

## PowerToys使用指南
- 1. spotlight：```Alt+space```
- 2. Windows 快捷键说明：```长按Windows键3秒```


## VScode使用教程

### vs code整段右移或者左移

选中按TAB右移，按SHIFT+TAB左移

### 在VScode中使用C#开发
**环境**
- VSC本体
- .NET Core SDK，可以访问dot.net下载
- VSC扩展：C#、Code Runner

**操作步骤**
- 建立一个文件夹
- 打开终端（TERMINAL） 输入``` dotnet new console``` 会自动生成Hello World文件
- 按F5即可运行 
- 还需要做一点修改。左边点开.vscode文件夹：
```
launch.json："console": "integratedTerminal", "internalConsoleOptions": "neverOpen";  .NET Core Attach可以删掉。
tasks.json：给build加上"group": { "kind": "build", "isDefault": true },（打group就>会自动提示）
```

如使用runner 需要设置环境变量
```
csc是编译C#的程序。安装VS，把 安装路径下\Microsoft VisualStudio
\2019\Community\MSBuild\Current\Bin\Roslyn\加到Path里。
```




### 在每一个数字中间加上一个“，”

知乎上的一个问题，如输入a[6]={1,2,3,4,5,6}输出1，2，3，4，5，6；
记录一个大佬的回答
```C
#include <stdio.h>
int main(void){
    int a[6]={1,23,4,5,6};
    for(i=0;i<6;i++){
        printf(",%d"+!i,a[i]);
    }
    return 0;
}
```
[//]: #(转自知乎:谷雨同学)


```C#
        #region XML转JSON
        /// <summary>
        /// Get xml to json obj
        /// </summary>
        /// <param name="xmlPath">xml localpath</param>
        public static JArray XML2JSON(string xmlPath)
        {
            XmlDocument xmlDoc = new XmlDocument();
            xmlDoc.Load(xmlPath);
            //string json = JsonConvert.SerializeXmlNode(xmlDoc);
            string xmlStr = xmlDoc.InnerXml;
            Services.ArrayOfAsset asset = XmlUtil.Deserialize(typeof(Services.ArrayOfAsset), xmlStr) as Services.ArrayOfAsset;
            //add head 
            Services.XMLFormat xMLFormat = new Services.XMLFormat();
            xMLFormat.Asset = asset.Asset;
            object a = xMLFormat.Asset;
            string strJson = JsonConvert.SerializeObject(a);
            //Services.Asset jo = JsonConvert.DeserializeObject<Services.Asset>(strJson);
            JArray json = (JArray)new JsonSerializer().Deserialize(new JsonTextReader(new StringReader(strJson)));
            return json;
        }
        #endregion
```

int是带符号的，表示范围是：-2147483648到2147483648，即-2^31到2^31次方。

uint则是不带符号的，表示范围是：2^32即0到4294967295。