---
title: "CF 102192H - Dây tương tự K"
description: "Chúng ta cần quyết định xem hai chuỗi khác rỗng có thuộc cùng một lớp tương đương hay không theo khái niệm k-tương tự được xác định đệ quy. Mối quan hệ bắt đầu bằng một chuỗi giống với chính nó."
date: "2026-08-18T10:07:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102192
codeforces_index: "H"
codeforces_contest_name: "2018 Chinese Multi-University Training, Nanjing U Contest"
rating: 0
weight: 102192
solve_time_s: 814
verified: true
draft: false
---

[CF 102192H - Chuỗi tương tự K](https://codeforces.com/problemset/problem/102192/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 13m 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần quyết định xem hai chuỗi khác rỗng có thuộc cùng một lớp tương đương hay không theo khái niệm k-tương tự được xác định đệ quy. Mối quan hệ bắt đầu bằng một chuỗi giống với chính nó. Quy tắc duy nhất phụ thuộc trực tiếp vào k nói rằng hai chuỗi khác rỗng S và T có thể liên hệ với nhau khi tổng chiều dài của chúng lớn nhất là k và các phép nối ST và TS đã được biết là tương tự nhau. Sau đó, các chuỗi con tương tự có thể được thay thế bên trong bất kỳ chuỗi lớn hơn nào và tính bắc cầu cho phép chúng ta thay thế chuỗi. 

Cách hữu ích để suy nghĩ về định nghĩa không phải là so sánh chuỗi đệ quy mà là một hệ thống viết lại. Quy tắc 2 tạo ra một tập hợp nhỏ các quy tắc thay thế cơ bản, trong khi quy tắc 3 và 4 nói rằng chúng ta có thể áp dụng những thay thế đó ở bất kỳ đâu và theo bất kỳ trình tự nào. Toàn bộ vấn đề là phải hiểu những quy tắc cơ bản đó trông như thế nào đối với từng giá trị có thể có của k. 

Tổng chiều dài của tất cả các chuỗi đầu vào tối đa là 3 triệu, mặc dù một chuỗi riêng lẻ có thể chứa 200.000 ký tự. Điều đó ngay lập tức loại trừ bất kỳ thuật toán nào khám phá một không gian trạng thái lớn của chuỗi và thậm chí giải pháp O(n log n) sẽ phức tạp hơn mức cần thiết nếu cấu trúc có thể giảm xuống một số ít trường hợp. Quét tuyến tính cho mỗi trường hợp thử nghiệm nằm trong giới hạn thoải mái, trong khi mọi thứ bậc hai có thể yêu cầu khoảng 4 × 10^10 thao tác ký tự trên trường hợp thử nghiệm có kích thước tối đa. 

Có một số trường hợp khó khăn trong đó việc triển khai có vẻ hợp lý lại đưa ra câu trả lời sai. Ví dụ, khi k = 2, đầu vào```
2
ab
ab
```có câu trả lời`yes`, trong khi```
2
a
aa
```có câu trả lời`no`. Việc triển khai bất cẩn coi khả năng so sánh các chuỗi con có tổng chiều dài tối đa k là quyền xóa hoặc sao chép ký tự sẽ chấp nhận trường hợp thứ hai một cách không chính xác. Với k = 2, quy tắc không tầm thường duy nhất có dạng`a -> a`, vì vậy thực sự không có gì có thể thay đổi. 

Ranh giới giữa k = 4 và k = 5 là một cái bẫy khác. Coi như```
4
abcba
aba
```Câu trả lời là`no`. Sự giảm k = 4 không sụp đổ`abcba`về dạng bình thường giống như`aba`. Tuy nhiên, với k = 5,```
5
abcba
aba
```có câu trả lời`yes`, bởi vì khi k đạt tới 5, mối quan hệ sẽ được đặc trưng hoàn toàn bởi ký tự đầu tiên và ký tự cuối cùng. Việc xử lý tất cả k >= 4 giống hệt nhau sẽ làm mất đi sự khác biệt này. 

Lỗi phổ biến thứ ba là chỉ sử dụng ký tự đầu tiên và cuối cùng quá sớm. Vì```
3
aab
ab
```câu trả lời là`yes`, vì các bản sao liên tiếp của cùng một ký tự có thể được chèn hoặc xóa. Nếu không có```
4
abca
aba
```câu trả lời là`no`, mặc dù cả hai chuỗi đều bắt đầu và kết thúc bằng cùng một ký tự. Điều kiện điểm cuối chỉ đủ từ k = 5 trở đi. 

Những phân loại này và các dạng thông thường của chúng cũng được đưa ra trong bài xã luận chính thức của cuộc thi. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng làm theo định nghĩa theo nghĩa đen. Chúng ta có thể coi mọi cặp chuỗi tương tự đã biết là quy tắc viết lại, tạo ra tất cả các chuỗi có thể truy cập được từ A và dừng khi B xuất hiện. Điều này đúng về mặt khái niệm vì quy tắc 3 và 4 nói chính xác rằng độ tương tự k là sự đóng cửa phản xạ, đối xứng, bắc cầu của các thay thế được tạo ra bởi quy tắc 2. 

Vấn đề là số lượng trạng thái có thể. Ngay cả việc hạn chế chúng ta ở các chuỗi có độ dài tối đa n, vẫn có 

[ 
\sum_{i=1}^{n} 26^i = \frac{26^{n+1}-26}{25} 
] 

có thể có các chuỗi chữ thường. Với n = 200000 thì giá trị này lớn về mặt thiên văn. Tìm kiếm đệ quy cũng có thể truy cập lại các lớp tương đương giống nhau thông qua các chuỗi thay thế khác nhau, do đó việc ghi nhớ không cứu được cách tiếp cận. Lực lượng vũ phu chỉ hữu ích cho việc khám phá cấu trúc trên các dây nhỏ. 

Điều quan trọng cần lưu ý là quy tắc 2 là điểm duy nhất mà k quan trọng. Khi đã biết các quy tắc cơ bản do quy tắc 2 tạo ra, các quy tắc còn lại chỉ cho phép những thay thế đó được sử dụng bên trong các chuỗi lớn hơn. Phân tích chính thức trước tiên nhận thấy rằng hai chuỗi k giống nhau phải có cùng ký tự đầu tiên và ký tự cuối cùng, sau đó phân loại các quy tắc thay thế hữu ích theo k. 

Với k = 1 và k = 2, không thể sửa đổi hữu ích được, do đó các chuỗi phải bằng nhau về mặt nghĩa đen. Với k = 3, các quy tắc hữu ích là`a -> aa`Và`aa -> a`, độc lập cho mỗi ký tự. Do đó, mỗi lần chạy tối đa của một ký tự có thể được thay thế bằng một bản sao duy nhất, tạo ra một chuỗi rút gọn duy nhất. 

Với k = 4, có thêm các quy tắc tương ứng với`a -> aba`và ngược lại của nó. Sau lần đầu tiên loại bỏ các bản sao liên tiếp, các quy tắc này có thể được biểu diễn bằng cách rút gọn`aba -> a`. Một ngăn xếp đưa ra chính xác mức giảm này: nếu ký tự mới bằng ký tự đó hai vị trí dưới đầu trang thì ký tự trên cùng có thể bị xóa. Ngăn xếp kết quả là một dạng chuẩn cho lớp tương đương. 

Tại k = 5, các quy tắc mới có sẵn có thể giảm bất kỳ chuỗi nào thành hai ký tự điểm cuối của nó. Bản thân các quy tắc bổ sung xuất hiện tại k = 5 đều có thể đạt được từ các quy tắc k = 4, do đó từ thời điểm này trở đi không cần bất biến mới nào. Ký tự đầu tiên và cuối cùng đều cần và đủ. Sự phân loại tương tự vẫn đúng cho mọi k > 5. 

Điều này cung cấp số lần quét tuyến tính không đổi, thay vì tìm kiếm theo chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26^n) trạng thái trong trường hợp xấu nhất | O(26^n) | Quá chậm | 
| Tối ưu | O( | A | + | B | ) | O( | A | + | B | ) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc k, A và B. Nếu k là 1 hoặc 2, hãy so sánh trực tiếp hai chuỗi. Không có sự thay thế hữu ích nào có thể thay đổi một trong hai chuỗi trong những trường hợp này. 
2. Nếu k là 3, hãy nén mọi chuỗi ký tự bằng nhau tối đa thành một ký tự. Ví dụ,`aaabbccca`trở thành`abca`. So sánh hai chuỗi nén. Điều này có tác dụng vì thao tác hữu ích duy nhất là thêm hoặc xóa một bản sao khỏi một lần chạy. 
3. Nếu k là 4, trước tiên hãy thực hiện thao tác nén trùng lặp liên tiếp tương tự. Sau đó quét chuỗi kết quả bằng một ngăn xếp. Với mỗi ký tự c, so sánh nó với ký tự tại`stack[-2]`khi vị trí đó tồn tại. Nếu chúng bằng nhau, hãy xóa đỉnh ngăn xếp hiện tại thay vì đẩy c. Nếu không thì nhấn c. 
4. Áp dụng cùng một mức giảm k = 4 cho cả hai chuỗi và so sánh các ngăn xếp kết quả. Ngăn xếp giảm bằng nhau có nghĩa là hai chuỗi có thể được chuyển đổi thành một chuỗi khác bằng cách thay thế k = 4 có sẵn. 
5. Nếu k ít nhất là 5, chỉ so sánh ký tự đầu tiên và ký tự cuối cùng của A và B. Chúng giống nhau về k chính xác khi cả hai điểm cuối đều đồng ý. 
6. Đầu ra`yes`khi các hình thức kinh điển tương ứng đồng ý, và`no`nếu không thì. 

Tại sao nó hoạt động: bất biến là mỗi phép biến đổi được thuật toán sử dụng sẽ bảo toàn chính xác lớp tương đương được tạo bởi quy tắc 2. Đối với k = 3, bất biến là chuỗi các lần chạy riêng biệt. Với k = 4, mức giảm ngăn xếp không thay đổi sau mỗi lần thay thế cơ bản, bao gồm cả các quy tắc đã có sẵn cho k = 3, do đó hai chuỗi trong cùng một lớp tương đương có cùng kết quả ngăn xếp. Ngược lại, mọi phép giảm được thực hiện bởi ngăn xếp đều tương ứng với một thay thế k = 4 hợp lệ, do đó, các kết quả ngăn xếp bằng nhau có thể được kết nối thông qua các thay thế hợp lệ. Đối với k >= 5, các quy tắc cơ bản cho phép loại bỏ mọi phần bên trong của chuỗi trong khi vẫn giữ lại các điểm cuối của nó, làm cho cặp điểm cuối trở thành bất biến hoàn toàn. Bản thân các ký tự đầu tiên và cuối cùng được giữ nguyên theo mọi quy tắc, vì vậy các điểm cuối khác nhau không bao giờ có thể tương đương nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def compress_runs(s):
    res = []
    for c in s:
        if not res or res[-1] != c:
            res.append(c)
    return res

def reduce_k4(s):
    stack = []
    for c in s:
        if stack and stack[-1] == c:
            continue

        if len(stack) >= 2 and stack[-2] == c:
            stack.pop()
        else:
            stack.append(c)

    return stack

def similar(k, a, b):
    if k <= 2:
        return a == b

    if k == 3:
        return compress_runs(a) == compress_runs(b)

    if k == 4:
        return reduce_k4(a) == reduce_k4(b)

    return a[0] == b[0] and a[-1] == b[-1]

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        k = int(input())
        a = input().strip()
        b = input().strip()

        ans.append("yes" if similar(k, a, b) else "no")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`similar`Hàm thực hiện trực tiếp bốn trường hợp cấu trúc. Không cần phải kiểm tra k ngoài việc nó là 1, 2, 3, 4 hay ít nhất là 5.`compress_runs`sử dụng danh sách như một chồng các ký tự liên tiếp riêng biệt. Một ký tự chỉ được thêm vào khi nó khác với ký tự trước đó, do đó, một lệnh chạy như`aaaa`đóng góp đúng một`a`.`reduce_k4`chứa việc thực hiện hơi tinh tế duy nhất. đầu tiên`if`xử lý các ký tự bằng nhau liên tiếp, vốn đã có thể xóa được vì quy tắc k = 3 khả dụng khi k = 4. Điều kiện thứ hai kiểm tra xem ký tự mới có khớp với ký tự ở hai vị trí phía dưới trên cùng hay không. Trong trường hợp đó ký tự trên cùng đại diện cho phần giữa của một`aba`mẫu và có thể bị xóa. 

Thứ tự của hai điều kiện này rất quan trọng. Phải bỏ qua các bản sao liên tiếp trước khi kiểm tra ký tự hai mặt sau, vì dạng chuẩn k = 4 được xác định sau khi việc rút gọn k = 3 đã được kết hợp. 

Không có lo ngại về tràn số nguyên vì thuật toán chỉ lưu trữ các ký tự và chỉ mục. Bản thân các chuỗi đầu vào chi phối việc sử dụng bộ nhớ và tổng kích thước đầu vào được giới hạn bởi 3 triệu ký tự. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với mẫu đầu tiên, k = 3, A là`ba`, và B là`baa`. Thuật toán sử dụng nén chạy. 

| Bước | Một tiểu bang | Bang B | 
| --- | --- | --- | 
| Bắt đầu |`ba`|`baa`| 
| Đọc`b`|`b`|`b`| 
| Đọc đầu tiên`a`|`ba`|`ba`| 
| Đọc thứ hai`a`| không thay đổi | không thay đổi | 
| Dạng rút gọn |`ba`|`ba`| 
| Kết quả | bằng |`yes`| 

thứ hai`a`ở B thuộc cùng một lần chạy liên tiếp như lần đầu tiên`a`, do đó có thể loại bỏ nó bằng quy tắc k = 3`aa -> a`. Đây chính xác là phép biến đổi được sử dụng trong ví dụ ban đầu từ định nghĩa. 

### Mẫu 2 

Đối với mẫu thứ hai, k = 2, A là`aab`, và B là`ab`. Vì k quá nhỏ để tạo ra sự thay thế hữu ích nên thuật toán phải so sánh các chuỗi gốc. 

| Bước | k | A | B | Quyết định | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 2 |`aab`|`ab`| k <= 2 | 
| So sánh | 2 |`aab`|`ab`| khác nhau | 
| Kết quả | 2 |`aab`|`ab`|`no`| 

Điều này chứng tỏ tại sao thuật toán không được áp dụng quy tắc nén chạy k = 3 khi k = 2. Hai chuỗi có cùng điểm cuối nhưng vẫn không giống nhau về k. 

### Bổ sung k = 4 dấu vết 

Xét k = 4, A =`ababa`. Sau khi nén trùng lặp liên tiếp, chuỗi không thay đổi. Sự phát triển của ngăn xếp là: 

| Nhân vật | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | 
|`a`|`[]`| đẩy |`[a]`| 
|`b`|`[a]`| đẩy |`[a,b]`| 
|`a`|`[a,b]`| đẩy |`[a,b,a]`| 
|`b`|`[a,b,a]`|`b == stack[-2]`, bật |`[a,b]`| 
|`a`|`[a,b]`|`a == stack[-2]`, bật |`[a]`| 

Toàn bộ chuỗi giảm xuống`a`. Điều tương tự cũng xảy ra với`aba`, do đó hai chuỗi tương tự k với k = 4. Dấu vết cho thấy lý do tại sao điều kiện ngăn xếp nhìn ngược lại hai vị trí thay vì chỉ kiểm tra xem ký tự hiện tại có bằng đầu hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | A | + | B | ) | Mỗi ký tự đầu vào được xử lý một số lần không đổi. | 
| Không gian | O( | A | + | B | ) | Các biểu diễn rút gọn tạm thời chứa tối đa một mục nhập cho mỗi ký tự đầu vào. | 

Trong tất cả các trường hợp thử nghiệm, tổng độ dài đầu vào tối đa là 3 triệu ký tự. Do đó, thuật toán chỉ thực hiện một số thao tác ký tự tuyến tính, dễ dàng tương thích với giới hạn 2 giây. Độ dài chuỗi riêng lẻ tối đa là 200.000 cũng thấp hơn nhiều so với điểm mà việc quét tuyến tính dựa trên danh sách Python trở thành vấn đề. 

## Trường hợp thử nghiệm```python
import sys
import io

def compress_runs(s):
    res = []
    for c in s:
        if not res or res[-1] != c:
            res.append(c)
    return res

def reduce_k4(s):
    stack = []
    for c in s:
        if stack and stack[-1] == c:
            continue
        if len(stack) >= 2 and stack[-2] == c:
            stack.pop()
        else:
            stack.append(c)
    return stack

def similar(k, a, b):
    if k <= 2:
        return a == b
    if k == 3:
        return compress_runs(a) == compress_runs(b)
    if k == 4:
        return reduce_k4(a) == reduce_k4(b)
    return a[0] == b[0] and a[-1] == b[-1]

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        k = int(input())
        a = input().strip()
        b = input().strip()
        out.append("yes" if similar(k, a, b) else "no")
    return "\n".join(out)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve() + "\n"
    finally:
        sys.stdin = old_stdin

# Provided samples
sample = """4
3
ba
baa
2
aab
ab
1
acesrc
acesrc
100
roundgod
zyb
"""
assert run(sample) == "yes\nno\nyes\nno\n", "provided samples"

# Minimum-size inputs and k = 1.
assert run("""2
1
a
a
1
a
b
""") == "yes\nno\n", "minimum-size strings"

# k = 2 must not perform k = 3 compression.
assert run("""2
2
aab
ab
2
aa
aa
""") == "no\nyes\n", "k=2 boundary"

# k = 3 removes consecutive repetitions.
assert run("""2
3
aaab
ab
3
abca
abcaa
""") == "yes\nyes\n", "k=3 run compression"

# k = 4 uses the two-back stack reduction.
assert run("""2
4
ababa
aba
4
abca
aba
""") == "yes\nno\n", "k=4 stack reduction"

# k = 5 changes the criterion to endpoints.
assert run("""2
5
abcba
aba
5
abcba
abbc
""") == "yes\nno\n", "k=5 endpoint boundary"

# Maximum-size all-equal strings.
n = 200000
big = "a" * n
assert run(f"""2
1
{big}
{big}
3
{big}b
ab
""") == "yes\nyes\n", "maximum-size and all-equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a / a`Và`1 / a / b`|`yes`,`no`| Độ dài tối thiểu và k = 1 | 
|`2 / aab / ab`|`no`| Ngăn chặn hành vi k = 3 vô tình tại k = 2 | 
|`3 / aaab / ab`|`yes`| Giảm chạy liên tiếp | 
|`4 / ababa / aba`|`yes`| Giảm ngăn xếp hai mặt sau | 
|`4 / abca / aba`|`no`| Trường hợp A k = 4 có điểm cuối bằng nhau nhưng dạng chuẩn tắc khác nhau | 
|`5 / abcba / aba`|`yes`| Chính xác k = 5 ranh giới | 
| 200000 bản sao`a`|`yes`| Kích thước đầu vào tối đa và các chuỗi hoàn toàn bằng nhau | 

## Vỏ cạnh 

Với k = 2, hãy xem xét```
2
aab
ab
```Thuật toán đi vào`k <= 2`phân nhánh và so sánh`aab == ab`, điều đó là sai. Nó xuất ra`no`. Điều này tránh việc xóa nhầm một trong các liên tiếp`a`ký tự, một thao tác chỉ thực hiện được sau khi k đạt tới 3. 

Với k = 3, hãy xét```
3
aaab
ab
```Chạy các thay đổi nén`aaab`vào trong`ab`, trong khi`ab`đã giảm rồi. Hai dạng chính tắc bằng nhau nên thuật toán đưa ra`yes`. Số lượng bản sao lặp lại trong một lần chạy không thành vấn đề ở giá trị k này. 

Với k = 4, hãy xét```
4
ababa
aba
```Chuỗi đầu tiên tạo ra trạng thái ngăn xếp`[a]`,`[a,b]`,`[a,b,a]`,`[a,b]`,`[a]`, trong khi cái thứ hai tạo ra`[a]`,`[a,b]`,`[a]`. Cả hai đều kết thúc tại`[a]`, vậy câu trả lời là`yes`. Điều này thực hiện việc giảm ngăn xếp không cục bộ thay vì chỉ loại bỏ trùng lặp liên tiếp. 

Đối với ranh giới k = 4 so với k = 5, hãy xem xét```
4
abca
aba
```Số k = 4 lần giảm nghỉ phép`abca`không thay đổi trong khi`aba`giảm xuống`a`, vậy câu trả lời là`no`. Chỉ thay đổi k thành 5 sẽ cho```
5
abca
aba
```Cả hai chuỗi đều bắt đầu bằng`a`và kết thúc bằng`a`, do đó tiêu chí điểm cuối được áp dụng và câu trả lời trở thành`yes`. 

Đối với đầu vào có kích thước tối đa, lấy hai chuỗi gồm 200.000 bản sao`a`. Ngay cả khi k = 1 thì chúng vẫn bằng nhau ngay lập tức. Việc triển khai thực hiện so sánh chuỗi đơn và không bao giờ xây dựng cấu trúc phụ trợ lớn, do đó trường hợp này vẫn tuyến tính ở kích thước đầu vào. 

Việc phân loại thành k = 1, k = 2, k = 3, k = 4 và k >= 5 là cái nhìn sâu sắc trung tâm. Khi các dạng thông thường đó được nhận dạng, định nghĩa đệ quy rõ ràng sẽ thu gọn thành một số lần quét chuỗi thời gian tuyến tính.
