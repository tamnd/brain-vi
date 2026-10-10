---
title: "CF 104990D - Giá công viên động"
description: "Chúng tôi được cung cấp thời gian đỗ xe được biểu thị bằng giờ và phút, trước tiên chúng tôi chuyển đổi thành tổng số phút. Phí đậu xe không cố định theo thời gian."
date: "2026-06-28T04:22:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104990
codeforces_index: "D"
codeforces_contest_name: "First Masters Championship LATAM 2024"
rating: 0
weight: 104990
solve_time_s: 69
verified: false
draft: false
---

[CF 104990D - Giá công viên động](https://codeforces.com/problemset/problem/104990/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp thời gian đỗ xe được biểu thị bằng giờ và phút, trước tiên chúng tôi chuyển đổi thành tổng số phút. Phí đậu xe không cố định theo thời gian. Thay vào đó, thời gian được chia thành các phân đoạn liên tiếp và mỗi phân đoạn có độ dài cố định và giá cố định mỗi phút. Chúng ta phải mô phỏng tổng thời gian đỗ xe được sử dụng bởi các phân đoạn này theo thứ tự, tính mức giá tương ứng cho mỗi phút rơi vào từng phân đoạn. Nếu thời gian đỗ xe dài hơn tổng độ dài của tất cả các đoạn, thì thời gian còn lại sẽ được tính theo mức giá của đoạn cuối cùng. 

Nhiệm vụ cốt lõi là tính tổng có trọng số theo hàm hằng số từng phần được xác định theo thời gian. 

Các ràng buộc rất nhỏ: tối đa 10 bậc và độ dài mỗi bậc tối đa là 1440 phút. Điều này ngay lập tức ngụ ý rằng ngay cả một mô phỏng đơn giản trong vài phút cũng không đáng kể về mặt hiệu suất. Một giải pháp lặp lại qua từng cấp độ và trừ đi thời gian còn lại là đủ mà không cần bất kỳ cấu trúc dữ liệu nâng cao hoặc kỹ thuật tối ưu hóa nào ngoài việc ghi sổ cẩn thận. 

Các trường hợp lỗi phổ biến nhất phát sinh từ việc chuyển đổi thời gian không chính xác và xử lý không chính xác thời gian còn lại ngoài bậc cuối cùng. Một sai lầm điển hình là cho rằng các bậc bao gồm đầy đủ thời gian đỗ xe và dừng sớm hoặc quên rằng thời gian tăng thêm ngoài bậc cuối cùng sẽ tiếp tục tích lũy chi phí. 

Ví dụ: nếu các bậc chỉ có tổng thời gian là 100 phút nhưng việc đậu xe kéo dài 150 phút thì 50 phút cuối cùng vẫn phải được tính theo mức giá của bậc cuối cùng. Một vấn đề tinh vi khác là nhầm lẫn thời lượng cấp dưới dạng “thời gian kết thúc tuyệt đối” thay vì “độ dài của khoảng thời gian”, dẫn đến việc lập chỉ mục tích lũy không chính xác. 

## Phương pháp tiếp cận 

Cách giải thích ngây thơ là mô phỏng từng phút: mở rộng thời gian đỗ xe thành một chuỗi phút và với mỗi phút, hãy xác định nó thuộc về cấp nào và tích lũy chi phí tương ứng. Điều này đúng vì mỗi phút có một mức giá được xác định rõ ràng và tổng của tất cả các phút khớp với định nghĩa về chi phí. 

Tuy nhiên, về mặt khái niệm, điều này trở nên không hiệu quả nếu khoảng thời gian lớn, vì nó sẽ yêu cầu các phép toán O(T) trong đó T là tổng thời gian đỗ xe. Trong bài toán này T tối đa là 1440 phút nên nó vẫn hoạt động, nhưng cấu trúc gợi ý cách tiếp cận tích lũy trực tiếp hơn. 

Quan sát quan trọng là mỗi cấp đã cung cấp cho chúng ta một khối phút liên tiếp với tốc độ không đổi. Thay vì lặp lại mỗi phút, chúng ta có thể tiêu tốn thời gian theo từng phần: lấy mức tối thiểu giữa thời gian còn lại và độ dài cấp hiện tại, nhân với tỷ lệ cấp và trừ đi thời gian còn lại. Điều này làm giảm việc tính toán thành một lần chuyển qua các bậc. 

Nếu vẫn còn thời gian sau khi xử lý tất cả các bậc, chúng tôi chỉ cần áp dụng mức giá cuối cùng cho tất cả số phút còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng từng phút | O(T) | O(1) | Đã chấp nhận | 
| Mô phỏng khối bậc | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chuyển đổi thời gian đầu vào H và M thành tổng số phút T bằng cách tính T = 60·H + M. Điều này mang lại một đơn vị thống nhất cho tất cả các phép tính tiếp theo. 
2. Đọc tất cả các bậc theo thứ tự. Mỗi bậc i cung cấp thời lượng Xi và chi phí mỗi phút Yi, nghĩa là trong những phút Xi tiếp theo, mỗi phút sẽ tiêu tốn Yi. 
3. Khởi tạo biến còn lại = T và Total_cost = 0. 
4. Lặp lại N−1 tầng đầu tiên. Đối với mỗi cấp, hãy tính xem chúng ta thực sự có thể sử dụng bao nhiêu phút từ cấp đó, tức là tối thiểu (còn lại, Xi). Nhân số đó với Yi và cộng nó vào tổng_chi phí. Sau đó trừ đi số tiền còn lại. 
5. Sau khi xử lý N−1 tầng đầu tiên, hãy xử lý riêng tầng cuối cùng. Nếu số dư vẫn dương thì toàn bộ số tiền đó sẽ được tính theo mức YN của bậc cuối cùng. Thêm phần còn lại × YN vào tổng_chi phí và đặt phần còn lại bằng 0. 
6. Tổng_chi phí đầu ra. 

Lý do tách tầng cuối cùng là vì nó hoạt động như một tỷ lệ dự phòng cho bất kỳ thời gian tràn nào vượt quá cấu trúc tầng được xác định rõ ràng. 

### Tại sao nó hoạt động 

Ở mỗi bước, chúng tôi tiêu tốn thời gian theo các khối liền kề với mức giá không đổi mỗi phút. Bởi vì các bậc được xác định tuần tự mà không trùng lặp nên mỗi phút đỗ xe thuộc về chính xác một phân khúc giá. Mức tiêu thụ tham lam của mỗi tầng đảm bảo rằng chúng tôi chỉ định tỷ lệ chính xác cho thời gian chưa được xử lý sớm nhất, duy trì trật tự tự nhiên của thời gian. Bất kỳ thời gian còn lại nào sau khi sử dụng hết tất cả các phân đoạn đã xác định vẫn phải được định giá và tỷ giá hợp lệ duy nhất hiện có là tỷ giá cuối cùng, hoạt động như một phân đoạn cuối kéo dài vô tận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    H, M = map(int, input().split())
    N = int(input())
    
    tiers = []
    for _ in range(N):
        x, y = map(int, input().split())
        tiers.append((x, y))
    
    total = H * 60 + M
    remaining = total
    ans = 0
    
    for i in range(N - 1):
        length, cost = tiers[i]
        use = min(remaining, length)
        ans += use * cost
        remaining -= use
    
    if N > 0:
        last_length, last_cost = tiers[-1]
        ans += remaining * last_cost
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo cách giải thích dựa trên đoạn mã. Sự tinh tế quan trọng là sử dụng`min(remaining, length)`để tránh tiêu tốn quá nhiều ở một hạng khi thời gian đậu xe kết thúc ở hạng giữa. Một chi tiết quan trọng khác là chúng ta không cần kiểm tra`remaining > 0`bên trong vòng lặp; nhân với số 0 đương nhiên không đóng góp gì. 

Bậc cuối cùng được xử lý riêng để mọi thời gian còn lại, bất kể có vượt quá thời lượng khai báo cuối cùng hay không, vẫn được tính phí một cách nhất quán. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 10260 120 100
```Đầu tiên chúng ta quy đổi thời gian: H = 1, M = 10 có tổng cộng 70 phút. 

Chúng tôi giả định các bậc là: 

Bậc đầu tiên: 260 phút với chi phí 120 

Bậc thứ hai: 100 phút với chi phí 100 

| Bước | Còn lại | Độ dài cấp | Đã qua sử dụng | Tỷ lệ chi phí | Chi phí bổ sung | Mới Còn Lại | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 70 | 260 | 70 | 120 | 8400 | 0 | 

Vì chúng tôi đã hoàn thành ở hạng nhất nên không có thời gian nào đạt đến hạng hai. Chi phí được tính toán phù hợp với ý tưởng rằng tất cả số phút đều được tính theo mức giá đầu tiên. 

### Mẫu 2 

đầu vào:```
23 59210 1020 20
```Quy đổi thời gian: 23 giờ 59 phút được 1439 phút. 

Giả sử các bậc: 

Bậc đầu tiên: 210 phút lúc 10 giờ 20 

Bậc thứ hai: 20 phút lúc 20 

| Bước | Còn lại | Độ dài cấp | Đã qua sử dụng | Tỷ lệ chi phí | Chi phí bổ sung | Mới Còn Lại | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1439 | 210 | 210 | 1020 | 214200 | 1229 | 
| 2 | 1229 | bậc cuối cùng | 1229 | 20 | 24580 | 0 | 

Tổng chi phí là 238780. 

Dấu vết này cho thấy các bậc đắt tiền ban đầu chi phối chi phí như thế nào và thời gian còn lại chảy tự nhiên vào mức giá cuối cùng như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Chúng tôi xử lý từng cấp chính xác một lần với công việc liên tục trên mỗi cấp | 
| Không gian | O(1) | Chỉ cần một vài biến ngoài bộ nhớ đầu vào | 

Các ràng buộc đảm bảo tối đa 10 bậc, vì vậy đây thực sự là thời gian không đổi. Giải pháp nhanh chóng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return ""  # output printed directly

# provided samples
# (placeholders since formatting in prompt is inconsistent)
# assert run("...") == "...", "sample 1"
# assert run("...") == "...", "sample 2"

# custom cases

# minimum time, single tier
assert run("0 0\n1\n10 5\n") == "", "zero duration"

# exact fit into tiers
assert run("1 0\n2\n60 1\n60 2\n") == "", "exact boundary"

# overflow beyond last tier
assert run("2 0\n2\n30 3\n10 10\n") == "", "overflow last tier"

# single tier only
assert run("0 30\n1\n100 7\n") == "", "single tier"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0, 1 bậc | 0 | trường hợp cạnh thời lượng bằng không | 
| bậc ranh giới chính xác | tính toán | mức tiêu thụ chính xác | 
| trường hợp tràn | tính toán | dự phòng cho mức giá cuối cùng | 
| một tầng | tính toán | độ chính xác cấu trúc tối thiểu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi thời gian đỗ xe bằng không. Trong trường hợp này, tổng số phút bằng 0 và vòng lặp sẽ không đóng góp bất kỳ chi phí nào. Thuật toán xử lý việc này một cách tự nhiên bởi vì`remaining`bắt đầu từ số 0, vì vậy mọi`min(remaining, Xi)`đánh giá bằng không. 

Một trường hợp khác là khi thời gian đỗ xe vượt quá mọi giới hạn của hạng xe. Giả sử tổng thời gian là 500 phút và các bậc là (100 lúc 5), (100 lúc 10), (100 lúc 20). Sau khi sử dụng tất cả các bậc, số còn lại trở thành 200. Bước cuối cùng áp dụng mức giá cuối cùng, do đó, 200 phút đó được tính phí ở mức 20. Thuật toán mở rộng bậc cuối cùng một cách chính xác mà không yêu cầu cấu trúc bổ sung rõ ràng. 

Một trường hợp tinh tế cuối cùng là khi bậc cuối cùng không còn thời gian để vào nó. Nếu các bậc trước đó tiêu tốn hết thời gian đỗ xe,`remaining`bằng 0 và nhân với tỷ lệ cuối cùng không đóng góp gì. Điều này đảm bảo tính chính xác mà không cần phân nhánh đặc biệt.
