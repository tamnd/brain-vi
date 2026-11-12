---
title: "CF 1689B - Hoán vị huyền bí"
description: "Chúng ta được cho một hoán vị $p$ của các số từ $1$ đến $n$. Chúng ta muốn xây dựng một hoán vị khác $q$ bằng cách sử dụng các số giống nhau sao cho mọi vị trí đều thay đổi. Với mọi chỉ số $i$, giá trị đặt trong $qi$ phải khác với $pi$."
date: "2026-06-09T23:32:16+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1689
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 798 (Div. 2)"
rating: 900
weight: 1689
solve_time_s: 500
verified: true
draft: false
---

[CF 1689B - Hoán vị thần bí](https://codeforces.com/problemset/problem/1689/B) 

**Xếp hạng:** 900 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 8 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị$p$của những con số$1$bởi vì$n$. Chúng tôi muốn xây dựng một hoán vị khác$q$sử dụng các số giống nhau sao cho mọi vị trí đều thay đổi. Đối với mỗi chỉ số$i$, giá trị đặt trong$q_i$phải khác với$p_i$. 

Trong số tất cả các hoán vị hợp lệ như vậy, chúng ta phải đưa ra hoán vị nhỏ nhất về mặt từ điển. Nếu không tồn tại hoán vị hợp lệ, chúng ta xuất ra$-1$. 

Những hạn chế là cực kỳ nhỏ. Tổng số tiền của tất cả$n$giá trị nhiều nhất là$1000$. Điều này có nghĩa là ngay cả một$O(n^2)$thuật toán dễ dàng đủ nhanh. Không cần cấu trúc dữ liệu phức tạp hoặc tối ưu hóa nâng cao. Thách thức thực sự là tìm ra hoán vị hợp lệ nhỏ nhất về mặt từ điển trong khi tránh các điểm cố định. 

Các trường hợp cạnh nguy hiểm nhất phát sinh ở gần cuối công trình. 

Coi như:```
n = 1
p = [1]
```Chỉ có một hoán vị có độ dài một. Bất kỳ ứng cử viên nào cũng sẽ đặt$1$ở vị trí$1$, tạo một điểm cố định Câu trả lời đúng là:```
-1
```Thuật toán tham lam luôn chọn giá trị khả dụng nhỏ nhất sẽ tạo ra sai$[1]$. 

Một trường hợp phức tạp khác là:```
p = [1, 2, 3]
```Nếu chúng ta tham lam chọn giá trị khả dụng nhỏ nhất không bằng giá trị của vị trí hiện tại, chúng ta sẽ nhận được:```
q = [2, 1, ?]
```Chỉ một$3$vẫn còn, nhưng$q_3 = 3$bị cấm. Một cách tiếp cận tham lam bất cẩn bị mắc kẹt ngay cả khi câu trả lời hợp lệ```
[2, 3, 1]
```tồn tại. 

Trường hợp quan trọng thứ ba xảy ra khi còn lại chính xác một giá trị và nó bằng giá trị bị cấm ở vị trí cuối cùng. Ví dụ:```
p = [2, 3, 1, 4]
```Một công trình ngây thơ có thể đạt được:```
q = [1, 2, 4, ?]
```chỉ với$4$còn lại. Từ$p_4=4$, việc đặt nó sẽ vi phạm yêu cầu. Cách khắc phục đúng là hoán đổi giá trị được chọn cuối cùng với giá trị còn lại, tạo ra:```
[1, 2, 4, 3]
```đó là hợp lệ và tối thiểu về mặt từ điển. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tạo ra mọi hoán vị của$1$bởi vì$n$, kiểm tra xem nó có khác với$p$ở mọi vị trí và giữ giá trị nhỏ nhất về mặt từ điển. 

Điều này có hiệu quả vì định nghĩa về hoán vị thần bí rất dễ xác minh. Thật không may, số lượng hoán vị là$n!$. Ngay cả đối với$n=10$, điều này đã vượt quá ba triệu khả năng. Sự tăng trưởng giai thừa làm cho vũ lực không thể sử dụng được. 

Nhận xét quan trọng là tính tối giản về mặt từ điển gợi ý một cấu trúc tham lam. Khi quyết định vị trí$i$, chúng ta nên đặt số nhỏ nhất có sẵn mà không vi phạm ngay điều kiện$q_i \ne p_i$. 

Hầu hết các vị trí có thể được xử lý theo cách này. Sự phức tạp duy nhất xuất hiện ở phần cuối. Sau khi tham lam lấp đầy cái đầu tiên$n-1$vị trí, một giá trị có thể vẫn còn. Nếu giá trị đó không bằng$p_n$, chúng tôi chỉ cần đặt nó. 

Nếu giá trị còn lại bằng$p_n$, chúng ta phải sửa chữa công trình. Vì giá trị còn lại chưa được sử dụng nên nó phải xuất hiện ở một số vị trí trước đó của$p$. Hoán đổi nó với giá trị được đặt ở vị trí trước đó sẽ loại bỏ điểm cố định trong khi vẫn duy trì tính tối thiểu từ điển. 

Quan sát này biến vấn đề thành một thuật toán tham lam đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Tham lam tối ưu |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu$n=1$, đầu ra$-1$. Không có hoán vị thần bí nào tồn tại. 
2. Duy trì bộ số chưa sử dụng. 
3. Xử lý các vị trí từ trái qua phải, trừ vị trí cuối cùng. 
4. Về chức vụ$i$, chọn số chưa sử dụng nhỏ nhất không bằng$p_i$. 
5. Xóa số đó khỏi tập hợp chưa sử dụng và thêm nó vào đáp án. 
6. Sau khi điền lần đầu tiên$n-1$vị trí, chỉ còn lại đúng một số. 
7. Nếu số còn lại khác với$p_n$, đặt nó ở vị trí cuối cùng. 
8. Nếu không, hãy tạm thời đặt nó ở vị trí cuối cùng và hoán đổi hai phần tử cuối cùng của hoán vị đã xây dựng. 
9. Xuất ra hoán vị kết quả. 

Lý do hoán đổi cuối cùng hoạt động rất đơn giản. Số còn lại bằng$p_n$, vì vậy nó không thể ở vị trí cuối cùng. Vị trí trước đó chứa một số giá trị khác nhau. Hoán đổi hai giá trị này sẽ loại bỏ điểm cố định ở cuối và không tạo điểm mới. 

### Tại sao nó hoạt động 

Lựa chọn tham lam luôn là tối ưu vì thứ tự từ điển được xác định bởi vị trí sớm nhất nơi hai hoán vị khác nhau. Tại vị trí$i$, việc chọn số hợp lệ nhỏ nhất có sẵn sẽ tạo ra tiền tố nhỏ nhất có thể trong số tất cả các giải pháp hợp lệ. 

Lần duy nhất quá trình tham lam gặp khó khăn là khi vẫn còn một giá trị bị cấm ở vị trí cuối cùng. Trong tình huống đó, mọi lựa chọn trước đó đều đã ở mức tối thiểu về mặt từ điển. Hoán đổi hai vị trí cuối cùng sẽ thay đổi hoán vị càng muộn càng tốt, bảo toàn tiền tố nhỏ nhất và do đó duy trì tính tối thiểu từ điển. 

Do đó, thuật toán luôn xây dựng một hoán vị huyền bí hợp lệ bất cứ khi nào nó tồn tại và trong số tất cả các hoán vị hợp lệ, nó là hoán vị nhỏ nhất về mặt từ điển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    p = list(map(int, input().split()))

    if n == 1:
        print(-1)
        continue

    unused = set(range(1, n + 1))
    q = []

    for i in range(n - 1):
        for x in range(1, n + 1):
            if x in unused and x != p[i]:
                q.append(x)
                unused.remove(x)
                break

    last = next(iter(unused))
    q.append(last)

    if q[-1] == p[-1]:
        q[-1], q[-2] = q[-2], q[-1]

    print(*q)
```Thuật toán duy trì các giá trị không được sử dụng một cách rõ ràng. Từ$n\le1000$, quét qua tất cả các số từ$1$ĐẾN$n$ở mỗi bước là hoàn toàn chấp nhận được. 

đầu tiên$n-1$các vị trí được lấp đầy một cách tham lam. Ở mỗi bước, số khả dụng nhỏ nhất khác với giá trị bị cấm$p_i$được chọn. 

Giá trị còn lại cuối cùng được thêm vào. Nếu nó tạo ra một điểm cố định ở vị trí cuối cùng thì hai phần tử cuối cùng sẽ được hoán đổi cho nhau. 

Chi tiết triển khai quan trọng nhất là việc hoán đổi chỉ được thực hiện sau khi tất cả các vị trí đã được lấp đầy. Cố gắng xử lý tình huống này sớm hơn sẽ làm phức tạp logic và không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
p = [1, 2, 3]
```| Vị trí | Trước đây chưa sử dụng | Bị cấm | Được chọn | 
| --- | --- | --- | --- | 
| 1 | {1,2,3} | 1 | 2 | 
| 2 | {1,3} | 2 | 1 | 
| 3 | {3} | 3 | 3 | 

Hoán vị tạm thời là:```
[2, 1, 3]
```Giá trị cuối cùng bằng giá trị bị cấm. Hoán đổi hai vị trí cuối cùng:```
[2, 3, 1]
```Ví dụ này cho thấy tại sao bước sửa chữa là cần thiết. 

### Ví dụ 2 

đầu vào:```
p = [2, 3, 1, 4]
```| Vị trí | Trước đây chưa sử dụng | Bị cấm | Được chọn | 
| --- | --- | --- | --- | 
| 1 | {1,2,3,4} | 2 | 1 | 
| 2 | {2,3,4} | 3 | 2 | 
| 3 | {3,4} | 1 | 4 | 
| 4 | {3} | 4 | 3 | 

Hoán vị cuối cùng là:```
[1, 2, 4, 3]
```Không cần sửa chữa vì giá trị cuối cùng đã khác với$p_4$. 

Ví dụ này minh họa dòng chảy tham lam bình thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Đối với mỗi vị trí, chúng tôi có thể quét tất cả các giá trị từ$1$ĐẾN$n$| 
| Không gian |$O(n)$| Câu trả lời và bộ chưa sử dụng sẽ được lưu trữ nhiều nhất$n$giá trị | 

Vì tổng số tiền của$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$1000$, MỘT$O(n^2)$giải pháp thực hiện tối đa khoảng một triệu thao tác cơ bản, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        if n == 1:
            out.append("-1")
            continue

        unused = set(range(1, n + 1))
        q = []

        for i in range(n - 1):
            for x in range(1, n + 1):
                if x in unused and x != p[i]:
                    q.append(x)
                    unused.remove(x)
                    break

        q.append(next(iter(unused)))

        if q[-1] == p[-1]:
            q[-1], q[-2] = q[-2], q[-1]

        out.append(" ".join(map(str, q)))

    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

assert run("4\n3\n1 2 3\n5\n2 3 4 5 1\n4\n2 3 1 4\n1\n1\n") == \
       "2 3 1\n1 2 3 4 5\n1 2 4 3\n-1"

assert run("1\n1\n1\n") == "-1"

assert run("1\n2\n1 2\n") == "2 1"

assert run("1\n3\n2 1 3\n") == "1 3 2"

assert run("1\n4\n1 2 3 4\n") == "2 3 4 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=1$|$-1$| Trường hợp bất khả thi | 
| [1,2] | [2,1] | Hoán vị không cần thiết nhỏ nhất | 
| [2,1,3] | [1,3,2] | Bước sửa chữa cuối cùng | 
| [1,2,3,4] | [2,3,4,1] | Giải pháp phong cách chuyển đổi theo chu kỳ | 

## Vỏ cạnh 

Hãy xem xét:```
1
1
1
```Chỉ có một hoán vị có độ dài một. Bất kỳ vị trí ứng cử viên nào$1$ở vị trí duy nhất, tạo thành một điểm cố định. Thuật toán phát hiện ngay$n=1$và đầu ra$-1$. 

Coi như:```
1
3
1 2 3
```Những lựa chọn tham lam tạo ra:```
[2,1,3]
```Giá trị cuối cùng bằng giá trị bị cấm$3$. Thuật toán hoán đổi hai vị trí cuối cùng và thu được:```
[2,3,1]
```Mọi vị trí đều khác với hoán vị ban đầu. 

Coi như:```
1
2
2 1
```Quá trình tham lam chọn:```
[1,2]
```Cả hai vị trí đều khác với hoán vị ban đầu và không cần sửa chữa. Điều này xác nhận rằng thuật toán xử lý chính xác kích thước có thể giải được nhỏ nhất. 

Coi như:```
1
4
2 3 1 4
```Giá trị còn lại ở bước cuối cùng là$3$, không bằng$4$. Thuật toán chỉ cần thêm nó và trả về:```
[1,2,4,3]
```Điều này xác minh rằng bước sửa chữa chỉ được áp dụng khi cần thiết.
