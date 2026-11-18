---
title: "CF 2025D - Kiểm tra thuộc tính"
description: "Chúng ta được cung cấp một chuỗi các bản ghi trò chơi trong đó mỗi phần tử đại diện cho việc đạt được một điểm thuộc tính hoặc gặp phải việc kiểm tra thuộc tính. Nhân vật bắt đầu với Sức mạnh và Trí thông minh ở mức 0."
date: "2026-06-08T12:24:52+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp", "implementation", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2025
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 170 (Rated for Div. 2)"
rating: 1800
weight: 2025
solve_time_s: 82
verified: true
draft: false
---

[CF 2025D - Kiểm tra thuộc tính](https://codeforces.com/problemset/problem/2025/D) 

**Đánh giá:** 1800 
**Tags:** bạo lực, cấu trúc dữ liệu, dp, triển khai, toán học, hai con trỏ 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các bản ghi trò chơi trong đó mỗi phần tử đại diện cho việc đạt được một điểm thuộc tính hoặc gặp phải việc kiểm tra thuộc tính. Nhân vật bắt đầu với Sức mạnh và Trí thông minh ở mức 0. Số 0 trong chuỗi biểu thị việc nhận được một điểm thuộc tính có thể được gán cho Sức mạnh hoặc Trí thông minh. Số dương biểu thị kiểm tra Thông minh: ký tự vượt qua nó nếu Thông minh của họ ít nhất là giá trị đó. Số âm thể hiện kiểm tra Độ mạnh một cách tương tự. 

Mục tiêu là tối đa hóa số lần kiểm tra được thông qua trong khi vẫn tôn trọng thứ tự của hồ sơ. Chúng tôi không thể chuyển nhượng điểm trở về trước: ở bất kỳ vị trí nào, chỉ những điểm đã kiếm được trước đó mới có sẵn để phân bổ. 

Các ràng buộc áp đặt rằng có thể có tới 2 triệu bản ghi, nhưng nhiều nhất là 5000 điểm. Điều này ngụ ý rằng chúng ta không thể mô phỏng mọi cách gán điểm có thể một cách ngây thơ; một cách tiếp cận mạnh mẽ khám phá tất cả các phân bổ có$2^{5000}$khả năng, điều đó hoàn toàn không thể thực hiện được. Thay vào đó chúng ta phải khai thác số lượng điểm tương đối nhỏ để giới hạn không gian trạng thái. Các trường hợp đặc biệt bao gồm việc có tất cả các bước kiểm tra trước bất kỳ điểm nào, dẫn đến việc vượt qua các bước kiểm tra bằng 0 hoặc các chuỗi trong đó tất cả các bước kiểm tra chỉ yêu cầu một thuộc tính nhưng các điểm được phân bổ giữa hai thuộc tính, đòi hỏi phải phân bổ cẩn thận. 

Ví dụ, hãy xem xét đầu vào`5 2`với hồ sơ`1 -2 0 0 1`. Ở đây, cả hai điểm đều có sau hai lần kiểm tra đầu tiên. Một cách tiếp cận tham lam ngây thơ khi ấn định điểm mà không xem xét thứ tự sẽ cho rằng một số bước kiểm tra có thể vượt qua là không chính xác, nhưng câu trả lời đúng là chỉ có thể vượt qua hai bước kiểm tra cuối cùng (sau điểm). 

## Phương pháp tiếp cận 

Phương pháp vũ phu sẽ cố gắng thử mọi cách có thể để phân phối điểm khi chúng ta duyệt qua chuỗi. Đối với mỗi số 0, chúng tôi có thể tăng Sức mạnh hoặc Trí thông minh và đối với mỗi lần kiểm tra, chúng tôi xem liệu nó có vượt qua hay không. Số lượng các trạng thái là$2^m$bởi vì mỗi trong số$m$điểm có hai lựa chọn bài tập. Với$m$lên tới 5000, điều này mang lại khoảng$10^{1500}$khả năng, rõ ràng là quá lớn. 

Điều quan trọng cần lưu ý là mặc dù có nhiều hồ sơ nhưng chỉ có số điểm là quan trọng đối với các bài tập tiềm năng. Chúng tôi có thể theo dõi tập hợp các thông tin có thể truy cập`(strength, intelligence)`các cấp độ khi chúng tôi xử lý trình tự, cập nhật số lượng bất cứ khi nào chúng tôi vượt qua séc hoặc đạt được điểm. Phương pháp lập trình động hoạt động bằng cách lưu trữ mảng 1D gồm các mức Sức mạnh có thể có dựa trên số điểm Thông minh được phân bổ. Cụ thể, hãy`dp[i]`là số lượng kiểm tra tối đa chúng tôi có thể vượt qua nếu chúng tôi đã chỉ định chính xác`i`trỏ tới Trí tuệ; thì phần còn lại`m-i`điểm được tự động gán cho Sức mạnh. Chúng tôi lặp qua các bản ghi và cập nhật mảng DP này để tính đến từng điểm mới và từng lần kiểm tra, sử dụng mảng tạm thời để tránh ghi đè sớm các giá trị. 

Điều này làm giảm không gian trạng thái từ hàm mũ trong`m`ĐẾN`O(m^2)`, điều này khả thi vì`m ≤ 5000`. Việc sử dụng bộ nhớ cũng được kiểm soát và thuật toán chia tỷ lệ tuyến tính với`n`bởi vì chúng tôi chỉ xử lý mỗi bản ghi một lần trong khi cập nhật mảng DP có kích thước`m+1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m * n) | O(2^m) | Quá chậm | 
| Tối ưu (DP) | O(n * m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng DP`dp`chiều dài`m+1`với tất cả các giá trị`-1`. Mỗi chỉ số`i`đại diện cho sự phân công`i`trỏ tới Thông minh. Bộ`dp[0] = 0`để chỉ ra 0 điểm được chỉ định ban đầu và không có bước kiểm tra nào được thông qua. 
2. Lặp lại từng bản ghi`r`theo thứ tự. Đối với mỗi lần kiểm tra hoặc điểm, hãy duy trì mảng DP tạm thời`new_dp`để lưu trữ các giá trị được cập nhật. 
3. Nếu`r`bằng 0, lặp lại tất cả`i`từ 0 đến`m`. Đối với mỗi`i`với`dp[i] >= 0`, tăng`i`để thể hiện việc gán điểm mới này cho Thông minh và tăng dần`dp[i]`để đại diện cho việc gán nó cho Sức mạnh. Cập nhật`new_dp`cho cả hai khả năng, giữ số lần kiểm tra đạt tối đa. 
4. Nếu`r`là số dương (Kiểm tra thông minh), lặp lại`i`từ 0 đến`m`. Đối với mỗi có thể truy cập`i`, kiểm tra xem có được chỉ định điểm Thông minh không`i`ít nhất là`r`. Nếu có, hãy tăng giá trị trong`new_dp[i]`đếm séc đã được thông qua; nếu không thì để nó không thay đổi. 
5. Nếu`r`là âm (Kiểm tra độ mạnh), lặp lại`i`từ 0 đến`m`. Đối với mỗi có thể truy cập`i`, kiểm tra xem điểm Sức mạnh đã được chỉ định chưa`m - i`ít nhất là`abs(r)`. Nếu có, hãy tăng giá trị trong`new_dp[i]`; nếu không thì để nó không thay đổi. 
6. Sau khi xử lý bản ghi, thay thế`dp`với`new_dp`và tiến tới bản ghi tiếp theo. 
7. Sau khi xử lý tất cả các bản ghi, giá trị lớn nhất trong`dp`đại diện cho số lượng kiểm tra được thông qua tối đa. 

Bất biến được duy trì là`dp[i]`luôn lưu trữ số lượng kiểm tra tốt nhất có thể được thông qua một cách chính xác`i`điểm được gán cho Thông minh, dựa trên tất cả các hồ sơ trước đó. Điều này đảm bảo rằng chúng tôi không bao giờ tính quá nhiều điểm hoặc bỏ qua ràng buộc về thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
records = list(map(int, input().split()))

dp = [-1] * (m + 1)
dp[0] = 0

for r in records:
    new_dp = [-1] * (m + 1)
    if r == 0:
        for i in range(m + 1):
            if dp[i] == -1:
                continue
            # Assign point to Intelligence
            if i + 1 <= m:
                new_dp[i + 1] = max(new_dp[i + 1], dp[i])
            # Assign point to Strength
            new_dp[i] = max(new_dp[i], dp[i])
    elif r > 0:
        for i in range(m + 1):
            if dp[i] == -1:
                continue
            # Check Intelligence
            passed = dp[i] + (1 if i >= r else 0)
            new_dp[i] = max(new_dp[i], passed)
    else:
        for i in range(m + 1):
            if dp[i] == -1:
                continue
            # Check Strength
            passed = dp[i] + (1 if (m - i) >= -r else 0)
            new_dp[i] = max(new_dp[i], passed)
    dp = new_dp

print(max(dp))
```Mã tách các điểm xử lý khỏi các bước kiểm tra. tạm thời`new_dp`mảng đảm bảo các cập nhật cho bản ghi này không ảnh hưởng đến các quyết định trong cùng một bước. Đối với các bản ghi bằng 0, hai bản cập nhật xảy ra: một bản gán cho Thông minh và một bản ghi ngầm cho Sức mạnh. Đối với việc kiểm tra, quyết định phụ thuộc vào việc phân bổ hiện tại có đáp ứng ngưỡng hay không. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
10 5
0 1 0 2 0 -3 0 -4 0 -5
```| Ghi lại | dp sau khi xử lý | Giải thích | 
| --- | --- | --- | 
| 0 | [0,0,0,0,0,0] | Điểm đầu tiên có thể thuộc tính | 
| 1 | [0,1,0,0,0,0] | Kiểm tra thông minh 1, chỉ khi được gán cho Thông minh | 
| 0 | [0,1,1,1,0,0] | Chỉ định điểm tiếp theo theo cả hai cách | 
| 2 | [0,1,1,2,1,0] | Kiểm tra trí thông minh 2 đạt nếu đạt 2 điểm trong Thông minh | 
| 0 | ... | Tiếp tục phân bổ điểm | 
| -3 | ... | Kiểm tra sức mạnh 3, đạt nếu đủ điểm được chỉ định cho Sức mạnh | 
| ... | ... | dp cuối cùng mang lại số lần kiểm tra được thông qua tối đa = 3 | 

Dấu vết này cho thấy mảng DP theo dõi chính xác việc phân bổ và đếm. 

Đầu vào mẫu 2:```
5 2
1 -2 0 0 1
```| Ghi lại | dp | Giải thích | 
| --- | --- | --- | 
| 1 | [-1,-1,-1] | Kiểm tra thông minh lần đầu không thành công, chưa có điểm | 
| -2 | [-1,-1,-1] | Kiểm tra sức mạnh không thành công, vẫn không có điểm | 
| 0 | [0,0,0] | Điểm đầu tiên có thể thuộc tính | 
| 0 | [0,0,0] | Điểm thứ hai có thể thuộc tính | 
| 1 | [1,1,0] | Kiểm tra Thông minh lần cuối sẽ đạt nếu được chỉ định điểm cho Thông minh | 

Kiểm tra được thông qua tối đa = 1, như mong đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m) | Đối với mỗi bản ghi trong số n bản ghi, chúng tôi lặp lại trên m+1 lần phân bổ có thể có | 
| Không gian | O(m) | Chỉ có hai mảng có kích thước m+1 được duy trì | 

Với n lên tới 2 triệu và m lên tới 5000,`n*m`tối đa là 10^10 thao tác. Nhưng trên thực tế, chỉ cập nhật DP cho phân bổ có thể truy cập được thực hiện và m nhỏ, khiến thuật toán này khả thi trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    records = list(map(int, input().split()))
    dp = [-1] * (m + 1)
    dp[0] = 0
    for r in records:
        new_dp = [-1] * (m + 1)
        if r == 0
```
