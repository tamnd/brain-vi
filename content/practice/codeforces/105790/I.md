---
title: "CF 105790I - Itwise Bor"
description: "Chúng tôi có một loạt các giá trị độ sáng của sao. Chúng ta phải chia mảng thành các nhóm liền kề. Vẻ đẹp của một nhóm là bitwise OR của tất cả các giá trị bên trong nhóm đó. Đối với một phân vùng của mảng, chúng tôi tính tổng các điểm đẹp của tất cả các nhóm."
date: "2026-06-26T03:50:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105790
codeforces_index: "I"
codeforces_contest_name: "UDESC Selection Contest 2024-1"
rating: 0
weight: 105790
solve_time_s: 43
verified: true
draft: false
---

[CF 105790I - Itwise Bor](https://codeforces.com/problemset/problem/105790/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt các giá trị độ sáng của sao. Chúng ta phải chia mảng thành các nhóm liền kề. Vẻ đẹp của một nhóm là bitwise OR của tất cả các giá trị bên trong nhóm đó. 

Đối với một phân vùng của mảng, chúng tôi tính tổng các điểm đẹp của tất cả các nhóm. Trong số tất cả các phân vùng có thể, chúng tôi muốn tổng tối đa có thể. Nếu một số phân vùng đạt được tổng tối đa đó, chúng ta phải xuất phân vùng sử dụng ít nhóm nhất. 

Đầu vào chứa một mảng có độ dài`N`, Ở đâu`N`có thể lớn như`3·10^5`và mỗi giá trị đều nhỏ hơn`2^30`. Đầu ra bao gồm hai số:`S`= tổng số người đẹp nhóm tối đa có thể.`K`= số nhóm tối thiểu trong số tất cả các phân vùng đạt được`S`. 

Kích thước của`N`ngay lập tức loại trừ bất kỳ chương trình động nào kiểm tra tất cả các mảng con. Có khoảng`N²/2`mảng con, sẽ vào khoảng 45 tỷ khi`N = 300000`. Thậm chí một`O(N log N)`giải pháp sẽ thoải mái, trong khi`O(N²)`là hoàn toàn không thể. 

Khó khăn chính là hiểu cách hoạt động theo bit OR khi một số phần tử được hợp nhất thành một nhóm. 

Hãy xem xét một ví dụ đơn giản:```
2
1 2
```Phân vùng`[1][2]`mang lại vẻ đẹp tổng thể`1 + 2 = 3`. 

Phân vùng`[1,2]`mang lại vẻ đẹp`1|2 = 3`. 

Số tiền tối đa vẫn là`3`, nhưng phân vùng thứ hai sử dụng ít nhóm hơn nên câu trả lời là:```
3 1
```Một giải pháp bất cẩn luôn bắt đầu một nhóm mới bất cứ khi nào có thể sẽ quay trở lại`K = 2`, không phải là tối thiểu. 

Một trường hợp thú vị khác là:```
2
1 1
```Các nhóm riêng biệt đưa ra`1 + 1 = 2`. 

Một nhóm hợp nhất đưa ra`1|1 = 1`. 

Phiên bản được hợp nhất sẽ mất giá trị vì cùng một bit chỉ được tính một lần bên trong OR. Câu trả lời đúng là:```
2 2
```Bất kỳ giải pháp nào hợp nhất các nhóm bất cứ khi nào OR không giảm sẽ thất bại ở đây. 

Trường hợp cạnh cuối cùng là sự hiện diện của số không:```
3
0 5 0
```Phân vùng`[0,5,0]`có vẻ đẹp`5`. 

Phân vùng`[0][5][0]`cũng có vẻ đẹp tổng hợp`5`. 

Vì cả hai đều đạt được số tiền tối đa như nhau nên chúng ta phải chọn nhóm có ít nhóm hơn, cụ thể là một nhóm. 

Xử lý những tình huống ràng buộc này một cách chính xác là điều cần thiết. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta hãy nghĩ xem điều gì sẽ xảy ra khi một số phần tử được xếp vào một nhóm. 

Giả sử một nhóm chứa các giá trị có OR là`X`. 

Mỗi bit xuất hiện ở bất kỳ đâu trong nhóm đều góp phần chính xác một lần vào`X`. 

Bây giờ hãy so sánh điều này với việc giữ tất cả các phần tử riêng biệt. Nếu một bit cụ thể xuất hiện trong nhiều phần tử thì khi tách ra, nó sẽ đóng góp nhiều lần vào tổng số, nhưng bên trong một OR nó chỉ đóng góp một lần. 

Điều này ngay lập tức đưa ra một nhận xét quan trọng: 

Đối với bất kỳ nhóm nào,```
OR(group) ≤ sum(elements of group)
```bởi vì mỗi bit chỉ có thể mất tính đa bội khi áp dụng OR. 

Tổng hợp tất cả các nhóm, vẻ đẹp tổng thể không bao giờ có thể vượt quá tổng của tất cả các phần tử mảng. 

Tổng vẻ đẹp đạt đến giới hạn trên này một cách chính xác khi không có bit nào được tính hai lần trong bất kỳ nhóm nào. 

Điều đó có nghĩa là bên trong mỗi nhóm, không có vị trí bit nào có thể xuất hiện ở hai phần tử khác nhau. 

Tương tự, với mọi cặp phần tử trong cùng một nhóm:```
their common set bits must be empty
```hoặc thực tế hơn, trong khi xây dựng một nhóm, mặt nạ OR hiện tại và giá trị tiếp theo phải đáp ứng:```
(current_mask & value) == 0
```Nếu điều kiện này được giữ thì việc cộng giá trị sẽ chỉ tạo ra các bit mới và OR của nhóm sẽ tăng chính xác bằng chính giá trị đó. 

Ý tưởng mạnh mẽ là thử tất cả các phân vùng và kiểm tra tổng vẻ đẹp của chúng. Vì có`2^(N-1)`phân vùng có thể, nó trở nên không khả thi ngay lập tức. 

Quan sát trên thay đổi hoàn toàn vấn đề. Tổng vẻ đẹp tối đa có thể luôn là tổng của tất cả các phần tử mảng. Nhiệm vụ duy nhất còn lại là tìm số nhóm tối thiểu có thể đạt được số tiền đó. 

Để đạt được tổng tối đa, mỗi nhóm phải chứa các bit rời rạc theo cặp. Trong khi quét từ trái sang phải, bất cứ khi nào phần tử tiếp theo chia sẻ một chút với mặt nạ OR của nhóm hiện tại, việc giữ nó trong cùng một nhóm sẽ làm giảm tổng vẻ đẹp xuống dưới mức tối đa. Vì vậy, một nhóm mới trở thành bắt buộc. 

Mặt khác, nếu phần tử tiếp theo không có điểm chung với mặt nạ của nhóm hiện tại thì việc hợp nhất nó là miễn phí: vẫn có thể đạt được vẻ đẹp tối đa và việc sử dụng cùng một nhóm giúp giảm thiểu tổng số nhóm. 

Điều này tự nhiên dẫn đến việc quét tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các phân vùng | O(2^N) | O(N) | Quá chậm | 
| Quét mặt nạ bit tham lam | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`total_sum`dưới dạng tổng của tất cả các giá trị mảng. 
2. Bắt đầu nhóm đầu tiên. Duy trì`mask`, bitwise OR của tất cả các phần tử hiện có trong nhóm. 
3. Xử lý mảng từ trái sang phải. 
4. Đối với giá trị hiện tại`x`, kiểm tra xem`mask & x`là khác không. 
5. Nếu giao điểm khác 0 thì một số bit đã tồn tại trong nhóm hiện tại. Giữ`x`trong nhóm này sẽ khiến bit đó chỉ được tính một lần bên trong OR, làm giảm vẻ đẹp tối đa có thể đạt được. Bắt đầu một nhóm mới và thiết lập`mask = x`. 
6. Nếu giao điểm bằng 0, tất cả các bit của`x`là người mới trong nhóm hiện tại. Thêm nó vào nhóm hiện tại và cập nhật`mask |= x`. 
7. Đếm số lần một nhóm mới được bắt đầu. Điều này mang lại số lượng nhóm tối thiểu mà vẫn đạt được tổng vẻ đẹp tối đa. 
8. Đầu ra`total_sum`và số lượng nhóm. 

### Tại sao nó hoạt động 

Tổng của tất cả các phần tử mảng là giới hạn trên tuyệt đối của câu trả lời vì OR của một nhóm không bao giờ có thể vượt quá tổng các phần tử của nó. 

Một phân vùng đạt đến giới hạn trên này một cách chính xác khi không có vị trí bit nào xuất hiện hai lần trong bất kỳ nhóm nào. Bất cứ khi nào`mask & x ≠ 0`, đặt`x`vào nhóm hiện tại sẽ tạo ra một bit trùng lặp như vậy và buộc tổng vẻ đẹp nằm dưới giới hạn trên. Việc cắt giảm là bắt buộc. 

Bất cứ khi nào`mask & x = 0`, sáp nhập`x`vào nhóm hiện tại chỉ giới thiệu các bit mới, do đó OR của nhóm tăng chính xác`x`. Không có vẻ đẹp nào bị mất đi và tránh bị cắt giảm số lượng nhóm. 

Chiến lược tham lam chỉ cắt giảm khi điều đó là không thể tránh khỏi. Mỗi vết cắt mà nó tạo ra đều bị ép buộc trong mọi phân vùng tối ưu. Do đó, nó đạt được tổng vẻ đẹp tối đa và sử dụng số nhóm nhỏ nhất có thể trong số tất cả các phân vùng đạt được tổng đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    total_sum = sum(a)

    groups = 1
    mask = 0

    for x in a:
        if mask & x:
            groups += 1
            mask = x
        else:
            mask |= x

    print(total_sum, groups)

solve()
```Phần đầu tiên tính tổng của tất cả các phần tử. Giá trị này đã là tổng vẻ đẹp tối đa có thể. 

Biến`mask`lưu trữ OR của nhóm hiện tại. Nó tóm tắt chính xác vị trí bit nào đã có mặt. 

Đối với mỗi phần tử`x`, biểu thức`mask & x`kiểm tra xem một số bit có xuất hiện hai lần trong cùng một nhóm hay không. Nếu điều đó xảy ra, một nhóm mới phải bắt đầu ngay lập tức. 

Khi không tồn tại bit chung, chúng ta mở rộng nhóm hiện tại bằng cách cập nhật`mask |= x`. 

Bộ đếm nhóm bắt đầu từ một vì ngay cả một mảng phần tử đơn cũng chứa một nhóm. 

Việc triển khai chỉ sử dụng bộ nhớ bổ sung không đổi và thực hiện một lần truyền qua mảng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
4 1 2 1 3
```| Vị trí | Giá trị | Mặt nạ hiện tại Trước đây | Xung đột? | Nhóm | Mặt nạ hiện tại Sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 0 | Không | 1 | 4 | 
| 2 | 1 | 4 | Không | 1 | 5 | 
| 3 | 2 | 5 | Không | 1 | 7 | 
| 4 | 1 | 7 | Có | 2 | 1 | 
| 5 | 3 | 1 | Có | 3 | 3 |`total_sum = 4 + 1 + 2 + 1 + 3 = 11`Trả lời:```
11 3
```Dấu vết này cho thấy ba giá trị đầu tiên có thể cùng tồn tại vì các bit được đặt của chúng rời rạc. Giá trị thứ tư chia sẻ một chút với nhóm hiện tại, khiến việc cắt giảm là điều không thể tránh khỏi. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```| Vị trí | Giá trị | Mặt nạ hiện tại Trước đây | Xung đột? | Nhóm | Mặt nạ hiện tại Sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | Không | 1 | 1 | 
| 2 | 2 | 1 | Không | 1 | 3 | 
| 3 | 3 | 3 | Có | 2 | 3 |`total_sum = 6`Trả lời:```
6 2
```Hai giá trị đầu tiên sử dụng các bit khác nhau nên chúng có thể được hợp nhất. Giá trị thứ ba chứa các bit đã có trong nhóm, buộc phải tạo một phân đoạn mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Quét một lần từ trái sang phải | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Với`N ≤ 3·10^5`, quét tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ là không đổi ngoài chính mảng đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    total_sum = sum(a)

    groups = 1
    mask = 0

    for x in a:
        if mask & x:
            groups += 1
            mask = x
        else:
            mask |= x

    return f"{total_sum} {groups}"

# provided samples
assert run("5\n4 1 2 1 3\n") == "11 3", "sample 1"
assert run("3\n1 2 3\n") == "6 2", "sample 2"
assert run("4\n7 7 7 7\n") == "28 4", "sample 3"
assert run("5\n1 3 4 8 2\n") == "18 3", "sample 4"

# custom cases
assert run("1\n0\n") == "0 1", "minimum size"
assert run("3\n0 5 0\n") == "5 1", "zeros can merge"
assert run("2\n1 1\n") == "2 2", "duplicate bit forces split"
assert run("4\n1 2 4 8\n") == "15 1", "all bits disjoint"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`0 1`| Đầu vào nhỏ nhất có thể | 
|`0 5 0`|`5 1`| Giá trị 0 không tạo ra xung đột | 
|`1 1`|`2 2`| Các bit trùng lặp yêu cầu cắt | 
|`1 2 4 8`|`15 1`| Toàn bộ mảng có thể vẫn là một nhóm | 

## Vỏ cạnh 

Hãy xem xét:```
2
1 1
```Thuật toán bắt đầu với`mask = 0`. 

Sau khi xử lý lần đầu`1`,`mask = 1`. 

Đối với thứ hai`1`, chúng tôi nhận được:```
mask & x = 1
```thế là một nhóm mới được tạo ra. Kết quả là:```
2 2
```Điều này đúng vì việc hợp nhất chúng sẽ tạo ra giá trị OR`1`, làm giảm vẻ đẹp tổng thể. 

Bây giờ hãy xem xét:```
3
0 5 0
```Số 0 đầu tiên không tạo ra bit nào. 

giá trị`5`không có xung đột với mặt nạ hiện tại. 

Số 0 cuối cùng cũng không có xung đột. 

Không có sự cắt giảm nào được thực hiện, đưa ra:```
5 1
```Vẻ đẹp vẫn ở mức tối đa và số lượng nhóm được giảm thiểu. 

Cuối cùng:```
4
7 7 7 7
```Mọi phần tử mới đều xung đột với mặt nạ hiện tại vì tất cả các bit đều đã có sẵn. 

Thuật toán bắt đầu một nhóm mới trước mỗi phần tử tiếp theo, tạo ra:```
28 4
```Mọi lần cắt đều là bắt buộc, vì bất kỳ sự hợp nhất nào cũng sẽ làm giảm tổng vẻ đẹp xuống dưới tổng các phần tử.
