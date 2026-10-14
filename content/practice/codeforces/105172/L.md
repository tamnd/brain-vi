---
title: "CF 105172L - Nanami, Nanami, Nanami..."
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có một danh sách các số nguyên. Chúng tôi được phép xóa chính xác một phần tử khỏi danh sách này."
date: "2026-06-27T08:27:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105172
codeforces_index: "L"
codeforces_contest_name: "The 20th Southeast University Programming Contest (Summer)"
rating: 0
weight: 105172
solve_time_s: 75
verified: true
draft: false
---

[CF 105172L - Nanami, Nanami, Nanami...](https://codeforces.com/problemset/problem/105172/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có một danh sách các số nguyên. Chúng tôi được phép xóa chính xác một phần tử khỏi danh sách này. Sau khi làm như vậy, chúng ta nhìn vào các số còn lại và cố gắng chọn một giá trị$d$vẫn xuất hiện trong danh sách và hoàn toàn nhỏ hơn giá trị đã xóa$c$. Mục tiêu của chúng tôi là chọn loại bỏ và giá trị kết quả$d$để có thể$d$là càng lớn càng tốt. 

Nói một cách đơn giản hơn, chúng tôi chọn một phần tử làm “ngưỡng”$c$, xóa một lần xuất hiện của nó, rồi tìm số lớn nhất trong mảng còn lại mà vẫn nhỏ hơn$c$. Chúng tôi muốn tối đa hóa điều đó tốt nhất có thể$d$trên mọi lựa chọn của$c$. 

Các ràng buộc rất nhỏ: mỗi mảng có tối đa 100 phần tử và giá trị tối đa là 100. Điều này ngay lập tức gợi ý rằng ngay cả các giải pháp bậc ba hoặc bậc hai cũng có thể được chấp nhận, nhưng chúng ta vẫn nên hướng tới một cấu trúc làm cho logic trở nên minh bạch thay vì mô phỏng mọi thao tác loại bỏ một cách rõ ràng. 

Một điểm tinh tế là việc loại bỏ một phần tử chỉ ảnh hưởng đến tính khả dụng của giá trị chính xác đó. Nếu chúng ta chọn$d \neq c$, sau đó loại bỏ$c$không bao giờ loại bỏ tất cả các lần xuất hiện của$d$. Điều này có nghĩa là hầu hết ý tưởng “loại bỏ và tính toán lại” đều không cần thiết. 

Các trường hợp cạnh đáng suy nghĩ đến từ các mảng trong đó giá trị tối đa xuất hiện nhiều lần hoặc có nhiều bản sao: 

Nếu tất cả các giá trị đều bằng nhau, chẳng hạn như`3 3`, không có cặp hợp lệ$d < c$, bởi vì dù bạn loại bỏ cái gì thì cũng không có giá trị nào nhỏ hơn. Bài toán đảm bảo tình huống này sẽ không xảy ra theo cách suy biến, vì nó đảm bảo tồn tại ít nhất hai giá trị phân biệt. 

Nếu mảng là`1 2 2`, loại bỏ một`2`vẫn còn lá`1 2`, và tốt nhất$d$vẫn còn`1`. Một mô phỏng ngây thơ có thể nghĩ sai khi loại bỏ những thay đổi tối đa duy nhất cho câu trả lời, nhưng thực tế không phải vậy. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi chỉ số$i$, đối xử$a[i]$là giá trị bị loại bỏ$c$. Sau khi xóa nó, hãy quét mảng còn lại và kiểm tra mọi giá trị$a[j]$cái đó nhỏ hơn$c$, lấy mức tối đa trong số đó. Điều này đòi hỏi$O(n)$công việc cho mỗi lần loại bỏ, và vì có$O(n)$sự lựa chọn, tổng độ phức tạp là$O(n^2)$mỗi trường hợp thử nghiệm. Với$n \le 100$, điều này đã an toàn rồi. 

Tuy nhiên, cách tiếp cận này tính toán lại cùng một cấu trúc nhiều lần. Quan sát quan trọng là thao tác loại bỏ hầu như không liên quan: việc loại bỏ một lần xuất hiện của một giá trị không làm thay đổi việc giá trị khác có tồn tại trong mảng hay không. Vì vậy, câu hỏi duy nhất quan trọng là liệu có tồn tại giá trị nào lớn hơn$d$. Nếu một giá trị như vậy tồn tại, chúng ta luôn có thể chọn nó làm$c$và việc loại bỏ một lần xuất hiện của nó sẽ không ảnh hưởng$d$. 

Vậy bài toán quy về việc tìm giá trị lớn nhất$d$sao cho tồn tại ít nhất một giá trị lớn hơn trong mảng. Đó chính xác là giá trị khác biệt lớn thứ hai trong tập hợp số. 

Do đó, chúng ta có thể bỏ qua hoàn toàn bội số và chỉ làm việc với các giá trị duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Đã chấp nhận | 
| Tối ưu (quét riêng biệt) |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và trích xuất tất cả các giá trị riêng biệt. 

Thao tác này sẽ loại bỏ các bản sao vì các bản sao không thay đổi thứ tự giữa các giá trị mà chỉ thay đổi tần số. 
2. Sắp xếp các giá trị riêng biệt theo thứ tự giảm dần. 

Việc sắp xếp mang lại cho chúng ta thứ hạng rõ ràng từ lớn nhất đến nhỏ nhất, mã hóa trực tiếp điều kiện$d < c$. 
3. Lấy phần tử thứ hai trong danh sách riêng biệt được sắp xếp này. 

Giá trị này là số lớn nhất vẫn có ít nhất một giá trị lớn hơn nó. 
4. Xuất giá trị này cho test case. 

Lý do đằng sau bước 3 là cốt lõi của giải pháp. Bất kỳ ứng cử viên nào$d$phải có một số$c > d$. Điều đó có nghĩa ngay lập tức$d$không thể là giá trị phân biệt lớn nhất. Trong số tất cả các ứng cử viên còn lại, lựa chọn tốt nhất có thể là lựa chọn lớn nhất vẫn dưới mức tối đa, là giá trị khác biệt lớn thứ hai. 

### Tại sao nó hoạt động 

hãy để$M$là giá trị lớn nhất trong mảng. Bất kỳ hợp lệ$d$phải thỏa mãn$d < M$, bởi vì nếu$d = M$, không có giá trị nào lớn hơn hoàn toàn để dùng làm$c$. Đối với bất kỳ giá trị$d < M$, chúng ta luôn có thể chọn$c = M$(hoặc bất kỳ sự xuất hiện nào của$M$) và loại bỏ nó. Vì việc loại bỏ một phần tử không loại bỏ được tất cả các lần xuất hiện của$d$,$d$vẫn còn trong mảng. Do đó, mọi giá trị ngoại trừ giá trị tối đa đều khả thi và chúng tôi tối đa hóa bằng cách chọn giá trị lớn nhất trong số chúng, là giá trị khác biệt lớn thứ hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        vals = sorted(set(a), reverse=True)
        print(vals[1])

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng trường hợp kiểm thử một cách độc lập, chuyển đổi mảng thành một tập hợp để loại bỏ các bản sao, sắp xếp theo thứ tự giảm dần và xuất ra phần tử thứ hai. Yêu cầu tinh tế duy nhất là đảm bảo rằng mảng có ít nhất hai giá trị riêng biệt, được đảm bảo bởi câu lệnh vấn đề. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp để xem sự chuyển đổi thành “giá trị phân biệt tối đa thứ hai” xuất hiện như thế nào. 

### Ví dụ 1 

đầu vào:`1 4 2`Các giá trị riêng biệt:`[4, 2, 1]`| Bước | Bộ riêng biệt | Đã sắp xếp Desc | Giá trị được chọn | 
| --- | --- | --- | --- | 
| Bắt đầu | {1, 2, 4} | - | - | 
| Sắp xếp | - | [4, 2, 1] | - | 
| Đầu ra | - | - | 2 | 

Đây,`4`là giá trị duy nhất không thể đóng vai trò$d$, vì vậy giá trị hợp lệ tốt nhất có thể là`2`. 

### Ví dụ 2 

đầu vào:`5 2 1 1 3 1 4`Các giá trị riêng biệt:`[5, 4, 3, 2, 1]`| Bước | Bộ riêng biệt | Đã sắp xếp Desc | Giá trị được chọn | 
| --- | --- | --- | --- | 
| Bắt đầu | {1, 2, 3, 4, 5} | - | - | 
| Sắp xếp | - | [5, 4, 3, 2, 1] | - | 
| Đầu ra | - | - | 4 | 

giá trị`5`không thể sử dụng được như$d$, Và`4`trở thành giá trị lớn nhất mà vẫn có phần tử lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$mỗi trường hợp thử nghiệm | Sắp xếp các yếu tố riêng biệt chiếm ưu thế trong công việc | 
| Không gian |$O(n)$| Lưu trữ cho bộ riêng biệt | 

Được cho$n \le 100$Và$t \le 100$, điều này diễn ra thoải mái trong giới hạn ngay cả khi sắp xếp lặp đi lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        vals = sorted(set(a), reverse=True)
        out.append(str(vals[1]))
    return "\n".join(out)

# provided samples
assert run("""5
2
1 2
3
1 2 3
3
1 4 2
5
1 2 9 4 3
7
5 2 1 1 3 1 4
""") == """1
2
2
4
4"""

# minimum size with two distinct values
assert run("""1
2
1 100
""") == "1"

# duplicates everywhere except one max
assert run("""1
5
7 7 7 7 1
""") == "1"

# already sorted descending
assert run("""1
4
10 9 8 7
""") == "9"

# mixed duplicates
assert run("""1
6
2 2 3 3 3 1
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 100 / 1 100`|`1`| trường hợp hai phần tử đơn giản nhất | 
| tất cả 7s + 1 |`1`| trùng lặp không ảnh hưởng đến kết quả | 
|`10 9 8 7`|`9`| đã đặt hàng đầu vào | 
| trùng lặp hỗn hợp |`2`| xử lý các giá trị lặp lại | 

## Vỏ cạnh 

Khi giá trị tối đa xuất hiện nhiều lần, việc loại bỏ một lần xuất hiện vẫn để lại giá trị đó trong mảng. Điều này có nghĩa là việc chọn mức tối đa là luôn hợp lệ$c$, đảm bảo rằng mọi giá trị riêng biệt nhỏ hơn vẫn có thể truy cập được với tư cách là ứng viên$d$. Thuật toán xử lý việc này một cách tự nhiên vì các nội dung trùng lặp sẽ bị loại bỏ trước khi sắp xếp, do đó tần suất không bao giờ ảnh hưởng đến thứ tự. 

Khi các giá trị được đóng gói chặt chẽ, chẳng hạn như các số nguyên liên tiếp, giá trị phân biệt lớn thứ hai vẫn được xác định rõ. Ví dụ, trong`[1, 2]`, câu trả lời là`1`và việc xóa một trong hai phần tử không làm thay đổi thực tế là chỉ một giá trị có thể đóng vai trò là giá trị hợp lệ$d$. 

Khi có nhiều bản sao của các giá trị trung gian, chẳng hạn như`[1, 2, 2, 3]`, sự có mặt của thêm`2`s không thay đổi thứ tự của các giá trị riêng biệt và thuật toán vẫn trả về chính xác`2`.
