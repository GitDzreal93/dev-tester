## 给定一个字符串，怎么判断是不是ip地址？手写这段代码，并写出测试用例

#### code

**java版**

```java
public class IpTest{
    
    public static boolean isIpLegal(String str){
        //检查ip是否为空
        if(str == null){
            return false;
        }
        //检查ip长度，最短为:x.x.x.x(7位)，最长为xxx.xxx.xxx.xxx(15位)
        if(str.length()<7 || str.length()>15 ){
            return false;
        }
        //对输入字符串的首末字符判断，如果是"."则是非法IP
        if(str.charAt(0) == '.' || str.charAt(str.length()-1) == '.'){
            return false;
        }
        //按"."分割字符串，并判断分割出来的个数，如果不是4个，则是非法IP
        String[] arr = str.split("\\.");
        if(arr.length != 4){
            return false;
        }
        //对分割出来的每个字符串进行单独判断
        for(int i=0; i<arr.length; i++){
            //如果每个字符串不是一位字符，且以'0'开头，则是非法IP，如：01.002.03.004
            if(arr[i].length()>1 && arr[i].charAt(0) == '0'){
                return false;
            }
        }
        //对每个字符串中的每个字符进行逐一判断，如果不是数字0-9，则是非法IP
        for(int j=0; j<arr[i].length(); j++){
            if(arr[i].charAt(j) < '0' || arr[i].charAt(j) > '9'){
                return false;
            }
        }
        //对拆分的每一个字符串进行转换成数字，并判断是否在0~255之间
        for(int i=0; i<arr.length; i++){
            int temp = Integer.parseInt(arr[i]);
            if(i==0){
                if(temp < 1 || temp > 255){
                    return false;
                }
            }else{
                if(temp < 0 || temp > 255){
                return false;
                }
            }
        }
        return true;
    } 
    
    public static void main(String[] args){
      //输入一个字符串
      Scanner scanner = new Scanner(System.in);
      String ipStr = scanner.next();
      boolean isIpLegal = isIpLegal(isStr);
      if(isIpLegal){
          System.out.println(ipStr + "合法");
      }else{
          System.out.println(ipStr + "非法");
      }
   }
}
```

**python版**

```python
def ip_check(ip_str):
# 题目要求入参只有字符串类型
    if not isinstance(ip_str, str):
        return False
    if len(ip_str) < 7 or len(ip_str) > 15:
        return False
    if ip_str.startswith('.') or ip_str.endswith('.'):
        return False
    ip_lst = ip_str.split('.')
    if len(ip_lst) != 4:
        return False
    for idx, ip_num in enumerate(ip_lst):
        if not ip_num.isdigit():
            return False
        if len(ip_num) > 1 and ip_num.startswith('0'):
            return False
        if idx == 0:
            if int(ip_num) < 1 or int(ip_num) > 255:
                return False
        else:
            if int(ip_num) < 0 or int(ip_num) >255:
                return False
    return True
```



#### 有关ip地址的知识点：

##### 有效可用的ip地址

| A类                 | 1.0.0.0 - 126.255.255.254         |
| ------------------- | --------------------------------- |
| **A私有**           | **10.0.0.0 - 10.255.255.254**     |
| **B类**             | **128.0.0.0 - 191.255.255.254**   |
| **B私有**           | **172.16.0.0 - 172.31.255.254**   |
| **C类**             | **192.0.0.0 - 223.255.255.254**   |
| **C私有**           | **192.168.0.0 - 192.168.255.254** |
| **Windows自动分配** | **169.254.0.0 - 169.254.255.254** |

##### 有效但不可用的IP地址

| D类      | 244.0.0.0 - 239.255.255.254     |
| -------- | ------------------------------- |
| **E类**  | **240.0.0.0 - 255.255.255.254** |
| **全网** | **0.x.x.x, x.x.x.0**            |
| **广播** | **x.x.x.255**                   |
| **回环** | **127.0.0.0 - 127.255.255.254** |

#### 测试用例

##### 等价划分类

| 输入            | 结果                            |
| --------------- | ------------------------------- |
| 64.11.22.33     | 有效可用                        |
| 10.12.13.14     | 有效可用，不能直接访问公网      |
| 151.123.234.56  | 有效可用                        |
| 172.20.123.56   | 有效可用，不能直接访问公网      |
| 192.127.35.65   | 有效可用                        |
| 192.168.128.128 | 有效可用，不能直接访问公网      |
| 169.254.15.200  | 有效可用，不能直接访问公网      |
| 224.1.2.3       | 有效不可用，超过有效范围（D类） |
| 250.11.22.33    | 有效不可用，超过有效范围（E类） |
| 0.200.3.4       | 有效不可用，全网地址            |
| 64.11.22.0      | 有效不可用，全网地址            |
| 10.12.13.255    | 有效不可用，广播地址            |
| 127.50.60.70    | 有效不可用，回环地址            |

