---
title: "CF 1896A - Hoán đổi lởm chởm"
description: "Chúng ta được cung cấp một hoán vị và một thao tác hoán đổi rất cụ thể. Hoán đổi chỉ được phép ở vị trí i nơi phần tử đạt mức tối đa cục bộ. Nói cách khác, a[i] phải lớn hơn cả hai lân cận của nó."
date: "2026-06-08T21:37:07+07:00"
tags: ["codeforces", "competitive-programming", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1896
codeforces_index: "A"
codeforces_contest_name: "CodeTON Round 7 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 800
weight: 1896
solve_time_s: 109
verified: true
draft: false
---

[CF 1896A - Hoán đổi lởm chởm](https://codeforces.com/problemset/problem/1896/A) 

**Đánh giá:** 800 
**Thẻ:** sắp xếp 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hoán vị và một thao tác hoán đổi rất cụ thể. 

Hoán đổi chỉ được phép ở một vị trí`i`trong đó phần tử là cực đại cục bộ. Nói cách khác,`a[i]`phải lớn hơn cả hai hàng xóm của nó. Khi một vị trí như vậy tồn tại, chúng tôi có thể trao đổi`a[i]`với phần tử ngay bên phải của nó. 

Nhiệm vụ không thực sự là sắp xếp hoán vị. Chúng ta chỉ cần xác định liệu một số chuỗi phép toán hợp lệ cuối cùng có thể chuyển đổi hoán vị thành thứ tự tăng dần hay không. 

Những hạn chế nhỏ đến mức đáng ngạc nhiên. Độ dài hoán vị tối đa là 10 và có tối đa 5000 trường hợp thử nghiệm. Thực sự có thể thực hiện được một tìm kiếm mạnh mẽ trên tất cả các trạng thái có thể truy cập được vì không gian trạng thái được giới hạn bởi`10!`, nhưng số tiền đó vẫn lớn hơn nhiều so với mức cần thiết. Vấn đề được thiết kế xoay quanh một quan sát đơn giản về những gì hoạt động có thể và không thể thay đổi. 

Cái bẫy chính là giả định rằng các phần tử có thể di chuyển tự do thông qua các lần hoán đổi lặp đi lặp lại. Các hoạt động là vô cùng hạn chế. 

Coi như:```
3
3 1 2
```Phần tử đầu tiên là`3`. Không có phép toán hợp lệ nào liên quan đến vị trí 1 vì phép toán chỉ cho phép các chỉ số từ 2 đến`n-1`. Hoán vị không bao giờ có thể được sắp xếp, vì vậy câu trả lời là`NO`. 

Một trường hợp dễ bỏ sót khác là:```
5
1 3 2 5 4
```Điều này trông chỉ được sắp xếp một phần, nhưng cả hai`3`Và`5`là cực đại địa phương. Việc thực hiện các giao dịch hoán đổi được phép mang lại:```
1 3 2 5 4
→ 1 2 3 5 4
→ 1 2 3 4 5
```Câu trả lời là`YES`. 

Trường hợp cạnh quan trọng nhất là khi phần tử nhỏ nhất chưa ở phía trước. 

Ví dụ:```
5
2 1 3 4 5
```Hoán vị gần như đã được sắp xếp, nhưng phần tử`1`không bao giờ có thể di chuyển đến vị trí 1. Câu trả lời là`NO`. 

Hiểu lý do tại sao điều này xảy ra sẽ dẫn trực tiếp đến giải pháp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi mọi hoán vị là một trạng thái và áp dụng nhiều lần mọi thao tác hợp lệ. Chúng tôi có thể thực hiện tìm kiếm biểu đồ và kiểm tra xem hoán vị đã sắp xếp có thể truy cập được hay không. 

Điều này hoạt động vì`n ≤ 10`, nhưng nó vẫn không cần thiết. Trong trường hợp xấu nhất có tới`10! = 3,628,800`tiểu bang. Mặc dù nhiều trường hợp không thể truy cập được nhưng việc khám phá các trạng thái cho hàng nghìn trường hợp kiểm thử sẽ là quá mức. 

Quan sát quan trọng đến từ việc kiểm tra xem điều gì xảy ra với phần tử đầu tiên. 

Mọi thao tác được phép đều sử dụng một chỉ mục giữa`2`Và`n-1`và hoán đổi vị trí`i`Và`i+1`. 

Chức vụ`1`không bao giờ được chạm tới. 

Điều đó có nghĩa là phần tử đầu tiên của hoán vị không bao giờ thay đổi. 

Phần tử đầu tiên phải là gì trong một hoán vị được sắp xếp? Vì mảng là một hoán vị của`1..n`, hoán vị được sắp xếp luôn bắt đầu bằng`1`. 

Vì vậy, nếu phần tử đầu tiên không`1`, việc sắp xếp là không thể. 

Điều đáng ngạc nhiên là nếu phần tử đầu tiên đã bằng`1`, việc sắp xếp luôn luôn có thể thực hiện được. 

Tại sao? Mọi phần tử lớn hơn`1`cuối cùng có thể được di chuyển sang phải thông qua một chuỗi các giao dịch hoán đổi tối đa cục bộ hợp lệ. Quan sát giải pháp chính thức là có`1`cố định ở phía trước vừa cần vừa đủ. 

Toàn bộ vấn đề giảm xuống việc kiểm tra một giá trị duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số trạng thái có thể truy cập) | O(số trạng thái có thể truy cập) | Không cần thiết | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị. 
2. Kiểm tra phần tử đầu tiên. 
3. Nếu phần tử đầu tiên bằng`1`, in`"YES"`. 

Một hoán vị được sắp xếp phải bắt đầu bằng`1`, và khi nào`1`đã được cố định ở phía trước, các thao tác được phép là đủ để sắp xếp các phần tử còn lại. 
4. Nếu không thì in`"NO"`. 

Vị trí đầu tiên không bao giờ có thể tham gia vào bất kỳ hoạt động nào, vì vậy một giá trị khác với`1`không bao giờ có thể được thay thế bởi`1`. 

### Tại sao nó hoạt động 

Điều bất biến là vị trí 1 không bao giờ thay đổi. 

Mọi thao tác đều hoán đổi vị trí`i`Và`i+1`Ở đâu`2 ≤ i ≤ n-1`. Vì vị trí 1 không bao giờ được tham gia nên phần tử đầu tiên không đổi trong toàn bộ quá trình. 

Một hoán vị được sắp xếp của`1..n`phải bắt đầu bằng`1`. Nếu phần tử đầu tiên không`1`, đạt đến hoán vị được sắp xếp là không thể. 

Ngược lại, nếu phần tử đầu tiên là`1`, các phần tử còn lại luôn có thể được sắp xếp lại theo thứ tự đã sắp xếp bằng cách sử dụng các hoán đổi tối đa cục bộ được phép. Thực tế này là quan sát cốt lõi đằng sau vấn đề và làm cho điều kiện vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    if a[0] == 1:
        print("YES")
    else:
        print("NO")
```Việc thực hiện tuân theo bằng chứng trực tiếp. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi đọc hoán vị và chỉ kiểm tra phần tử đầu tiên của nó. Không cần mô phỏng. 

Một lỗi phổ biến là cố gắng lập mô hình hoán đổi một cách rõ ràng. Hoạt động này trông giống như một quá trình sắp xếp, nhưng bất biến ở vị trí đầu tiên hoàn toàn xác định câu trả lời. 

Một lỗi khác có thể xảy ra là kiểm tra xem hoán vị đã được sắp xếp chưa. Một hoán vị như:```
1 3 2
```ban đầu không được sắp xếp, tuy nhiên câu trả lời vẫn là`"YES"`bởi vì thao tác được phép có thể hoàn thành việc sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hoán vị đầu vào:```
1 3 2 5 4
```| Bước | Yếu tố đầu tiên | Quyết định | 
| --- | --- | --- | 
| Đọc mảng | 1 | Kiểm tra tình trạng | 
| Tình trạng | 1 == 1 | CÓ | 

Đầu ra:```
YES
```Ví dụ này chứng tỏ sự đủ của điều kiện. Mảng không được sắp xếp ban đầu, nhưng có`1`cố định ở phía trước đảm bảo rằng việc sắp xếp có thể đạt được. 

### Ví dụ 2 

Hoán vị đầu vào:```
5 1 2 3 4
```| Bước | Yếu tố đầu tiên | Quyết định | 
| --- | --- | --- | 
| Đọc mảng | 5 | Kiểm tra tình trạng | 
| Tình trạng | 5 != 1 | KHÔNG | 

Đầu ra:```
NO
```Ví dụ này thể hiện tính bất biến. Vì vị trí 1 không bao giờ thay đổi nên giá trị`5`không bao giờ có thể rời khỏi vị trí đầu tiên, khiến cho việc hoán vị được sắp xếp đầy đủ là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ phần tử đầu tiên được kiểm tra | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Ngay cả với 5000 trường hợp thử nghiệm, tổng công việc cũng không đáng kể. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ans.append("YES" if a[0] == 1 else "NO")

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

# provided sample
assert run(
"""6
3
1 2 3
5
1 3 2 5 4
5
5 4 3 2 1
3
3 1 2
4
2 3 1 4
5
5 1 2 3 4
"""
) == "YES\nYES\nNO\nNO\nNO\nNO"

# minimum size, already sorted
assert run(
"""1
3
1 2 3
"""
) == "YES"

# minimum size, impossible
assert run(
"""1
3
2 1 3
"""
) == "NO"

# n = 10, first element is 1
assert run(
"""1
10
1 10 9 8 7 6 5 4 3 2
"""
) == "YES"

# n = 10, first element not 1
assert run(
"""1
10
10 1 2 3 4 5 6 7 8 9
"""
) == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 3`| CÓ | Hoán vị được sắp xếp hợp lệ tối thiểu | 
|`2 1 3`| KHÔNG | Trường hợp nhỏ nhất không thể | 
|`1 10 9 8 7 6 5 4 3 2`| CÓ | Kích thước lớn, tình trạng còn đủ | 
|`10 1 2 3 4 5 6 7 8 9`| KHÔNG | Kích thước lớn, bất biến vị trí đầu tiên | 

## Vỏ cạnh 

Hãy xem xét:```
1
3
2 1 3
```Thuật toán kiểm tra phần tử đầu tiên và tìm thấy`2`. Vì vị trí 1 không bao giờ có thể tham gia hoán đổi,`2`sẽ ở đó mãi mãi. Đầu ra là:```
NO
```Bây giờ hãy xem xét:```
1
5
1 5 4 3 2
```Thuật toán thấy rằng phần tử đầu tiên đã có`1`và in:```
YES
```Mặc dù phần còn lại của hoán vị không được sắp xếp nhiều, nhưng định lý đằng sau lời giải đảm bảo rằng các phép toán được phép có thể sắp xếp nó. 

Cuối cùng, hãy xem xét trường hợp đã được sắp xếp:```
1
4
1 2 3 4
```Phần tử đầu tiên là`1`, do đó thuật toán đưa ra:```
YES
```Không cần thực hiện thao tác nào và điều kiện vẫn xác định chính xác câu trả lời.
