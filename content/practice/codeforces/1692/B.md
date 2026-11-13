---
title: "CF 1692B - Tất cả khác biệt"
description: "Chúng tôi được cung cấp một mảng các số nguyên. Trong một thao tác, chúng ta phải loại bỏ chính xác hai phần tử khỏi mảng. Hai phần tử bị loại bỏ có thể là bất kỳ giá trị nào, miễn là chúng đến từ các vị trí khác nhau."
date: "2026-06-09T23:01:11+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1692
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 799 (Div. 4)"
rating: 800
weight: 1692
solve_time_s: 139
verified: true
draft: false
---

[CF 1692B - All Distinct](https://codeforces.com/problemset/problem/1692/B) 

**Đánh giá:** 800 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Trong một thao tác, chúng ta phải loại bỏ chính xác hai phần tử khỏi mảng. Hai phần tử bị loại bỏ có thể là bất kỳ giá trị nào, miễn là chúng đến từ các vị trí khác nhau. 

Mục tiêu của chúng tôi là thực hiện không hoặc nhiều thao tác như vậy để mảng còn lại chỉ chứa các giá trị riêng biệt. Trong số tất cả các cách để đạt được điều đó, chúng tôi muốn mảng còn lại càng dài càng tốt. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải xuất ra độ dài tối đa có thể có của một mảng cuối cùng có các phần tử khác nhau. 

Những hạn chế là rất nhỏ. Mỗi mảng chứa tối đa 50 phần tử và có tối đa 1000 trường hợp thử nghiệm. Ngay cả những cách tiếp cận tương đối kém hiệu quả cũng sẽ phù hợp một cách thoải mái trong giới hạn. Thách thức không phải là hiệu suất mà là nhận ra cấu trúc của vấn đề. 

Quan sát quan trọng là sự trùng lặp là trở ngại duy nhất. Nếu một mảng chứa$k$các giá trị riêng biệt thì bất kỳ mảng cuối cùng hợp lệ nào cũng có thể chứa tối đa một bản sao của mỗi giá trị, do đó độ dài của nó không bao giờ vượt quá$k$. 

Phần tinh tế nhất là hoạt động của chính nó. Chúng tôi không thể loại bỏ một yếu tố duy nhất. Mỗi lần xóa sẽ xóa chính xác hai phần tử. Điều đó có nghĩa là số phần tử bị loại bỏ phải luôn là số chẵn. 

Hãy xem xét mảng:```
1 1 2
```có$k=2$những giá trị riêng biệt. Giữ một bản sao của mỗi giá trị sẽ để lại độ dài$2$, có vẻ lý tưởng. Tuy nhiên, đạt được chiều dài$2$sẽ yêu cầu loại bỏ chính xác một phần tử và điều đó là không thể vì việc loại bỏ xảy ra theo cặp. Chúng ta phải loại bỏ hai phần tử, để lại độ dài$1$. 

Do đó câu trả lời đúng là:```
1
```Một trường hợp dễ bỏ sót khác là:```
1 1 2 2
```có$k=2$những giá trị riêng biệt và$n-k=2$các phần tử trùng lặp. Vì số lượng bản sao đã là số chẵn nên chúng ta có thể xóa chính xác các bản sao đó và giữ lại:```
1 2
```Câu trả lời là:```
2
```Một giải pháp bất cẩn chỉ đếm các giá trị riêng biệt sẽ thất bại trong trường hợp số lượng bản sao là số lẻ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các chuỗi loại bỏ cặp có thể có và kiểm tra xem mảng cuối cùng nào chứa các giá trị riêng biệt. Ngay cả đối với các mảng nhỏ, số lượng trình tự loại bỏ có thể tăng lên một cách bùng nổ. Mỗi thao tác chọn hai chỉ mục, sau đó thao tác tiếp theo sẽ hoạt động trên trạng thái mảng khác. Điều này nhanh chóng trở nên không khả thi. 

Để tìm ra giải pháp tốt hơn, chúng ta nên nghĩ xem mảng cuối cùng sẽ trông như thế nào. 

Giả sử mảng chứa$k$những giá trị riêng biệt. Bất kỳ mảng cuối cùng hợp lệ nào cũng có thể chứa tối đa một lần xuất hiện của mỗi giá trị, do đó, mảng riêng biệt dài nhất có thể sẽ có độ dài lý tưởng$k$. 

Để có được một mảng như vậy, chúng tôi sẽ loại bỏ chính xác:$$n-k$$các phần tử. 

Đây chính xác là những bản sao bổ sung ngoài lần xuất hiện đầu tiên của mỗi giá trị. 

Nếu như$n-k$chẵn thì chúng ta có thể loại bỏ chính xác những trùng lặp đó. Mảng còn lại có độ dài$k$, và mọi phần tử đều khác nhau. 

Nếu như$n-k$là số lẻ thì việc loại bỏ chỉ các phần tử trùng lặp là không thể vì mọi thao tác đều loại bỏ hai phần tử. Chúng ta phải loại bỏ một phần tử bổ sung như một phần của một cặp nào đó. Điều đó làm giảm độ dài cuối cùng thêm một lần nữa, tạo ra độ dài$k-1$. 

Toàn bộ vấn đề giảm xuống việc đếm các giá trị riêng biệt và kiểm tra tính chẵn lẻ của số lượng trùng lặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng. 
2. Tính số giá trị phân biệt, ký hiệu là`k`. 
3. Tính số phần tử trùng lặp:$$d = n - k$$Đây là những thành phần phải được loại bỏ nếu chúng ta muốn giữ lại một bản sao của mỗi giá trị. 
4. Kiểm tra xem`d`là chẵn. 

Nếu như`d`chẵn, chúng ta có thể loại bỏ chính xác những trùng lặp đó bằng cách loại bỏ cặp, vì vậy câu trả lời là`k`. 
5. Nếu`d`là số lẻ thì một phần tử thừa cũng phải biến mất vì tổng số phần tử bị loại bỏ phải là số chẵn. 

Trong trường hợp đó câu trả lời là`k - 1`. 
6. Xuất kết quả. 

### Tại sao nó hoạt động 

Mỗi mảng cuối cùng hợp lệ có thể chứa tối đa một bản sao của mỗi giá trị riêng biệt, do đó độ dài của nó không thể vượt quá số lượng giá trị riêng biệt$k$. 

Xóa tất cả các bản sao yêu cầu xóa chính xác$n-k$các phần tử. Khi số này là số chẵn thì phù hợp với yêu cầu hoạt động và chúng ta có thể giữ nguyên tất cả$k$những giá trị riêng biệt. 

Khi$n-k$là số lẻ, chỉ xóa các phần tử trùng lặp sẽ yêu cầu một số phần tử bị loại bỏ là số lẻ, điều này không thể đạt được vì mọi thao tác đều loại bỏ hai phần tử. Ít nhất một phần tử bổ sung phải được loại bỏ, giảm độ dài tối đa có thể đạt được đi một phần tử. Do đó độ dài tốt nhất có thể trở thành$k-1$. 

Vì đây là hai khả năng duy nhất nên thuật toán là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    distinct = len(set(a))
    duplicates = n - distinct

    if duplicates % 2 == 0:
        print(distinct)
    else:
        print(distinct - 1)
```Việc thực hiện tuân theo quan sát toán học trực tiếp. 

Bộ lưu trữ một bản sao của mỗi giá trị, vì vậy`len(set(a))`cho biết số phần tử phân biệt. Sự khác biệt`n - distinct`bằng số phần tử trùng lặp phải được loại bỏ để chỉ để lại các giá trị duy nhất. 

Tính chẵn lẻ của số lượng này xác định liệu chúng ta có thể dừng lại sau khi loại bỏ các phần tử trùng lặp hay liệu một phần tử bổ sung cũng phải được loại bỏ. 

Không có điều kiện biên phức tạp. Khi tất cả các giá trị là khác biệt,`duplicates = 0`, là số chẵn nên đáp án vẫn là`n`. Khi tất cả các giá trị đều bằng nhau,`distinct = 1`, và công thức vẫn cho kết quả đúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
2 2 2 3 3 3
```| n | khác biệt (k) | trùng lặp (d) | d % 2 | trả lời | 
| --- | --- | --- | --- | --- | 
| 6 | 2 | 4 | 0 | 2 | 

Mảng chỉ chứa các giá trị 2 và 3. Chúng ta có thể giữ một bản sao của mỗi giá trị và loại bỏ bốn phần tử còn lại. Vì bốn là số chẵn nên việc loại bỏ cặp hoạt động hoàn hảo và độ dài cuối cùng là 2. 

### Ví dụ 2 

đầu vào:```
5
9 1 9 9 1
```| n | khác biệt (k) | trùng lặp (d) | d % 2 | trả lời | 
| --- | --- | --- | --- | --- | 
| 5 | 2 | 3 | 1 | 1 | 

Có hai giá trị riêng biệt. Việc giữ một bản sao của mỗi phần sẽ yêu cầu loại bỏ ba phần tử, điều này là không thể vì việc xóa xảy ra theo cặp. Một phần tử nữa phải biến mất nên độ dài cuối cùng tối đa trở thành 1. 

Ví dụ này thể hiện quan sát tính chẵn lẻ quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Xây dựng tập hợp và đếm các giá trị riêng biệt | 
| Không gian | O(n) | Tập hợp có thể chứa tất cả các phần tử mảng nếu chúng khác biệt | 

Với$n \le 50$, giải pháp này thấp hơn nhiều so với giới hạn. Ngay cả trên tất cả các trường hợp thử nghiệm, tổng khối lượng công việc là rất nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        distinct = len(set(a))
        duplicates = n - distinct

        if duplicates % 2 == 0:
            ans.append(str(distinct))
        else:
            ans.append(str(distinct - 1))

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue()

# provided sample
assert run(
"""4
6
2 2 2 3 3 3
5
9 1 9 9 1
4
15 16 16 15
4
10 100 1000 10000
"""
) == "2\n1\n2\n4"

# minimum size
assert run(
"""1
1
7
"""
) == "1"

# all equal values
assert run(
"""1
5
3 3 3 3 3
"""
) == "1"

# odd number of duplicates
assert run(
"""1
3
1 1 2
"""
) == "1"

# even number of duplicates
assert run(
"""1
4
1 1 2 2
"""
) == "2"

# maximum-size style case
assert run(
"""1
50
1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
"""
) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`1`| Kích thước mảng tối thiểu | 
|`3 3 3 3 3`|`1`| Tất cả các giá trị bằng nhau | 
|`1 1 2`|`1`| Số trùng lặp lẻ | 
|`1 1 2 2`|`2`| Số lượng trùng lặp thậm chí | 
| Năm mươi giá trị giống nhau |`0`| Số lượng trùng lặp lớn và xử lý chẵn lẻ | 

## Vỏ cạnh 

Hãy xem xét:```
1
3
1 1 2
```Chúng tôi có:```
distinct = 2
duplicates = 1
```Số lượng trùng lặp là số lẻ. Việc giữ một bản sao của mỗi giá trị sẽ yêu cầu loại bỏ chính xác một phần tử, nhưng các thao tác sẽ loại bỏ hai phần tử cùng một lúc. Thuật toán trả về:```
2 - 1 = 1
```cái nào đúng. 

Bây giờ hãy xem xét:```
1
4
1 1 2 2
```Chúng tôi có:```
distinct = 2
duplicates = 2
```Số lượng trùng lặp là chẵn. Chúng ta có thể xóa thêm một`1`và thêm một cái nữa`2`, loại bỏ chính xác hai phần tử. Thuật toán trả về:```
2
```đó là tối ưu. 

Cuối cùng, hãy xem xét:```
1
5
7 7 7 7 7
```Chúng tôi có:```
distinct = 1
duplicates = 4
```Vì 4 là số chẵn nên chúng ta có thể loại bỏ 4 phần tử và giữ lại một phần tử`7`. Các đầu ra thuật toán:```
1
```đó là mảng riêng biệt dài nhất có thể. 

Những ví dụ này bao gồm các trường hợp chẵn lẻ xác định toàn bộ giải pháp.
