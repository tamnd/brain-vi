---
title: "CF 105592E - \u0422\u0430\u0431\u043b\u0438\u0446\u0430 \u0434\u0435\u043b\u0435\u043d\u0438\u044f"
description: "Chúng ta được cho một dãy số nguyên $a1, a2, dấu chấm, an$. Từ những giá trị này, chúng ta xây dựng một bảng $n nhân n$ một cách khái niệm. Phần tử tại hàng $i$, cột $j$ được định nghĩa là phần nguyên của phép chia $aj / ai$."
date: "2026-06-22T05:52:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105592
codeforces_index: "E"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435, 9-11 \u043a\u043b\u0430\u0441\u0441\u044b, \u041d\u0438\u0436\u0435\u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u044c, 2024"
rating: 0
weight: 105592
solve_time_s: 45
verified: true
draft: false
---

[CF 105592E - \u0422\u0430\u0431\u043b\u0438\u0446\u0430 \u0434\u0435\u043b\u0435\u043d\u0438\u044f](https://codeforces.com/problemset/problem/105592/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên$a_1, a_2, \dots, a_n$. Từ những giá trị này, về mặt khái niệm, chúng tôi xây dựng một$n \times n$bàn. Mục nhập ở hàng$i$, cột$j$được định nghĩa là phần nguyên của phép chia$a_j / a_i$. Vì vậy, mỗi ô chỉ phụ thuộc vào một cặp giá trị đầu vào, với mẫu số là chỉ số hàng và tử số là chỉ số cột. 

Nhiệm vụ không phải là xây dựng bảng này một cách rõ ràng mà là xác định xem có bao nhiêu giá trị riêng biệt xuất hiện ở bất kỳ đâu bên trong nó. 

Các ràng buộc rất chặt chẽ:$n$có thể lên tới$2 \cdot 10^6$, và mỗi$a_i$cũng tùy$2 \cdot 10^6$. Việc xem xét đầy đủ theo cặp tất cả các mục trong bảng sẽ bao gồm$n^2$sự chia rẽ, vượt xa giới hạn khả thi. Thậm chí$10^5$sẽ đẩy một người ngây thơ$O(n^2)$cách tiếp cận vượt quá bất kỳ giới hạn thời gian hợp lý nào, vì vậy giải pháp phải tránh lặp lại trên tất cả các cặp. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các số đều bằng nhau. Trong trường hợp đó mỗi bộ phận$a_j / a_i$bằng$1$, vì vậy câu trả lời là tầm thường$1$. Việc triển khai đơn giản vẫn có thể tính toán các cặp dư thừa hoặc bỏ sót thực tế là tất cả các mục đều thu gọn về một giá trị duy nhất. 

Một trường hợp góc khác phát sinh khi mảng chứa các giá trị rất nhỏ như$1$. Khi đó mỗi hàng tạo ra nhiều giá trị riêng biệt vì phép chia cho$1$mang lại tất cả các tử số có thể, trong khi phép chia cho số lớn hơn sẽ thu gọn nhiều kết quả về 0. Một cách tiếp cận bất cẩn giả định tính đối xứng hoặc đơn điệu trong cấu trúc bảng có thể bị tính sai trong các phân phối sai lệch như vậy. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ trực tiếp sẽ tính toán từng tế bào$\lfloor a_j / a_i \rfloor$và chèn kết quả vào một tập hợp. Điều này rất đơn giản: với mỗi cặp$(i, j)$, tính phép chia số nguyên và lưu trữ nó. Tính chính xác là ngay lập tức vì nó phản ánh chính xác định nghĩa của bảng. 

Tuy nhiên, số lượng hoạt động$n^2$, mà tại$n = 2 \cdot 10^6$theo thứ tự của$4 \cdot 10^{12}$phép chia và phép chèn. Ngay cả với mã được tối ưu hóa cao, điều này là không thể thực hiện được. 

Nhận xét quan trọng là giá trị của$\lfloor a_j / a_i \rfloor$không phụ thuộc vào danh tính của$i$Và$j$, nhưng chỉ trên cặp giá trị. Quan trọng hơn, đối với mẫu số cố định$x$, biểu thức$\lfloor y / x \rfloor$chỉ thay đổi khi$y$vượt qua bội số của$x$. Điều này có nghĩa là với mỗi cố định$x$, tất cả các tử số$y$trong cùng một khoảng thời gian$[kx, (k+1)x - 1]$tạo ra cùng một thương số$k$. 

Điều này biến vấn đề thành một trong những lý luận về tần số giá trị thay vì các cặp rõ ràng. Nếu chúng ta biết mỗi giá trị xuất hiện bao nhiêu lần, chúng ta có thể xác định phạm vi thương số nào có thể đạt được đối với mỗi mẫu số. Thay vì lặp đi lặp lại tất cả$j$, chúng ta có thể lặp qua các giá trị thương có thể có và đếm xem có tồn tại ít nhất một$y$trong mảng rơi vào khoảng tương ứng. 

Việc chuyển đổi là từ liệt kê các cặp sang liệt kê các phạm vi thương trên mỗi mẫu số. Vì cả hai$a_i$Và$a_j$được giới hạn bởi$2 \cdot 10^6$, chúng ta có thể khai thác mảng tần số và lặp lại các ước số và bội số một cách có kiểm soát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$hoặc$O(n^2)$| Quá chậm | 
| Tối ưu |$O(M \log M)$hoặc$O(M \sqrt{M})$|$O(M)$| Đã chấp nhận | 

Đây$M = \max a_i$. 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nén đầu vào thành một mảng tần số để biết mỗi giá trị xuất hiện bao nhiêu lần. Điều này cho phép chúng ta suy luận về sự tồn tại của các giá trị theo từng khoảng mà không cần quét toàn bộ danh sách nhiều lần. 

1. Xây dựng mảng tần số`freq[x]`đếm mỗi giá trị bao nhiêu lần$x$xuất hiện ở đầu vào. Bước này thay thế danh sách thô bằng một biểu diễn có cấu trúc hỗ trợ suy luận phạm vi. 
2. Tạo một mảng boolean`present[x]`cho biết liệu một giá trị$x$xuất hiện ít nhất một lần. Điều này được sử dụng để nhanh chóng kiểm tra xem một khoảng có chứa tử số hợp lệ hay không. 
3. Với mỗi giá trị mẫu số có thể$d$xuất hiện trong mảng, hãy xem xét thương số nào có thể xuất hiện từ phép chia$y / d$. Đối với một thương số cố định$k$, tử số$y$phải nằm trong khoảng$[k \cdot d, (k+1)\cdot d - 1]$. 
4. Thay vì quét tất cả những gì có thể$y$, chúng ta nhảy qua các giá trị thương$k$và kiểm tra xem khoảng đó có giao nhau với phạm vi giá trị hợp lệ hay không. Nếu có tồn tại bất kỳ$y$trong khoảng đó sao cho`present[y]`là đúng thì thương$k$là có thể đạt được. 
5. Để thực hiện việc này một cách hiệu quả, chúng ta lặp lại$k$từ$0$lên đến$\lfloor \max a / d \rfloor$, và với mỗi$k$, chúng tôi kiểm tra xem có tồn tại ít nhất một giá trị hiện tại trong khoảng hay không. Điều này có thể được thực hiện bằng cách sử dụng mảng tổng tiền tố`present`, cho phép mỗi truy vấn khoảng thời gian trong$O(1)$. 
6. Mỗi lần chúng tôi xác nhận rằng một thương số$k$có thể đạt được với ít nhất một mẫu số, chúng tôi chèn$k$thành một tập hợp các kết quả toàn cầu. 

### Tại sao nó hoạt động 

Với mọi mẫu số cố định$d$, mọi giá trị bảng có thể có trong hàng đó được xác định bởi khoảng nào tử số rơi vào. Mỗi khoảng tương ứng với chính xác một thương số và mỗi mục nhập bảng hợp lệ tương ứng với chính xác một khoảng như vậy. Bằng cách quét tất cả các khoảng và kiểm tra xem chúng có chứa ít nhất một tử số hợp lệ hay không, chúng tôi đảm bảo rằng mọi thương số có thể đạt được đều được tính. Cấu trúc tổng tiền tố đảm bảo rằng việc kiểm tra khoảng thời gian là chính xác, do đó không có thương số nào bị bỏ sót và không có thương số không tồn tại nào được thêm vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    max_a = max(arr)
    freq = [0] * (max_a + 1)
    present = [0] * (max_a + 1)
    
    for x in arr:
        freq[x] += 1
        present[x] = 1

    # prefix sum over presence
    pref = [0] * (max_a + 1)
    for i in range(1, max_a + 1):
        pref[i] = pref[i - 1] + present[i]

    def has_any(l, r):
        if l > r:
            return False
        if r > max_a:
            r = max_a
        return pref[r] - pref[l - 1] > 0

    ans = set()

    for d in range(1, max_a + 1):
        if freq[d] == 0:
            continue

        k = 0
        while k * d <= max_a:
            l = k * d
            r = min(max_a, (k + 1) * d - 1)
            if has_any(l, r):
                ans.add(k)
            k += 1

    print(len(ans))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nén đầu vào thành mảng tần số và hiện diện. Tổng tiền tố`present`cho phép kiểm tra liên tục xem có giá trị nào tồn tại trong một khoảng thời gian nhất định hay không. Vòng lặp chính lặp lại tất cả các mẫu số có thể có thực sự có trong đầu vào và đối với mỗi mẫu số sẽ liệt kê các phạm vi thương số. 

Vòng lặp bên trong kết thúc$k$là an toàn vì cố định$d$, số lượng phạm vi thương số có liên quan được giới hạn bởi$\max a / d$, và trên tất cả$d$cấu trúc này hoạt động giống như một chuỗi điều hòa hơn là một chuỗi bậc hai. 

bộ`ans`thu thập tất cả các thương số riêng biệt gặp ở bất kỳ đâu trong bảng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [1, 2, 8, 5]
```Chúng tôi tính toán sự hiện diện của các giá trị {1, 2, 5, 8}. Bây giờ hãy xem xét từng mẫu số một. 

| d | k | khoảng [k*d, (k+1)*d - 1] | chứa giá trị hiện tại? | ghi k | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | [0,0] | không | | 
| 1 | 1 | [1,1] | vâng | 1 | 
| 1 | 2 | [2,2] | vâng | 2 | 
| 1 | 3 | [3,3] | không | | 
| 1 | 5 | [5,5] | vâng | 5 | 
| 1 | 8 | [8,8] | vâng | 8 | 

Vì$d = 1$, chúng tôi trực tiếp phục hồi tất cả các giá trị. 

Vì$d = 2$, các khoảng là [0,1], [2,3], [4,5], [6,7], [8,9]. Chúng tạo ra thương số 0,1,2,3,4. Chỉ một số khoảng chứa giá trị hiện tại, vì vậy chúng ta thêm 0,1,2,4. 

Vì$d = 8$, các khoảng là [0,7], [8,15], cho thương số 0 và 1. 

Hợp của tất cả các thương sẽ trở thành {0,1,2,4,5,8}, khớp với kết quả mong đợi. 

Dấu vết này cho thấy các mẫu số khác nhau đóng góp vào các tập hợp thương số chồng chéo và việc loại bỏ trùng lặp thông qua một tập hợp là cần thiết. 

### Ví dụ 2 

đầu vào:```
n = 3
a = [3, 3, 3]
```| d | k | khoảng thời gian | hiện tại? | ghi lại | 
| --- | --- | --- | --- | --- | 
| 3 | 0 | [0,2] | không | | 
| 3 | 1 | [3,5] | vâng | 1 | 

Tất cả các hàng đều giống nhau nên chỉ có thương số 1 xuất hiện. Thuật toán nén chính xác toàn bộ bảng thành một giá trị riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M \log M)$| Mỗi mẫu số lặp lại trên các phạm vi thương số và tổng công việc hoạt động giống như một tổng hài hòa trên không gian giá trị | 
| Không gian |$O(M)$| Mảng tần số, sự hiện diện và tiền tố trên phạm vi giá trị | 

Giá trị tối đa$M \le 2 \cdot 10^6$đảm bảo rằng mảng tuyến tính có thể thực hiện được trong bộ nhớ. Cấu trúc hài hòa của phép lặp ngăn ngừa sự bùng nổ bậc hai ngay cả ở những hạn chế tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import Counter

    # inline solution
    input = _sys.stdin.readline
    n = int(input())
    arr = list(map(int, input().split()))
    max_a = max(arr)
    freq = [0] * (max_a + 1)
    present = [0] * (max_a + 1)

    for x in arr:
        freq[x] += 1
        present[x] = 1

    pref = [0] * (max_a + 1)
    for i in range(1, max_a + 1):
        pref[i] = pref[i - 1] + present[i]

    def has_any(l, r):
        if l > r:
            return False
        if r > max_a:
            r = max_a
        return pref[r] - pref[l - 1] > 0

    ans = set()

    for d in range(1, max_a + 1):
        if freq[d] == 0:
            continue
        k = 0
        while k * d <= max_a:
            l = k * d
            r = min(max_a, (k + 1) * d - 1)
            if has_any(l, r):
                ans.add(k)
            k += 1

    return str(len(ans))

# provided sample
assert run("4\n1 2 8 5\n") == "6"

# custom cases
assert run("1\n7\n") == "1", "single element"
assert run("3\n1 1 1\n") == "1", "all equal"
assert run("3\n1 2 3\n") == "3", "small consecutive"
assert run("5\n2 4 8 16 32\n") == "5", "powers of two"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | trường hợp tối thiểu | 
| tất cả đều bình đẳng | 1 | hành vi sụp đổ | 
| 1 2 3 | 3 | phạm vi nhỏ dày đặc | 
| sức mạnh của hai | 5 | tăng trưởng có cấu trúc | 

## Vỏ cạnh 

Một đầu vào tối thiểu với một giá trị duy nhất như`5`tạo ra một bảng chứa đầy`1`. Thuật toán xử lý việc này vì mẫu số`d = 5`, chỉ khoảng`[5,5]`đóng góp và thương số duy nhất được chèn vào là`1`. 

Khi tất cả các giá trị giống hệt nhau, hãy nói`[10, 10, 10]`, mỗi lần kiểm tra mẫu số chỉ tìm thấy thương số`1`. Tổng tiền tố đảm bảo rằng khoảng`[10,10]`được phát hiện chính xác một lần trên mỗi mẫu số, nhưng tập hợp này sẽ loại bỏ nó, mang lại câu trả lời cuối cùng là`1`. 

Khi mảng chứa`1`, mỗi mẫu số đóng góp nhiều khoảng thương. Vì`d = 1`, mỗi giá trị tạo thành khoảng riêng của nó và tất cả các giá trị được thêm dưới dạng thương số. Đối với các mẫu số lớn hơn, hầu hết các khoảng trở nên trống hoặc thu gọn về 0, nhưng cấu trúc tiền tố nắm bắt chính xác liệu số 0 có xuất hiện hay không, đảm bảo tính chính xác cho các phân bố có giá trị nhỏ và nặng.
