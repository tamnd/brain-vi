---
title: "CF 1525D - Ghế Bành"
description: "Chúng tôi có một hàng n ghế bành, một số trong số đó ban đầu đã có người ngồi. Mục tiêu của chúng tôi là chuyển mọi người đến một chiếc ghế bành hiện đang trống sao cho tất cả những chỗ ngồi ban đầu đều trống."
date: "2026-06-10T17:27:11+07:00"
tags: ["codeforces", "competitive-programming", "dp", "flows", "graph-matchings", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1525
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 109 (Rated for Div. 2)"
rating: 1800
weight: 1525
solve_time_s: 136
verified: true
draft: false
---

[CF 1525D - Ghế bành](https://codeforces.com/problemset/problem/1525/D) 

**Đánh giá:** 1800 
**Tags:** dp, dòng chảy, khớp biểu đồ, tham lam 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng`n`ghế bành, một số trong số đó ban đầu đã có người ngồi. Mục tiêu của chúng tôi là chuyển mọi người đến một chiếc ghế bành hiện đang trống sao cho tất cả những chỗ ngồi ban đầu đều trống. Điều đáng chú ý là chúng tôi di chuyển từng người một và thời gian cần thiết để di chuyển một người khỏi chỗ ngồi`i`đến chỗ ngồi`j`chính xác là`|i - j|`. Chúng tôi muốn tổng thời gian để di chuyển mọi người là tối thiểu. 

Mảng đầu vào`a`chiều dài`n`mã hóa trạng thái ban đầu, với`1`biểu thị chỗ ngồi đã có người và`0`một chỗ ngồi miễn phí. Đầu ra là một số nguyên duy nhất, tổng thời gian di chuyển tối thiểu. 

Những ràng buộc cho chúng ta biết rằng`n`có thể lên tới 5000, đủ nhỏ để cho phép lập trình động hoặc các thuật toán giống như dòng chảy với khoảng`O(n^2)`hoạt động. Chúng ta cũng biết rằng số ghế được sử dụng nhiều nhất là`n/2`, do đó luôn có ít nhất số ghế trống bằng số ghế đã có người, điều này đảm bảo tồn tại một giải pháp hợp lệ. 

Các trường hợp đặc biệt bao gồm việc để trống tất cả các ghế, sẽ trả về 0 ngay lập tức và các cấu hình trong đó các ghế đã có người được nhóm lại với nhau, có thể gây ra các cách tiếp cận tham lam ngây thơ cố gắng ghép ghế trống gần nhất với người gần nhất mà không cần xem xét tổng thể. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách phân công người có thể vào các ghế trống và tính toán tổng thời gian di chuyển. Điều này tương đương với việc liệt kê tất cả các hoán vị chỗ trống cho người dân. Nếu có`k`chỗ ngồi bị chiếm đóng và`m`ghế trống, số bài tập là`m! / (m - k)!`, điều này khó có thể thực hiện được đối với`k`hoặc`m`lớn hơn 10 hoặc 15. Cách tiếp cận này đúng nhưng quá chậm để`n`lên tới 5000. 

Cái nhìn sâu sắc quan trọng là vấn đề có thể được giảm xuống thành một trường hợp phù hợp giữa hai bên có trọng số. Hãy coi các ghế đã có người là một tập hợp các đỉnh và các ghế trống là tập hợp còn lại. Chi phí để xếp một người vào một chỗ ngồi là khoảng cách giữa họ. Tuy nhiên, một thuật toán Hungary đầy đủ sẽ chạy trong`O(n^3)`hoặc`O(k^3)`thời gian, là ranh giới nhưng có thể được tối ưu hóa bằng lập trình động. 

Chúng ta có thể khai thác thứ tự một chiều của chỗ ngồi. Cho phép`occupied`là danh sách được sắp xếp các vị trí của mọi người và`empty`danh sách sắp xếp chỗ ngồi miễn phí. Định nghĩa`dp[i][j]`là tổng chi phí tối thiểu để chỉ định lần đầu tiên`i`mọi người vào một tập hợp con của cái đầu tiên`j`ghế trống. Việc lặp lại rất đơn giản: hoặc chúng ta bỏ qua ghế trống hiện tại (không có nhiệm vụ) hoặc chúng ta chỉ định người hiện tại vào đó và thêm chi phí di chuyển. Điều này làm giảm giải pháp xuống còn`O(k * m)`bài toán quy hoạch động, khả thi vì`k`Và`m`mỗi cái nhiều nhất là 2500. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m! / (m-k)!) | O(m) | Quá chậm | 
| DP với danh sách được sắp xếp | O(k * m) | O(k * m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào và tách các chỉ số chiếm dụng (`occupied`) và trống (`empty`) ghế. 
2. Khởi tạo mảng DP`dp`với kích thước`(k + 1) x (m + 1)`Ở đâu`k`là số người và`m`số ghế trống. Đặt tất cả các mục thành vô cùng, ngoại trừ`dp[0][0] = 0`. 
3. Lặp lại từng người`i`từ 0 đến`k`. Với mỗi người, hãy lặp lại từng ghế trống`j`từ 0 đến`m`. Tại mỗi bước, tuyên truyền hai lựa chọn: 

- Bỏ qua ghế trống`j`:`dp[i][j+1] = min(dp[i][j+1], dp[i][j])`. 
- Chỉ định người`i`đến chỗ trống`j`:`dp[i+1][j+1] = min(dp[i+1][j+1], dp[i][j] + abs(occupied[i] - empty[j]))`. 
4. Sau khi điền vào bảng DP thì đáp án là`dp[k][m]`, tổng chi phí tối thiểu để phân công tất cả mọi người. 

Tại sao nó hoạt động: Bất biến DP là`dp[i][j]`luôn lưu trữ chi phí tối thiểu của việc chỉ định đầu tiên`i`mọi người vào một số tập hợp con của tập đầu tiên`j`ghế trống. Bằng cách xem xét cả bỏ qua và gán ở mỗi bước, chúng tôi đảm bảo rằng tất cả các trình tự có thể được xem xét và bằng cách điền vào bảng theo thứ tự, mọi phụ thuộc đều được thỏa mãn trước khi sử dụng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

occupied = [i for i, val in enumerate(a) if val == 1]
empty = [i for i, val in enumerate(a) if val == 0]

k = len(occupied)
m = len(empty)

INF = 10**18
dp = [ [INF] * (m + 1) for _ in range(k + 1) ]
dp[0][0] = 0

for i in range(k + 1):
    for j in range(m):
        if dp[i][j] == INF:
            continue
        # Option 1: skip current empty chair
        dp[i][j+1] = min(dp[i][j+1], dp[i][j])
        # Option 2: assign current person if possible
        if i < k:
            dp[i+1][j+1] = min(dp[i+1][j+1], dp[i][j] + abs(occupied[i] - empty[j]))

print(dp[k][m])
```Giải pháp đầu tiên là tách danh sách ghế trống và ghế trống. Bảng DP được khởi tạo cẩn thận với số lượng vô hạn để thể hiện các trạng thái không thể truy cập được. Chúng tôi lặp lại tất cả các nhiệm vụ có thể thực hiện bằng cách sử dụng tùy chọn bỏ qua để cho phép linh hoạt trong việc chọn chỗ trống để phân công. Tổng chi phí tối thiểu được tìm thấy tại`dp[k][m]`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`1 0 0 1 0 0 1`Chiếm lĩnh:`[0, 3, 6]`Trống:`[1, 2, 4, 5]`| tôi | j | giá trị dp[i][j] | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 0 | 1 | 0 | 
| 1 | 2 | 1 (` | 
| 2 | 4 | 2 (`1 + | 
| 3 | 5 | 3 (`2 + | 

Trả lời:`3`. Xác nhận việc di chuyển tối thiểu đạt được bằng cách di chuyển mỗi người đến chỗ ngồi gần nhất mà không có xung đột. 

### Ví dụ 2 

đầu vào:`1 2 3 0 1 0 0 1`Chiếm lĩnh:`[0, 1, 4, 7]`Trống:`[3, 5, 6]`DP tuyên truyền các lựa chọn bỏ qua các ghế trống khi cần thiết. Cuối cùng`dp[k][m]`mang lại tổng chi phí tối thiểu, xử lý việc phân nhóm ghế có người một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k * m) | Mỗi người có thể xem xét từng ghế trống một lần. k, m 2500. | 
| Không gian | O(k * m) | Bảng DP lưu trữ chi phí tối thiểu cho tất cả các nhiệm vụ từng phần. | 

Với n 5000,`k*m`2500 * 2500 ≈ 6,25×10^6, chạy tốt trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    occupied = [i for i, val in enumerate(a) if val == 1]
    empty = [i for i, val in enumerate(a) if val == 0]
    k = len(occupied)
    m = len(empty)
    INF = 10**18
    dp = [ [INF] * (m + 1) for _ in range(k + 1) ]
    dp[0][0] = 0
    for i in range(k + 1):
        for j in range(m):
            if dp[i][j] == INF:
                continue
            dp[i][j+1] = min(dp[i][j+1], dp[i][j])
            if i < k:
                dp[i+1][j+1] = min(dp[i+1][j+1], dp[i][j] + abs(occupied[i] - empty[j]))
    return str(dp[k][m])

# provided samples
assert run("7\n1 0 0 1 0 0 1\n") == "3"
assert run("3\n0 0 0\n") == "0"
# custom cases
assert run("5\n1 1 0 0 1\n") == "2"  # minimal rearrangement
assert run("2\n1 0\n") == "1"        # simplest nontrivial case
assert run("6\n1 0 1 0 1 0\n") == "3" # alternating seats
assert run("4\n0 0 0 1\n") == "0"     # only one person at the end
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 

|---|
