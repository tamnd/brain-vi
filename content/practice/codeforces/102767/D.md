---
title: "CF 102767D - Singhal và hoán vị"
description: "Chúng tôi có nhiều tập hợp số và chúng tôi muốn đếm xem có thể tạo ra bao nhiêu mảng khác nhau bằng cách sắp xếp lại các số đó sao cho mảng có một đỉnh duy nhất. Đỉnh là giá trị lớn nhất trong mảng."
date: "2026-07-28T23:30:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102767
codeforces_index: "D"
codeforces_contest_name: "Codedigger Training Contest -Number Theory"
rating: 0
weight: 102767
solve_time_s: 78
verified: true
draft: false
---

[CF 102767D - Singhal và Hoán vị](https://codeforces.com/problemset/problem/102767/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có nhiều tập hợp số và chúng tôi muốn đếm xem có thể tạo ra bao nhiêu mảng khác nhau bằng cách sắp xếp lại các số đó sao cho mảng có một đỉnh duy nhất. Đỉnh là giá trị lớn nhất trong mảng. Mọi thứ trước mức tối đa này phải tăng một cách nghiêm ngặt và mọi thứ sau mức tối đa này phải giảm đi một cách nghiêm ngặt. 

Ví dụ: nếu nhiều tập hợp là`{1, 2, 3, 1}`, giá trị lớn nhất là`3`, do đó hình dạng duy nhất có thể có là một chuỗi tăng dần nào đó kết thúc tại`3`, theo sau là dãy giảm dần. Mảng`(1,2,3,1)`hoạt động vì bên trái phát triển nghiêm ngặt và bên phải giảm nghiêm ngặt. 

Đầu vào cung cấp kích thước của mảng và các giá trị phải được sắp xếp lại. Đầu ra là số mảng riêng biệt thỏa mãn điều kiện cực đại. 

Hạn chế chính là số lượng phần tử có thể đủ lớn để không thể thử mọi hoán vị. Với`n`các yếu tố, lực lượng vũ phu sẽ kiểm tra lên đến`n!`sắp xếp, trở nên không thể sử dụng được ngay cả đối với các giá trị rất nhỏ của`n`. Giải pháp cần kiểm tra tần số của các giá trị và đếm trực tiếp các công trình hợp lệ. 

Phần khó khăn là xử lý các giá trị trùng lặp. Một giá trị nhỏ hơn giá trị tối đa có thể xuất hiện ở bên trái, bên phải hoặc cả hai, nhưng mỗi bên phải hoàn toàn đơn điệu, do đó không bên nào có thể chứa cùng một giá trị hai lần. Bản thân giá trị cực đại là đặc biệt vì nó phải là đỉnh duy nhất. 

Hãy xem xét nhiều bộ:```
3
1 2 2
```Đầu ra đúng là:```
0
```Một cách tiếp cận bất cẩn có thể khiến người ta`2`là đỉnh cao và cái khác`2`ở một bên. Tuy nhiên, hoặc`(1,2,2)`hoặc`(2,2,1)`chứa các giá trị cực đại liền kề bằng nhau nên không có đỉnh duy nhất. 

Một trường hợp cạnh khác là giá trị xuất hiện dưới mức tối đa ba lần:```
5
1 2 2 2 3
```Đầu ra đúng là:```
0
```giá trị`2`sẽ cần phải được phân bổ giữa hai phía của đỉnh. Vì mỗi bên có thể chứa nó nhiều nhất một lần nên không thể đặt ba bản sao. 

Trường hợp ranh giới cuối cùng là khi tất cả các giá trị không tối đa là duy nhất:```
4
1 2 3 4
```Đầu ra đúng là:```
8
```Mỗi trong số`1`,`2`, Và`3`có thể được gán độc lập cho phía tăng hoặc phía giảm. Mỗi phép gán tạo ra một mảng hợp lệ khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi hoán vị của nhiều tập hợp đầu vào và kiểm tra xem nó có tuyệt vời hay không. Đối với một hoán vị ứng cử viên, chúng ta có thể tìm vị trí tối đa và xác minh rằng các giá trị tăng trước nó và giảm sau nó. Điều này đếm chính xác tất cả các câu trả lời vì mọi sự sắp xếp có thể đều được kiểm tra. 

Vấn đề là số lượng hoán vị. Với`n`có những giá trị riêng biệt`n!`những sắp xếp có thể. Thậm chí`10!`đã có hàng triệu khả năng, và sự tăng trưởng giai thừa nhanh chóng khiến điều này trở nên không thể. 

Cấu trúc của một mảng tuyệt vời mang lại một cách suy nghĩ nhỏ hơn nhiều về vấn đề. Sau khi cố định giá trị lớn nhất ở giữa, các giá trị còn lại chỉ cần gán cho một trong hai bên. Phía bên trái sẽ tự động được sắp xếp theo thứ tự tăng dần và phía bên phải sẽ tự động được sắp xếp theo thứ tự giảm dần. 

Đối với giá trị nhỏ hơn giá trị tối đa, quy tắc sắp xếp nghiêm ngặt có nghĩa là mỗi bên có thể chứa giá trị này nhiều nhất một lần. Nếu một giá trị xuất hiện một lần, chúng ta có thể chọn một trong hai bên. Nếu xuất hiện hai lần thì mỗi bên phải giữ một bản. Nếu nó xuất hiện nhiều hơn hai lần thì không có vị trí hợp lệ. 

Giá trị tối đa không thể xuất hiện nhiều lần. Bản sao thứ hai sẽ phải được đặt ở một trong hai mặt, nhưng nó sẽ tạo ra một giá trị tối đa khác và vi phạm cấu trúc đỉnh đơn. 

Điều này làm giảm vấn đề từ tạo hoán vị đến đếm tần số. Chúng ta chỉ cần đếm xem có bao nhiêu giá trị xuất hiện chính xác một lần dưới mức tối đa, bởi vì mỗi giá trị đó đóng góp hai lựa chọn độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của mọi giá trị trong mảng. 

Thông tin tần số là đủ vì thứ tự cuối cùng của mỗi bên là bắt buộc. Chúng ta chỉ cần biết mỗi giá trị có thể được phân phối theo bao nhiêu cách. 
2. Tìm giá trị lớn nhất và kiểm tra tần số của nó. 

Nếu mức tối đa xuất hiện nhiều lần thì câu trả lời là 0 vì một mảng tuyệt vời yêu cầu một đỉnh duy nhất. 
3. Kiểm tra mọi giá trị nhỏ hơn giá trị tối đa. 

Nếu một giá trị xuất hiện nhiều hơn hai lần thì câu trả lời là 0 vì hai mặt đơn điệu không thể chứa ba bản sao có cùng giá trị. 

Nếu một giá trị xuất hiện chính xác hai lần thì nó phải xuất hiện một lần ở mỗi bên. Điều này tạo ra không có sự lựa chọn. 

Nếu một giá trị xuất hiện chính xác một lần, nó có thể được đặt trước mức tối đa hoặc sau mức tối đa. Nhân câu trả lời với hai cho giá trị này. 
4. Xuất ra số lượng lựa chọn tích lũy. 

Số lượng các lựa chọn luôn là lũy thừa của hai vì mỗi giá trị linh hoạt đều đóng góp một quyết định nhị phân độc lập. 

Tại sao nó hoạt động: 

Phần tử tối đa cố định tâm của mọi mảng hợp lệ. Sau khi chọn giá trị nhỏ hơn thuộc về bên trái và bên phải, thứ tự bắt buộc vì bên trái phải tăng và bên phải phải giảm. Quy tắc tần số mô tả chính xác khi nào một giá trị có thể được đặt ở các cạnh đó. Mỗi mảng tuyệt vời hợp lệ tương ứng với một phép gán giá trị duy nhất cho các cạnh và mỗi phép gán hợp lệ sẽ tạo ra một mảng tuyệt vời, do đó việc đếm là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1

        mx = max(a)

        if freq[mx] > 1:
            ans.append("0")
            continue

        ways = 1
        possible = True

        for x, c in freq.items():
            if x == mx:
                continue
            if c > 2:
                possible = False
                break
            if c == 1:
                ways *= 2

        ans.append(str(ways if possible else 0))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Từ điển`freq`lưu trữ bao nhiêu bản sao của mỗi giá trị tồn tại. Điều này tránh tùy thuộc vào thứ tự của mảng ban đầu vì chỉ có nhiều tập hợp mới quan trọng. 

Giá trị tối đa được kiểm tra đầu tiên. Điều này xử lý yêu cầu cao nhất đặc biệt trước khi xử lý các giá trị nhỏ hơn. 

Đối với các giá trị nhỏ hơn, tần số bằng hai có nghĩa là giá trị phải xuất hiện một lần ở cả hai phía của giá trị lớn nhất, trong khi tần số bằng một sẽ cho hai vị trí có thể có. Phép nhân chỉ xảy ra đối với những lần xuất hiện đơn lẻ này. 

Số nguyên Python không bị tràn nên việc nhân đôi lặp lại là an toàn. Việc lặp lại bản đồ tần số cũng giữ cho giải pháp tuyến tính về số lượng giá trị đầu vào. 

## Ví dụ đã hoạt động 

Ví dụ 1: 

đầu vào:```
4
1 2 3 4
```| Bước | Giá trị đã được kiểm tra | Tần số | Cách hiện tại | 
| --- | --- | --- | --- | 
| Ban đầu | Tối đa = 4 | 1 | 1 | 
| Kiểm tra 1 | 1 | 1 | 2 | 
| Kiểm tra 2 | 2 | 1 | 4 | 
| Kiểm tra 3 | 3 | 1 | 8 | 

Ba giá trị nhỏ hơn đều là duy nhất, vì vậy mỗi giá trị có thể di chuyển độc lập sang hai bên của mức tối đa. Câu trả lời là:```
8
```Ví dụ 2: 

đầu vào:```
5
1 2 2 3 3
```| Bước | Giá trị đã được kiểm tra | Tần số | Cách hiện tại | 
| --- | --- | --- | --- | 
| Ban đầu | Tối đa = 3 | 2 | 0 | 

Mức tối đa xuất hiện hai lần, do đó không tồn tại mảng tuyệt vời hợp lệ. Câu trả lời là:```
0
```Dấu vết này cho thấy tại sao việc kiểm tra tối đa phải diễn ra trước khi đếm các lựa chọn. Không thể sửa chữa mức tối đa trùng lặp bằng cách phân bổ nó cho cả hai bên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số được chèn vào bản đồ tần số một lần và mỗi giá trị riêng biệt được kiểm tra một lần. | 
| Không gian | O(n) | Bản đồ tần số có thể chứa mọi giá trị đầu vào riêng biệt. | 

Tổng số phần tử trong các trường hợp thử nghiệm đủ nhỏ để có một giải pháp tuyến tính. Thuật toán tránh hoàn toàn việc tạo hoán vị, đây là cách thực tế duy nhất để xử lý các đầu vào lớn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1

        mx = max(a)

        if freq[mx] > 1:
            out.append("0")
            continue

        ways = 1
        ok = True

        for x, c in freq.items():
            if x == mx:
                continue
            if c > 2:
                ok = False
                break
            if c == 1:
                ways *= 2

        out.append(str(ways if ok else 0))

    return "\n".join(out)

assert solve("""5
4
1 2 3 4
3
1 2 2
5
1 2 2 2 3
5
1 2 2 3 3
3
5 4 3
""") == """8
0
0
0
4"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 3 4`|`8`| Tất cả các giá trị là duy nhất và mỗi giá trị nhỏ hơn sẽ tạo ra một sự lựa chọn. | 
|`1 2 2`|`0`| Xung đột bên tối đa trùng lặp. | 
|`1 2 2 2 3`|`0`| Nhiều hơn hai bản sao của một giá trị không tối đa. | 
|`1 2 2 3 3`|`0`| Xử lý tối đa lặp đi lặp lại. | 
|`5 4 3`|`4`| Tối đa ở cuối và tất cả các giá trị nhỏ hơn duy nhất. | 

## Vỏ cạnh 

Đối với đầu vào:```
3
1 2 2
```tần số của giá trị tối đa`2`là hai. Thuật toán ngay lập tức trả về số 0. Điều này phù hợp với yêu cầu rằng một mảng tuyệt vời có một đỉnh chứ không phải nhiều lần xuất hiện tối đa. 

Đối với đầu vào:```
5
1 2 2 2 3
```mức tối đa là duy nhất, nhưng giá trị`2`xuất hiện ba lần. Thuật toán phát hiện rằng hai bên không đủ để phân tách tất cả các bản sao trong khi vẫn giữ cả hai bên được sắp xếp nghiêm ngặt nên trả về 0. 

Đối với đầu vào:```
4
1 2 3 4
```giá trị tối đa là duy nhất và mọi giá trị nhỏ hơn xuất hiện một lần. Thuật toán nhân đôi số lượng ba lần, tạo ra`2 * 2 * 2 = 8`. Mỗi kết quả tương ứng với việc chọn cạnh của đỉnh cho mỗi giá trị nhỏ hơn và sau đó thứ tự được xác định tự động.
