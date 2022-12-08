### obsidian有个官方的模板

https://github.com/obsidianmd/obsidian-sample-plugin

首先 use template，然后新建一个仓库，取名为自己的名字。

### 克隆到本地的obsidian对应的插件目录中

我的mac的本地的obsidian路径为

```
/Users/yanyudong/Desktop/obsidian-files/.obsidian/plugins
```

PS: mac上显示隐藏文件的快捷键： command + shift + . 。
克隆到该目录中，然后用vscode打开目录。

### npm install & npm run dev

看到目录后可以发现，入口文件是main.ts
通过执行npm run dev可以打包为main.js。

PS：注意manifest.json中的id必须唯一

当前目录下执行

```
npm install
npm run dev
```

这时候打开obsidian。

### 插件调试

obsidian打开console的快捷键：

windows： cmd + alt + i
mac: command + alt + i

然后点击  setting ->  community plugins
可以看到此时多了个obsidian-sample-plugin的插件。
启用后即可。

### main.ts

plugin每次加载都会执行MyPlugin的onload

![](Pasted%20image%2020221208175911.png)


onload里面可以注册命令行回调函数，即可在obsidian中输入命令行后执行。

```js
this.addCommand({
	id: 'replace-image-link',
	name: 'Replace image link',
	editorCallback: (editor: Editor, view: MarkdownView) => {
	console.log("editor.getDoc", editor.getDoc())
	console.log("editor.getValue", editor.getValue())
	const content = editor.getValue();
	const reg = /\!\[\]\(([\s\S]*?)\)/g; // 正则：替换图片的链接
	const result = content.replaceAll(reg, (name, a) => { return `![](/${a})`})
	// editor.replaceRange("我用什么把你留住", { line: 0, ch: 0 })
	console.log("view", view)
	view.setViewData(result, false)
	}
});
```

main.ts 注释：

```js
import { App, Editor, MarkdownView, MarkdownEditView, Modal, Notice, Plugin, PluginSettingTab, Setting } from 'obsidian';

  

// Remember to rename these classes and interfaces!

  

interface MyPluginSettings {

mySetting: string;

}

  

const DEFAULT_SETTINGS: MyPluginSettings = {

mySetting: 'default'

}

  

export default class MyPlugin extends Plugin {

settings: MyPluginSettings;

  

async onload() {

console.log("plugin is onload")

await this.loadSettings();

  

// 左侧的图标按钮

// This creates an icon in the left ribbon.

const ribbonIconEl = this.addRibbonIcon('dice', 'Sample Plugin', (evt: MouseEvent) => {

// Called when the user clicks the icon.

new Notice('This is a notice!');

console.log("this.app", this.app);

console.log("this.app.workspace.getLayout", this.app.workspace.getLayout());

console.log("this.app.vault.getMarkdownFiles", this.app.vault.getMarkdownFiles());

// new Notice(JSON.stringify(this.app));

});

// Perform additional things with the ribbon

ribbonIconEl.addClass('my-plugin-ribbon-class');

  

// const ribbonIconEl1 = this.addRibbonIcon('dice', 'reload', (evt: MouseEvent) => {

// // Called when the user clicks the icon.

// this.load();

// });

  

// 底部的一行字样

// This adds a status bar item to the bottom of the app. Does not work on mobile apps.

const statusBarItemEl = this.addStatusBarItem();

statusBarItemEl.setText('Status Bar Text Fuck');

  

// 注册事件 命令行触发

this.addCommand({

id: 'show-editor-object',

name: 'show editor object',

editorCallback: (editor: Editor, view: MarkdownView) => {

console.log("editor.getDoc", editor.getDoc())

console.log("editor.getValue", editor.getValue())

// editor.replaceRange("我用什么把你留住", { line: 0, ch: 0 })

console.log("view", view)

view.setViewData("哈哈哈哈", false)

// console.log("view", view.setViewData("哈哈哈哈", false))

}

});

  

// 注册事件 命令行触发, 打开弹框

// This adds a simple command that can be triggered anywhere

this.addCommand({

id: 'open-sample-modal-simple',

name: 'Open sample modal (simple)',

callback: () => {

new SampleModal(this.app).open();

}

});

  

// 注册事件 命令行触发，将当前选中的文本替换为Sample editor command

// This adds an editor command that can perform some operation on the current editor instance

this.addCommand({

id: 'sample-editor-command',

name: 'Sample editor command',

editorCallback: (editor: Editor, view: MarkdownView) => {

console.log(editor.getSelection());

editor.replaceSelection('Sample Editor Command');

}

});

  
  

// This adds a complex command that can check whether the current state of the app allows execution of the command

this.addCommand({

id: 'open-sample-modal-complex',

name: 'Open sample modal (complex)',

checkCallback: (checking: boolean) => {

// Conditions to check

const markdownView = this.app.workspace.getActiveViewOfType(MarkdownView);

if (markdownView) {

// If checking is true, we're simply "checking" if the command can be run.

// If checking is false, then we want to actually perform the operation.

if (!checking) {

new SampleModal(this.app).open();

}

  

// This command will only show up in Command Palette when the check function returns true

return true;

}

}

});

  

// 配置项的标签添加

// This adds a settings tab so the user can configure various aspects of the plugin

this.addSettingTab(new SampleSettingTab(this.app, this));

  

// 注册全局点击事件触发

// If the plugin hooks up any global DOM events (on parts of the app that doesn't belong to this plugin)

// Using this function will automatically remove the event listener when this plugin is disabled.

this.registerDomEvent(document, 'click', (evt: MouseEvent) => {

console.log('click', evt);

// new Notice('This is a notice!');

});

  

// When registering intervals, this function will automatically clear the interval when the plugin is disabled.

this.registerInterval(window.setInterval(() => console.log('setInterval'), 5 * 60 * 1000));

}

  

onunload() {

  

}

  

async loadSettings() {

this.settings = Object.assign({}, DEFAULT_SETTINGS, await this.loadData());

}

  

async saveSettings() {

await this.saveData(this.settings);

}

}

  

// 弹框

class SampleModal extends Modal {

constructor(app: App) {

super(app);

}

  

onOpen() {

const { contentEl } = this;

contentEl.setText('Woah!');

}

  

onClose() {

const { contentEl } = this;

contentEl.empty();

}

}

  

// 配置项的样式

class SampleSettingTab extends PluginSettingTab {

plugin: MyPlugin;

  

constructor(app: App, plugin: MyPlugin) {

super(app, plugin);

this.plugin = plugin;

}

  

display(): void {

const { containerEl } = this;

  

containerEl.empty();

  

containerEl.createEl('h2', { text: 'Settings for my awesome plugin.' });

  

new Setting(containerEl)

.setName('Setting #1')

.setDesc('It\'s a secret')

.addText(text => text

.setPlaceholder('Enter your secret')

.setValue(this.plugin.settings.mySetting)

.onChange(async (value) => {

console.log('Secret: ' + value);

this.plugin.settings.mySetting = value;

await this.plugin.saveSettings();

}));

}

}
```

