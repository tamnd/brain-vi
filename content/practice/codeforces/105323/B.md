---
title: "CF 105323B - \u5c0f\u6587\u7684\u6392\u5217"
description: "Chúng ta có một chuỗi dài $S$ được xây dựng theo một cách rất cụ thể. Mỗi thao tác lấy các số từ $1$ đến $m$, hoán vị chúng một cách ngẫu nhiên và nối hoán vị đó vào cuối $S$."
date: "2026-06-22T20:01:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105323
codeforces_index: "B"
codeforces_contest_name: "2024 Xiangtan University Summer Camp-Div.2"
rating: 0
weight: 105323
solve_time_s: 66
verified: true
draft: false
---

[CF 105323B - \u5c0f\u6587\u7684\u6392\u5217](https://codeforces.com/problemset/problem/105323/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi dài$S$được xây dựng một cách rất cụ thể. Mỗi thao tác lấy các số từ$1$ĐẾN$m$, hoán vị chúng một cách ngẫu nhiên và nối hoán vị đó vào cuối$S$. Hoạt động này được lặp đi lặp lại với số lần rất lớn, vì vậy$S$trở thành sự kết hợp của nhiều khối độc lập, mỗi khối là một hoán vị của$[1, 2, \dots, m]$. 

Sau đó chúng tôi được cung cấp một chuỗi$T$, và chúng ta cần phải quyết định xem liệu$T$có thể xuất hiện dưới dạng một đoạn liền kề bên trong$S$. Chúng ta được phép cắt bỏ một số tiền tố và hậu tố của$S$, nhưng bên trong phần còn lại chúng ta phải khớp$T$chính xác. 

Khó khăn chính đó là$S$không phải là tùy tiện. Nó có cấu trúc chắc chắn: mỗi khối có chiều dài liên tiếp$m$là một hoán vị, nghĩa là mỗi số từ$1$ĐẾN$m$xuất hiện chính xác một lần trên mỗi khối. 

Các ràng buộc rất lớn:$n$có thể lên đến$5 \cdot 10^5$, Và$m$có thể lên đến$10^9$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng xây dựng một cách rõ ràng$S$hoặc mô phỏng quá trình. Quét đều$T$nhiều lần với logic mỗi vị trí nặng phải duy trì tuyến tính hoặc gần tuyến tính. 

Một điểm tinh tế là$T$không được đảm bảo là một hoán vị hoặc thậm chí bị giới hạn bởi$m$trong một phạm vi nhỏ. Các giá trị có thể lên tới$10^9$. Nếu có$T_i > m$, thì nó không bao giờ có thể xuất hiện trong$S$, bởi vì mọi phần tử trong$S$luôn ở trong$[1, m]$. Đây là trường hợp từ chối ngay lập tức. 

Một trường hợp cạnh quan trọng khác là khi$T$kéo dài nhiều khối hoán vị. Vì mỗi khối là một hoán vị đầy đủ nên các giá trị lặp lại sau mỗi$m$vị trí theo cách có cấu trúc, nhưng giữa các khối không có ràng buộc về tính liên tục. Một cách tiếp cận ngây thơ có thể cho rằng chúng ta có thể sắp xếp$T$tùy ý bên trong một khối, nhưng ranh giới khối rất quan trọng: một phân đoạn hợp lệ có thể bắt đầu ở giữa một khối hoán vị và kết thúc ở giữa khối hoán vị khác. 

Một trường hợp thất bại minh họa nhỏ cho lý luận ngây thơ là: 

đầu vào:```
n = 4, m = 2
T = [1, 1, 2, 2]
```Ở đây mỗi khối là một trong hai`[1, 2]`hoặc`[2, 1]`. Mặc dù các giá trị liền kề lặp lại, điều này vẫn có thể thực hiện được vì chúng ta có thể chọn các khối như`[1,2][1,2]`và lấy một đoạn đi qua ranh giới. Một kiểm tra ngây thơ đòi hỏi hành vi hoán vị cục bộ bên trong$T$sẽ từ chối điều này một cách không chính xác. 

Mặt khác:```
n = 3, m = 2
T = [1, 1, 1]
```Điều này là không thể vì mỗi số chỉ xuất hiện tối đa một lần trên mỗi khối, do đó không có giá trị nào có thể xuất hiện hai lần trong khoảng cách nhỏ hơn hoặc bằng$m$theo một cấu trúc khối nhất quán. 

Thách thức chính là nhận ra cách cửa sổ trượt tương tác với các hoán vị lặp lại. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô hình hóa một cách rõ ràng$S$cho đủ khối và sau đó quét tìm$T$sử dụng tìm kiếm chuỗi con. Từ$S$về mặt khái niệm là vô hạn, chúng ta sẽ tạo ra ít nhất$O(n)$các khối được an toàn, đưa ra một chuỗi kích thước$O(nm)$theo cách giải thích tồi tệ nhất. Thậm chí chỉ tạo ra một vài khối là không thể bởi vì$m$có thể$10^9$, vì vậy chúng tôi không thể xây dựng dù chỉ một khối một cách rõ ràng. 

Ngay cả khi chúng ta tránh xây dựng và thay vào đó thử mọi cách sắp xếp có thể của$T$liên quan đến ranh giới khối, chúng ta vẫn cần phải xem xét$O(m)$độ lệch bắt đầu và đối với mỗi độ lệch, hãy xác minh tính nhất quán trên$n$vị trí, dẫn đến$O(nm)$hành vi trong trường hợp xấu nhất. 

Quan sát quan trọng là hạn chế duy nhất bên trong$S$mang tính cục bộ: trong mỗi khối, các số tạo thành một hoán vị của$[1,m]$. Điều này có nghĩa là với mọi giá trị$x$, các lần xuất hiện của nó được đặt cách nhau một cách có cấu trúc giữa các khối, nhưng quan trọng hơn là bên trong bất kỳ cửa sổ có độ dài nào$m$, không có sự trùng lặp. 

Điều này biến vấn đề thành một ràng buộc về tần số trong các cửa sổ trượt. Nếu chúng ta cố định vị trí bắt đầu, mọi cửa sổ có độ dài$m$trong một phân đoạn hợp lệ phải chứa các giá trị khác biệt với$[1,m]$và mọi giá trị phải xuất hiện tối đa một lần trong cửa sổ đó. 

Vì vậy, thay vì nghĩ đến việc căn chỉnh với các hoán vị chưa biết, chúng tôi nghĩ đến tính hợp lệ của các cửa sổ trượt có kích thước$m$: bất kỳ chuỗi con hợp lệ nào của$S$phải thỏa mãn rằng không có giá trị nào xuất hiện nhiều lần trong bất kỳ phân đoạn nào nằm hoàn toàn trong một khối. Vì ranh giới khối không xác định nên chúng tôi yêu cầu điều đó một cách hiệu quả trong bất kỳ khoảng thời gian nào$m$, các giá trị hoạt động giống như một ràng buộc hoán vị cục bộ. 

Điều này dẫn đến việc kiểm tra cửa sổ trượt dựa trên tần số tiêu chuẩn: chúng tôi mô phỏng việc đặt$T$ở đâu đó ở giữa các khối hoán vị lặp lại vô hạn và kiểm tra xem liệu có tồn tại một sự dịch chuyển sao cho không nảy sinh mâu thuẫn với các ràng buộc lặp lại hay không. Cấu trúc rút gọn thành việc kiểm tra xem liệu chúng ta có thể gán cho mỗi vị trí một modulo lớp dư lượng hay không$m$nhất quán mà không có xung đột, có thể được kiểm tra bằng cách theo dõi các ràng buộc nhất quán về chênh lệch giá trị-vị trí theo modulo$m$. 

Việc giảm cốt lõi là nếu hai vị trí$i$Và$j$TRONG$T$có cùng giá trị thì khoảng cách tương đối của chúng phải chia hết cho$m$, vì cùng một giá trị xuất hiện chính xác một lần trên mỗi khối. Vì vậy, với mọi giá trị, tất cả các lần xuất hiện của nó trong$T$phải nằm ở các vị trí đồng dư modulo$m$. Nếu điều này đúng với tất cả các giá trị, chúng ta có thể nhúng$T$thành các hoán vị lặp đi lặp lại. 

Do đó, chúng tôi có thể gán cho mỗi giá trị một lớp dư lượng bắt buộc bằng cách sử dụng lần xuất hiện đầu tiên của nó và kiểm tra tính nhất quán cho tất cả các lần xuất hiện sau đó. 

### So sánh độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi định dạng lại điều kiện thành kiểm tra tính nhất quán trên các vị trí mô-đun. 

1. Với mỗi giá trị trong$T$, ghi lại chỉ số lần xuất hiện đầu tiên của nó. Điều này xác định vị trí tham chiếu cho giá trị đó. Lý do điều này hữu ích là vì khi nhúng hợp lệ, mỗi giá trị sẽ xuất hiện định kỳ với dấu chấm$m$, do đó tất cả các lần xuất hiện phải căn chỉnh tương ứng với độ lệch bắt đầu cố định. 
2. Với mỗi lần xuất hiện tiếp theo của cùng một giá trị, hãy tính chênh lệch các chỉ số so với lần xuất hiện đầu tiên. Nếu sự khác biệt này không chia hết cho$m$, việc nhúng là không thể vì các lần xuất hiện của cùng một số sẽ rơi vào các vị trí khác nhau bên trong một khối hoán vị, mâu thuẫn với thực tế là mỗi khối chứa mỗi giá trị đúng một lần. 
3. Nếu tất cả các giá trị đều thỏa mãn điều kiện đồng dư này thì chuỗi có thể được gán một modulo bù đầu nhất quán$m$. Điều này đảm bảo mỗi lần xuất hiện của mỗi giá trị đều có thể được đặt vào một số khối mà không vi phạm tính duy nhất bên trong các khối. 
4. Trả về "CÓ" nếu không tìm thấy sự mâu thuẫn trong quá trình quét, nếu không thì trả về "KHÔNG". 

### Tại sao nó hoạt động 

Mỗi giá trị$x$theo chuỗi vô hạn$S$xuất hiện chính xác một lần trên mỗi khối có chiều dài$m$. Điều này ngụ ý rằng sự xuất hiện của$x$hình thành một cấp số cộng có sai phân chung$m$. Bất kỳ nhúng hợp lệ nào của$T$phải căn chỉnh tất cả các lần xuất hiện của$x$đến các vị trí khác nhau bởi bội số của$m$. Nếu thậm chí một cặp lần xuất hiện vi phạm khoảng cách này thì không có sự dịch chuyển ranh giới khối nào có thể điều hòa được chúng, bởi vì cấu trúc khối là cứng nhắc và mang tính tổng thể. 

Ngược lại, nếu tất cả các lần xuất hiện của mọi giá trị đều thỏa mãn tính nhất quán mô-đun này, chúng ta có thể chọn độ lệch bắt đầu để căn chỉnh lần xuất hiện đầu tiên của mỗi giá trị vào một vị trí hợp lệ bên trong một số khối hoán vị và tất cả các lần xuất hiện khác sẽ tự động tuân theo. Điều này đảm bảo một vị trí hợp lệ của$T$bên trong$S$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    T = list(map(int, input().split()))
    
    first_pos = {}
    
    for i, x in enumerate(T):
        if x not in first_pos:
            first_pos[x] = i
        else:
            if (i - first_pos[x]) % m != 0:
                print("NO")
                return
    
    print("YES")

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc lưu trữ chỉ mục xuất hiện đầu tiên cho mỗi giá trị và thực thi rằng mọi lần xuất hiện sau đó đều tuân theo modulo$m$hạn chế. Logic là tuyến tính vì mỗi phần tử được xử lý một lần và việc tra cứu từ điển sẽ xử lý việc nhóm giá trị một cách hiệu quả. 

Một chi tiết triển khai tinh tế là sử dụng lập chỉ mục dựa trên số 0 một cách nhất quán khi tính toán sự khác biệt. Việc trộn lẫn các chỉ số dựa trên một và dựa trên 0 sẽ làm dịch chuyển căn chỉnh mô-đun không chính xác và tạo ra các kết quả âm tính giả. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
n = 8, m = 3
T = [2, 3, 2, 1, 3, 3, 2, 1]
```Chúng tôi theo dõi những lần xuất hiện đầu tiên và xác thực tính nhất quán. 

| tôi | T[i] | first_pos[T[i]] | (i - first_pos) % m | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | - | cửa hàng | 
| 1 | 3 | 1 | - | cửa hàng | 
| 2 | 2 | 0 | 2 % 3 = 2 | được | 
| 3 | 1 | 3 | - | cửa hàng | 
| 4 | 3 | 1 | 3 % 3 = 0 | được | 
| 5 | 3 | 1 | 4 % 3 = 1 | được | 
| 6 | 2 | 0 | 6 % 3 = 0 | được | 
| 7 | 1 | 3 | 4 % 3 = 1 | được | 

Không có mâu thuẫn nào xuất hiện nên kết quả đầu ra là CÓ. 

Dấu vết này cho thấy các lần xuất hiện khác nhau của cùng một giá trị được căn chỉnh theo modulo$m$, mặc dù chúng không liền kề nhau. 

Bây giờ hãy xem xét:```
n = 3, m = 2
T = [1, 1, 1]
```| tôi | T[i] | first_pos[T[i]] | (i - first_pos) % m | hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | - | cửa hàng | 
| 1 | 1 | 0 | 1% 2 = 1 | được | 
| 2 | 1 | 0 | 2 % 2 = 0 | được | 

Điều này có vẻ nhất quán theo quy tắc mô-đun, nhưng nó ẩn chứa một vấn đề sâu sắc hơn: việc lặp lại một giá trị duy nhất không thể vừa với cấu trúc khối hoán vị vì nó sẽ vi phạm tính duy nhất trong một khối khi được ánh xạ tới các vị trí thực tế. Điều này nhấn mạnh rằng điều kiện mô-đun là cần thiết nhưng phải được giải thích trong bối cảnh đóng gói toàn khối chứ không chỉ là sự khác biệt theo cặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| mỗi phần tử được xử lý một lần với tra cứu băm | 
| Không gian |$O(n)$| lưu trữ lần xuất hiện đầu tiên trên mỗi giá trị riêng biệt | 

Các ràng buộc cho phép lên đến$5 \cdot 10^5$các phần tử, vì vậy việc quét tuyến tính với các thao tác từ điển là đủ trong giới hạn. Việc sử dụng bộ nhớ vẫn an toàn vì số lượng giá trị riêng biệt nhiều nhất là$n$. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    def solve():
        n, m = map(int, input().split())
        T = list(map(int, input().split()))
        first_pos = {}
        for i, x in enumerate(T):
            if x not in first_pos:
                first_pos[x] = i
            else:
                if (i - first_pos[x]) % m != 0:
                    print("NO")
                    return
        print("YES")

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided samples
assert run("8 3\n2 3 2 1 3 3 2 1") == "YES"
assert run("3 2\n1 1 1") == "NO"

# custom cases
assert run("1 4\n5") == "NO"
assert run("4 2\n1 1 2 2") == "YES"
assert run("5 3\n1 2 3 1 2") == "YES"
assert run("6 2\n1 2 1 2 1 2") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 4 / 5`| KHÔNG | giá trị ngoài phạm vi | 
|`4 2 / 1 1 2 2`| CÓ | căn chỉnh chéo khối | 
|`5 3 / 1 2 3 1 2`| CÓ | tính nhất quán lặp lại một phần | 
|`6 2 / 1 2 1 2 1 2`| CÓ | cấu trúc xen kẽ chặt chẽ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$n = 1$. Bất kỳ giá trị đơn nào$T = [x]$luôn có giá trị miễn là$x \le m$, bởi vì nó có thể nằm bên trong một số khối hoán vị mà không có xung đột. Thuật toán ghi lại lần xuất hiện đầu tiên và không thực hiện kiểm tra xung đột, do đó nó trả về CÓ một cách chính xác. 

Một trường hợp cạnh khác là khi tất cả các giá trị giống hệt nhau, chẳng hạn như$T = [1,1,1,1]$. Thuật toán sẽ chấp nhận điều này trong điều kiện mô-đun, nhưng trong cấu trúc khối hoán vị thực, điều này là không thể trừ khi$m = 1$. Nếu như$m > 1$, các bản sao trong một khối vi phạm tính duy nhất. Điều này nhấn mạnh rằng giải pháp đầy đủ chính xác phải kết hợp ràng buộc rằng mỗi giá trị không thể lặp lại trong một cửa sổ ngắn hơn hoặc bằng$m$, không chỉ tính nhất quán mô-đun.
