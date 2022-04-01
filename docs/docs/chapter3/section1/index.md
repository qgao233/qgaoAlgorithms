# 格尔软件笔试(java)

## 第1题 二叉搜索树

```
package com.bermuda.algorithms.jobTest;

import java.util.Stack;


/**
 * 实现要求：
 * 1、根据已有的代码片段，创建二叉搜索树；
 * 2、用中序遍历输出排序结果，结果形如：0，1，2 ，3 ，4， 5， 6， 7， 8， 9，
 * 3、使用递归、非递归二种方式实现遍历；
 * 4、注意编写代码注释。
 */
public class BinarySearchTree {

    private static class Node{
        public int val;
        public Node left;
        public Node right;

        public Node(){}
        public Node(int val){this.val=val;}
    }

    public static void main(String[] args) {

        final int[] values = { 1, 3, 4, 5, 2, 8, 6, 7, 9, 0 };

        System.out.println("Create BST: ");
        Node root = createBinaryTree(values);

        System.out.println("Traversing the BST with recursive algorithm: ");
        inOrderTransvalWithRecursive(root);
        System.out.println();

        System.out.println("Traversing the BST with iterative algorithm: ");
        inOrderTransvalWithIterate(root);
    }

    // 构建二叉树
    public static Node createBinaryTree(int[] values) {
        // 第一个结点为根结点
        Node root = new Node(values[0]);
        for(int i = 1; i < values.length; i++){
            insertBinaryTree(root, values[i]);
        }
        return root;
    }

    //插入结点
    public static Node insertBinaryTree(Node root, int value){
        //到叶子结点了
        if(root == null) return new Node(value);

        //插入的新值小于等于当前结点的值，往左子树走，否则往右子树走
        if(value <= root.val) root.left = insertBinaryTree(root.left, value);
        else root.right = insertBinaryTree(root.right, value);
        
        return root;
    }

    // 递归实现
    public static void inOrderTransvalWithRecursive(Node node) {
        if(node == null) return ;

        inOrderTransvalWithRecursive(node.left);

        //中序遍历位置
        System.out.print(node.val + ", ");

        inOrderTransvalWithRecursive(node.right);
    }

    // 非递归实现
    public static void inOrderTransvalWithIterate(Node root) {
        // 使用栈来替代递归的方法栈
        Stack<Node> nodeStack = new Stack<>();
        // 在树上移动的指针
        Node cur = root;
        while(cur != null || !nodeStack.isEmpty()){
            //类似递归时左子树的入栈
            while(cur != null){
                nodeStack.push(cur);
                cur = cur.left;
            }
            if(!nodeStack.isEmpty()){
                //处理当前结点，即中序遍历位置
                Node node = nodeStack.pop();
                System.out.print(node.val + ", ");
                //处理右子树
                if(node.right != null) cur = node.right;
            }
        }
    }
}
```

## 第2题 SOCKET ECHO

```
package com.bermuda.algorithms.jobTest;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;
import java.net.SocketException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Iterator;
import java.util.Scanner;
import java.util.concurrent.*;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.regex.Pattern;

/**
 * 实现要求：
 * 1、根据代码片段实现一个简单的SOCKET ECHO程序；
 * 2、接受到客户端连接后，服务端返回一个欢迎消息;
 * 3、接受到"bye"消息后， 服务端返回一个结束消息，并结束当前连接;
 * 4、采用telnet作为客户端，通过telnet连接本服务端；
 * 5、服务端支持接受多个telnet客户端连接;
 * 6、服务端支持命令操作，支持查看当前连接数、断开指定客户端连接；
 * 7、注意代码注释书写。
 *
 */
public class EchoApplication {

    public static void main(String[] args) throws IOException, InterruptedException {

        final int listenPort = 12345;

        // 启动服务端
        EchoServer server = new EchoServer(listenPort);
        server.startService();

        // 服务端启动后，运行结果示例：
        /**
         java -cp ./classes EchoApplication

         2020-03-31 16:58:44.049 - Welcome to My Echo Server.(from SERVER)
         The current connections:
         Id.			Client				LogonTime
         -----------------------------------------------------
         1			127.0.0.1:32328		2020-03-31 16:59:13
         2			127.0.0.1:43434		2020-03-31 17:03:02
         3			127.0.0.1:39823		2020-03-31 07:03:48

         Enter(h for help): h
         The commands:
         ----------------------------------------------------
         q		query current connections
         d id		disconnect client
         x		quit server
         h		help

         Enter(h for help): d 1
         2020-03-31 16:58:44.049 - The connection '127.0.0.1:32328' has been disconnected.
         The current connections:
         Id.			Client				LogonTime
         -----------------------------------------------------
         1			127.0.0.1:43434		2020-03-31 17:03:02
         2			127.0.0.1:39823		2020-03-31 07:03:48

         Enter(h for help): x
         2020-03-31 16:58:44.049 - The server has exited. Bye!
         */

        // 在telnet控制台输入，服务端直接原文返回输入信息
        // 客户端结果示例：
        /**
         2020-03-31 16:58:44.049 - Welcome to My Echo Server.(from SERVER)

         Enter: hello!
         2020-03-31 16:58:55.452 - hello!(from SERVER)

         Enter: This is KOAL.
         2020-03-31 16:59:06.565 - This is KOAL.(from SERVER)

         Enter: What can i do for you?
         2020-03-31 16:59:12.828 - What can i do for you?(from SERVER)

         Enter: bye!
         2020-03-31 16:59:16.502 - Bye bye!(from SERVER)
         */
    }

}

class EchoServer {

    private static final int CORE_POOL_SIZE = 5;
    private static final int MAX_POOL_SIZE = 10;                    //io密集型 2n
    private static final int QUEUE_CAPACITY = 20;                   //任务队列大小
    private static final Long KEEP_ALIVE_TIME = 1L;                 //空闲线程存活的最长时间
    private static final TimeUnit TIME_UNIT = TimeUnit.SECONDS;     //时间单位

    private static final SimpleDateFormat DATE_FORMAT = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss.SSS");
    private static final String RETURN_STRING = "(from SERVER)";

    private ThreadPoolExecutor threadPool;                   //创建线程池实例，以应对多个连接
    private CopyOnWriteArrayList<Client> clientList = new CopyOnWriteArrayList<>(); //存储已连客户端的列表

    private int listenPort;                                         //监听接口
    public boolean isInteruptMainThread;                            //是否中断主线程的标志位


    public EchoServer(int listenPort){
        this.listenPort = listenPort;
    }

    //开启服务器
    public void startService() {
        ServerSocket serverSocket;
        try {
            //创建服务器Socket
            serverSocket = new ServerSocket(listenPort);
        } catch (IOException e) {
            System.out.println("创建服务器Socket失败,"+e.getMessage());
            return;
        }
        //创建线程池实例，以应对多个连接
        threadPool = new ThreadPoolExecutor(
                CORE_POOL_SIZE,
                MAX_POOL_SIZE,
                KEEP_ALIVE_TIME,
                TIME_UNIT,
                new ArrayBlockingQueue<>(QUEUE_CAPACITY),
                new NamingThreadFactory(Executors.defaultThreadFactory(),"服务器线程池"),
                new ThreadPoolExecutor.CallerRunsPolicy()   //主线程执行多余的任务
        );

        //启动一条额外线程用来接收用户输入的命令
        startCommandThread(this);

        //用于中断主线程的死循环
        while(!isInteruptMainThread){
            try {
                final Socket clientSocket = serverSocket.accept();
                //每来一条连接，就启动一条线程与之对应。
                threadPool.execute(()->{
                    //获取socket的输入流与输出流
                    BufferedReader socketIn = null;
                    PrintWriter socketOut = null;
                    try {
                        socketIn = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
                        socketOut = new PrintWriter(clientSocket.getOutputStream());

                        //记录连接到服务器上的客户端
                        String ip = clientSocket.getInetAddress().getHostAddress()+":"+clientSocket.getPort();
                        Thread curThread = Thread.currentThread();
                        Client newClient = new Client(ip,DATE_FORMAT.format(new Date()),curThread,socketIn);
                        clientList.add(newClient);

                        //欢迎信息
                        socketOut.print(DATE_FORMAT.format(new Date())+" - Welcome to My Echo Server.(from SERVER)\n");
                        socketOut.flush();

                        String inTemp = "";

                        //中断，让服务器可以随时终止某条连接
                        while(!inTemp.equals("bye") && !curThread.isInterrupted()){

                            inTemp = socketIn.readLine();//读取client传来的消息

                            socketOut.print(DATE_FORMAT.format(new Date())+" - "+inTemp+RETURN_STRING+"\n\nEnter: ");//加上日期格式，原封不动地回显
                            socketOut.flush();    //赶快刷新使client收到，也可以换成socketOut.println(inTemp, ture)

                        }
                        //从列表记录中删除已经断开的连接
                        Iterator iterator = clientList.iterator();
                        while(iterator.hasNext()){
                            Client c = (Client)iterator.next();
                            if(c.getCurThread() == Thread.currentThread()){
                                clientList.remove(c);
                            }
                        }
                        socketOut.print("\n"+DATE_FORMAT.format(new Date())+" - "+"The server has closed connection with u !!!"+RETURN_STRING+"\n\nEnter: ");//加上日期格式，原封不动地回显
                        socketOut.flush();

                    } catch (IOException e) {
                        e.printStackTrace();
                    }
//                    catch (Exception ie) {
//                        // InterruptedException异常保证，当InterruptedException异常产生时，线程被终止。
//                        System.out.println("test");
//                    }
                    finally {
                        try{
                            if(socketIn != null)
                                socketIn.close();
                            if(socketOut != null)
                                socketOut.close();
                            clientSocket.close();
                        }catch (IOException e){
                            e.printStackTrace();
                        }

                    }
                });
            } catch (IOException e) {
                System.out.println(DATE_FORMAT.format(new Date())+" - The server has exited. Bye!");
                return;
            }
        }
    }

    //开启接收命令的线程
    private void startCommandThread(final EchoServer echoServer){

        //涉及到命令的工具包
        final class CommandHelper{
            //校验命令的基本合法性
            public boolean validateCommand(String input){
                input = input.trim().toLowerCase();
                if(input.equals("")) return false;

                String[] command = input.split(" ");
                if(command.length > 2){
                    return false;
                }
                return true;
            }
            //判断输入是否是整数
            public boolean isInteger(String str) {
                Pattern pattern = Pattern.compile("^[-\\+]?[\\d]*$");
                return pattern.matcher(str).matches();
            }
            //打印命令无效信息
            public void printInvalidInfo(){
                System.out.println("invalid command !!!");
                System.out.print("\nEnter(h for help): ");
            }
        }

        new Thread(()->{
            //打印欢迎信息
            System.out.println(DATE_FORMAT.format(new Date())+" - Welcome to My Echo Server.(from SERVER)");
            echoServer.printClientList();

            //用户输入
            Scanner sc = new Scanner(System.in);
            String input = sc.nextLine();
            CommandHelper commandHelper = new CommandHelper();

            boolean isShutdown = false;

            while(!input.equals("x")){
                //1. 先校验输入的基本合法性
                boolean isValid = commandHelper.validateCommand(input);
                if(!isValid){
                    commandHelper.printInvalidInfo();
                    //继续接收输入
                    input = sc.nextLine();
                    continue;
                }
                //2. 再处理连接断开命令"d"
                String[] command = input.split(" ");
                if(command.length == 2 && command[0].equals("d") && commandHelper.isInteger(command[1])){
                    if(!echoServer.closeConnectionWith(Integer.parseInt(command[1]))){
                        System.out.println("client "+command[1]+" does not exist !!!");
                    }
                    System.out.print("\nEnter(h for help): ");
                }
                //3. 再处理其它命令
                else if (command.length == 1){
                    switch (input){
                        //打印已连接的客户端列表
                        case "q":
                            echoServer.printClientList();
                            break;
                        //关闭服务器
                        case "x":
                            echoServer.stopService();
                            isShutdown = true;
                            break;
                        //帮助
                        case "h":
                            EchoServer.printHelpInfo();
                            break;
                        default:
                            commandHelper.printInvalidInfo();
                    }
                }else commandHelper.printInvalidInfo();

                //跳出循环
                if(isShutdown) break;
                //继续接收输入
                input = sc.nextLine();
            }


        },"commandThread").start();
    }

    //打印帮助信息
    public static void printHelpInfo(){
        StringBuilder sb = new StringBuilder();
        sb.append("The commands:\n");
        sb.append("-----------------------------------------------------\n");
        sb.append("q\t\tquery current connections\n");
        sb.append("d id\t\tdisconnect client\n");
        sb.append("x\t\tquit server\n");
        sb.append("h\t\thelp\n");
        sb.append("\nEnter(h for help): ");
        System.out.print(sb.toString());
    }

    //关闭和某个客户端的连接
    public boolean closeConnectionWith(int id){
        if(id - 1 >= clientList.size() || id < 1) return false;
        Client client = clientList.get(id-1);
        clientList.remove(id-1);

        Thread t = client.getCurThread();
        BufferedReader socketIn = client.getSocketIn();
        t.interrupt();      //中断子线程
        try {
            socketIn.close();   //中断阻塞在io上的子线程
        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.print(DATE_FORMAT.format(new Date())+" - ");
        System.out.println("The connection '"+client.getIp()+"' has been disconnected.");
        printClientList();
        return true;
    }

    //打印已连接的客户端
    public void printClientList(){
        StringBuilder sb = new StringBuilder();
        sb.append("The current connections:\n");
        sb.append("Id.\t\t\tClient\t\t\t\tLoginTime\n");
        sb.append("-----------------------------------------------------\n");
        for(int i = 1; i<=clientList.size(); i++){
            sb.append(i+"\t\t\t");
            sb.append(clientList.get(i-1).getIp()+"\t\t");
            sb.append(clientList.get(i-1).getLoginTime()+"\n");
        }
        sb.append("\nEnter(h for help): ");
        System.out.print(sb.toString());
    }

    //停止服务
    public boolean stopService(){
        //关闭主线程
        isInteruptMainThread = true;
        //关闭线程池
        threadPool.shutdownNow();
        return true;
    }

    //存储客户端的相关信息
    private class Client{
        private String ip;          //ip+port
        private String loginTime;   //登录时间
        private Thread curThread;   //客户端进行交互的线程
        private BufferedReader socketIn; //客户端的输入资源，只有关闭他，才能中断io阻塞

        public Client(String ip, String loginTime,Thread curThread,BufferedReader socketIn){
            this.ip = ip;
            this.loginTime = loginTime;
            this.curThread = curThread;
            this.socketIn = socketIn;
        }

        public String getIp() {
            return ip;
        }

        public String getLoginTime() {
            return loginTime;
        }

        public Thread getCurThread() {
            return curThread;
        }

        public BufferedReader getSocketIn() {
            return socketIn;
        }
    }

    //线程池的自定义命名
    private final class NamingThreadFactory implements ThreadFactory {

        private final AtomicInteger threadNum = new AtomicInteger();
        private final ThreadFactory delegate;
        private final String name;

        /**
         * 创建一个带名字的线程池生产工厂
         */
        public NamingThreadFactory(ThreadFactory delegate, String name) {
            this.delegate = delegate;
            this.name = name;
        }

        @Override
        public Thread newThread(Runnable r) {
            Thread t = delegate.newThread(r);
            t.setName(name + " [#" + threadNum.incrementAndGet() + "]");
            return t;
        }

    }
}
```

## 第3题 简易日志类

```
package com.bermuda.algorithms.jobTest;

import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;

/**
 * 实现要求：
 * 1、根据代码片段，参考log4j/slf4j等公共日志库，编写一个自定义的简易日志类；
 * 2、至少支持文件输出、控制台输出二种日志输出方式，支持同时输出到文件和控制台；
 * 3、支持DEBUG/INFO/WARN/ERROR四种日志级别；
 * 4、请合理进行设计模式，进行接口类、抽象类等设计；
 * 5、注意代码注释书写。
 */

public abstract class KLLogger {

    public static void main(String[] args) {

        final KLLogger logger = KLLogger.getLogger(KLLogger.class);

        logger.setAppender(new ConsoleAppender());

        logger.setLogLevel(DebugLevel.DEBUG);
        logger.debug("debug 1...");
        logger.info("info 1...");
        logger.warn("warn 1...");
        logger.error("error 1...");

        logger.setAppender(new FileAppender());

        logger.setLogLevel(DebugLevel.ERROR);
        logger.debug("debug 2...");
        logger.info("info 2...");
        logger.warn("warn 2...");
        logger.error("error 2...");

    }

    public static KLLogger getLogger(Class clazz){
        KLLoggerFactory loggerFactory = getLoggerFactory();
        return loggerFactory.getLogger(clazz.getName());
    }

    public static KLLoggerFactory getLoggerFactory(){
        return new MyLoggerFactory();
    }

    protected abstract void setAppender(MyLoggerAppender appender);
    protected abstract void setLogLevel(DebugLevel debugLevel);
    protected abstract void debug(String msg);
    protected abstract void info(String msg);
    protected abstract void warn(String msg);
    protected abstract void error(String msg);

}

interface KLLoggerFactory{
    KLLogger getLogger(String name);
}

/**
 * 真正的日志实现类
 * debug<info<warn<error
 */
class MyLogger {

    private static final SimpleDateFormat DATE_FORMAT = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");

    private String name;                                //当前是哪个类log的
    private ArrayList<MyLoggerAppender> appenders;      //当前log的所有输出方式
    private DebugLevel currDebugLevel;                        //当前log的输出等级

    public MyLogger(String name) {
        this.name = name;
    }

    public void setAppender(MyLoggerAppender appender) {
        if(appenders == null) appenders = new ArrayList<>();
        appenders.add(appender);
    }

    public void setLogLevel(DebugLevel debugLevel) {
        this.currDebugLevel = debugLevel;
    }

    //遍历所有的打印方式，进行日志输出
    private void print(String msg, String level){
        for(MyLoggerAppender appender : appenders){
            appender.print("["+level+"] "+DATE_FORMAT.format(new Date())+" "+name+" : "+msg);
        }
    }

    public void debug(String msg) {
        //若debug < 当前日志等级，则不输出，否则输出
        if(DebugLevel.DEBUG.compareTo(currDebugLevel) < 0) return;
        else print(msg, "debug");
    }

    public void info(String msg) {
        if(DebugLevel.INFO.compareTo(currDebugLevel) < 0) return;
        else print(msg, "info");
    }

    public void warn(String msg) {
        if(DebugLevel.WARN.compareTo(currDebugLevel) < 0) return;
        else print(msg, "warn");
    }

    protected void error(String msg) {
        if(DebugLevel.ERROR.compareTo(currDebugLevel) < 0) return;
        else print(msg, "error");
    }
}

//对mylogger进行适配
class MyLoggerAdapter extends KLLogger{
    private MyLogger myLogger;

    public MyLoggerAdapter(MyLogger myLogger) {
        this.myLogger = myLogger;
    }

    @Override
    public void setAppender(MyLoggerAppender appender) {
        myLogger.setAppender(appender);
    }

    @Override
    public void setLogLevel(DebugLevel debugLevel) {
        myLogger.setLogLevel(debugLevel);
    }

    @Override
    public void debug(String msg) {
        myLogger.debug(msg);
    }

    @Override
    public void info(String msg) {
        myLogger.info(msg);
    }

    @Override
    public void warn(String msg) {
        myLogger.warn(msg);
    }

    @Override
    protected void error(String msg) {
        myLogger.error(msg);
    }
}

//获得日志实现类的工厂
class MyLoggerFactory implements KLLoggerFactory {
    @Override
    public KLLogger getLogger(String name) {
        return new MyLoggerAdapter(new MyLogger(name));
    }
}

//抽象出具体功能
abstract class MyLoggerAppender{
    protected abstract void print(String msg);
}

//控制台输出
class ConsoleAppender extends MyLoggerAppender{

    @Override
    public void print(String msg) {
        System.out.println(msg);
    }
}

//文件输出
class FileAppender extends MyLoggerAppender{

    private static final String FILE_NAME = "mylogger_output";      //默认文件名

    private String fileName;                                        //文件名

    public FileAppender(){
        this.fileName = FILE_NAME;
    }

    public FileAppender(String fileName){
        this.fileName = fileName;
    }

    @Override
    public void print(String msg) {
        //日志文件放在当前项目根目录下，以追加的形式进行写
        try(BufferedWriter bw = new BufferedWriter(new FileWriter(fileName+".log",true))){
            bw.write(msg+"\n");
            bw.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

//日志打印级别
enum DebugLevel {
    DEBUG, INFO, WARN, ERROR;
}
```
