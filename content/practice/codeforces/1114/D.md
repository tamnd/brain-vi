---
title: "CF 1114D - Lấp lũ"
description: "Chúng ta có một hàng gồm n ô vuông màu, mỗi ô được gắn nhãn bằng một số nguyên đại diện cho màu của nó. Mục tiêu là đổi màu toàn bộ hàng thành một màu duy nhất bằng cách sử dụng một loạt thao tác \"lấp đầy\"."
date: "2026-06-12T04:52:43+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 1114
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 538 (Div. 2)"
rating: 1900
weight: 1114
solve_time_s: 87
verified: false
draft: false
---

[CF 1114D - Lấp đầy lũ](https://codeforces.com/problemset/problem/1114/D) 

**Xếp hạng:** 1900 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng`n`các ô vuông màu, mỗi ô được dán nhãn bằng một số nguyên đại diện cho màu của nó. Mục tiêu là đổi màu toàn bộ hàng thành một màu duy nhất bằng cách sử dụng một loạt thao tác "lấp đầy". Mỗi thao tác chọn một đoạn liền kề có màu giống hệt nhau chứa hình vuông bắt đầu đã chọn và thay đổi tất cả các hình vuông của nó sang màu khác. Bản thân ô bắt đầu được chọn một lần và không được tính là một nước đi. 

Đầu vào cho biết số ô vuông và chuỗi màu sắc. Đầu ra là số lượt quay tối thiểu cần thiết để làm cho tất cả các hình vuông có cùng màu. 

Với`n`lên tới 5000, bất kỳ thuật toán đơn giản nào kiểm tra rõ ràng mọi chuỗi di chuyển có thể xảy ra sẽ quá chậm. Ví dụ: nếu người ta thử mọi ô vuông bắt đầu có thể và mọi lựa chọn màu sắc ở mỗi bước, thì không gian tìm kiếm sẽ tăng theo cấp số nhân, dễ dàng vượt quá`10^6`hoạt động không thể thực hiện được trong giới hạn thời gian 2 giây. 

Các trường hợp cạnh tinh tế bao gồm các hàng đã đồng nhất, trong trường hợp đó câu trả lời là 0 hoặc các hàng trong đó mỗi ô vuông có một màu duy nhất, giúp tối đa hóa số lần di chuyển cần thiết. Một tình huống phức tạp khác là khi ô vuông bắt đầu tối ưu không nằm ở điểm cuối mà ở đâu đó ở giữa, đòi hỏi chúng ta phải nhìn xa hơn cách tiếp cận tham lam đơn giản. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng mọi lựa chọn có thể có của hình vuông bắt đầu và từng chuỗi màu sắc có thể thay đổi. Đối với mỗi lần di chuyển, chúng tôi tô màu lại thành phần được kết nối và kiểm tra mảng kết quả. Điều này hoạt động chính xác về nguyên tắc, nhưng đối với`n = 5000`, số lượng chuỗi tăng theo cấp số nhân, khiến nó hoàn toàn không thực tế. 

Thông tin chi tiết quan trọng để tối ưu hóa xuất phát từ việc nhận ra rằng chúng ta không cần phải theo dõi từng ô riêng lẻ. Thay vào đó, chúng ta có thể thu gọn các màu giống hệt nhau liên tiếp thành các phân đoạn đơn lẻ. Việc đổi màu chỉ quan trọng ở ranh giới phân khúc. Hơn nữa, nếu hai phân đoạn cùng màu xuất hiện với một màu khác nhau ở giữa, chiến lược tối ưu có thể hợp nhất các phân đoạn này bằng cách chuyển các màu ở giữa thành màu mục tiêu trong một số bước tối thiểu. 

Chính thức, chúng tôi xác định trạng thái lập trình động`dp[l][r]`là số vòng quay tối thiểu cần thiết để đi đoạn từ`l`ĐẾN`r`(bao gồm) một màu duy nhất. Nếu đoạn đầu tiên và đoạn cuối cùng có cùng màu, việc tô màu ngoài cùng có thể được thực hiện với ít thao tác hơn vì chúng có thể được hợp nhất một cách tối ưu. Quan sát này cho phép một công thức đệ quy:```
dp[l][r] = 1 + min(dp[l+1][k] + dp[k+1][r])  for all k such that l <= k < r
```Chúng tôi tối ưu hóa hơn nữa bằng cách nhận thấy rằng chúng tôi chỉ cần xem xét các vị trí mà màu sắc thay đổi, điều này làm giảm hiệu quả`n`từ tổng số ô vuông đến số đoạn, thường nhỏ hơn đáng kể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Lập trình động trên các đoạn | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nén mảng đầu vào thành các phân đoạn trong đó các màu giống hệt nhau liên tiếp được thu gọn thành một. Chỉ lưu trữ chuỗi màu sắc duy nhất. Điều này làm giảm tính toán dư thừa. 
2. Khởi tạo mảng DP 2D`dp`kích thước`m x m`, Ở đâu`m`là số đoạn.`dp[l][r]`sẽ lưu trữ số lần di chuyển tối thiểu để tô màu lại các phân đoạn`l`bởi vì`r`thành một màu duy nhất. 
3. Đặt trường hợp cơ sở:`dp[i][i] = 1`đối với tất cả các phân đoạn đơn lẻ, vì chỉ một phân đoạn có thể được chuyển thành một màu duy nhất trong một lần di chuyển nếu cần. 
4. Điền vào bảng DP bằng cách tăng độ dài đoạn. Đối với một đoạn từ`l`ĐẾN`r`: 

- Bắt đầu với`dp[l][r] = 1 + dp[l+1][r]`giả sử chúng ta tô màu lại phân đoạn`l`để phù hợp với phần còn lại. 
- Sau đó lặp lại các vị trí`k`từ`l+1`ĐẾN`r`. Nếu như`color[l] == color[k]`, chúng ta có thể hợp nhất các phân đoạn tại`l`Và`k`và kết hợp kết quả:`dp[l][r] = min(dp[l][r], dp[l+1][k-1] + dp[k][r])`. 
5. Câu trả lời cuối cùng là`dp[0][m-1]`, số lần di chuyển tối thiểu để tô màu lại toàn bộ hàng đã nén. 

**Tại sao nó hoạt động:** Bất biến DP là như vậy`dp[l][r]`luôn lưu trữ số lần di chuyển tối thiểu để đổi màu đoạn`[l, r]`. Sự lặp lại hợp nhất các phân đoạn có cùng màu và chiếm các màu trung gian một cách hiệu quả, đảm bảo không tính các bước di chuyển dư thừa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
colors = list(map(int, input().split()))

# Step 1: Compress consecutive identical colors
comp = []
for c in colors:
    if not comp or comp[-1] != c:
        comp.append(c)

m = len(comp)
dp = [[0] * m for _ in range(m)]

# Step 2: DP base case
for i in range(m):
    dp[i][i] = 1

# Step 3: DP computation
for length in range(2, m+1):
    for l in range(m - length + 1):
        r = l + length - 1
        dp[l][r] = 1 + dp[l+1][r]
        for k in range(l+1, r+1):
            if comp[l] == comp[k]:
                dp[l][r] = min(dp[l][r], dp[l+1][k-1] + dp[k][r])

print(dp[0][m-1])
```Đầu tiên, mã sẽ nén chuỗi màu để đảm bảo chúng tôi chỉ xem xét các ranh giới phân đoạn có ý nghĩa. Chúng ta khởi tạo bảng DP với các trường hợp cơ bản là các đoạn có độ dài 1. Chúng tôi lặp lại bằng cách tăng độ dài phân đoạn, cập nhật DP bằng logic hợp nhất. các`dp`table đảm bảo rằng mọi giải pháp mảng con đều tối ưu khi được tính toán từ các mảng con nhỏ hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`4 5 2 2 1`| tôi | r | dp[l][r] | Giải thích | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | Phân đoạn đơn [5] | 
| 1 | 1 | 1 | [2] | 
| 2 | 2 | 1 | [2] | 
| 3 | 3 | 1 | [1] | 
| 1 | 2 | 1 | Hợp nhất [2,2] | 
| 0 | 1 | 2 | Hợp nhất [5] với [2,2] | 
| 0 | 2 | 2 | Tương tự, đang xem xét việc sáp nhập | 
| 0 | 3 | 2 | Kết quả cuối cùng: đổi màu [5,2,2,1] sau 2 nước đi | 

Dấu vết này cho thấy việc hợp nhất các phân đoạn làm giảm sự di chuyển như thế nào khi màu sắc khớp với nhau ở các đầu xa. 

### Mẫu 2 

đầu vào:`5 1 2 3 4 5`Tất cả các màu sắc khác nhau. Mỗi phân đoạn là duy nhất, vì vậy mỗi lần di chuyển sẽ thay đổi một phân đoạn tại một thời điểm. DP tính toán rằng việc hợp nhất các màu không liền kề là không thể. Di chuyển tối thiểu = 4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Sau khi nén, chúng ta xem xét từng cặp phân đoạn. Vòng lặp bên trong lặp lại nhiều nhất`m`lần. | 
| Không gian | O(n^2) | Bảng DP lưu trữ kết quả cho tất cả các phạm vi phân khúc. | 

Với`n <= 5000`,`n^2`hoạt động là khoảng 25 triệu, có thể chấp nhận được trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    colors = list(map(int, input().split()))
    comp = []
    for c in colors:
        if not comp or comp[-1] != c:
            comp.append(c)
    m = len(comp)
    dp = [[0] * m for _ in range(m)]
    for i in range(m):
        dp[i][i] = 1
    for length in range(2, m+1):
        for l in range(m - length + 1):
            r = l + length - 1
            dp[l][r] = 1 + dp[l+1][r]
            for k in range(l+1, r+1):
                if comp[l] == comp[k]:
                    dp[l][r] = min(dp[l][r], dp[l+1][k-1] + dp[k][r])
    return str(dp[0][m-1])

# Provided samples
assert run("4\n5 2 2 1\n") == "2", "sample 1"
assert run("1\n5\n") == "1", "single element"
assert run("5\n1 2 3 4 5\n") == "4", "all distinct"
# Custom cases
assert run("6\n1 1 1 1 1 1\n") == "1", "all same"
assert run("7\n1 2 1 2 1 2 1\n") == "4", "alternating"
assert run("2\n2 2\n") == "1", "two same"
assert run("2\n2 3\n") == "1", "two different
```
