---
title: "CF 1950E - Chuỗi con lặp lại gần như ngắn nhất"
description: "Chúng ta được cho một chuỗi s có độ dài n. Chúng ta muốn tìm độ dài L nhỏ nhất có thể sao cho tồn tại một chuỗi mẫu k có độ dài L, được lặp lại chính xác n/L lần, tạo ra một chuỗi c có độ dài n và c khác với s ở nhiều nhất một vị trí."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "number-theory", "strings"]
categories: ["algorithms"]
codeforces_contest: 1950
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 937 (Div. 4)"
rating: 1500
weight: 1950
solve_time_s: 82
verified: true
draft: false
---

[CF 1950E - Chuỗi con lặp lại gần như ngắn nhất](https://codeforces.com/problemset/problem/1950/E) 

**Đánh giá:** 1500 
**Tags:** vũ lực, thực hiện, lý thuyết số, dây 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi`s`chiều dài`n`. Chúng tôi muốn tìm độ dài nhỏ nhất có thể`L`sao cho tồn tại một chuỗi mẫu`k`chiều dài`L`, lặp lại chính xác`n / L`lần, tạo ra một chuỗi`c`chiều dài`n`, Và`c`khác với`s`ở nhiều nhất một vị trí. 

Nói cách khác, sau khi thay đổi nhiều nhất một ký tự của`s`, chuỗi kết quả phải trở thành tuần hoàn hoàn hảo với độ dài khoảng thời gian`L`. Trong số tất cả các khoảng thời gian hợp lệ, chúng tôi cần khoảng thời gian ngắn nhất. 

Nhận xét quan trọng từ câu lệnh là chuỗi lặp lại phải bao phủ toàn bộ độ dài chuỗi, do đó độ dài khoảng thời gian ứng cử viên phải chia`n`. 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`. Điều này đủ lớn để việc so sánh mọi mẫu có thể với mọi chuỗi con có thể một cách ngây thơ có thể trở nên tốn kém. Một thuật toán xung quanh`O(n · d(n))`, Ở đâu`d(n)`là số ước, hoàn toàn an toàn vì ngay cả những số lớn nhất bên dưới`2 · 10^5`có tương đối ít ước số. MỘT`O(n²)`Cách tiếp cận quá chậm trong trường hợp xấu nhất. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
n = 4
s = abaa
```Câu trả lời là`1`, bởi vì`"aaaa"`khác nhau ở đúng một vị trí. Một giải pháp chỉ kiểm tra xem chuỗi gốc đã định kỳ hay chưa sẽ từ chối dấu chấm một cách không chính xác`1`. 

Coi như:```
n = 4
s = abba
```Câu trả lời là`4`. Cả hai thời kỳ`1`cũng không phải thời kỳ`2`có thể được sửa bằng một thay đổi ký tự. Một giải pháp tham lam chọn ký tự thường xuyên nhất trong mỗi loại dư lượng có thể kết luận sai khoảng thời gian đó`2`hoạt động. 

Coi như:```
n = 6
s = aaabaa
```Câu trả lời là`1`. Chuỗi này khác với`"aaaaaa"`ở đúng một vị trí. Trường hợp này cho thấy một ký tự xấu có thể xuất hiện ở bất cứ đâu, không nhất thiết phải ở bên trong khối dấu chấm đầu tiên. 

Một trường hợp tế nhị khác là:```
n = 8
s = ababcbab
```Giai đoạn`2`không hoạt động. Sự không khớp xuất hiện trong một số so sánh liên quan đến cùng một ký tự sai. Việc đếm những bất đồng theo cặp thay vì đếm những thay đổi về ký tự thực tế có thể đánh giá quá cao những sửa đổi cần thiết. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực tự nhiên là thử mọi ước số`L`của`n`, tạo ra mọi chuỗi thời gian có thể có độ dài`L`và kiểm tra xem việc lặp lại nó có khớp không`s`có nhiều nhất một điểm không khớp. 

Vấn đề là có rất nhiều chuỗi thời gian có thể xảy ra theo cấp số nhân. Thậm chí hạn chế chúng ta đối với các chuỗi dấu chấm xuất hiện bên trong`s`không giúp ích ngay lập tức. Không gian tìm kiếm trở nên khổng lồ. 

Chúng ta cần khai thác thực tế là chỉ có một ký tự có thể sai. 

Giả sử chúng ta ấn định độ dài khoảng thời gian ứng viên`L`. Mỗi vị trí`i`thuộc lớp dư lượng`i mod L`. Trong một chuỗi tuần hoàn hoàn hảo, tất cả các ký tự trong cùng một lớp dư lượng phải bằng nhau. 

Ví dụ, với`L = 3`:```
positions: 0 1 2 3 4 5 6 7 8
residues : 0 1 2 0 1 2 0 1 2
```Mỗi lớp dư lượng phải chứa một ký tự lặp lại. 

Nếu toàn bộ chuỗi có thể được sửa chữa với nhiều nhất một sửa đổi, thì sau khi nhóm các vị trí theo lớp dư lượng, tất cả trừ một vị trí có thể đã đồng ý với mẫu tuần hoàn cuối cùng. 

Quan sát quan trọng là nếu sai nhiều nhất một ký tự thì chuỗi dấu chấm đúng phải bằng một trong các khối độ dài hiện có`L`. 

Tại sao? 

Giả sử chuỗi sử dụng định kỳ cuối cùng`k`. Vì chỉ có một vị trí trong toàn bộ chuỗi sai nên mọi khối ngoại trừ một khối đều giống hệt với`k`. Do đó, một số khối của chuỗi gốc phải bằng`k`. 

Điều này ngay lập tức làm giảm không gian tìm kiếm. Đối với số chia cố định`L`, chúng ta chỉ cần kiểm tra các ứng viên được lấy từ các khối có độ dài hiện có`L`. 

có`n / L`khối. Việc kiểm tra tất cả chúng một cách đơn giản sẽ quá tốn kém, nhưng vì chỉ một khối có thể chứa ký tự sai nên chỉ cần kiểm tra hai khối đầu tiên là đủ. 

Nếu tồn tại một khoảng thời gian hợp lệ thì tất cả các khối đúng đều bằng khoảng thời gian thực. Nhiều nhất một khối khác nhau. Trong số hai khối đầu tiên, ít nhất một khối phải đúng. 

Vậy với mỗi ước số`L`, chúng tôi kiểm tra:```
candidate = first block
candidate = second block (if it exists)
```Đối với mỗi ứng cử viên, chúng tôi so sánh mẫu lặp lại với toàn bộ chuỗi và đếm các mẫu không khớp. Nếu số lượng không khớp nhiều nhất là một thì`L`là hợp lệ. 

Điều này biến vấn đề thành việc kiểm tra một số lượng nhỏ các ứng cử viên cho mỗi ước số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ trong khoảng thời gian | O(L) | Quá chậm | 
| Tối ưu | O(n · d(n)) | O(L) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Liệt kê tất cả các ước số`L`của`n`theo thứ tự tăng dần. 
2. Với mỗi ước số, chia chuỗi thành các khối có độ dài`L`. 
3. Xây dựng giai đoạn ứng cử viên bằng khối đầu tiên. 
4. So sánh ứng cử viên được lặp lại với toàn bộ chuỗi và đếm các kết quả không khớp. 
5. Nếu số lượng không khớp nhiều nhất là một, hãy quay lại`L`ngay lập tức vì các ước số được xử lý theo thứ tự tăng dần. 
6. Mặt khác, nếu có nhiều hơn một khối, hãy tạo ứng cử viên thứ hai bằng cách sử dụng khối thứ hai. 
7. So sánh ứng cử viên thứ hai này với toàn bộ chuỗi. 
8. Nếu số lượng không khớp nhiều nhất là một, hãy quay lại`L`. 
9. Tiếp tục với số chia tiếp theo. 
10. Nếu không có ước số nhỏ hơn nào hoạt động,`L = n`luôn hoạt động vì chính chuỗi đó có thể được sử dụng làm dấu chấm. 

### Tại sao nó hoạt động 

Giả sử một số ước số`L`là hợp lệ. Cho phép`k`là chu kỳ thực sự của chuỗi được sửa chữa. 

Vì chỉ có một ký tự trong toàn bộ chuỗi có thể sai nên tối đa một khối độ dài`L`khác với`k`. Mọi khối khác đều chính xác bằng`k`. 

Trong số hai khối đầu tiên, ít nhất một khối không thể là khối bị hỏng. Do đó ít nhất một trong số chúng bằng chu kỳ thực`k`. 

Khi thuật toán kiểm tra khối đó dưới dạng ứng cử viên, ứng cử viên được lặp lại chính xác là chuỗi đã sửa chữa. Số lượng không khớp của nó so với chuỗi gốc nhiều nhất là một, do đó thuật toán chấp nhận`L`. 

Ngược lại, nếu thuật toán chấp nhận một ứng cử viên thì việc lặp lại ứng cử viên đó sẽ tạo ra một chuỗi khác với`s`ở nhiều nhất một vị trí. Đây chính xác là điều kiện mà bài toán yêu cầu. 

Vì các ước số được xử lý theo thứ tự tăng dần nên ước số được chấp nhận đầu tiên là câu trả lời tối thiểu có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def works(s, n, d, pat):
    mism = 0
    for i in range(n):
        if s[i] != pat[i % d]:
            mism += 1
            if mism > 1:
                return False
    return True

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        s = input().strip()

        divisors = []
        i = 1
        while i * i <= n:
            if n % i == 0:
                divisors.append(i)
                if i * i != n:
                    divisors.append(n // i)
            i += 1

        divisors.sort()

        answer = n

        for d in divisors:
            block_cnt = n // d

            cand1 = s[:d]
            if works(s, n, d, cand1):
                answer = d
                break

            if block_cnt > 1:
                cand2 = s[d:2 * d]
                if works(s, n, d, cand2):
                    answer = d
                    break

        print(answer)

solve()
```Chức năng trợ giúp`works`kiểm tra xem một khoảng thời gian ứng viên có thể tạo ra một chuỗi khác với`s`ở nhiều nhất một vị trí. Thay vì xây dựng chuỗi lặp lại một cách rõ ràng, nó so sánh ký tự`s[i]`với`pat[i % d]`. Điều này tránh thêm bộ nhớ và giữ cho việc kiểm tra tuyến tính. 

Việc tạo số chia sử dụng phép liệt kê căn bậc hai tiêu chuẩn. Mỗi cặp ước số được cộng lại với nhau, sau đó danh sách được sắp xếp sao cho các khoảng thời gian được kiểm tra từ nhỏ nhất đến lớn nhất. 

Với mỗi ước số, thuật toán chỉ kiểm tra hai khối đầu tiên. Đây là sự tối ưu hóa quan trọng. Đối số chính xác cho thấy rằng nếu tồn tại một khoảng thời gian hợp lệ thì một trong hai khối này phải bằng khoảng thời gian thực. 

Lối ra sớm bên trong`works`là quan trọng. Khi có nhiều hơn một điểm không khớp xuất hiện, ứng cử viên đó không bao giờ có thể trở thành hợp lệ, do đó không có lý do gì để tiếp tục quét chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
abaa
```Các ước số ứng cử viên là`1, 2, 4`. 

| d | Ứng viên | Không khớp | hợp lệ | 
| --- | --- | --- | --- | 
| 1 | một | 1 | Có | 

Chuỗi lặp lại là`"aaaa"`. Nó khác với`"abaa"`chỉ ở vị trí 2, vì vậy câu trả lời là`1`. 

Ví dụ này chứng minh tại sao chúng ta phải cho phép một sửa đổi. Chuỗi ban đầu không định kỳ với dấu chấm`1`, nhưng nó trở nên tuần hoàn sau khi sửa một ký tự đơn. 

### Ví dụ 2 

đầu vào:```
8
hshahaha
```Số chia là`1, 2, 4, 8`. 

| d | Ứng viên | Không khớp | hợp lệ | 
| --- | --- | --- | --- | 
| 1 | h | 3 | Không | 
| 2 | hs | 4 | Không | 
| 2 | ha | 1 | Có | 

Khối thứ hai`"ha"`là đúng thời hạn. 

Lặp đi lặp lại nó mang lại:```
hahahaha
```khác với chuỗi gốc ở đúng một vị trí. 

Dấu vết này minh họa tại sao chỉ kiểm tra khối đầu tiên là không đủ. Ký tự bị lỗi có thể nằm bên trong khối đầu tiên, làm cho khối thứ hai trở thành dấu chấm chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · d(n)) | Mỗi số chia thực hiện tối đa hai lần quét toàn bộ chuỗi | 
| Không gian | O(d(n)) | Lưu trữ danh sách số chia | 

Đối với mỗi ước số, chúng tôi kiểm tra tối đa hai ứng cử viên và mỗi bài kiểm tra sẽ quét chuỗi một lần. Số ước của bất kỳ số nguyên nào lên đến`2 · 10^5`nhỏ nên tổng công vẫn nằm trong giới hạn. Việc sử dụng bộ nhớ không đáng kể so với 256 MB có sẵn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    def works(s, n, d, pat):
        mism = 0
        for i in range(n):
            if s[i] != pat[i % d]:
                mism += 1
                if mism > 1:
                    return False
        return True

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        s = input().strip()

        divs = []
        i = 1
        while i * i <= n:
            if n % i == 0:
                divs.append(i)
                if i * i != n:
                    divs.append(n // i)
            i += 1

        divs.sort()

        for d in divs:
            if works(s, n, d, s[:d]):
                ans.append(str(d))
                break

            if n // d > 1 and works(s, n, d, s[d:2*d]):
                ans.append(str(d))
                break

    print("\n".join(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old
    return out.getvalue()

# provided sample
assert run(
"""5
4
abaa
4
abba
13
slavicgslavic
8
hshahaha
20
stormflamestornflame
"""
) == """1
4
13
2
10
"""

# minimum size
assert run(
"""1
1
a
"""
) == """1
"""

# all equal
assert run(
"""1
6
aaaaaa
"""
) == """1
"""

# one wrong character in otherwise period-1 string
assert run(
"""1
6
aaabaa
"""
) == """1
"""

# first block corrupted, second block is correct
assert run(
"""1
8
hshahaha
"""
) == """2
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, s=a`|`1`| Đầu vào nhỏ nhất có thể | 
|`aaaaaa`|`1`| Đã hoàn toàn định kỳ | 
|`aaabaa`|`1`| Ký tự đơn không chính xác ở bất kỳ đâu trong chuỗi | 
|`hshahaha`|`2`| Khoảng thời gian đúng đến từ khối thứ hai | 

## Vỏ cạnh 

Hãy xem xét:```
1
4
abaa
```Đối với số chia`1`, thời gian ứng cử viên là`"a"`. Lặp đi lặp lại nó tạo ra`"aaaa"`. Số lượng không khớp chính xác là một. Thuật toán ngay lập tức trở lại`1`. 

Điều này xác minh rằng chúng tôi xử lý chính xác các chuỗi trở thành định kỳ sau một lần sửa đổi. 

Coi như:```
1
4
abba
```Đối với số chia`1`, số lượng không khớp vượt quá một. Đối với số chia`2`, cả hai ứng cử viên`"ab"`Và`"ba"`tạo ra nhiều hơn một sự không phù hợp. Chỉ có số chia`4`thành công. 

Thuật toán trả về`4`, chứng tỏ rằng nó không nhầm lẫn chấp nhận một khoảng thời gian ngắn hơn đơn giản chỉ vì một số lớp cặn trông giống nhau. 

Coi như:```
1
8
hshahaha
```Khối đầu tiên là`"hs"`và khối thứ hai là`"ha"`. Chu kỳ thực sự là`"ha"`, nhưng khối đầu tiên chứa ký tự bị hỏng. Chỉ kiểm tra khối đầu tiên sẽ thất bại. Thuật toán cũng kiểm tra khối thứ hai, chỉ tìm thấy một khối không khớp và trả về chính xác`2`. 

Đây chính xác là tình huống thúc đẩy việc kiểm tra cả hai khối đầu tiên. 

Coi như:```
1
6
aaabaa
```Đối với số chia`1`, chuỗi lặp lại`"aaaaaa"`khác nhau ở đúng một vị trí. Thuật toán tính các điểm không khớp trên toàn cầu thay vì tính các điểm bất đồng giữa các lớp dư lượng, do đó câu trả lời được báo cáo chính xác là`1`. Điều này tránh việc đếm quá mức ảnh hưởng của một nhân vật xấu.
