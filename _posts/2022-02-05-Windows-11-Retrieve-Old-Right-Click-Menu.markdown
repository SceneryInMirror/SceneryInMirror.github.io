---
title: "Windows 11 Retrieve Old Right-Click Menu"
layout: post
date: 2022-02-05 16:00
img: usc.png
headerImage: True
tag: [Learning]
category: blog
author: ykx
description: Newly released Windows 11 improves the UI, which provides a better user experience. However, some changes may be inconvenient, for example, the layout of Right-Click Menu. I did a little modification to personalize some settings.
---

## Retrieve Old Right-Click Menu

> ref: https://zhuanlan.zhihu.com/p/417591763
> For me, I always use shortcuts to create a new folder or file, but in the new menu, I need to use shift+F10 to open the full-version of menu first, which is quite annoying. To avoid that, I found this solution.

We can use Registry Editor to retrieve the old version of Right-Click Menu.

Step 1: Use Win+R to call the *Run* window shown below, and type *regedit* to open Registry Editor.

<img src="https://github.com/SceneryInMirror/SceneryInMirror…ob/master/assets/images/windows11_old_right_click_menu/image-20220205161803504.png?raw=true" alt="step1" />

Step 2: Find the folder "*HKEY_CURRENT_USER\Software\Classes\CLSID*", right click on it, and select *New->Key*. Name the new key as "*{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}*".

Step 3: Right click on the new key we just added, select *New->Key*, and name it as "*InprocServer32*".

<img src="https://github.com/SceneryInMirror/SceneryInMirror…ob/master/assets/images/windows11_old_right_click_menu/image-20220205162415580.png?raw=true" alt="step3" />

Step 4: Rerun explorer.exe by running the following command in a terminal:

~~~
taskkill /f /im explorer.exe & start explorer.exe
~~~

<img src="https://github.com/SceneryInMirror/SceneryInMirror…ob/master/assets/images/windows11_old_right_click_menu/image-20220205162626763.png?raw=true" alt="step4" />

Then it should work.

To retrieve the new menu in Window 11, you can delete the key we add in the above steps.



