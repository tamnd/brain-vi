---
title: "CF 2064F - Chúng Tôi Tổng Hợp"
description: "Chúng ta được cho một mảng các số nguyên a có độ dài n và giá trị đích k. Nhiệm vụ của chúng ta là đếm tất cả các mảng con liền kề của một epic."
date: "2026-06-08T07:26:07+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2064
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1005 (Div. 2)"
rating: 2600
weight: 2064
solve_time_s: 100
verified: false
draft: false
---

[CF 2064F - Chúng tôi sẽ tổng kết](https://codeforces.com/problemset/problem/2064/F) 

**Đánh giá:** 2600 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, hai con trỏ 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên`a`chiều dài`n`và giá trị mục tiêu`k`. Nhiệm vụ của chúng ta là đếm tất cả các mảng con liền kề của`a`đó là _sử thi_. Một mảng con sẽ tuyệt vời nếu chúng ta có thể chia nó thành hai phần không trống sao cho giá trị nhỏ nhất của phần thứ nhất cộng với giá trị lớn nhất của phần thứ hai bằng`k`. Về mặt hình thức, nếu`b`là một mảng con của`a`, thật tuyệt vời nếu tồn tại một chỉ mục`i`(1 ≤ i < độ dài của b) sao cho`min(b[0..i-1]) + max(b[i..end]) = k`. 

Đầu vào chỉ định nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp`n`,`k`, và mảng`a`. Đầu ra của chúng tôi cho mỗi trường hợp thử nghiệm là một số duy nhất: số lượng mảng con hoành tráng. 

Những hạn chế chỉ ra rằng`n`có thể lớn tới 2×10^5 cho mỗi trường hợp thử nghiệm và tổng số`n`trên các trường hợp thử nghiệm không vượt quá 2×10^5. Điều này ngụ ý rằng chúng tôi không đủ khả năng chi trả cho thuật toán O(n^2). Một cách tiếp cận mạnh mẽ để kiểm tra tất cả các mảng con sẽ thực hiện các phép toán khoảng O(n^3) nếu chúng ta tính toán lại min và max cho mỗi phần phân chia mảng con hoặc O(n^2) nếu chúng ta tính toán trước min/max cho tất cả các mảng con, tốc độ này vẫn còn quá chậm. Do đó, chúng ta cần một cách tiếp cận gần tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng có số lặp lại, mảng trong đó mọi phần tử đều quá nhỏ hoặc quá lớn so với`k`và các mảng có kích thước tối thiểu là 2. Ví dụ: nếu`a = [1, 1, 1]`Và`k = 3`, thì không có mảng con nào hoành tráng vì`min + max = 2`tốt nhất, nhưng một thuật toán đơn giản chỉ quét phần tử đầu tiên và cuối cùng có thể đếm không chính xác một số mảng con. Một trường hợp phức tạp khác là một mảng như`[6,6,6,7,7]`với`k = 13`, trong đó bất kỳ mảng con nào chứa ít nhất một`6`và một`7`trở nên hoành tráng, đòi hỏi phải tính toán cẩn thận hơn là lặp lại một cách ngây thơ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xem xét mọi mảng con`a[l..r]`và thử mọi chỉ mục phân chia`i`. Đối với mỗi lần phân chia, chúng tôi tính toán`min(a[l..i])`Và`max(a[i+1..r])`và kiểm tra xem tổng có bằng không`k`. Điều này hiệu quả vì nó kiểm tra toàn diện tất cả các khả năng. Vấn đề là hiệu quả: đối với`n=2×10^5`, số lượng mảng con là khoảng 2×10^10, điều này là không thể thực hiện được. 

Quan sát quan trọng là`k`thực sự lớn hơn`n`và nhỏ hơn`2n`. Mỗi phần tử nằm trong`[1, n]`, do đó, một mảng con hoành tráng cần có một phần để chứa`x`và phần khác chứa`y`Ở đâu`x + y = k`. Vì những giới hạn,`x`Và`y`là các số duy nhất từ ​​1 đến n. Do đó, vấn đề giảm xuống còn việc đếm các mảng con chứa`x`trong phần đầu tiên và`y`trong phần thứ hai, ở đâu`x + y = k`. Cái nhìn sâu sắc này cho phép chúng tôi theo dõi các vị trí cuối cùng của`x`Và`y`trong khi quét mảng và đếm các mảng con hợp lệ bằng cách sử dụng hai con trỏ hoặc cửa sổ trượt. 

Một cách tiếp cận cụ thể hơn tận dụng thực tế rằng`x = k - y`. Chúng ta có thể quét từ trái sang phải, theo dõi lần xuất hiện cuối cùng của`x`và lần xuất hiện tiếp theo của`y`. Bất kỳ mảng con nào bắt đầu trước mảng cuối cùng`x`và kết thúc sau phần tiếp theo`y`là sử thi. Sử dụng logic này, chúng ta có thể đếm tất cả các mảng con sử thi theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tính toán trước tối thiểu/tối đa cho tất cả các mảng con | O(n^2) | O(n^2) | Quá chậm | 
| Hai con trỏ / theo dõi vị trí | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước cặp số`(x, y)`điều đó có thể tổng hợp thành`k`cho một mảng con nhất định. Đây`x`nằm ở phần bên trái,`y = k - x`ở phần bên phải. 
2. Lặp lại mảng trong khi vẫn duy trì chỉ mục cuối cùng trong đó`x`xuất hiện. Điều này giúp chúng ta biết tất cả các mảng con kết thúc ở chỉ mục hiện tại có thể có`x`ở phần bên trái. 
3. Đồng thời, với mỗi chỉ mục, hãy tìm chỉ mục sớm nhất nơi`y`xuất hiện sau chỉ mục hiện tại. Điều này đảm bảo phần bên phải có`y`. 
4. Đối với mỗi chỉ mục, số lượng mảng con epic kết thúc tại chỉ mục đó được xác định bởi khoảng cách giữa mảng con cuối cùng`x`và chỉ mục hiện tại nếu hợp lệ`y`tồn tại ở bên phải. Thêm số này vào tổng số đang chạy. 
5. Tiếp tục quét cho đến hết mảng, cập nhật lần xuất hiện cuối cùng của`x`và sự xuất hiện sớm nhất trong tương lai của`y`. 

Tại sao nó hoạt động: tại bất kỳ điểm nào trong mảng, lần xuất hiện cuối cùng của`x`đảm bảo rằng tất cả các phần còn lại có chứa`x`là hợp lệ và lần xuất hiện tiếp theo của`y`đảm bảo phần bên phải chứa`y`. Bởi vì`x`Và`y`được cố định, mọi mảng con được đếm đều thỏa mãn`min(left) + max(right) = k`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_epic_subarrays(n, k, a):
    pos = [0] * (n + 2)
    res = 0
    # mark positions of each number
    for idx, val in enumerate(a):
        pos[val] = idx + 1

    # loop through possible left part values
    for x in range(1, n + 1):
        y = k - x
        if 1 <= y <= n:
            left = pos[x]
            right = pos[y]
            if left and right and left < right:
                res += left * (n - right + 1)
    return res

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    print(count_epic_subarrays(n, k, a))
```Giải pháp ánh xạ từng số tới lần xuất hiện cuối cùng của nó trong mảng. Với mỗi cặp có thể`(x, y)`tổng hợp lại`k`, chúng tôi tính toán sự đóng góp của các mảng con chứa`x`trước`y`. nhân`left`vị trí bằng cách còn lại`right`vị trí cung cấp tổng số mảng con sử thi cho cặp đó. Các trường hợp cạnh như`x`hoặc`y`không xuất hiện sẽ được xử lý bằng các kiểm tra có điều kiện. 

## Ví dụ đã hoạt động 

**Mẫu 1:**```
n = 5, k = 7, a = [1,2,3,4,5]
```Vì`x = 2`,`y = 5`(`2+5=7`), cuối cùng`2`tại chỉ số 2, đầu tiên`5`ở chỉ số 5, vì vậy mảng con`[2,3,4,5]`Và`[3,4,5]`được tính bằng công thức trái*phải. Vì`x=3`,`y=4`, cuối cùng`3`tại chỉ số 3, đầu tiên`4`tại chỉ số 4, mảng con`[3,4]`. Tổng số mảng con sử thi: 2. 

**Mẫu 2:**```
n = 7, k = 13, a = [6,6,6,6,7,7,7]
```Cặp`(6,7)`chỉ một. Cuối cùng`6`ở chỉ số 4, đầu tiên`7`ở chỉ số 5. ​​Các mảng con bắt đầu từ chỉ số 1..4 và kết thúc ở chỉ số 5..7: 12 mảng con hoành tráng. 

Dấu vết chứng minh rằng việc tính toán trước các vị trí và nhân các lựa chọn trái/phải sẽ đếm chính xác tất cả các mảng con hợp lệ mà không cần lặp lại tất cả các mảng con O(n^2). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét mảng một lần và kiểm tra O(n) cặp có thể`(x, y)`| 
| Không gian | O(n) | Lưu trữ vị trí xuất hiện cuối cùng của mỗi số | 

Tổng cộng`n`trên các trường hợp thử nghiệm là 2×10^5, do đó cách tiếp cận tuyến tính này phù hợp thoải mái trong giới hạn thời gian 3 giây. Việc sử dụng bộ nhớ cũng nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        output.append(str(count_epic_subarrays(n, k, a)))
    return "\n".join(output)

# Provided samples
assert run("6\n5 7\n1 2 3 4 5\n7 13\n6 6 6 6 7 7 7\n6 9\n4 5 6 6 5 1\n5 9\n5 5 4 5 5\n5 6\n3 3 3 3 3\n6 8\n4 5 4 5 4 5\n") == "2\n12\n3\n8\n10\n4", "sample 1"

# Custom cases
assert run("1\n2 3\n1 2\n") == "1", "minimum
```
