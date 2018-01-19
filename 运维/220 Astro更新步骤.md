1. 编译打包  
  ```http://192.168.0.212:5050/jenkins/view/sit_sugo_astro-220/job/sit_sugo_astro-220-test/```  
  点击立即构建，编译打包astro

2. 等待编译打包完成后，验证打包是否成功  
  进入200终端，进入目录：  
```    cd /var/www/html/yum/SG/centos6/beta/test/  ```  
  查看安装包是否为最新的：  
```    ll -rt  ```  
  查看安装包时间  
  
3. 修改安装包包名  
  进入界面：```192.168.0.220:8080(admin, admin)  ```   
  点击Astro-sugo，配置，高级astro-env，修改配置项astro.package.name的值为需要安装的安装包包名，保存  
  

4. 更新astro  
  更新astro：  
     
	点击Astro-sugo, ASTRO CLIENTs, dev220.sugo.net, 下拉到最下方，点击已安装, ASTRO CLIENT, UPGRADE_ASTRO  
	等待更新完即可
	