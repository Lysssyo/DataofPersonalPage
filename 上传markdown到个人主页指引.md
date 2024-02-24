# 上传markdown到个人主页指引

1. 选好要该md要放的页面（"杂七杂八的踩坑"、“后端开发”、“算法”、“我的”）。
2. 选好之后，填好D:\AAA_MyPage\ViewIndex中的相应的View中的Index.md的相关信息。其中Tittle一项可以为中文，但是，如果要新增的md文档有图片，在新建md文件时，要先用英文，以创建名字为英文的assets文件夹，可以之后再改为中文。另外，新建的md文件的文件名要与Index中的tittle严格一致，文件名可以与其对应的assets不一致。
3. 完成后，更新GitHub仓库“DataofPersonalPage”的对应分支（即更新Index.md，将新建的md文件拉入对应分支），如果有图片，还需更新Github仓库“WXMiniProgramPic”（把新建的md文件的assets文件夹拖入仓库），push到远程仓。
4. Gitee的仓库“DataofPersonalPage”同步Github仓库，如果有图片，还需同步“WXMiniProgramPic”仓库。