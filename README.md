# Permissions-module
###用户权限模块
###模型：
####用户表
1、Account
	private int id; 标识符
	private String name; 用户名
	private String appkey; 身份验证key
	private String secretkey;
	private int status; 
	private String ext1; 前台用户id
记录用户信息

接口表
2、Interface
	private int id;
	private String title; 标题
	private String description; 描述
	private String address; 地址
	private String method; 提交方法
	private int ownerId; 创建者
	private int tableId; 应用在那张表
	private String createdTime; 创建时间
	private int status; 是否可用
	private String type; 类型
	private String updateTime; 修改时间
记录接口的详细信息

权限表
3、Accessibities
	private int id;
	private int accountId; 用户id
	private int interfaceId; 接口id
	private String accessTime; 访问时间
	private int status; 状态
	private String createdTime; 创建时间
	private String expiredAt; 过期时间
	private String createdBy; 谁给谁创建
	private String updateTime; 修改时间
记录用户访问接口的权限

表信息表
4、TableInfo
	private int id;
	private int ownerId; 用户id
	private String tableName; 表名称
	private String fieldsInfo; 字段信息
	private String createTime; 创建时间
	private int status; 状态
	private String updateTime; 修改时间
记录创建的表的信息

数据库表
5、DbInfo
	private int id;
	private int ownerId; 用户id
	private String dbName; 数据库名称
	private Date createTime; 创建时间
	private int status; 状态
记录创建的数据库的信息

Controller

1、AccessibitiesController
方法
resetAccessibitiesStatus  重置权限状态
getAccessibitiesByID 根据id查看权限
deleteAccessibitiesStatus  删除权限
getAccountByInterfaceId 根据接口查看使用的用户
suspendAccessibitiesStatus 暂缓权限状态
addAccessibities 新增一条权限
getAccessibities 查看权限信息

2、AccountController
方法
getAccountByName 根据用户名查看用户信息
getAccountById 根据id获取用户信息
resetKey 重置appkey、secretkey
createAccount 新建用户
getAllAccount 获取所有用户信息
deleteAccountByName 根据用户名删除用户

3、DbController
方法
created  创建数据库表
getDbInfo 查看数据库信息

4、InterfaceController
方法
checkInterface 判断访问接口是否有权限
updateInterfaceById 根据id修改接口信息
searchInterfaceByTable 根据表名称看接口信息
getInterface 查看所有接口信息
getInterfaceByID 根据id查看接口信息
addInterface 新增接口
deleteInterface 删除接口

5、TableInfoController
方法
getTableInfoByName 根据表名查看表信息
getTableInfoById 根据id查看表信息 
suspendTableInfo修改表的状态成暂缓
addTableInfo 新增表信息
resetTableInfo 重置表的状态
deleteTableInfo 删除表信息

测试用例：
AccountController：
一、getAccountByName 根据用户名查看用户信息
	//判断用户是否存在
		1、 @Test
	 public void getAccessibitiesById() {
	   		String res = restTemplate.getForObject("http://192.168.1.123:8082/account/Administrator", String.class);
				assertThat(res, containsString("OK"));
    }
存在Administrator返回true

	2、 @Test
	 public void getAccountByName1() {
		 String res = restTemplate.getForObject("http://192.168.1.123:8082/account/zhangsan", String.class);
		 assertThat(res, containsString("OK"));
	 }
不存在zhangsan返回false

	3、 @Test
	 public void getAccountByName1() {
		 String res = restTemplate.getForObject("http://192.168.1.123:8082/account/*eoro=d", String.class);
		 assertThat(res, containsString("OK"));
	 }
用户名不符合，返回 Invalid Account Name: 5~20 digit and alphabet only

二、getAccountById 根据id获取用户信息
	  @Test
	1、public void getAccessibitiesInfo() {
		String res = restTemplate.getForObject("http://localhost:8082/account", String.class);
		assertThat(res, containsString("OK"));
	}

有数据返回true
没有数据返回false

三、resetKey 重置appkey、secretkey
1、//重置appkey、sercurtkey
		 @Test
	public void resetkey() {
		Object obj = "{\"name\":\"zmcdhr\"}";
		String res = restTemplate.postForObject("http://192.168.1.123:8082/account/zhu/resetkey", obj, String.class);
		assertThat(res, containsString("OK"));
	}
修改成功返回true

2、	public void resetkey() {
		Object obj = "{\"name\":\"zmcdhr\"}";
		String res = restTemplate.postForObject("http://192.168.1.123:8082/account/zmvla/resetkey", obj, String.class);
		assertThat(res, containsString("OK"));
	}
返回false，没有这个用户



四、createAccount 新建用户

1、	// 创建用户
	 @Test
	public void createAccount() {
		Object obj = "{\"name\":\"zmcdhr\",\"ext1\":\"1\"}";
		String res = restTemplate.postForObject("http://192.168.1.123:8082/account", obj, String.class);
		assertThat(res, containsString("OK"));
	}
2、创建成功返回true
	// 创建用户
	 @Test
	public void createAccount() {
		Object obj = "{\"name\":\"zm1*cdhr\",\"ext1\":\"1\"}";
		String res = restTemplate.postForObject("http://192.168.1.123:8082/account", obj, String.class);
		assertThat(res, containsString("OK"));
	}
返回false不符合条件。

五、getAllAccount 获取所有用户信息
// @Test
	public void getAllAccount() {
		String res = restTemplate.getForObject("http://192.168.1.123:8082/account", String.class);
		assertThat(res, containsString("OK"));
	}

六、deleteAccountByName 根据用户名删除用户
正确返回true，
错误返回false


第二个Controller
DbController
一、新建数据库
1、
public void resetkey() {
		Object obj = "{\"name\":\"test1\"}";
		String res = restTemplate.postForObject("http://localhost:8082/db ", obj, String.class);
		assertThat(res, containsString("OK"));
	}
true返回创建成功。

2、查看数据库信息
	public void updateAccessibityDelete() {
//		Object obj = "{access_time:1}";
		String res = restTemplate.getForObject("http://192.168.1.123:8082/db ",
				String.class);
		assertThat(res, containsString("OK"));
	}
查看数据库信息，



第三个Controller
TableController：
一、	根据id查看table信息

public void getTableInfoById() {
		 Object obj = "";
		 String res = restTemplate.postForObject(baseUri + "/table/tableId_1", obj, String.class);
		 assertThat(res, containsString("OK"));
	 }

没有table返回false
 
public void getTableInfoById() {
		 Object obj = "";
		 String res = restTemplate.postForObject(baseUri + "/table/tableId_13", obj, String.class);
		 assertThat(res, containsString("OK"));
	 }
 
True返回table信息

二、	根据表名获取表信息

public void getTableInfoById() {
		 Object obj = "";
		 String res = restTemplate.getForObject(baseUri + "/table/test", obj, String.class);
		 assertThat(res, containsString("OK"));
	 }
有表名返回true

public void getTableInfoById() {
		 Object obj = "";
		 String res = restTemplate.getForObject(baseUri + "/table/testx", obj, String.class);
		 assertThat(res, containsString("OK"));
	 }
没有表名返回Empty value

三、	新增表信息

public void getTableInfoById() {
		 Object obj = "{tableName:test,fieldsInfo:asdlfj,accountname:administr1ator}";
		 String res = restTemplate.postForObject(baseUri + "/table/tableId_13", obj, String.class);
		 assertThat(res, containsString("OK"));
	 }
返回false 用户不存在

public void getTableInfoById() {
		 Object obj = "{tableName:tes2t,fieldsInfo:asdlfj,accountname:zhangsan}";
		 String res = restTemplate.postForObject(baseUri + "/table/tableId_13", obj, String.class);
		 assertThat(res, containsString("OK"));
	 }
添加成功。

四、	重置表状态

public void getTableInfoById() {
		 Object obj = "{tableName:tes2t,fieldsInfo:asdlfj,accountname:zhangsan}";
		 String res = restTemplate.postForObject(baseUri + "/table/13/reset", obj, String.class);
		 assertThat(res, containsString("OK"));
	 }



Reset-0可用

public void getTableInfoById() {
		 Object obj = "{tableName:tes2t,fieldsInfo:asdlfj,accountname:zhangsan}";
		 String res = restTemplate.postForObject(baseUri + "/table/13/suspend", obj, String.class);
		 assertThat(res, containsString("OK"));
	 }

suspend-1暂缓

public void getTableInfoById() {
		 Object obj = "{tableName:tes2t,fieldsInfo:asdlfj,accountname:zhangsan}";
		 String res = restTemplate.postForObject(baseUri + "/table/13/delete", obj, String.class);
		 assertThat(res, containsString("OK"));
	 }

返回true修改成功，返回false修改的表为空
delete-2不可用


第四个Controller
accessibityController
一、	新增权限
public void addAccessibities() {
		 Object obj = "{ accessTime:1,accountName:ads,interfaceId:1,status:0}";
		 String res = restTemplate.postForObject(baseUri + "/accessibities", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }
添加成功返回true
public void addAccessibities() {
		 Object obj = "{ accessTime:1,accountName:a2ds,interfaceId:1,status:10}";
		 String res = restTemplate.postForObject(baseUri + "/accessibities", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }
添加失败
1、	用户不存在

public void addAccessibities() {
		 Object obj = "{ accessTime:1,accountName:a2ds,interfaceId:1,status:10}";
		 String res = restTemplate.postForObject(baseUri + "/accessibities", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }

2、	Address不存在

public void addAccessibities() {
		 Object obj = "{ accessTime:1,accountName:a2ds,interfaceId:1,status:10}";
		 String res = restTemplate.postForObject(baseUri + "/accessibities", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }

3、	状态错误

二、根据id查看权限
public void addAccessibities() {
		 Object obj = "{ accessTime:1,accountName:a2ds,interfaceId:1,status:10}";
		 String res = restTemplate.postForObject(baseUri + "/accessibities /accessibitiesId_1", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }
true返回权限信息

public void addAccessibities() {
		 Object obj = "{ accessTime:1,accountName:a2ds,interfaceId:1,status:10}";
		 String res = restTemplate.postForObject(baseUri + "/accessibities /accessibitiesId_1", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }
没有返回false

二、	查看所有权信息

public void addAccessibities() {
		 String res = restTemplate.getForObject(baseUri + "/accessibities ", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }
有数据返回权限信息，没有数据返回空。

三、	修改权限状态


修改权限状态

public void addAccessibities() {
		 Object obj = "{ accessTime:1,accountName:a2ds,interfaceId:1,status:10}";
		 String res = restTemplate.postForObject(baseUri + "/accessibities /13/reset", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }
Reset-0可用
public void addAccessibities() {
		  
		 String res = restTemplate.postForObject(baseUri + "/accessibities /13/suspend", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }
suspend-1暂缓

public void addAccessibities() {
			 String res = restTemplate.postForObject(baseUri + "/accessibities /13/delete", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }
delete-2不可用

第五个controller

InterfaceController

一、查看interface是否有权限
	public void checkInterface() {
		String res = restTemplate.getForObject(baseUri + "/interface/check?n=zhangsan&a=/add/as&m=GET",
				String.class);
		assertThat(res, containsString("OK"));// 显示表中字段，没有ok，测试错误。
	}
返回true 有权限访问这个interface
一、查看interface是否有权限
	public void checkInterface() {
		String res = restTemplate.getForObject(baseUri + "/interface/check?n=zhangsan&a=/add/adddds&m=GET",
				String.class);
		assertThat(res, containsString("OK"));// 显示表中字段，没有ok，测试错误。
	}

没有这个接口 无权限

二、新增addInterface

public void addAccessibities() {
		 Object obj = "{ address:1,accountName:a2ds,interfaceId:1,status:10}";
		 String res = restTemplate.postForObject(baseUri + "/ interfaceId ", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }

True新增成功

public void addAccessibities() {
		 Object obj = "{ tableName:121,accountName:a2ds,interfaceId:1ds,status:10}";
		 String res = restTemplate.postForObject(baseUri + "/ interfaceId ", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }
返回错误
1、	状态不符合


public void addAccessibities() {
		 Object obj = "{ tableName:121,accountName:a2ds,interfaceId:1ds,status:10}";
		 String res = restTemplate.postForObject(baseUri + "/ interfaceId ", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }
返回错误

2、	参数不合法



public void addAccessibities() {
		 Object obj = "{ tableName:121,accountName:a2ds,interfaceId:1ds,status:10}";
		 String res = restTemplate.postForObject(baseUri + "/ interfaceId ", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }
返回错误

3、	accountName 不存在


public void addAccessibities() {
		 Object obj = "{ tableName:121,accountName:a2ds,interfaceId:1ds,status:10}";
		 String res = restTemplate.postForObject(baseUri + "/ interfaceId ", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }
返回错误

4、	tableName不存在


四、	根据id查看接口

public void getInterfaceByID () {
		 Object obj = "{ address:1,accountName:a2ds,interfaceId:1,status:10}";
		 String res = restTemplate.getForObject(baseUri + "/interfaceId_14", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }
True 返回接口信息

public void getInterfaceByID () {
		 Object obj = "{ address:1,accountName:a2ds,interfaceId:1,status:10}";
		 String res = restTemplate.getForObject(baseUri + "/interfaceId_143", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }
False 没有接口信息返回空

五、	获取所有接口信息

public void getInterface() {
		 String res = restTemplate.getForObject(baseUri + "/interface", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }
True查看所有用户信息，没有返回空


六、	修改接口信息

public void getInterfaceByID () {
		 Object obj = "{ address:1,tableName:a2ds,status:1,type:10,title:10,description:10}";
		 String res = restTemplate.getForObject(baseUri + "/interfaceId_143", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }

成功true


public void getInterfaceByID () {
		 Object obj = "{ address:1dd22,tableName:a2asdfds,status:d1,type:1d0,title:1d0,description:d10}";
		 String res = restTemplate.postForObject(baseUri + "/interfaceId_143", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }

False
1、	状态错误

public void getInterfaceByID () {
		 Object obj = "{ address:1dd22,tableName:a2asdfds,status:d1,type:1d0,title:1d0,description:d10}";
		 String res = restTemplate.postForObject(baseUri + "/interfaceId_143", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }

返回错误
2、	没有接口信息


public void getInterfaceByID () {
		 Object obj = "{ address:1dd22,tableName:a2asdfds,status:d1,type:1d0,title:1d0,description:d10}";
		 String res = restTemplate.postForObject(baseUri + "/interfaceId_143", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }
返回错误

3、	没有表信息



删除接口

True返回删除成功，false失败


根据表名称查看下面的接口信息

public void getInterfaceByID () {
		 Object obj = "{ address:1dd22,tableName:a2asdfds,status:d1,type:1d0,title:1d0,description:d10}";
		 String res = restTemplate.getForObject(baseUri + "/interfaceId_143", obj, String.class);
		 assertThat(res, containsString("FAIL"));
	 }

有数据显示所有表的接口 


