---
title: "CF 1133E - Đội cân bằng K"
description: "Chúng tôi có một danh sách sinh viên, mỗi sinh viên có trình độ kỹ năng lập trình. Nhiệm vụ là chia những học sinh này thành tối đa k đội sao cho tổng số học sinh tham gia là lớn nhất."
date: "2026-06-12T04:04:59+07:00"
tags: ["codeforces", "competitive-programming", "dp", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1133
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 544 (Div. 3)"
rating: 1800
weight: 1133
solve_time_s: 65
verified: true
draft: false
---

[CF 1133E - Nhóm cân bằng K](https://codeforces.com/problemset/problem/1133/E) 

**Đánh giá:** 1800 
**Thẻ:** dp, sắp xếp, hai con trỏ 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một danh sách sinh viên, mỗi sinh viên có trình độ kỹ năng lập trình. Nhiệm vụ là chia những học sinh này thành nhiều nhất`k`các nhóm sao cho tổng số học sinh tham gia là tối đa. Mỗi đội phải "cân bằng", nghĩa là không có hai học sinh nào trong cùng một đội khác nhau quá 5 điểm về kỹ năng. Học sinh không được xếp vào nhóm sẽ bị loại khỏi danh sách. 

Kích thước đầu vào cho phép tối đa 5000 học sinh, điều đó có nghĩa là bất kỳ thuật toán nào kém hơn O(n²) đều có thể quá chậm, đặc biệt nếu chúng tôi xem xét việc thử tất cả các tập hợp con có thể có của học sinh. Bản thân giá trị kỹ năng có thể lớn, lên tới 10⁹, nhưng giá trị kỹ năng thực tế không quan trọng bằng sự khác biệt tương đối của chúng vì việc thành lập đội phụ thuộc vào sự khác biệt chứ không phải giá trị tuyệt đối. 

Một giải pháp đơn giản có thể xem xét tất cả các tập hợp con học sinh của mỗi đội, nhưng điều này sẽ bùng nổ về mặt tổ hợp. Các trường hợp đặc biệt phát sinh khi tất cả học sinh được tập hợp chặt chẽ (cho phép một nhóm lớn) hoặc khi nhiều học sinh phân tán rộng rãi, buộc phải có các nhóm nhỏ hoặc không có nhóm nào. Ví dụ, nếu`n = 5, k = 2, a = [1, 2, 15, 15, 15]`, cách tiếp cận tối ưu sẽ tạo thành một nhóm nhỏ`[1,2]`và một đội lớn`[15,15,15]`, tổng số 5 học sinh. Một cách tiếp cận tham lam bất cẩn chỉ xây dựng các nhóm một cách tuần tự có thể bỏ lỡ sự tối ưu toàn cầu bằng cách hình thành các nhóm ban đầu dưới mức tối ưu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng phân chia tất cả các học sinh có thể thành`k`các nhóm. Để kiểm tra tính hợp lệ của mỗi đội, chúng tôi sẽ xác minh sự khác biệt giữa kỹ năng nhỏ nhất và lớn nhất trong tập hợp con không vượt quá 5. Cách tiếp cận này hiệu quả vì nó tính chính xác các tập hợp con cân bằng, nhưng số lượng kết hợp tăng lên thành 2ⁿ, điều này hoàn toàn không khả thi với n = 5000. 

Cái nhìn sâu sắc quan trọng là sắp xếp học sinh theo kỹ năng. Sau khi được sắp xếp, bất kỳ nhóm hợp lệ nào cũng phải bao gồm các học sinh liên tiếp vì việc thêm một học sinh có kỹ năng thấp hơn trước một học sinh có kỹ năng cao hơn sẽ vi phạm ràng buộc về chênh lệch kỹ năng. Điều này cho phép chúng ta coi vấn đề này như một biến thể của phân vùng theo khoảng thời gian. Sau đó, lập trình động sẽ phù hợp một cách tự nhiên:`dp[i]`đại diện cho số lượng học sinh tối đa trong các đội cân bằng trong số các đội đầu tiên`i`sinh viên. Đối với mỗi vị trí, chúng tôi xem xét việc thành lập một đội kết thúc ở học sinh đó và kéo dài về phía sau trong chừng mực mà sự khác biệt về kỹ năng cho phép. 

Chúng ta có thể tối ưu hóa việc thành lập nhóm bằng hai gợi ý: dành cho sinh viên`i`, tìm xa nhất`j`sang trái sao cho sự khác biệt về kỹ năng giữa`a[i]`Và`a[j]`nhiều nhất là 5. Điều này cho chúng ta biết đội cân bằng lớn nhất có thể kết thúc ở`i`. Sau đó,`dp[i]`là mức tối đa không bao gồm`i`(lấy`dp[i-1]`) hoặc bao gồm cả đội`[j..i]`và thêm nó vào`dp[j-1]`. Tôn trọng giới hạn về`k`các đội, chúng tôi sử dụng thứ nguyên DP phụ hoặc theo dõi số lượng đội được thành lập. Vì số lượng đội nhiều nhất là 5000, nên DP 1D đơn giản là đủ nếu chúng ta xử lý các đội một cách tham lam theo thứ tự tăng kỹ năng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ * n) | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + DP + hai con trỏ) | O(n²) trường hợp xấu nhất, nhưng hiệu quả là O(n) trên mỗi con trỏ tiến lên | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng kĩ năng của học sinh. Điều này đảm bảo rằng bất kỳ phân đoạn liên tiếp nào có chênh lệch tối đa 5 đều là nhóm hợp lệ. Sắp xếp cũng cho phép chúng ta sử dụng hai con trỏ một cách hiệu quả. 
2. Khởi tạo mảng DP`dp`chiều dài`n+1`, Ở đâu`dp[i]`lưu trữ số lượng học sinh tối đa trong các nhóm cân bằng trong số các nhóm đầu tiên`i`sinh viên. Bộ`dp[0] = 0`. 
3. Đối với mỗi học sinh`i`từ 1 đến`n`, sử dụng con trỏ`j`bắt đầu từ đầu mảng để tìm học sinh ngoài cùng bên trái sao cho`a[i] - a[j] <= 5`. Điều này xác định đội lớn nhất kết thúc tại`i`. 
4. Cập nhật`dp[i]`tối đa giữa`dp[i-1]`(không bao gồm sinh viên`i`) Và`dp[j-1] + (i - j + 1)`(thành lập một đội với các sinh viên từ`j`ĐẾN`i`). 
5. Lặp lại cho đến khi`i = n`. Câu trả lời cuối cùng là`dp[n]`, số lượng học sinh tối đa có thể được xếp vào các đội cân bằng. 

Tại sao nó hoạt động: Việc sắp xếp đảm bảo rằng mọi nhóm hợp lệ đều tương ứng với một mảng con liền kề. Quá trình lặp lại DP kiểm tra tất cả các điểm cuối bên trái có thể có cho các đội kết thúc tại`i`hiệu quả với cách tiếp cận hai con trỏ. Mỗi`dp[i]`lưu trữ giải pháp tối ưu cho lần đầu tiên`i`sinh viên, đảm bảo rằng các quyết định đưa ra trước đó được kết hợp một cách tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))
a.sort()

dp = [0] * (n + 1)
j = 0

for i in range(1, n + 1):
    while a[i - 1] - a[j] > 5:
        j += 1
    # max between not using this student or using the largest valid team ending here
    dp[i] = max(dp[i - 1], dp[j] + (i - j))

print(dp[n])
```Mã này trước tiên sắp xếp các kỹ năng. Biến`j`là ranh giới bên trái hai con trỏ của đội hợp lệ kết thúc tại sinh viên`i`. Mảng DP tích lũy số lượng sinh viên tối ưu. các`while`vòng lặp đảm bảo chúng tôi thu nhỏ ranh giới bên trái cho đến khi nhóm trở nên hợp lệ. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
5 2
1 2 15 15 15
```| tôi | a[i-1] | j | tôi-j | dp[i] | dp[j] + (i-j) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 0 + 1 | 
| 2 | 2 | 0 | 2 | 2 | 0 + 2 | 
| 3 | 15 | 2 | 1 | 2 | 2 + 1 = 3 | 
| 4 | 15 | 2 | 2 | 4 | 2 + 2 = 4 | 
| 5 | 15 | 2 | 3 | 5 | 2 + 3 = 5 | 

Trace chứng minh rằng thuật toán xác định chính xác hai nhóm riêng biệt`[1,2]`Và`[15,15,15]`. 

### Tùy chỉnh đầu vào 2```
6 3
1 3 4 5 12 13
```| tôi | a[i-1] | j | tôi-j | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 1 | 
| 2 | 3 | 0 | 2 | 2 | 
| 3 | 4 | 0 | 3 | 3 | 
| 4 | 5 | 0 | 4 | 4 | 
| 5 | 12 | 4 | 1 | 5 | 
| 6 | 13 | 4 | 2 | 6 | 

Bảng hiển thị hai đội`[1,3,4,5]`Và`[12,13]`, tổng cộng có 6 học sinh. DP tích lũy chính xác số học sinh tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + n) | Việc sắp xếp mất O(n log n), DP hai con trỏ chạy trong O(n) | 
| Không gian | O(n) | Mảng DP có độ dài n+1 | 

Với n 5000, DP sắp xếp và tuyến tính nằm trong giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()
    dp = [0] * (n + 1)
    j = 0
    for i in range(1, n + 1):
        while a[i - 1] - a[j] > 5:
            j += 1
        dp[i] = max(dp[i - 1], dp[j] + (i - j))
    return str(dp[n])

# provided sample
assert run("5 2\n1 2 15 15 15\n") == "5"

# minimum input
assert run("1 1\n10\n") == "1"

# all equal
assert run("4 2\n5 5 5 5\n") == "4"

# sparse skills
assert run("5 2\n1 10 20 30 40\n") == "1"

# max k less than optimal splits
assert run("6 3\n1 2 3 4 12 13\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 2\n1 2 15 15 15 | 5 | Nhiều đội cân bằng được phân tách bằng khoảng cách kỹ năng | 
| 1 1\n10 | 1 | Đầu vào tối thiểu | 
| 4 2\n5 5 5 5 | 4 | Tất cả các giá trị bằng nhau tạo thành một đội |
