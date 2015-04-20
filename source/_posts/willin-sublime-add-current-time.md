title: "Willin: Sublime Add Current Time"
date: 2015-04-20 14:17:23
categories: Dev
tags: [sublime]
---

## 1. Add a New Plugin

> Toolbar -> Tools -> New Plugin

```
import datetime  
import sublime, sublime_plugin

class AddTimeCommand(sublime_plugin.TextCommand):
	def run(self, edit):
		self.view.run_command("insert_snippet", 
			{
				"contents": "%s" % datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
			}
		)
```

Save as `AddTime.py`


## 2. Add a Shortcut

> Toolbar -> Sublime Text -> Preferences -> Key Bindings - User

Append a new line:

```
{"keys": ["command+shift+."],"command": "add_time"}
```



> 2015-04-20 14:20:57
