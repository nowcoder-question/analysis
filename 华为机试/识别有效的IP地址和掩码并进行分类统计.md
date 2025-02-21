## 题目
[题目链接](https://www.nowcoder.com/practice/de538edd6f7e4bc3a5689723a7435682?tpId=37&tqId=36842&sourceUrl=/exam/oj&channenl=wgithub&fromPut=wgithub)

## 解题思路
1. 对每行输入进行处理：
   - 按空格分割得到 IP 地址和子网掩码
   - 分别验证 IP 地址和子网掩码的合法性

2. IP 地址合法性检查：
   - 按点分割成 4 个数字
   - 每个数字必须在 0-255 范围内
   - 检查是否属于私有 IP 范围

3. 子网掩码合法性检查：
   - 按点分割成 4 个数字
   - 每个数字必须在 0-255 范围内
   - 转换成二进制后必须是连续的 1 后跟连续的 0

4. IP 地址分类：
   - A类：1.0.0.0 - 126.255.255.255
   - B类：128.0.0.0 - 191.255.255.255
   - C类：192.0.0.0 - 223.255.255.255
   - D类：224.0.0.0 - 239.255.255.255
   - E类：240.0.0.0 - 255.255.255.255

---

## 代码

``` cpp []
#include <bits/stdc++.h>
using namespace std;

// 思路：（首先排除0.*.*.* 和127.*.*.* 的IP地址
// ，因为这种IP地址不属于任何一类需要计数的类型）
// 1.判断子网掩码是否正确：错误属于IP或掩码错误情况，相应计数加1，继续下一条字符串判断,重新开始1；正确进入2
// 2.此时掩码正确，判断IP地址是否正确：错误属于IP或掩码错误情况，相应计数加1，继续下一条字符串判断,重新开始1；正确进入3
// 3.此时IP正确，判段属于A/B/C/D/E哪类地址，同时判断是否属于私有地址，相应计数加1，继续下一条字符串判断，重新开始1
int main() {
    string t;
    int na = 0;
    int nb = 0;
    int nc = 0;
    int nd = 0;
    int ne = 0;
    int ni = 0;  // number of invalid;
    int np = 0;  // number of private;
    while (getline(cin, t)) {
        int len = t.length();
        int tilde = t.find('~');  // seperate: ~
        string s_ip = t.substr(0, tilde);
        string s_mk = t.substr(tilde + 1, len - tilde - 1);

        // Step-1: Ignoring 0.*.*.* and 127.*.*.*;
        int ipPoint1 = s_ip.find('.');
        string ip1 = s_ip.substr(0, ipPoint1);
        if (ip1 == "0" || ip1 == "127") {
            continue;
        }

        // 判断子网掩码是否正确
        int maskPoint1 = s_mk.find('.');  // 查找子网掩码中第1个点
        string mask1 = s_mk.substr(0, maskPoint1);
        if (mask1 == "" || stoi(mask1) > 255) {  // 不能为空位
            ni++;
            continue;
        }
        bitset<8> bMask1(
            stoi(mask1));  // 将字符串形式的数字转为整型后，获取其8位二进制值

        int maskPoint2 = s_mk.find(
            '.',
            maskPoint1 + 1);  // 从maskPoint1 + 1位置开始，查找子网掩码中第2个点
        string mask2 = s_mk.substr(maskPoint1 + 1, maskPoint2 - maskPoint1 - 1);
        if (mask2 == "" || stoi(mask2) > 255) {  // 不能为空位
            ni++;
            continue;
        }
        bitset<8> bMask2(stoi(mask2));

        int maskPoint3 =
            s_mk.find('.', maskPoint2 + 1);  // 查找子网掩码中第一个点
        string mask3 = s_mk.substr(maskPoint2 + 1, maskPoint3 - maskPoint2 - 1);
        if (mask3 == "" || stoi(mask3) > 255) {  // 不能为空位
            ni++;
            continue;
        }
        bitset<8> bMask3(stoi(mask3));

        string mask4 =
            s_mk.substr(maskPoint3 + 1, s_mk.length() - maskPoint3 - 1);
        if (mask4 == "" || stoi(mask4) > 255) {  // 不能为空位
            ni++;
            continue;
        }
        bitset<8> bMask4(stoi(mask4));

        // Error-Mask: All zero or all one;
        if (bMask1.count() + bMask2.count() + bMask3.count() + bMask4.count() ==
                32 ||
            bMask1.count() + bMask2.count() + bMask3.count() + bMask4.count() ==
                0) {
            ni++;
            continue;
        }
        int vMask[32] = {0};  // 将二进制的子网掩码放入整型数组vMask，用于判断子网掩码中的连续1
        for (int i = 0; i < 32; i++) {
            if (i >= 0 && i <= 7) {
                vMask[i] = bMask1[7 - i];
            } else if (i >= 8 && i <= 15) {
                vMask[i] = bMask2[7 - i + 8];
            } else if (i >= 16 && i <= 23) {
                vMask[i] = bMask3[7 - i + 16];
            } else if (i >= 24 && i < 31) {
                vMask[i] = bMask4[7 - i + 24];
            }
        }

        bool ErrorContinue =
            false;  // 判断是否存在连续的1 ，不连续1非法 i,j 双指针判断
        for (int i = 0, j = 1; j < 32;) {
            if (vMask[i] == 0 &&
                vMask[j] ==
                    1) {  // 对一串子网掩码而言，出现 ****01***即是非法的掩码
                ErrorContinue = true;
                break;
            }
            i++;
            j++;
        }
        if (ErrorContinue) {
            ni++;
            continue;
        }

        // Step-3: Verify IP correctness;
        if (ip1 == "" || stoi(ip1) > 255) {
            ni++;
            continue;
        }

        int ipPoint2 = s_ip.find('.', ipPoint1 + 1);
        string ip2 = s_ip.substr(ipPoint1 + 1, ipPoint2 - ipPoint1 - 1);
        if (ip2 == "" || stoi(ip2) > 255) {
            ni++;
            continue;
        }
        int ipPoint3 = s_ip.find('.', ipPoint2 + 1);
        string ip3 = s_ip.substr(ipPoint2 + 1, ipPoint3 - ipPoint2 - 1);
        if (ip3 == "" || stoi(ip3) > 255) {
            ni++;
            continue;
        }

        string ip4 = s_ip.substr(ipPoint3 + 1, s_ip.length() - ipPoint3 - 1);
        if (ip4 == "" || stoi(ip4) > 255) {
            ni++;
            continue;
        }

        // Step-4: All clear, classify IP to A-B-C-D-E;
        int judge1 = stoi(ip1);
        int judge2 = stoi(ip2);
        if (judge1 >= 1 && judge1 <= 126) {  // A 类
            na++;
            if (judge1 == 10) {
                np++;
            }
        } else if (judge1 >= 128 && judge1 <= 191) {  // B 类
            nb++;
            if (judge1 == 172 && judge2 >= 16 && judge2 <= 31) {
                np++;
            }
        } else if (judge1 >= 192 && judge1 <= 223) {  // C 类
            nc++;
            if (judge2 == 168) {
                np++;
            }
        } else if (judge1 >= 224 && judge1 <= 239) {  // D 类
            nd++;
        } else if (judge1 >= 240 && judge1 <= 255) {  // E 类
            ne++;
        }
    }

    cout << na << " " << nb << " " << nc << " " << nd << " " << ne << " " << ni
         << " " << np << " " << endl;
}

```

``` java []
import java.util.Scanner;
import java.util.BitSet;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int na = 0;  // A类IP数量
        int nb = 0;  // B类IP数量
        int nc = 0;  // C类IP数量
        int nd = 0;  // D类IP数量
        int ne = 0;  // E类IP数量
        int ni = 0;  // 错误IP数量
        int np = 0;  // 私有IP数量
        
        while (sc.hasNextLine()) {
            String t = sc.nextLine();
            int tilde = t.indexOf('~');
            String s_ip = t.substring(0, tilde);
            String s_mk = t.substring(tilde + 1);
            
            // Step-1: 忽略 0.*.*.* 和 127.*.*.* 的IP地址
            int ipPoint1 = s_ip.indexOf('.');
            String ip1 = s_ip.substring(0, ipPoint1);
            if (ip1.equals("0") || ip1.equals("127")) {
                continue;
            }
            
            // 判断子网掩码是否正确
            int maskPoint1 = s_mk.indexOf('.');
            String mask1 = s_mk.substring(0, maskPoint1);
            try {
                if (mask1.isEmpty() || Integer.parseInt(mask1) > 255) {
                    ni++;
                    continue;
                }
                BitSet bMask1 = BitSet.valueOf(new long[]{Integer.parseInt(mask1)});
                
                int maskPoint2 = s_mk.indexOf('.', maskPoint1 + 1);
                String mask2 = s_mk.substring(maskPoint1 + 1, maskPoint2);
                if (mask2.isEmpty() || Integer.parseInt(mask2) > 255) {
                    ni++;
                    continue;
                }
                BitSet bMask2 = BitSet.valueOf(new long[]{Integer.parseInt(mask2)});
                
                int maskPoint3 = s_mk.indexOf('.', maskPoint2 + 1);
                String mask3 = s_mk.substring(maskPoint2 + 1, maskPoint3);
                if (mask3.isEmpty() || Integer.parseInt(mask3) > 255) {
                    ni++;
                    continue;
                }
                BitSet bMask3 = BitSet.valueOf(new long[]{Integer.parseInt(mask3)});
                
                String mask4 = s_mk.substring(maskPoint3 + 1);
                if (mask4.isEmpty() || Integer.parseInt(mask4) > 255) {
                    ni++;
                    continue;
                }
                BitSet bMask4 = BitSet.valueOf(new long[]{Integer.parseInt(mask4)});
                
                // 检查掩码是否全0或全1
                int totalOnes = bMask1.cardinality() + bMask2.cardinality() + 
                               bMask3.cardinality() + bMask4.cardinality();
                if (totalOnes == 32 || totalOnes == 0) {
                    ni++;
                    continue;
                }
                
                // 检查掩码的连续性
                int[] vMask = new int[32];
                for (int i = 0; i < 8; i++) {
                    vMask[i] = bMask1.get(7 - i) ? 1 : 0;
                    vMask[i + 8] = bMask2.get(7 - i) ? 1 : 0;
                    vMask[i + 16] = bMask3.get(7 - i) ? 1 : 0;
                    vMask[i + 24] = bMask4.get(7 - i) ? 1 : 0;
                }
                
                boolean errorContinue = false;
                for (int i = 0; i < 31; i++) {
                    if (vMask[i] == 0 && vMask[i + 1] == 1) {
                        errorContinue = true;
                        break;
                    }
                }
                if (errorContinue) {
                    ni++;
                    continue;
                }
                
                // 验证IP地址的正确性
                if (ip1.isEmpty() || Integer.parseInt(ip1) > 255) {
                    ni++;
                    continue;
                }
                
                int ipPoint2 = s_ip.indexOf('.', ipPoint1 + 1);
                String ip2 = s_ip.substring(ipPoint1 + 1, ipPoint2);
                if (ip2.isEmpty() || Integer.parseInt(ip2) > 255) {
                    ni++;
                    continue;
                }
                
                int ipPoint3 = s_ip.indexOf('.', ipPoint2 + 1);
                String ip3 = s_ip.substring(ipPoint2 + 1, ipPoint3);
                if (ip3.isEmpty() || Integer.parseInt(ip3) > 255) {
                    ni++;
                    continue;
                }
                
                String ip4 = s_ip.substring(ipPoint3 + 1);
                if (ip4.isEmpty() || Integer.parseInt(ip4) > 255) {
                    ni++;
                    continue;
                }
                
                // IP分类统计
                int judge1 = Integer.parseInt(ip1);
                int judge2 = Integer.parseInt(ip2);
                
                if (judge1 >= 1 && judge1 <= 126) {  // A类
                    na++;
                    if (judge1 == 10) {
                        np++;
                    }
                } else if (judge1 >= 128 && judge1 <= 191) {  // B类
                    nb++;
                    if (judge1 == 172 && judge2 >= 16 && judge2 <= 31) {
                        np++;
                    }
                } else if (judge1 >= 192 && judge1 <= 223) {  // C类
                    nc++;
                    if (judge1 == 192 && judge2 == 168) {
                        np++;
                    }
                } else if (judge1 >= 224 && judge1 <= 239) {  // D类
                    nd++;
                } else if (judge1 >= 240 && judge1 <= 255) {  // E类
                    ne++;
                }
                
            } catch (NumberFormatException e) {
                ni++;
                continue;
            }
        }
        
        System.out.println(na + " " + nb + " " + nc + " " + nd + " " + ne + " " + ni + " " + np);
        sc.close();
    }
}
```

``` python []
def main():
    na = nb = nc = nd = ne = ni = np = 0  # 初始化计数器
    
    while True:
        try:
            t = input().strip()
            tilde = t.find('~')
            s_ip = t[:tilde]
            s_mk = t[tilde + 1:]
            
            # Step-1: 忽略 0.*.*.* 和 127.*.*.* 的IP地址
            ipPoint1 = s_ip.find('.')
            ip1 = s_ip[:ipPoint1]
            if ip1 == "0" or ip1 == "127":
                continue
            
            # 判断子网掩码是否正确
            try:
                maskPoint1 = s_mk.find('.')
                mask1 = s_mk[:maskPoint1]
                if not mask1 or int(mask1) > 255:
                    ni += 1
                    continue
                bMask1 = bin(int(mask1))[2:].zfill(8)
                
                maskPoint2 = s_mk.find('.', maskPoint1 + 1)
                mask2 = s_mk[maskPoint1 + 1:maskPoint2]
                if not mask2 or int(mask2) > 255:
                    ni += 1
                    continue
                bMask2 = bin(int(mask2))[2:].zfill(8)
                
                maskPoint3 = s_mk.find('.', maskPoint2 + 1)
                mask3 = s_mk[maskPoint2 + 1:maskPoint3]
                if not mask3 or int(mask3) > 255:
                    ni += 1
                    continue
                bMask3 = bin(int(mask3))[2:].zfill(8)
                
                mask4 = s_mk[maskPoint3 + 1:]
                if not mask4 or int(mask4) > 255:
                    ni += 1
                    continue
                bMask4 = bin(int(mask4))[2:].zfill(8)
                
                # 检查掩码是否全0或全1
                total_ones = (bMask1 + bMask2 + bMask3 + bMask4).count('1')
                if total_ones == 0 or total_ones == 32:
                    ni += 1
                    continue
                
                # 检查掩码的连续性
                vMask = []
                for b in [bMask1, bMask2, bMask3, bMask4]:
                    vMask.extend([int(x) for x in b])
                
                error_continue = False
                for i in range(31):
                    if vMask[i] == 0 and vMask[i + 1] == 1:
                        error_continue = True
                        break
                if error_continue:
                    ni += 1
                    continue
                
                # 验证IP地址的正确性
                if not ip1 or int(ip1) > 255:
                    ni += 1
                    continue
                
                ipPoint2 = s_ip.find('.', ipPoint1 + 1)
                ip2 = s_ip[ipPoint1 + 1:ipPoint2]
                if not ip2 or int(ip2) > 255:
                    ni += 1
                    continue
                
                ipPoint3 = s_ip.find('.', ipPoint2 + 1)
                ip3 = s_ip[ipPoint2 + 1:ipPoint3]
                if not ip3 or int(ip3) > 255:
                    ni += 1
                    continue
                
                ip4 = s_ip[ipPoint3 + 1:]
                if not ip4 or int(ip4) > 255:
                    ni += 1
                    continue
                
                # IP分类统计
                judge1 = int(ip1)
                judge2 = int(ip2)
                
                if 1 <= judge1 <= 126:  # A类
                    na += 1
                    if judge1 == 10:
                        np += 1
                elif 128 <= judge1 <= 191:  # B类
                    nb += 1
                    if judge1 == 172 and 16 <= judge2 <= 31:
                        np += 1
                elif 192 <= judge1 <= 223:  # C类
                    nc += 1
                    if judge1 == 192 and judge2 == 168:
                        np += 1
                elif 224 <= judge1 <= 239:  # D类
                    nd += 1
                elif 240 <= judge1 <= 255:  # E类
                    ne += 1
                    
            except ValueError:
                ni += 1
                continue
                
        except EOFError:
            break
    
    print(f"{na} {nb} {nc} {nd} {ne} {ni} {np}")

if __name__ == "__main__":
    main()
```

---

## 算法及复杂度
- 算法：字符串处理 + 位运算
- 时间复杂度：$\mathcal{O}(n)$，其中 n 是输入的行数
- 空间复杂度：$\mathcal{O}(1)$，只需要常数级别的额外空间
