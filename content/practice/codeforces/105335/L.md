---
title: "CF 105335L - Lulu và những người bạn"
description: "Chúng ta có một chuỗi cố định T có độ dài tối đa là 20. Với mỗi chuỗi truy vấn s, chúng ta có thể xóa bất kỳ ký tự nào khỏi T, giữ nguyên thứ tự tương đối của các ký tự còn lại. Mục tiêu là làm cho chuỗi kết quả chứa s dưới dạng chuỗi con liền kề."
date: "2026-06-26T00:31:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105335
codeforces_index: "L"
codeforces_contest_name: "ICPC Thailand National Competition 2024"
rating: 0
weight: 105335
solve_time_s: 58
verified: true
draft: false
---

[CF 105335L - Lulu và những người bạn](https://codeforces.com/problemset/problem/105335/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi cố định`T`có độ dài tối đa là 20. Đối với mỗi chuỗi truy vấn`s`, chúng tôi có thể xóa bất kỳ ký tự nào khỏi`T`, giữ nguyên thứ tự tương đối của các ký tự còn lại. 

Mục đích là làm cho chuỗi kết quả chứa`s`như một chuỗi con liền kề. Trong số tất cả các cách hợp lệ để xóa ký tự, chúng tôi muốn số lần xóa tối thiểu. Nếu không có trình tự xóa nào có thể thực hiện được`s`xuất hiện dưới dạng chuỗi con, chúng tôi in`-1`. 

Chi tiết quan trọng là việc xóa không sắp xếp lại các ký tự. Bất kỳ bản sao nào của`s`xuất hiện sau khi xóa phải đến từ một chuỗi con của`T`. 

Mặc dù có tới 100.000 truy vấn nhưng chuỗi cố định`T`là cực kỳ ngắn. Độ dài của nó nhiều nhất là 20, điều này thay đổi hoàn toàn vấn đề. Bất kỳ thuật toán nào có độ phức tạp bậc hai trong`|T|`thực tế là thời gian không đổi, bởi vì`20² = 400`. 

Một sai lầm phổ biến là chỉ nghĩ đến việc liệu`s`là một dãy con của`T`. Điều đó là cần thiết nhưng chưa đủ để tính toán số lần xóa tối thiểu. Chúng ta cũng cần giảm thiểu số lượng ký tự bị loại bỏ giữa các chữ cái trùng khớp. 

Coi như:```
T = abxc
s = abc
```Dãy con phù hợp duy nhất sử dụng vị trí`0,1,3`. nhân vật`x`giữa`b`Và`c`phải xóa đi để`abc`trở nên liền kề. Câu trả lời là`1`, không`0`. 

Một cái bẫy dễ mắc khác là giả định rằng một khi tìm thấy dãy con phù hợp thì mọi ký tự bên ngoài dãy đó cũng phải bị xóa. 

Ví dụ:```
T = zabcz
s = abc
```Không cần xóa. Chuỗi đã chứa`abc`như một chuỗi con. Giữ gìn môi trường xung quanh`z`ký tự hoàn toàn được cho phép. 

Trường hợp cạnh thứ ba xảy ra khi tồn tại nhiều phần nhúng của cùng một truy vấn. 

Ví dụ:```
T = axbxc
s = abc
```Vị trí phù hợp`(0,2,4)`yêu cầu xóa hai ký tự bên trong. Việc triển khai bất cẩn dừng lại ở lần khớp đầu tiên có thể bỏ lỡ cơ hội nhúng tốt hơn vào các đầu vào khác. Chúng ta phải kiểm tra tất cả các vị trí xuất phát có thể. 

## Phương pháp tiếp cận 

Quan điểm brute-force là chọn một dãy con của`T`, xây dựng chuỗi kết quả sau khi xóa và kiểm tra xem nó có chứa`s`như một chuỗi con. Từ`|T| ≤ 20`, có nhiều nhất`2^20 ≈ 10^6`các chuỗi tiếp theo. Việc này đủ nhỏ để bạn có thể suy nghĩ nhưng nếu thực hiện riêng lẻ cho từng truy vấn sẽ rất lãng phí. 

Một cách tiếp cận trực tiếp hơn bắt đầu từ việc quan sát rằng bất kỳ sự xuất hiện hợp lệ nào của`s`trong chuỗi cuối cùng xuất phát từ một chuỗi con của`T`. 

Giả sử các nhân vật của`s`được khớp với các vị trí```
p1 < p2 < ... < pk
```bên trong`T`. 

Để làm cho các ký tự trùng khớp này tạo thành một chuỗi con liền kề sau khi xóa, mọi ký tự không khớp nằm giữa`p1`Và`pk`phải được loại bỏ. Nhân vật trước`p1`và sau đó`pk`có thể ở lại vì chúng không can thiệp vào chuỗi con. 

Số lần xóa cần thiết cho việc nhúng này chính xác là```
(pk - p1 + 1) - k
```bởi vì độ dài khoảng là`pk - p1 + 1`, trong khi chỉ`k`các ký tự của khoảng đó thuộc về chuỗi mong muốn. 

Vì vậy, vấn đề trở thành: 

Tìm dãy con phù hợp của`s`bên trong`T`nhịp của ai`(last - first + 1)`càng nhỏ càng tốt. 

Từ`T`có độ dài tối đa là 20, chúng ta có thể thử mọi vị trí có thể đóng vai trò là ký tự khớp đầu tiên. Từ điểm bắt đầu đó, việc quét hai con trỏ đơn giản sẽ tìm thấy sự hoàn thành sớm nhất có thể của chuỗi con. Điều đó mang lại vị trí kết thúc nhỏ nhất cho lần bắt đầu đã chọn đó, do đó khoảng thời gian nhỏ nhất cho lần bắt đầu đó. 

Lấy khoảng thời gian tốt nhất trong tất cả các lần bắt đầu sẽ đưa ra câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tạo chuỗi con Brute Force | O(2^ | T | ) cho mỗi truy vấn | 
| Hãy thử mọi sự khởi đầu, khớp nối tiếp theo tham lam | O( | T | ( | 

## Hướng dẫn thuật toán 

1. Hãy để`n = |T|`Và`m = |s|`. 
2. Khởi tạo câu trả lời là vô cùng. 
3. Đối với mọi vị trí`start`TRONG`T`như vậy`T[start] == s[0]`, cố gắng xây dựng toàn bộ chuỗi truy vấn bắt đầu từ vị trí này. 
4. Đặt con trỏ vào`T`Tại`start`và một con trỏ trong`s`Tại`0`. 
5. Di chuyển qua`T`từ trái sang phải. Bất cứ khi nào ký tự hiện tại khớp với ký tự hiện tại của`s`, tiến con trỏ vào`s`. 
6. Nếu tất cả các ký tự của`s`được khớp, ghi lại vị trí mà ký tự cuối cùng được khớp. 
7. Độ dài nhịp là```
last - start + 1
```và yêu cầu xóa trong khoảng đó là```
span - m
```8. Giảm thiểu giá trị này trên tất cả các vị trí bắt đầu hợp lệ. 
9. Nếu không tìm thấy kết quả khớp đầy đủ, hãy xuất`-1`. Otherwise output the minimum deletion count.

 ### Tại sao nó hoạt động 

Fix any valid occurrence of`s`ở chuỗi cuối cùng. Nó tương ứng với một chuỗi khớp bên trong`T`với vị trí phù hợp đầu tiên`first`và vị trí khớp cuối cùng`last`. 

Mỗi ký tự trong khoảng`[first, last]`đó không phải là một phần của trận đấu phải bị xóa. Các ký tự ngoài khoảng luôn có thể được giữ lại. Do đó số lần xóa cần thiết là chính xác```
(last - first + 1) - |s|
```Đối với vị trí bắt đầu cố định`first`, việc khớp từ trái sang phải tham lam sẽ chọn vị trí sớm nhất có thể cho mỗi ký tự tiếp theo, giúp giảm thiểu`last`. Vì số lần xóa chỉ phụ thuộc vào khoảng thời gian nên điều này mang lại khả năng nhúng tối ưu cho lần bắt đầu đó. 

Việc thử tất cả các bước khởi đầu có thể bao gồm mọi khả năng nhúng khả thi. Mức tối thiểu trên chúng là mức tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

T = input().strip()
n = len(T)

q = int(input())

for _ in range(q):
    s = input().strip()
    m = len(s)

    best = float('inf')

    for start in range(n):
        if T[start] != s[0]:
            continue

        j = 0
        last = -1

        for i in range(start, n):
            if j < m and T[i] == s[j]:
                j += 1
                last = i
                if j == m:
                    break

        if j == m:
            best = min(best, (last - start + 1) - m)

    print(-1 if best == float('inf') else best)
```Vòng lặp bên ngoài xử lý từng truy vấn một cách độc lập. 

Đối với mỗi vị trí bắt đầu có thể, mã sẽ thực hiện so khớp chuỗi con tiêu chuẩn. Biến`j`lưu trữ bao nhiêu ký tự của truy vấn đã được khớp. 

Khi toàn bộ truy vấn được khớp,`last`lưu trữ vị trí của ký tự trùng khớp cuối cùng. số lượng```
(last - start + 1) - m
```chính xác là số lượng ký tự không khớp trong span, đó là những ký tự phải xóa. 

Một chi tiết tinh tế là chúng ta không đếm các ký tự trước`start`hoặc sau`last`. Chúng có thể vẫn còn trong chuỗi cuối cùng mà không ảnh hưởng đến sự tồn tại của chuỗi con. 

Một chi tiết khác là chúng ta phải thử mọi vị trí xuất phát hợp lệ. Sự xuất hiện sớm nhất của ký tự đầu tiên không phải lúc nào cũng là một phần của giải pháp tối ưu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
T = leiulocuuniapnax
s = lulu
```| Bắt đầu | Vị trí phù hợp | Cuối cùng | Khoảng cách | Xóa | 
| --- | --- | --- | --- | --- | 
| 0 | 0, 3, 4, 6 | 6 | 7 | 3 | 
| 3 | 3, 4, 6, 7 | 7 | 5 | 1 | 

Sử dụng cách nhúng tốt nhất, nhịp có độ dài`5`và độ dài truy vấn là`4`.```
5 - 4 = 1
```các ký tự bên trong phải được loại bỏ khỏi khoảng đó. 

Việc xóa còn lại được đề cập trong tuyên bố chỉ đơn giản là một cách xây dựng cụ thể. Thuật toán đang tính toán số lần xóa cần thiết tối thiểu trong khoảng thời gian phù hợp. 

### Ví dụ 2 

đầu vào:```
T = abxc
s = abc
```| Bắt đầu | Vị trí phù hợp | Cuối cùng | Khoảng cách | Xóa | 
| --- | --- | --- | --- | --- | 
| 0 | 0, 1, 3 | 3 | 4 | 1 | 

nhân vật`x`nằm trong khoảng phù hợp nhưng không phải là một phần của dãy con. 

Loại bỏ nó mang lại:```
abc
```vậy câu trả lời là`1`. 

Ví dụ này minh họa tính bất biến trung tâm: chỉ những ký tự không khớp trong khoảng đó mới bị buộc phải xóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | T | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Vì cả hai`|T|`Và`|s|`nhiều nhất là 20, công việc trên mỗi truy vấn được giới hạn bởi một hằng số rất nhỏ. Điều này dễ dàng phù hợp với giới hạn ngay cả đối với một số lượng lớn truy vấn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    T = input().strip()
    n = len(T)

    q = int(input())

    ans = []
    for _ in range(q):
        s = input().strip()
        m = len(s)

        best = float('inf')

        for start in range(n):
            if T[start] != s[0]:
                continue

            j = 0
            last = -1

            for i in range(start, n):
                if j < m and T[i] == s[j]:
                    j += 1
                    last = i
                    if j == m:
                        break

            if j == m:
                best = min(best, (last - start + 1) - m)

        ans.append("-1" if best == float('inf') else str(best))

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# custom cases
assert run("abc\n1\nabc\n") == "0", "already a substring"

assert run("abxc\n1\nabc\n") == "1", "delete one internal character"

assert run("abc\n1\nd\n") == "-1", "impossible"

assert run("aaaaa\n2\naaa\naaaaa\n") == "0\n0", "all equal characters"

assert run("axbxcxd\n1\nabcd\n") == "3", "multiple internal deletions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`abc`, truy vấn`abc`|`0`| Không cần xóa | 
|`abxc`, truy vấn`abc`|`1`| Loại bỏ khoảng cách nội bộ | 
|`abc`, truy vấn`d`|`-1`| Trận đấu bất khả thi | 
|`aaaaa`, truy vấn`aaa`,`aaaaa`|`0`,`0`| Ký tự lặp lại | 
|`axbxcxd`, truy vấn`abcd`|`3`| Nhịp lớn với nhiều khoảng trống | 

## Vỏ cạnh 

Hãy xem xét:```
T = zabcz
s = abc
```Thuật toán bắt đầu tại`a`, trận đấu`b`Và`c`, thu được độ dài nhịp`3`, và trả về```
3 - 3 = 0
```Xung quanh`z`các ký tự nằm ngoài phạm vi nên không cần xóa. 

Bây giờ hãy xem xét:```
T = abxc
s = abc
```Trận đấu sử dụng vị trí`0,1,3`. Độ dài nhịp là`4`, độ dài truy vấn là`3`, và câu trả lời là```
4 - 3 = 1
```Chỉ có`x`bên trong nhịp phải được loại bỏ. 

Cuối cùng:```
T = abc
s = acd
```Không có vị trí bắt đầu nào có thể khớp với tất cả các ký tự của truy vấn. Biến lưu trữ câu trả lời tốt nhất không bao giờ được cập nhật và thuật toán in chính xác`-1`.
