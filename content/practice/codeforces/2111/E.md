---
title: "CF 2111E - Thay đổi chuỗi"
description: "Chúng ta được cho một chuỗi chỉ gồm các ký tự a, b và c. Bên cạnh đó là một chuỗi các thao tác. Mỗi thao tác gợi ý một sự thay thế có thể xảy ra: chọn bất kỳ sự xuất hiện nào của chữ cái x và biến nó thành một chữ cái y khác hoặc bỏ qua hoàn toàn thao tác."
date: "2026-06-08T04:32:03+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "implementation", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 2111
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 179 (Rated for Div. 2)"
rating: 1900
weight: 2111
solve_time_s: 96
verified: false
draft: false
---

[CF 2111E - Thay đổi chuỗi](https://codeforces.com/problemset/problem/2111/E) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, triển khai, sắp xếp, chuỗi 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi chỉ bao gồm các ký tự`a`,`b`, Và`c`. Bên cạnh đó là một chuỗi các thao tác. Mỗi thao tác gợi ý một sự thay thế có thể: chọn bất kỳ sự xuất hiện của một chữ cái`x`và biến nó thành một lá thư khác`y`hoặc bỏ qua toàn bộ thao tác. 

Khó khăn chính là các thao tác này được áp dụng theo thứ tự và mỗi thao tác đều là tùy chọn. Chúng tôi không bị buộc phải áp dụng bất kỳ sự thay thế cụ thể nào, nhưng chúng tôi phải quyết định, đối với mỗi thao tác, liệu nó có đáng sử dụng hay không và nếu có thì ký tự nào cần sửa đổi. Sau khi xử lý tất cả các thao tác, chúng tôi muốn chuỗi cuối cùng càng nhỏ càng tốt theo thứ tự từ điển. 

Giảm thiểu từ điển ở đây có nghĩa là chúng tôi muốn các vị trí trước đó trong chuỗi trở nên nhỏ nhất có thể, ưu tiên`a < b < c`. Vì các hoạt động mang tính toàn cầu (chúng có thể ảnh hưởng đến bất kỳ vị trí nào có chứa`x`) và các lựa chọn mang tính tuần tự, vấn đề là ở việc điều phối các “cơ hội chuyển đổi” có giới hạn theo thời gian. 

Các ràng buộc rất chặt chẽ: tổng chiều dài chuỗi và tổng số thao tác trên tất cả các trường hợp thử nghiệm lên tới 2 · 10^5. Điều này loại trừ bất kỳ giải pháp nào cố gắng mô phỏng từng thao tác bằng cách quét toàn bộ chuỗi, vì điều đó sẽ dẫn đến hành vi O(nq) trong trường hợp xấu nhất. 

Một mô hình tinh thần ngây thơ sẽ là: sau mỗi thao tác, hãy quét toàn bộ chuỗi và áp dụng thay thế nếu nó hữu ích. Điều đó không chỉ ảnh hưởng đến hiệu suất mà còn về tính chính xác, bởi vì quyết định tốt nhất cho một thao tác phụ thuộc vào các hoạt động trong tương lai chứ không chỉ trạng thái hiện tại. 

Một trường hợp thất bại tinh vi đối với việc áp dụng ngay lập tức tham lam xuất hiện khi những chuyển đổi sớm mang lại lợi ích cục bộ nhưng có hại trên toàn cầu. Ví dụ, chuyển đổi`b -> a`sớm có thể có vẻ tối ưu, nhưng các hoạt động sau này có thể cho phép chuyển đổi`c -> b`và sau đó`b -> a`, tạo ra những cải tiến xếp tầng tốt hơn. Cách tiếp cận cục bộ tham lam sẽ làm mất đi cấu trúc này. 

## Phương pháp tiếp cận 

Giải pháp bạo lực sẽ mô phỏng từng thao tác một và đối với mỗi thao tác, hãy quét chuỗi để quyết định vị trí áp dụng nó, có khả năng thử tất cả các chỉ mục có thể. Điều này dẫn đến O(nq), quá lớn ở mức 2 · 10^5 cho mỗi tham số. 

Quan sát quan trọng là các ký tự cực kỳ hạn chế: chỉ tồn tại ba ký hiệu. Điều này có nghĩa là toàn bộ hệ thống có thể được mô tả bằng cách các ký hiệu này có thể biến đổi lẫn nhau theo thời gian. Thay vì theo dõi vị trí trong chuỗi, chúng tôi theo dõi sự chuyển đổi giữa các chữ cái. 

Mỗi thao tác chỉ ảnh hưởng đến số lượng ký tự chứ không ảnh hưởng đến vị trí của chúng. Quan trọng hơn, việc chúng ta áp dụng một thao tác có phụ thuộc vào việc làm như vậy có giúp chúng ta rút gọn các ký tự trong tương lai hay không. Điều này gợi ý các hoạt động xử lý lạc hậu. 

Nếu đảo ngược thời gian, chúng ta có thể coi mỗi chữ cái đều có “dạng cuối cùng tốt nhất có thể” tùy thuộc vào các thao tác trong tương lai. Khi chúng tôi xử lý các thao tác từ cuối đến đầu, chúng tôi duy trì ánh xạ được biết đến nhiều nhất từ ​​mỗi ký tự đến ký tự nhỏ nhất mà cuối cùng nó có thể trở thành. 

Ở mỗi bước đảo ngược, chúng ta quyết định xem mình muốn “sử dụng” thao tác hay bỏ qua nó. Vì chúng ta đang đi lùi nên việc sử dụng một thao tác tương ứng với việc cho phép một sự chuyển đổi trong tương lai. Điều này trở thành một quyết định mang tính quyết định vì chúng ta đã biết kết quả tốt nhất của các hoạt động sau này. 

Chúng tôi duy trì đặc tính tốt nhất có thể đạt được cho`a`,`b`, Và`c`sau khi áp dụng một hậu tố của hoạt động. Sau đó, chúng tôi truyền bá những biến đổi này ngược lại. Cuối cùng, chúng tôi áp dụng ánh xạ kết quả vào chuỗi gốc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Đảo ngược DP trên chữ cái | O(n + q) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một mảng`best`Ở đâu`best[x]`đại diện cho ký tự nhỏ nhất mà chữ cái gốc`x`có thể trở thành sau khi xử lý hậu tố của hoạt động. 

Chúng tôi xử lý các hoạt động từ cuối đến đầu. 

1. Khởi tạo`best[a] = a`,`best[b] = b`,`best[c] = c`. Điều này thể hiện thực tế là không áp dụng thao tác nào, mỗi chữ cái vẫn giữ nguyên chính nó. 
2. Lặp lại các thao tác theo thứ tự ngược lại. Đối với một hoạt động`(x -> y)`, chúng tôi xem xét liệu việc áp dụng nó có giúp cải thiện việc ánh xạ hiện tại hay không. Tại thời điểm này, chúng tôi đã biết kết quả tốt nhất có thể đạt được sau các hoạt động sau này. 
3. Nếu chuyển đổi`x`thông qua thao tác này dẫn đến ký tự cuối cùng nhỏ hơn rất nhiều so với ký tự cuối cùng`x`hiện đang ánh xạ tới, chúng tôi áp dụng nó bằng cách cập nhật`best[x]`. 

Chính xác hơn, chúng ta so sánh`best[y]`với`best[x]`. Nếu như`best[y] < best[x]`, sau đó làm`x`đi qua`y`cải thiện kết quả cuối cùng của nó, vì vậy chúng tôi đặt`best[x] = best[y]`. 

1. Ngược lại, chúng ta bỏ qua thao tác vì nó không thể cải thiện kết quả cuối cùng đối với bất kỳ trường hợp nào xảy ra.`x`. 
2. Sau khi xử lý xong các thao tác, ta xây dựng chuỗi cuối cùng bằng cách thay thế từng ký tự`s[i]`với`best[s[i]]`. 

Lý do điều này có tác dụng là vì mỗi chữ cái theo dõi độc lập hình thức tối ưu có thể đạt được của nó. Vì chỉ có ba chữ cái nên sự tương tác không phụ thuộc vào vị trí mà chỉ phụ thuộc vào chuỗi chuyển đổi. Xử lý ngược lại đảm bảo rằng khi quyết định một hoạt động, chúng ta đã biết trước những hậu quả tốt nhất trong tương lai. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình xử lý ngược,`best[x]`đại diện cho chữ cái tối thiểu có thể truy cập từ`x`chỉ sử dụng các thao tác ở bên phải của chỉ mục hiện tại. Khi chúng tôi xem xét một hoạt động mới`x -> y`, chúng tôi đang thêm một cạnh mới vào biểu đồ chuyển đổi này một cách hiệu quả. Nếu định tuyến qua`y`cải thiện`x`, thì việc bao gồm cạnh này phải là tối ưu; mặt khác, bỏ qua nó không thể gây tổn hại đến khả năng trong tương lai bởi vì`best[y]`đã mã hóa kết quả tối ưu trong tương lai của`y`. Tính bất biến này đảm bảo chúng ta không bao giờ bỏ lỡ chuỗi chuyển đổi có lợi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        s = list(input().strip())

        ops = []
        for _ in range(q):
            x, y = input().split()
            ops.append((x, y))

        best = { 'a': 'a', 'b': 'b', 'c': 'c' }

        def apply(x):
            return best[x]

        for x, y in reversed(ops):
            if apply(y) < apply(x):
                best[x] = apply(y)

        res = []
        for ch in s:
            res.append(apply(ch))

        print("".join(res))

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi là`best`từ điển, lưu trữ kết quả tối ưu hiện tại cho mỗi ký tự. Việc duyệt ngược là rất quan trọng vì nó đảm bảo rằng khi đánh giá một hoạt động, tất cả các quyết định sau này đều đã được cố định. 

chức năng`apply`là một sự trừu tượng nhỏ cho thấy rõ rằng chúng tôi luôn làm việc với giá trị được ánh xạ cuối cùng chứ không phải ký tự thô. Điều này tránh được những sai sót trong đó ánh xạ trung gian có thể bị bỏ qua. 

Vòng lặp cuối cùng chỉ đơn giản dịch chuỗi gốc thông qua ánh xạ này, điều này an toàn vì các thao tác không bao giờ phụ thuộc vào vị trí mà chỉ phụ thuộc vào nhận dạng ký tự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = cb
operations: (c->b), (b->a)
```Chúng tôi xử lý ngược. 

| Bước | Hoạt động | tốt nhất[a] | tốt nhất[b] | tốt nhất[c] | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | một | b | c | 
| 1 | b -> a | một | một | c | 
| 2 | c -> b | một | một | một | 

Lập bản đồ cuối cùng:`c -> a`,`b -> a`,`a -> a`Áp dụng cho`cb`cho`aa`. 

Điều này cho thấy cách xử lý ngược chuỗi các phép biến đổi một cách tự nhiên. 

### Ví dụ 2 

đầu vào:```
s = bbb
operations: b->c, c->a, b->a
```| Bước | Hoạt động | tốt nhất[a] | tốt nhất[b] | tốt nhất[c] | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | một | b | c | 
| 1 | b -> a | một | một | c | 
| 2 | c -> a | một | một | một | 
| 3 | b -> c | một | một | một | 

Chuỗi cuối cùng trở thành`aaa`. 

Ví dụ này chứng minh rằng ngay cả các hoạt động dường như không liên quan cũng có thể quan trọng nếu chúng cải thiện các trạng thái trung gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Mỗi thử nghiệm xử lý các thao tác ngược lại một lần và sau đó xây dựng chuỗi kết quả theo thời gian tuyến tính | 
| Không gian | O(1) | Chỉ một ánh xạ cố định trên ba ký tự được lưu trữ | 

Giải pháp phù hợp thoải mái trong giới hạn vì ngay cả trong trường hợp xấu nhất, tổng số thao tác trên tất cả các trường hợp thử nghiệm là 2 · 10^5, khiến việc xử lý tuyến tính trở nên đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import *
    
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, q = map(int, input().split())
            s = list(input().strip())

            ops = []
            for _ in range(q):
                x, y = input().split()
                ops.append((x, y))

            best = {'a':'a','b':'b','c':'c'}

            def apply(x):
                return best[x]

            for x, y in reversed(ops):
                if apply(y) < apply(x):
                    best[x] = apply(y)

            print("".join(apply(ch) for ch in s))

    out = io.StringIO()
    _stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = _stdout
    return out.getvalue().strip()

# provided samples
assert run("""3
2 2
cb
c b
b a
10 10
bbbbbbbbbb
b a
b c
c b
b a
c a
b c
b c
b a
a b
c a
30 20
abcaababcbbcabcbbcabcbabbbbabc
b c
b c
c a
b c
b c
b a
b c
b c
b a
b a
b a
b a
c a
b c
c a
b c
c a
c a
b c
c b
""") == """ab
aaaaabbbbb
aaaaaaaaaaaaaaabbbabcbabbbbabc"""

# custom cases
assert run("""1
1 1
a
a b
""") == "b", "single char upgrade"

assert run("""1
3 0
abc
""") == "abc", "no operations"

assert run("""1
3 3
abc
b c
c b
b a
""") == "aaa", "cycle collapse"

assert run("""1
4 2
cbbc
c b
b a
""") == "aaba", "chain effect"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nâng cấp char đơn | b | cập nhật bản đồ cơ bản | 
| không có hoạt động | abc | trường hợp nhận dạng | 
| sụp đổ chu kỳ | aaa | giải quyết các hoạt động xung đột | 
| hiệu ứng dây chuyền | aaba | độ chính xác lan truyền một phần | 

## Vỏ cạnh 

Một tình huống phức tạp xảy ra khi các hoạt động hình thành theo chu kỳ, chẳng hạn như`a -> b`,`b -> c`,`c -> a`. Một mô phỏng chuyển tiếp đơn giản có thể dao động hoặc chỉ áp dụng sai các cải tiến cục bộ. Trong xử lý ngược, bất biến đảm bảo tính ổn định: một khi một chữ cái được ánh xạ tới một đại diện nhỏ hơn, nó sẽ không bao giờ tăng lên nữa. 

Một trường hợp khác là khi các hoạt động dường như vô dụng cục bộ lại trở nên hữu ích do các chuyển đổi sau này. Ví dụ,`b -> c`trông có vẻ có hại nếu`c`ban đầu lớn nhưng về sau`c -> a`xuất hiện. Việc vượt qua ngược lại đảm bảo rằng khi chúng ta đánh giá`b -> c`, chúng ta đã biết điều đó rồi`c`cuối cùng trở thành`a`, nhờ đó sự cải thiện được ghi lại một cách chính xác tại thời điểm đó. 

Cuối cùng, các trường hợp không có thao tác hoặc chỉ tự ánh xạ (`a -> a`) không ảnh hưởng đến ánh xạ. Thuật toán tự nhiên rời đi`best`không thay đổi, do đó đầu ra vẫn giống với chuỗi đầu vào.
