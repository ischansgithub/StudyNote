## 准备：

- notepad++下载
- MinGW中的bin包

## 工作：

1.下载bin包：MinGw中的bin包放于百度网盘/各种软件安装包（有需要可以加微信cww738331493联系我，手把手教会)

2. 解压并将bin包的路径输入到系统变量中的path

3. 测试：打开cmd。输入：

   ```
   gcc -v
   g++ -v
   ```

   有反应则配置成功

4. 打开notepad++菜单栏中的运行、输入以下并保存快捷键：
   ```
   #编译
   cmd /k g++.exe -g -W -Wall -o $(CURRENT_DIRECTORY)\$(NAME_PART).exe $(FULL_CURRENT_PATH) & PAUSE & EXIT
   ```
   
   ```
   #运行
   cmd /k $(CURRENT_DIRECTORY)\$(NAME_PART).exe $(FULL_CURRENT_PATH & PAUSE & EXIT
   ```

   ```
   #编译+运行
   cmd /k cd /d "$(CURRENT_DIRECTORY)" & g++ "$(FILE_NAME)" -o "$(NAME_PART)" & "$(NAME_PART).exe"
   ```

   

