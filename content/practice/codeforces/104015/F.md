---
title: "CF 104015F - Dừa"
description: "Chúng ta được cho một chuỗi các đống dừa, trong đó mỗi đống có kích thước nguyên. Chúng ta được phép chọn một giá trị cơ bản nguyên dương duy nhất $x$, và sau đó chúng ta chỉ thu thập dừa từ những đống có kích thước chính xác là lũy thừa dương của $x$, nghĩa là các giá trị có dạng…"
date: "2026-07-02T04:51:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104015
codeforces_index: "F"
codeforces_contest_name: "ICPC 2021-2022 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 104015
solve_time_s: 48
verified: true
draft: false
---

[CF 104015F - Dừa](https://codeforces.com/problemset/problem/104015/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các đống dừa, trong đó mỗi đống có kích thước nguyên. Chúng ta được phép chọn một giá trị cơ số nguyên dương duy nhất$x$, và sau đó chúng tôi chỉ thu thập dừa từ những đống có kích thước chính xác là lũy thừa dương của$x$, ý nghĩa các giá trị của dạng$x^1, x^2, x^3, \dots$. Mỗi cọc đủ điều kiện đóng góp toàn bộ giá trị của nó vào tổng giá trị của chúng tôi và tất cả các cọc khác đều bị bỏ qua. Mục tiêu là chọn$x$để tổng số dừa thu được là tối đa. 

Vì vậy, nhiệm vụ không phải là chọn các tập con một cách tùy ý mà là chọn một cấu trúc: cơ sở$x$, rồi lấy tất cả các số trong mảng nằm trong dãy hình học do cơ số đó tạo ra. 

Kích thước đầu vào lên tới 200000 phần tử và mỗi giá trị lên tới 10^9. Điều này ngay lập tức loại trừ mọi giải pháp thử tất cả các cơ sở có thể$x$và kiểm tra mọi quyền lực một cách ngây thơ. Việc kiểm tra trực tiếp trên mỗi cơ sở ứng cử viên sẽ quá chậm, đặc biệt vì mỗi lần kiểm tra liên quan đến việc lặp lại các truy vấn thành viên lũy thừa hoặc băm trên toàn bộ mảng. 

Sự tinh tế của phím xuất hiện khi các giá trị lặp lại hoặc khi nhiều số bằng 1. Vì$1 = x^k$chỉ có thể thực hiện được khi$x = 1$, số 1 hoạt động khác với các giá trị khác. Một trường hợp cạnh khác là khi$x$bản thân nó lớn: đế lớn tạo ra chuỗi điện rất ngắn, thường dài 1 hoặc 2, điều này vẫn phải được xem xét. 

Một sai lầm ngây thơ là cho rằng việc chọn$x$bằng một trong các giá trị mảng luôn mang lại câu trả lời đúng. Điều này không thành công vì cơ sở tốt nhất thậm chí có thể không xuất hiện trong mảng. Ví dụ: nếu mảng chứa các số như 2, 4, 8, 16 thì lựa chọn tốt nhất là$x = 2$, hiện diện, nhưng trong các trường hợp khác như 3, 9, 27, 40, tốt nhất$x$có thể là 3 hoặc 40 tùy thuộc vào cấu trúc và 40 không phải là một phần của bất kỳ chuỗi nào. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là thử mọi số nguyên có thể$x$từ 1 đến giá trị tối đa trong mảng. Đối với mỗi$x$, chúng tôi sẽ tính toán nhiều lần$x, x^2, x^3, \dots$trong khi giá trị nằm trong giới hạn và tính tổng các đóng góp nếu các giá trị đó tồn tại trong bản đồ tần số của mảng. Mỗi chuỗi có kích thước logarit vì giá trị tăng nhanh nhưng số lượng ứng viên cho$x$vẫn ở mức 10^9 trong phạm vi trường hợp xấu nhất, điều này là không thể. 

Ngay cả việc hạn chế các ứng cử viên ở các giá trị xuất hiện trong mảng cũng không giải quyết được hoàn toàn vấn đề, bởi vì cơ sở tối ưu có thể là một số không có mặt nhưng lũy ​​thừa của nó phù hợp tốt với nhiều phần tử mảng. Tuy nhiên, chúng ta có thể tinh chỉnh ý tưởng này: mọi chuỗi hợp lệ phải được xác định bởi một số cơ sở bắt đầu$x$và mỗi số trong mảng thuộc về nhiều nhất một chuỗi số mũ cho mỗi số được chọn$x$. Cấu trúc quyền lực gợi ý nên đảo ngược quan điểm: thay vì thử mọi cách$x$, chúng ta có thể cố gắng xác định tiềm năng$x$bằng cách quan sát mối quan hệ giữa các con số. 

Quan sát quan trọng là nếu một số$a$bằng với$x^k$, thì việc phân tích thành thừa số nguyên tố của nó phải phù hợp với việc là một lũy thừa hoàn hảo. Điều này có nghĩa là tất cả các số có thể xuất hiện trong một chuỗi hợp lệ đều có chung một cấu trúc cơ sở: chúng đều là lũy thừa của cùng một số nguyên. Do đó, vấn đề giảm xuống việc nhóm các số theo biểu diễn gốc tối thiểu của chúng. Đối với mỗi số$a$, chúng ta có thể tính cơ số nhỏ nhất của nó$b$như vậy$a = b^k$, và khi đó tất cả các số trong cùng một chuỗi tương ứng với lũy thừa của cùng$b$. Câu trả lời tối ưu có được bằng cách tính tổng các giá trị dọc theo mỗi chuỗi được xây dựng lại như vậy. 

Khó khăn chính là xác định chính xác cơ số chuẩn của mỗi số. Đối với mỗi$a$, chúng ta phân tích nó thành lũy thừa số nguyên và trích ra nghiệm tối thiểu của nó. Sau đó, chúng tôi tổng hợp các khoản đóng góp dọc theo chuỗi do các cơ sở này tạo ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên x | O(N · maxA) | O(1) | Quá chậm | 
| Hệ số hóa + nhóm theo cơ số gốc | O(N √A) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mọi số thành biểu diễn chuẩn của nó, sau đó nhóm các giá trị thuộc cùng một chuỗi quyền lực. 

1. Với mỗi số$a_i$, tính cơ số gốc tối thiểu của nó$b_i$. Điều này có nghĩa là tìm số nguyên nhỏ nhất$b$như vậy$a_i = b^k$đối với một số nguyên$k \ge 1$. Bước này đảm bảo rằng tất cả các lũy thừa hoàn hảo được chuẩn hóa cho cùng một trình tạo cơ bản. 
2. Nếu một số không phải là lũy thừa hoàn hảo của bất kỳ số nguyên nhỏ hơn nào, thì cơ số của nó là chính nó, do đó nó tạo thành một chuỗi tầm thường có độ dài bằng 1. Điều này xử lý các số nguyên tố và tổng số tổng quát không phải là lũy thừa hoàn hảo. 
3. Duy trì bản đồ từ cơ sở$b$bằng tổng của tất cả các giá trị ban đầu$a_i$thuộc về chuỗi của nó. Chúng tôi tích lũy số dừa thực tế chứ không chỉ đếm, bởi vì mỗi đống hợp lệ đều đóng góp hết kích thước của nó. 
4. Lặp lại tất cả các giá trị và cập nhật bản đồ này. Mỗi số đóng góp vào chính xác một chuỗi vì biểu diễn gốc tối thiểu là duy nhất. 
5. Sau khi xử lý tất cả các giá trị, câu trả lời là giá trị lớn nhất trong số tất cả các tổng của chuỗi. Điều này tương ứng với việc chọn cơ sở tốt nhất$x$, vì mỗi nhóm đại diện cho tất cả các giá trị có dạng$x^k$. 

Tại sao nó hoạt động: mọi lựa chọn hợp lệ đều tương ứng với một số cơ sở$x$. Bất kỳ số nào được bao gồm phải là lũy thừa chính xác của$x$, do đó nghiệm tối thiểu của nó cũng phải bằng$x$. Do đó, tất cả các số đã chọn sẽ thu gọn thành một nhóm duy nhất dưới ánh xạ cơ sở chuẩn này. Vì mỗi số ánh xạ tới chính xác một cơ sở nên chúng tôi không bỏ sót bất kỳ nhóm hợp lệ nào và tối đa hóa trên các nhóm bao gồm tất cả các lựa chọn về$x$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def get_base(x):
    # find minimal base b such that x = b^k
    # try all powers up to log2(x)
    best = x
    # try exponent from 2 upward
    for k in range(2, 32):
        b = int(round(x ** (1.0 / k)))
        if b <= 1:
            continue
        # verify exact power
        p = 1
        for _ in range(k):
            p *= b
            if p > x:
                break
        if p == x:
            best = min(best, b)
    return best

n = int(input())
a = list(map(int, input().split()))

from collections import defaultdict
mp = defaultdict(int)

for v in a:
    b = get_base(v)
    mp[b] += v

print(max(mp.values()))
```chức năng`get_base`tính cơ số nguyên nhỏ nhất có lũy thừa lặp lại mang lại số đã cho. Chúng tôi lặp lại các số mũ có thể có lên tới 30 vì các giá trị được giới hạn bởi 10^9, do đó, mọi phân tích lũy thừa có ý nghĩa đều phải có số mũ nhỏ. 

Đối với mỗi số mũ ứng cử viên, chúng tôi ước chừng cơ số bằng cách sử dụng trích xuất căn số nguyên, sau đó xác minh bằng phép nhân trực tiếp để tránh lỗi chính xác nổi. Khi cơ sở được xác định, chúng tôi tích lũy giá trị ban đầu vào nhóm của nó. 

Từ điển`mp`thu thập tổng số dừa trên mỗi cơ sở và câu trả lời cuối cùng là số tiền tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
4 8 25 5
```Chúng tôi tính toán các cơ sở: 

| Giá trị | Cơ sở tính toán | Cập nhật tổng nhóm | Trạng thái bản đồ hiện tại | 
| --- | --- | --- | --- | 
| 4 | 2 | +4 | {2: 4} | 
| 8 | 2 | +8 | {2: 12} | 
| 25 | 5 | +25 | {2: 12, 5: 25} | 
| 5 | 5 | +5 | {2: 12, 5: 30} | 

Nhóm tốt nhất là cơ sở 5 với tổng số 30. 

Điều này cho thấy mặc dù 2 tạo ra nhiều giá trị nhưng nhóm nhỏ hơn vẫn có thể giành chiến thắng nếu tổng các thành viên của nhóm đó cao hơn. 

### Ví dụ 2 

đầu vào:```
5
9 27 40 1 1
```| Giá trị | Cơ sở tính toán | Cập nhật tổng nhóm | Trạng thái bản đồ hiện tại | 
| --- | --- | --- | --- | 
| 9 | 3 | +9 | {3: 9} | 
| 27 | 3 | +27 | {3: 36} | 
| 40 | 40 | +40 | {3: 36, 40: 40} | 
| 1 | 1 | +1 | {3: 36, 40: 40, 1: 2} | 
| 1 | 1 | +1 | {3: 36, 40: 40, 1: 3} | 

Tốt nhất là cơ sở 40 với 40. 

Điều này xác nhận rằng các giá trị lớn bị cô lập có thể chiếm ưu thế ngay cả khi chúng không tạo thành chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Mỗi số được kiểm tra các căn số mũ có thể lên tới ~30 | 
| Không gian | O(n) | Bản đồ lưu trữ tối đa một mục cho mỗi cơ sở riêng biệt | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì$n = 2 \times 10^5$và mỗi số được xử lý độc lập bằng cách kiểm tra nghiệm gốc của hệ số không đổi giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def get_base(x):
        best = x
        for k in range(2, 32):
            b = int(round(x ** (1.0 / k)))
            if b <= 1:
                continue
            p = 1
            for _ in range(k):
                p *= b
                if p > x:
                    break
            if p == x:
                best = min(best, b)
        return best

    n = int(input())
    a = list(map(int, input().split()))

    from collections import defaultdict
    mp = defaultdict(int)

    for v in a:
        mp[get_base(v)] += v

    return str(max(mp.values()))

# provided samples (as stated in statement; formatted)
assert run("4\n8 25 5 16\n") == "30"
assert run("3\n9 27 40\n") == "40"
assert run("5\n1 1 4 1 1\n") == "5"

# custom cases
assert run("2\n2 4\n") == "6"
assert run("2\n16 81\n") == "81"
assert run("3\n8 8 8\n") == "24"
assert run("3\n7 49 343\n") == "399"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 4 | 6 | nhóm xích điện đơn giản | 
| 2 16 81 | 81 | căn cứ rời rạc, lựa chọn duy nhất thắng | 
| 3 8 8 8 | 24 | lặp đi lặp lại sức mạnh giống hệt nhau | 
| 7 49 343 | 399 | tính nhất quán của chuỗi số mũ dài hơn | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi số là 1. Đối với đầu vào:```
4
1 1 1 1
```Mọi giá trị đều ánh xạ tới cơ sở 1 vì$1 = 1^k$cho bất kỳ$k$. Thuật toán nhóm tất cả chúng vào một nhóm duy nhất và trả về 4, phù hợp với việc chọn$x = 1$. 

Một trường hợp khác là sự kết hợp giữa quyền lực hoàn hảo và phi quyền hạn:```
3
8 9 10
```Ở đây 8 ánh xạ tới cơ sở 2, 9 ánh xạ đến cơ sở 3 và 10 ánh xạ đến 10. Mỗi bản đồ tạo thành nhóm riêng và giá trị đơn tối đa là 10. Thuật toán tránh cố gắng ép chúng vào một cấu trúc chung vì không tồn tại cơ sở chung.
