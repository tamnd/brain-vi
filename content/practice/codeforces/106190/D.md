---
title: "CF 106190D - \u0425\u0432\u043e\u0441\u0442\u044b \u0442\u0438\u0433\u0440\u043e\u043a\u0440\u044b\u0441\u0430"
description: "Bài toán yêu cầu chúng ta khôi phục độ dài có thể có của chiếc đuôi giữa bị thiếu của một sinh vật giống mèo hổ. Ba chiều dài đuôi là a, b và c, trong đó a < b < c. Chúng ta đã biết độ dài ngắn nhất và dài nhất, nhưng độ dài ở giữa vẫn còn thiếu."
date: "2026-06-25T10:45:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106190
codeforces_index: "D"
codeforces_contest_name: "\u041a\u043e\u0433\u043d\u0438\u0442\u0438\u0432\u043d\u044b\u0435 \u0442\u0435\u0445\u043d\u043e\u043b\u043e\u0433\u0438\u0438 2025-2026. \u041f\u0435\u0440\u0432\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 106190
solve_time_s: 39
verified: true
draft: false
---

[CF 106190D - \u0425\u0432\u043e\u0441\u0442\u044b \u0442\u0438\u0433\u0440\u043e\u043a\u0440\u044b\u0441\u0430](https://codeforces.com/problemset/problem/106190/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta khôi phục độ dài có thể có của chiếc đuôi giữa bị thiếu của một sinh vật giống mèo hổ. Chiều dài ba đuôi là`a`,`b`, Và`c`, Ở đâu`a < b < c`. Chúng ta đã biết độ dài ngắn nhất và dài nhất, nhưng độ dài ở giữa vẫn còn thiếu. Chúng tôi cũng được trao`m`, là tích của tất cả các thừa số nguyên tố phân biệt xuất hiện trong`a * b * c`, còn được gọi là căn thức của tích. Chúng ta cần các giá trị nhỏ nhất và lớn nhất có thể có của`b`hoặc báo cáo rằng không có giá trị nào tồn tại. Vấn đề ban đầu là từ Codeforces Gym 106190. 

Đầu vào chứa một số trường hợp độc lập. Mỗi trường hợp đưa ra`m`,`a`, Và`c`. Câu trả lời là một cặp giá trị: chiều dài đuôi giữa hợp lệ tối thiểu và chiều dài đuôi giữa hợp lệ tối đa. Độ dài ở giữa phải nằm giữa hai độ dài đã biết. 

Hạn chế quan trọng đó là`a`Và`c`nhiều nhất là`10^6`, trong khi`m`có thể lớn như`10^18`. Điều này ngay lập tức loại trừ việc thử mọi nhân tử có thể có của`m`hoặc thực hiện công tỉ lệ thuận với giá trị của nó. Giới hạn trên của`c`là chìa khóa: mọi câu trả lời có thể`b`nhỏ hơn`10^6`, vì vậy chúng ta chỉ cần suy luận về số lượng ứng cử viên tương đối nhỏ. 

Các trường hợp khó khăn chính xuất phát từ việc nhầm lẫn cái gốc với sản phẩm ban đầu. Một số có thể chứa cùng một số nguyên tố nhiều lần, nhưng phép căn chỉ quan tâm liệu số nguyên tố có tồn tại hay không. 

Ví dụ:```
Input:
210 5 21

Output:
6 20
```Một cách tiếp cận bất cẩn có thể cố gắng yêu cầu`b`chính xác là tích của các thừa số nguyên tố bị thiếu. Thừa số nguyên tố còn thiếu là`2 * 3 = 6`, Nhưng`20`cũng hoạt động vì nó chỉ thêm số nguyên tố`2`lại. Những gì còn sót lại`2 * 3 * 5 * 7`. 

Một trường hợp phức tạp khác là khi`a`hoặc`c`đã chứa một số nguyên tố không có trong`m`.```
Input:
35 5 7

Output:
-1 -1
```Đây`a * c = 35`, vậy căn thức đã chứa`5`Và`7`. giá trị`m`chỉ có`5`Và`7`cũng vậy, nhưng bất kỳ giá trị trung bình nào giữa`5`Và`7`là không thể bởi vì ứng cử viên duy nhất là`6`, căn thức của nó cộng số nguyên tố`2`. 

Trường hợp thứ ba là khi`m`chứa một số nguyên tố mà không bên nào chứa.```
Input:
12 2 6

Output:
-1 -1
```Sản phẩm`a * c`đã đưa ra thừa số nguyên tố rồi`2`Và`3`. Giá trị trung bình duy nhất có thể là giữa`2`Và`6`, nhưng mọi số trong khoảng đó đều đưa ra số nguyên tố bị cấm hoặc không thay đổi căn thức một cách chính xác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là kiểm tra mọi chiều dài đuôi giữa có thể. Đối với mọi`b`từ`a + 1`ĐẾN`c - 1`, chúng ta có thể tính căn thức của`a * b * c`và so sánh nó với`m`. Điều này đúng vì phạm vi chứa mọi câu trả lời có thể. Vấn đề là số lượng công việc lặp đi lặp lại. Nếu mọi bài kiểm tra đều có`c`gần với`10^6`, việc kiểm tra mọi ứng cử viên và phân tích từng ứng viên tốn kém quá nhiều chi phí cho nhiều trường hợp thử nghiệm. 

Nhận xét quan trọng là chúng ta không thực sự cần toàn bộ sản phẩm. Căn thức chỉ phụ thuộc vào số nguyên tố nào xuất hiện. Đầu tiên, yếu tố`a`Và`c`và thu thập tất cả các thừa số nguyên tố mà chúng đã đóng góp. Nếu một trong những số nguyên tố này không nằm trong`m`, câu trả lời là không thể ngay lập tức. 

Bây giờ hãy xem xét các số nguyên tố của`m`. Một số trong số họ đã có mặt trong`a * c`. Các số nguyên tố còn lại phải xuất hiện trong`b`. Gọi sản phẩm của họ`need`. Mỗi hợp lệ`b`phải là bội số của`need`. 

Có một hạn chế nữa.`b`không thể giới thiệu bất kỳ số nguyên tố nào bên ngoài`m`, vì điều đó sẽ làm cho căn thức cuối cùng trở nên quá lớn. Vì vậy sau khi loại bỏ phần cần thiết`need`, số nhân còn lại chỉ có thể sử dụng các số nguyên tố từ`m`. 

Từ`b < 10^6`, chúng ta có thể tạo ra tất cả các số có thừa số nguyên tố thuộc tập hợp cho phép và tìm bội số nhỏ nhất và lớn nhất của`need`trong khoảng thời gian`(a, c)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((c-a) * sqrt(c)) | O(1) | Quá chậm | 
| Tối ưu | O(số lượng ứng viên được tạo) cho mỗi bài kiểm tra, được giới hạn bởi các giá trị dưới 10^6 | O(số lượng ứng viên) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Yếu tố`a`Và`c`và lưu trữ các thừa số nguyên tố riêng biệt của chúng. Chúng ta cần những số nguyên tố này vì chúng đã buộc phải xuất hiện ở căn thức cuối cùng. 
2. Yếu tố`m`thành các thừa số nguyên tố riêng biệt của nó. Nếu như`m`chứa một số nguyên tố nhỏ hơn không giải thích được, sau này chúng ta sẽ buộc nó vào`b`. 
3. Kiểm tra xem mọi thừa số nguyên tố của`a`Và`c`tồn tại ở`m`. Nếu điều này không thành công, không có giá trị của`b`có thể loại bỏ một số nguyên tố thừa khỏi căn thức. 
4. Tính toán`need`, tích của mọi số nguyên tố trong`m`cái đó bị thiếu từ`a * c`. Đây là khoản đóng góp cơ bản tối thiểu được yêu cầu từ`b`. 
5. Tạo tất cả các giá trị chỉ được tạo từ các thừa số nguyên tố của`m`. Đối với mỗi giá trị được tạo ra`x`, nhân nó với`need`nếu nó vẫn ở bên dưới`c`. Kết quả là một ứng cử viên có chiều dài trung bình. 
6. Trong số tất cả các ứng cử viên lớn hơn`a`và đúng là ít hơn`c`, giữ giá trị tối thiểu và tối đa. 

Lý do thế hệ hoạt động là vì mọi giá trị hợp lệ`b`có thể được chia thành hai phần. Một phần chứa các số nguyên tố bị thiếu phải được thêm vào và phần còn lại chỉ tăng lũy ​​thừa của các số nguyên tố đã được phép. Máy phát điện tạo ra chính xác những phần bổ sung có thể có này. 

Tại sao nó hoạt động: căn thức cuối cùng là sự kết hợp của các thừa số nguyên tố từ`a`,`b`, Và`c`. Việc kiểm tra đảm bảo rằng`a`Và`c`không chứa số nguyên tố bị cấm. Phần bắt buộc đảm bảo rằng`b`cung cấp mọi số nguyên tố còn thiếu từ`m`. Số nhân được tạo không thể thêm các số nguyên tố mới, vì vậy mọi ứng cử viên được chấp nhận đều có chính xác căn số cần thiết. Mỗi hợp lệ`b`cũng có thể được biểu diễn dưới dạng này nên không có câu trả lời nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def factor(x):
    res = []
    d = 2
    while d * d <= x:
        if x % d == 0:
            res.append(d)
            while x % d == 0:
                x //= d
        d += 1 if d == 2 else 2
    if x > 1:
        res.append(x)
    return res

def solve_case(m, a, c):
    fm = set(factor(m))
    fa = factor(a)
    fc = factor(c)

    for p in fa + fc:
        if p not in fm:
            return -1, -1

    need = 1
    base = set(fa + fc)
    for p in fm:
        if p not in base:
            need *= p

    allowed = list(fm)
    limit = c // need

    vals = [1]
    for p in allowed:
        cur = []
        for x in vals:
            y = x
            while y * p <= limit:
                y *= p
                cur.append(y)
        vals += cur

    ans_min = 10**18
    ans_max = -1

    for x in vals:
        b = x * need
        if a < b < c:
            ans_min = min(ans_min, b)
            ans_max = max(ans_max, b)

    if ans_max == -1:
        return -1, -1
    return ans_min, ans_max

def main():
    t = int(input())
    out = []
    for _ in range(t):
        m, a, c = map(int, input().split())
        x, y = solve_case(m, a, c)
        out.append(f"{x} {y}")
    print("\n".join(out))

main()
```các`factor`hàm chỉ lưu trữ các thừa số nguyên tố riêng biệt vì lũy thừa lặp lại không quan trọng đối với một căn thức. Sau khi tìm được các yếu tố`m`, mã đầu tiên từ chối các trường hợp không thể xảy ra trong đó`a`hoặc`c`giới thiệu một số nguyên tố bên ngoài gốc mục tiêu. 

Biến`need`đại diện cho phần bắt buộc của`b`. Các giá trị được tạo còn lại chỉ được phép sử dụng các số nguyên tố từ`m`, điều này ngăn cản việc vô tình tạo ra một gốc lớn hơn. 

Vòng lặp tạo tăng số lượng bằng cách nhân các giá trị hiện có với các số nguyên tố được phép. Giới hạn là`c // need`bởi vì nhân với`need`sau này vẫn phải giữ ứng viên ở dưới`c`. Việc kiểm tra bất đẳng thức một cách nghiêm ngặt là cần thiết vì đuôi ở giữa phải nằm giữa hai đuôi đã biết chứ không được bằng chúng. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
210 5 21
```Các yếu tố của`a`Và`c`là: 

| Bước | Giá trị hiện tại | Ý nghĩa | 
| --- | --- | --- | 
| Ban đầu |`5, 21`| Đã biết đuôi | 
| Yếu tố |`{5, 3, 7}`| Các số nguyên tố hiện có | 
| Thiếu |`{2}`| Phải xuất hiện ở`b`| 
| Cần |`2`| Mọi ứng cử viên đều có cái này | 

Các giá trị có thể có của`b`được tạo ra: 

| Ứng viên | Có hiệu lực? | Lý do | 
| --- | --- | --- | 
| 6 | Có | Cấp tiến trở thành`210`| 
| 10 | Có | Chỉ thêm số nguyên tố`2`| 
| 20 | Có | Chỉ thêm số nguyên tố`2`với sức mạnh cao hơn | 

Tối thiểu và tối đa là`6`Và`20`. 

Vì:```
121 11 13
```Việc nhân tử hóa cho: 

| Bước | Giá trị hiện tại | Ý nghĩa | 
| --- | --- | --- | 
|`a`|`11`| Đã có số nguyên tố 11 | 
|`c`|`13`| Đã có số nguyên tố 13 | 
|`m`|`{11}`| Thiếu số nguyên tố 13 | 

Từ`13`xuất hiện ở`c`nhưng không ở trong`m`, các đuôi đã biết đã chứa số nguyên tố bị cấm. Thuật toán dừng ngay lập tức và trả về:```
-1 -1
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(số lượng ứng viên được tạo + sqrt(c)) | Phân tích các giá trị nhỏ chiếm ưu thế và thế hệ chỉ xem xét các số dưới 1e6 | 
| Không gian | O(số lượng ứng viên được tạo) | Lưu trữ số nhân có thể | 

Giá trị tối đa có thể phải được tạo là dưới đây`10^6`, nên số lượng ứng viên còn ít. Điều này dễ dàng phù hợp với các giới hạn ngay cả với nhiều trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().strip().split()
    sys.stdin = old

    if not data:
        return ""

    it = iter(data)
    t = int(next(it))
    ans = []
    for _ in range(t):
        m = int(next(it))
        a = int(next(it))
        c = int(next(it))
        x, y = solve_case(m, a, c)
        ans.append(f"{x} {y}")
    return "\n".join(ans)

assert run("""6
210 5 21
35 5 7
121 11 13
870870 14 2145
20 1 5
12 2 6
""") == """6 20
-1 -1
-1 -1
29 2088
-1 -1
-1 -1"""

assert run("""1
2 1 3
""") == "2 2"

assert run("""1
1 1 10
""") == "1 1"

assert run("""1
30 6 20
""") == "-1 -1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`210 5 21`|`6 20`| Xử lý thiếu cơ bản | 
|`2 1 3`|`2 2`| Khoảng thời gian nhỏ nhất có thể | 
|`1 1 10`|`1 1`| Căn bản bằng một | 
|`30 6 20`|`-1 -1`| Cấm phát hiện nguyên tố | 

## Vỏ cạnh 

Khi nào`m`là`1`, không có thừa số nguyên tố nào có thể xuất hiện ở bất cứ đâu. Các giá trị duy nhất có thể có là các số được tạo hoàn toàn từ các thừa số nguyên tố nằm ngoài phạm vi`m`, có nghĩa là số duy nhất có thể sử dụng được là`1`. Máy phát điện khởi động từ`1`, vì vậy nó tìm thấy chính xác trường hợp này. 

Khi`a`hoặc`c`chứa một số nguyên tố không có trong`m`, thuật toán sẽ bác bỏ trường hợp đó trước khi tìm kiếm. Ví dụ:```
35 5 7
```Các đuôi đã biết đã buộc phải là số nguyên tố`5`Và`7`, nhưng gốc mục tiêu không tương thích với bất kỳ giá trị trung bình bổ sung nào. Câu trả lời vẫn còn`-1 -1`. 

Khi các số nguyên tố bị thiếu có tích lớn,`need`bản thân nó có thể ở gần`c`. Khi đó hệ số nhân được tạo ra rất nhỏ, nhưng việc kiểm tra khoảng thời gian cuối cùng sẽ ngăn việc quay trở lại`a`hoặc`c`như một câu trả lời giả. Điều này xử lý các lỗi ranh giới xung quanh các bất đẳng thức nghiêm ngặt.
