This is the default repo which users can read from Elytra Notes.
You can set your own repo as source for it as well with the edit icon in the sidebar.

Some important points to note : 
- Every time you open a note or every time an image is loaded inside a note or independent image as well. An API call is made. And there is a limit to how many unauthorized API calls can be made through GitHub API, so try to go low :)
- For your own repo you would want to follow the following folder structure to access images as of now in Elytra Notes : 
```
parent folder
	assets folder
	file1
	file2
```
in the assets folder there would be images that you want to access in file1 and file2 and so on.
If you are using obsidian as your default note making app then you can configure it to do it manually by the following steps:
settings -> Files and Links -> set **Default location for new attachments** to `"In subfolder under current folder"` and **Subfolder** name to `"assets"`


Note : The notes in the ObsidianBackup source repo are not perfect and were solely meant for personal use. However if you find any errors and are knowledgeable to fix them, it would be highly appreciated. Any new content could be added or improvements to current content in your own fork for the repo as well. 

Elytra Notes was meant for free use and selling of the notes in any form is strictly prohibited.

Have fun