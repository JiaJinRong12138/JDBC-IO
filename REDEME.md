#笔记 ---- JDBC
##一、
>1.下载数据库
>> 这里使用的是MySQL数据库，下载及安装方法、
[MySQL下载及安装](http://www.runoob.com/mysql/mysql-install.html)
>>java链接数据库所需：
>>[Jar包](https://mvnrepository.com/artifact/mysql/mysql-connector-java)
>>步骤：
```
 //初始化
    private static void init(){
        driver = "com.mysql.cj.jdbc.Driver";
        username = "root";
        password = "1006";
        url = "jdbc:mysql://localhost:3306/javaend";
    }
    //创建链接
    private static void getConn() throws SQLException, ClassNotFoundException {
        //注册驱动
        Class.forName(driver);
        System.out.println("驱动注册成功");
        //创建链接
        connection = DriverManager.getConnection(url,username,password);
        System.out.println("数据库链接成功");
    }

    //创建statement
    private static void createStat() throws SQLException {
        statement = connection.createStatement();
        System.out.println("创建statement成功");
    }
    //关闭链接
    private static void close() throws SQLException {
        if(resultSet != null){
            resultSet.close();
        }
        if(statement != null){
            statement.close();
        }
        if(connection != null){
            connection = null;
        }
    }
```
##二、增删查改：
###增：statement.execute(插入操作)
###删：statement.execute(删除操作)
###改：statement.execute(修改/更新操作)
###查：ResultSet rs = statement.executeQuery(查询操作)’
##三、PreparedStatement()
###示例代码：
```
            String intsertSQL = "INSERT INTO jdbc_test VALUE(?, ?, ?);";
            ps = conn.prepareStatement(intsertSQL);
            ps.setInt(1, (int)(Math.random()*100));
            ps.setString(2, "欧阳铁柱");
            ps.setString(3, "1");
            ps.execute();
```
##四、开启事务
###示例代码：
```
//            开启事务
            Statement stat = connection.createStatement();
//            关闭自动提交
            connection.setAutoCommit(false);
            stat.execute("update jdbc_test set name = '"+name1+"' where id= 17");
            stat.execute("update jdbc_test set name = '"+name2+"' where id= 28");
//            手动提交
            connection.commit();
```


#笔记 ---- 异常处理
##关键词：try、 catch、 finally、  throw、 throws
##语法：
###try、 catch、 finally
```
try{
    //可能会出现异常的语句
}
catch(Exception e){
    //抛出异常，继续执行程序
}
finally{
    //不管是否抛出异常都要执行该语句
}
```
### throw、 throws
```
//throws 声明该方法可能会抛出异常
public static void main(String[] args) throws Exception {

        if(new Scanner(System.in).next().equals("h")){
        //throw 抛出异常（可以是自定义异常）
            throw new Exception("答案相同");
        }
    }
```

##使用Throwable抛出异常
```
    try {
        new FileInputStream(f);
        //使用Throwable进行异常捕捉
    } catch (Throwable t) {
        // TODO Auto-generated catch block
        t.printStackTrace();
    }
```

#笔记 ---- I/O流
##1、File类
###常用方法
#### >. 获取绝对路径
`getAbsolutePath()`
#### >. 获取绝对文件（类似与上一方法）
`getAbsoluteFile()`
#### >. 获取文件名
`getname()`
#### >. 获取所在文件夹路径
`getParent()`
#### >. 获取文件路径
`getPath()`
#### >.文件是否存在
`exists()`
#### >.是否是文件夹
`isDirectory()`
#### >.是否是文件
`isFile()`
#### >.是否是绝对路径
`isAbsolute()`
#### >.是否为隐藏
`isHidden()`
#### >.文件长度
`length()`
#### >.文件最后一次修改时间
`lastModified()`
#### >.修改文件名
`renameTo()`
#### >.返回当前文件夹下的所有文件(File[])
`listFiles()`
#### >.返回文件夹下的子文件夹和子文件（String[]）
`list()`
#### >.创建文件夹，如果父文件夹不存在，创建就无效
`mkdir()`
#### >.创建文件夹，如果父文件夹不存在，就会创建父文件夹
`mkdirs()`
#### >.创建一个空文件,如果父文件夹不存在，就会抛出异常
`createNewFile()`
#### >.列出所有盘符
`listRoots()`
#### >.删除文件
`delete()`
#### >.在JVM结束时删除文件(可用于创建临时文件)
`deleteOnExit()`
##2、流
###输入流：InputStream()
###输出流：OutputStream()
###字节流Demo
```
public static void main(String[] args)  {
        File f = new File("D:\\img\\test.txt");
        FileOutputStream fos = null;
        FileInputStream fis = null;
        if(!f.exists()){
            try {
                f.createNewFile();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        try {
            //实例化输出流
            fos = new FileOutputStream(f);
            
            //写入字节
            fos.write(new byte[]{65, 66});
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(fos != null){
                try {
                //关闭流
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

        try {
            //实例化输入流
            fis = new FileInputStream(f);
            byte[] bs = new byte[(int) f.length()];
            
            //读取字节
            fis.read(bs);
            for (byte b : bs){
                System.out.println(b);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(fis != null){
                try {
                //关闭流
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }
```
###注：关闭流的方式：
#### >1.使用finally
```
finally{
    fis.close();
}
```

#### >2.使用try
```
try(FileInputStream fis = new FileInputStream(f)){
    ...
}
```

###字符流Demo
```
 public static void main(String[] args)  {
        File f = new File("D:\\img\\test.txt");
        if(!f.exists()){
            try {
                f.createNewFile();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try(FileWriter fw = new FileWriter(f)){
                fw.write("HelloWorld");
            } catch (IOException e) {
                e.printStackTrace();
            }
            try(FileReader fr = new FileReader(f)) {
                char[] c = new char[(int) f.length()];
                fr.read(c);
                for (char r:c
                     ) {
                    System.out.print(r);
                }
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
```
###缓存流Demo
```
public static void main(String[] args)  {
        File f = new File("D:\\Text.txt");
        try(
                //创建字符流
                FileWriter fw = new FileWriter(f);
                //缓存流需要创建在一个存在的流上
                PrintWriter pw = new PrintWriter(fw);
                ) {
            pw.println("HelloWorld, 你好世界");
            //强制将缓存的数据写入硬盘，不管缓存是否满
            pw.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }
        try(
                FileReader fr = new FileReader(f);
                BufferedReader br = new BufferedReader(fr);
        ) {
            String line = null;
            while ((line = br.readLine()) != null){
                System.out.println(line);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```
###数据流Demo
```
public static void main(String[] args)  {
        File f = new File("D:\\Text.txt");
        if(!f.exists()){
            try {
                if(f.createNewFile()){
                    System.out.println("创建文件成功");
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        //写入数据
        try(
                FileOutputStream fos = new FileOutputStream(f);
                DataOutputStream dos = new DataOutputStream(fos);
                ) {
            dos.writeBoolean(true);
            dos.writeInt(314);
            dos.writeUTF("HHHHH");
        } catch (IOException e) {
            e.printStackTrace();
        }

        try(
                FileInputStream fis = new FileInputStream(f);
                DataInputStream dis = new DataInputStream(fis);
                ) {
            System.out.println(dis.readBoolean());
            System.out.println(dis.readInt());
            System.out.println(dis.readUTF());
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
```
###对象流Demo
```
package com.ioStream;


import java.io.*;

public class MainTest {
    public static void main(String[] args) {
        Test t = new Test("Jerry", 18);
        File f = new File("D:\\Test.txt");
        if(!f.exists()){
            try {
                f.createNewFile();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        try(
                //创建对象输出流
                FileOutputStream fos = new FileOutputStream(f);
                ObjectOutputStream oos = new ObjectOutputStream(fos);

                //创建对象输入流
                FileInputStream fis = new FileInputStream(f);
                ObjectInputStream ois = new ObjectInputStream(fis);

                ) {

            oos.writeObject(t);
            Test tt = (Test) ois.readObject();
            System.out.println(tt.toString());

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}

//需要进行序列化
class Test implements Serializable{
    private String name;
    private Integer age;

    public Test(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Test{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```
####注：在使用对象流是需要对传输的对象进行序列化（将对象信息转化为存储或传输的过程）
###System.in
```
try(
        InputStream is = System.in;
        ) {
    int i = is.read();
    System.out.println(i);
} catch (IOException e) {
    e.printStackTrace();
}
```




