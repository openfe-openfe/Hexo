---
title: 卫星遥感图像管理系统
comments: true
date: 2016-03-17 16:01:55
update: 2016-03-17 16:01:55
categories: 作品集
tags: ['Java','我的作品','Hadoop','PostgreSQL']
---


![Login UI](http://7xrnl9.com1.z0.glb.clouddn.com/image%2F%E5%8D%AB%E6%98%9F%E5%9B%BE%E5%83%8F%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F%2Flogin.png)

## 背景

在我大三那年比赛回来之后听说院里的云计算实验室要招人做一个协同创新项目，虽然面试已结束，但还是找到了老师获得了进入实验室的机会。

在实验室的日子还是蛮开心、蛮充实的，貌似对于一只程序员来说，人生绝大部分的回忆都是再和代码做斗争吧。

当时项目组的项目是从新疆遥感所接到的一个项目，主要的内容是搭建Hadoop分布式集群来存储海量的卫星遥感图像数据，并使用MapReduce对图像数据进行分布式计算。而当时我们将图像数据上传到集群中的方式都是通过命令行，还是蛮麻烦的，所以我们的赵老师就有了开发一个`卫星遥感图像管理系统`的想法。最后开发的任务落在了我的身上，经过N久的努力，也终于成功开发了出来，这也是第一个我可以稍微拿得出手的作品吧。

## 项目概述

> * 项目名：卫星遥感图像管理系统
> * 语言：Java
> * 运行平台：PC
> * 数据库：PostgreSQL
> * 架构模式：MVC

## 系统功能

系统的功能并不复杂，主要分为两个模块：

> * 用户模块
> * 文件模块

### 用户模块

> 1. 用户注册
> 2. 邮箱激活
> 3. 用户登录
> 4. 修改资料
> 5. 修改密码
> 6. 记住密码
> 7. 自动登录
> 8. 退出登录

### 文件模块

> 1. 文件上传(可上传多文件或文件夹)
> 2. 文件下载(可下载多文件或文件夹)
> 3. 查看文件列表
> 4. 新建文件夹
> 5. 删除文件
> 6. 文件分类
> 7. 文件搜索
> 8. 文件分类搜索
> 9. 刷新文件列表
> 10. 遥感图像3D可视化查看

## 系统设计

### 数据库设计

系统的E-R图如下图所示(省略部分字段)：

![E-R图](http://7xrnl9.com1.z0.glb.clouddn.com/image%2F%E5%8D%AB%E6%98%9F%E5%9B%BE%E5%83%8F%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F%2FE-R%E5%9B%BE.png)

根据功能需求，建立的数据表分别为：

> 1. 用户表（rs_users）
> 2. 文件信息表（rs_file_info）
> 3. 图像元数据表（rs_org_img）
> 4. 卫星表（rs_satelite）
> 5. 卫星传感器表（rs_sensor）

### 接口设计

用户操作接口：

```java
public interface UserDAO {
	public boolean addUser(RsUsers user);	// 添加用户
	public boolean updateUser(RsUsers user);	// 更新用户信息
	public boolean deleteUser(RsUsers user);	// 删除用户
	public boolean deleteUserFromName(String userName);	// 根据用户名删除用户
	public List<RsUsers> listAllUser();	// 查询所有用户
	public RsUsers getUser(String userName);	// 根据用户名获取用户信息
	public RsUsers getUser(int userId);	// 根据用户ID获取用户信息
	public Connection getConnection();	// 获取数据库连接
	public void setConnection(Connection connection);	// 设置数据库连接
}
```

文件操作接口：

```java
public interface FileInfoDAO {
	public boolean addFileInfo(RsFileInfo fileInfo);	// 添加单个文件
	public boolean addFileInfos(List<RsFileInfo> listFileInfos);	// 批量添加文件
	public boolean updateFileInfo(RsFileInfo fileInfo);	// 更新文件信息
	public boolean updateFileInfos(List<RsFileInfo> listFileInfos);	// 批量更新文件信息
	public boolean deleteFileInfos(List<String> filePaths);	// 批量删除文件信息
	public List<RsFileInfo> listFileInfos(String fatherDir,int fileDepth);	// 列出当前目录下的文件
	public RsFileInfo getFileInfo(String filePath);	// 通过文件路径获取文件信息
	public List<RsFileInfo> getAllFiles(String filePath);	// 获取路径下所有文件
	public List<RsFileInfo> getSearchFiles(String filePath);// 获取搜索文件的结果集
	public Connection getConnection();
	public void setConnection(Connection connection);
}
```

HDFS操作接口：

```java
public interface HdfsDAO {
	public boolean uploadFile(String localPath,String uploadPath);	// 上传文件
	public boolean downloadFile(String downloadPath,String savePath);	// 下载文件
	public boolean deleteFile(String filePath);  // 删除文件
	public boolean renameFile(String filePath,String newPath);	// 重命名文件名
	public boolean createNewDir(String dirPath); // 创建新的目录
	public FileStatus[] listCurrentPathFiles(String currentPath);
}
```

## 程序设计

### 软件架构
在程序设计上，我把程序的架构分为了四层：

> 1. DTO（数据传输对象）
> 2. DAO（数据访问对象）
> 3. Service（业务逻辑层）
> 4. UI（界面-使用Swing）

UI主要负责界面的显示，以Java事件驱动响应用户的请求，并负责调用Service层执行业务逻辑，而Service则负责各模块的复杂性的功能，Service层按照功能的需求调用DAO层的各种方法对数据库进行操作，DAO层则只需要负责对数据库进行简单的增、删、改、查即可，遵循单一职责原则。

### HDFS文件系统

卫星遥感图像数据分为结构化数据及非结构化数据，根据图像数据的特点，我们将结构化的数据存储在PostgreSQL空间数据库中，将非结构化的海量图像数据存储在Hadoop Distribute File System（Hadoop分布式文件系统）中。

```java
/**
 * Function: HDFS操作接口：上传，下载，新建，删除，获取文件等
 * Description: HDFS操作接口，实现与HDFS的文件交互操作
 * Copyright： Copyright(c) 2015
 * Company: 滁州学院 - 大数据与云计算实验室
 * 
 * @author Kylin
 * @version 1.0
 */
public class HdfsDAOImpl implements HdfsDAO {
	//HDFS 首选根目录地址
	private static String HDFS_PREFERRED_URL = Config.HDFS_ROOT_PATH+"/";
	public HdfsDAOImpl() {
		super();
	}
	public void init(){	//初始化
		Configuration config = new Configuration();
		FileSystem fs = null;
		try {
			fs = FileSystem.get(URI.create(HDFS_PREFERRED_URL), config);
		} catch (IOException e) {
			e.printStackTrace();
		}
		try {
			FileStatus[] filesList = fs.listStatus(new Path(HDFS_PREFERRED_URL));
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	/**
	 * Description: 上传文件
	 * @param localPath 需要上传的文件路径
	 * @param uploadPath 文件在HDFS中保存的路径
	 */
	@Override
	public boolean uploadFile(String localPath, String uploadPath){
		boolean result = false;
		Configuration config = new Configuration();
		FileSystem fileSystem = null;
		FileInputStream inputStream = null;  
		OutputStream outputStream = null;  
		try {
			fileSystem = FileSystem.get(URI.create(uploadPath), config);
			inputStream = new FileInputStream(new File(localPath));	//创建输入流；
			outputStream = fileSystem.create(new Path(uploadPath));//创建输出流；
			IOUtils.copyBytes(inputStream, outputStream, 4096, true);
			result = true;
		} catch (Exception e) {
			e.printStackTrace();
		}finally{
			try {
				outputStream.close();   //关闭输出流
				inputStream.close();	//关闭输入流
			} catch (IOException e) {
				e.printStackTrace();
			}
		}	
		return result;
	}
	/**
	 * Description: 下载文件
	 * @param downloadPath 下载的文件路径
	 * @param savePath 文件保存路径
	 */
	@Override
	public boolean downloadFile(String downloadPath, String savePath){
		boolean result = false;
		Configuration conf = new Configuration();
		FileSystem fs = null;
		FSDataInputStream hdfsInStream = null;
		OutputStream outputStream = null;
		byte[] ioBuffer = new byte[2048];
		int readLen = 0;
		try {
			fs = FileSystem.get(URI.create(downloadPath), conf);
			hdfsInStream = fs.open(new Path(downloadPath));
			outputStream = new FileOutputStream(savePath);
			readLen = hdfsInStream.read(ioBuffer);
		} catch (Exception e) {
			e.printStackTrace();
		}
		while (-1 != readLen) {
			try {
				outputStream.write(ioBuffer, 0, readLen);
				readLen = hdfsInStream.read(ioBuffer);
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
		if(-1 == readLen){
			result = true;
		}
		try {
			outputStream.close();
			hdfsInStream.close();
			fs.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
		return result;
	}
	/**
	 * Description: 删除文件
	 * @param filePath 删除文件的路径
	 */
	@Override
	public boolean deleteFile(String filePath){
		boolean result = false ;
		Configuration conf = new Configuration();
		Path deletePath = new Path(filePath);
		FileSystem hdfs = null;
		try {
			hdfs = deletePath.getFileSystem(conf);
			if (hdfs.exists(deletePath)) {
				hdfs.delete(deletePath, true);
				result = true ;
			}
		} catch (IOException e) {
			e.printStackTrace();
		}
		return result;
	}
	/**
	 * Description: 文件重命名
	 * @param filePath 文件路径
	 * @param newFilePath 新文件名
	 */
	@Override
	public boolean renameFile(String filePath,String newFilePath) {
		boolean result = false;
		Configuration conf = new Configuration();
	    Path oldPath = new Path(filePath);
	    Path newPath = new Path(newFilePath);
		FileSystem fs = null;
		try {
			fs = FileSystem.get(URI.create(filePath),conf);
			if(fs.rename(oldPath, newPath)){
				result = true;
			}
		} catch (IOException e) {
			e.printStackTrace();
		}
		return result;
	}
	/**
	 * Description: 新建文件夹
	 * @param dirPath 文件夹路径
	 */
	@Override
	public boolean createNewDir(String dirPath){
		boolean result = false;
		Configuration conf = new Configuration();
		Path createDirPath = new Path(dirPath);
		try {
			FileSystem hdfs = createDirPath.getFileSystem(conf);
			hdfs.mkdirs(createDirPath);
			result = true;
		} catch (IOException e) {
			e.printStackTrace();
		}
		return result;
	}
	/**
	 * Description: 获取当前目录下的所有文件
	 * @param currentPath 当前目录的路径
	 */
	@Override
	public FileStatus[] listCurrentPathFiles(String currentPath){
		Configuration config = new Configuration();
		FileSystem fs = null;
		FileStatus[] filesList = null;	
		try {
			fs = FileSystem.get(URI.create(currentPath), config);
			filesList = fs.listStatus(new Path(currentPath));
		} catch (Exception e) {
			e.printStackTrace();
		}
		return filesList;
	}
}	
```

## 界面设计

软件界面为了适应用户的使用，可以拖动边框改变窗体的大小，界面中的组件将根据窗体的改变以百分比的形式改变自身大小，从而达到适应性布局。当界面小到一定程度以后，界面组件为了不变形将保持最小尺寸，当界面的大小大于设定值时再重新更新组件大小。

界面主要有以下几个界面：

> 1. 登录界面
> 2. 注册界面
> 3. 找回密码界面
> 4. 激活账号界面
> 5. 主界面
> 6. 三维地球可视化界面

### 登录界面：

![Login](http://7xrnl9.com1.z0.glb.clouddn.com/image%2F%E5%8D%AB%E6%98%9F%E5%9B%BE%E5%83%8F%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F%2Flogin.png)

### 注册界面：

![Register](http://7xrnl9.com1.z0.glb.clouddn.com/image%2F%E5%8D%AB%E6%98%9F%E5%9B%BE%E5%83%8F%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F%2Fregister.png)

### 主界面：

![MainUI](http://7xrnl9.com1.z0.glb.clouddn.com/image%2F%E5%8D%AB%E6%98%9F%E5%9B%BE%E5%83%8F%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F%2Fmian.png)

### 三维地球可视化界面：

![3D-Earth](http://7xrnl9.com1.z0.glb.clouddn.com/image%2F%E5%8D%AB%E6%98%9F%E5%9B%BE%E5%83%8F%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F%2F3D-Earth.png)

### 遥感地图可视化界面：

![遥感图像](http://7xrnl9.com1.z0.glb.clouddn.com/image%2F%E5%8D%AB%E6%98%9F%E5%9B%BE%E5%83%8F%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F%2F5.png)

## 总结

在云计算实验室的日子还是蛮怀念的，可惜后来还是离开了实验室来到了中科大先研院实习，毕竟年轻见识太少，还是希望出去多见识见识这个多姿多彩的世界。

致谢：

赵老师：给了我尽情发挥的空间。

王**：我实验室的同事，帮助我开发了那个炫酷的三维地球模块，使用的是WorldWind。

