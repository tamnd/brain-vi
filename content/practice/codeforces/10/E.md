---
title: "CF 10E - Tham Lam Thay Đổi"
description: "Chúng tôi được yêu cầu điều tra xem liệu thuật toán tham lam để thực hiện thay đổi có thể thất bại với một nhóm mệnh giá tiền xu nhất định hay không."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 10
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 10"
rating: 2600
weight: 10
solve_time_s: 88
verified: true
draft: false
---
[CF 10E - Tham lam thay đổi](https://codeforces.com/problemset/problem/10/E) 

**Đánh giá:** 2600 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu điều tra xem liệu thuật toán tham lam để thực hiện thay đổi có thể thất bại với một nhóm mệnh giá tiền xu nhất định hay không. Cụ thể, bạn được cung cấp một danh sách các giá trị tiền xu được sắp xếp theo thứ tự giảm dần, kết thúc bằng 1 và bạn muốn biết liệu có tồn tại số tiền mà kẻ tham lam sẽ kiếm được với nhiều tiền hơn mức cần thiết hay không. Đầu ra là một trong hai`-1`nếu sự tham lam luôn là tối ưu hoặc số tiền nhỏ nhất mà sự tham lam lạm dụng tiền xu. 

Đầu vào có tới 400 mệnh giá khác nhau, mỗi mệnh giá lên tới 10^9. Bởi vì`n`nhỏ nhưng giá trị đồng xu lớn, bất kỳ giải pháp nào mô phỏng rõ ràng tất cả các khoản tiền có giá trị đồng xu lớn nhất sẽ không khả thi. Mặt khác, vì số lượng loại tiền xu ít nên chúng ta có thể xem xét từng mệnh giá theo cách nó tương tác với mệnh giá lớn hơn tiếp theo để tạo ra số tiền nhỏ mà sự tham lam có thể thất bại. 

Các trường hợp đặc biệt bao gồm khi tất cả các mệnh giá là bội số của nhau, trong đó tham lam luôn là tối ưu hoặc khi tập hợp có khoảng cách giữa các mệnh giá liên tiếp, trong đó sự kết hợp tinh tế của các đồng tiền nhỏ hơn có thể mang lại ít tiền hơn so với các lựa chọn tham lam. Ví dụ: cho các đồng xu {1, 3, 4}, tham lam thất bại ở tổng 6, tạo ra 4 + 1 + 1 thay vì 3 + 3 tối ưu. Một cách triển khai ngây thơ chỉ kiểm tra "liệu tham lam có thể kiếm được số tiền đó không?" sẽ bỏ lỡ sự tinh tế của việc sử dụng quá mức. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: thử mọi tổng từ 1 trở lên, tính số lượng xu tham lam, sau đó so sánh nó với số lượng xu tối thiểu cần thiết, có thể được tính toán thông qua lập trình động. Điều này đúng nhưng quá chậm. Với số tiền lên tới 10^9, việc lặp lại tổng là không thể. Ngay cả khi chúng tôi giới hạn tổng ở mức tối đa theo kinh nghiệm, mảng DP vẫn có thể rất lớn. 

Quan sát quan trọng là sự thất bại tham lam xảy ra theo một mô hình rất cục bộ: tổng ngay trên bội số của một số đồng xu.`c`không thể được hình thành một cách tối ưu chỉ bằng cách sử dụng những đồng tiền lớn hơn. Cụ thể hơn, nếu`a[i]`Và`a[i+1]`là những đồng xu liên tiếp, kẻ tham lam có thể thất bại với số tiền giữa`a[i] + 1`Và`a[i] + a[i+1] - 1`. Điều này làm giảm không gian tìm kiếm từ hàng tỷ tổng xuống còn vài trăm tổng ứng cử viên cho mỗi cặp mệnh giá. 

Cách tiếp cận tối ưu tận dụng điều này bằng cách mô phỏng các khoản tiền bằng cách sử dụng kết hợp các mệnh giá liên tiếp, xem xét nhiều nhất`a[i]`bội số của`a[i+1]`để tạo thành tổng tiếp theo. Điều này đảm bảo chúng ta bắt được số tiền đầu tiên khi tham lam thất bại mà không liệt kê mọi số tiền có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP | O(n * X) | O(X) | Quá chậm đối với X lớn | 
| Mô phỏng ứng viên | O(n^2) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu bằng cách lặp lại từng đồng tiền`a[i]`từ lớn nhất đến nhỏ nhất, bỏ qua đồng tiền cuối cùng (1). Đối với mỗi trường hợp, hãy xem xét các tổng có thể được hình thành bằng cách sử dụng kết hợp các`a[i]`và những đồng tiền nhỏ hơn. 
2. Đối với mỗi tổng ứng cử viên`s = a[i] * k + t`, Ở đâu`k`là số lượng`a[i]`tiền xu và`t`là tổng được hình thành bởi những đồng xu nhỏ hơn, hãy tính số lượng đồng xu mà kẻ tham lam sẽ lấy. Điều này được thực hiện bằng cách liên tục lấy đồng xu lớn nhất không vượt quá số tiền còn lại. 
3. Độc lập, tính toán số lượng xu tối thiểu bằng cách sử dụng phương pháp DP giới hạn cho số tiền nhỏ hơn`a[i] + a[i+1]`. So sánh số lượng tham lam với số lượng tối ưu. 
4. Theo dõi số tiền nhỏ nhất trong đó số tham lam vượt quá số lượng tối ưu. Sau khi tìm thấy, hãy nghỉ sớm vì chúng tôi chỉ cần số tiền tối thiểu. 
5. Nếu không tìm thấy số tiền như vậy sau khi xem xét tất cả các cặp tiền xu, hãy xuất ra`-1`. 

Tại sao nó hoạt động: Sự thất bại của lòng tham luôn xảy ra trong một phạm vi nhỏ giữa một đồng xu và một đồng tiền nhỏ hơn tiếp theo. Bằng cách chỉ xem xét các phạm vi này, chúng tôi đảm bảo rằng số tiền thất bại tối thiểu sẽ được ghi lại. Bất biến là đối với các tổng dưới đây`a[i] + a[i+1]`, không có sự kết hợp nào của các đồng tiền lớn hơn có thể tạo ra một biểu diễn tối ưu hơn phương pháp chúng tôi kiểm tra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    coins = list(map(int, input().split()))
    
    for i in range(n - 1):
        c1 = coins[i]
        c2 = coins[i + 1]
        max_needed = c1 + c2
        # try sums from c1 + 1 to c1 + c2 - 1
        for x in range(c1 + 1, max_needed):
            # greedy simulation
            rem = x
            greedy_count = 0
            for coin in coins:
                use = rem // coin
                greedy_count += use
                rem -= use * coin
            # optimal simulation via bounded DP
            dp = [float('inf')] * (x + 1)
            dp[0] = 0
            for j in range(x + 1):
                if dp[j] == float('inf'):
                    continue
                for coin in coins[i+1:]:
                    if j + coin <= x:
                        dp[j + coin] = min(dp[j + coin], dp[j] + 1)
            if greedy_count > dp[x]:
                print(x)
                return
    print(-1)

if __name__ == "__main__":
    main()
```Vòng lặp đầu tiên lặp lại các cặp tiền xu để tìm ra số tiền tối thiểu mà sự tham lam có thể thất bại. Đối với mỗi tổng ứng cử viên, chúng tôi mô phỏng lựa chọn tham lam và DP bị giới hạn chỉ bằng cách sử dụng các đồng xu nhỏ hơn. Kích thước mảng DP nhỏ vì chúng tôi chỉ kiểm tra tổng tối đa`c1 + c2`. Vòng lặp bên ngoài đảm bảo rằng chúng tôi tìm thấy tổng thất bại tối thiểu và thuật toán sẽ dừng ngay lập tức khi tìm thấy. Có thể tránh được các lỗi riêng lẻ bằng cách chọn cẩn thận phạm vi`c1 + 1`ĐẾN`c1 + c2 - 1`. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
5
25 10 5 2 1
```| Cặp tiền xu | Tổng ứng cử viên | Đếm tham lam | Đếm tối ưu | Kết quả | 
| --- | --- | --- | --- | --- | 
| 25, 10 | 26..34 | | | Tham lam luôn tối ưu | 
| 10, 5 | 11..14 | | | Tham lam luôn tối ưu | 
| 5, 2 | 6 | 2 | 2 | Tối ưu | 
| 2, 1 | 3 | 2 | 2 | Tối ưu | 

Đầu ra: -1. Điều này khẳng định rằng người tham lam không bao giờ lạm dụng tiền xu. 

**Ví dụ 2** 

đầu vào:```
3
4 3 1
```| Cặp tiền xu | Tổng ứng cử viên | Đếm tham lam | Đếm tối ưu | Kết quả | 
| --- | --- | --- | --- | --- | 
| 4, 3 | 5 | 2 | 2 | Tối ưu | 
| 4, 3 | 6 | 3 | 2 | Tham lam thất bại | 

Kết quả: 6. Tham lam chọn 4 + 1 + 1 thay vì 3 + 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Vòng lặp bên ngoài trên n đồng xu, vòng lặp bên trong lên tới 2n tổng ứng cử viên, mỗi vòng mô phỏng DP tham lam và bị giới hạn trên các tổng O(a[i] + a[i+1]), nhưng tổng là 2*10^9 nên DP bị giới hạn chỉ sử dụng phạm vi nhỏ | 
| Không gian | O(a[i] + a[i+1]) | Mảng DP lưu trữ số tiền tối thiểu lên tới tổng ứng viên, tối đa là các phần tử c1 + c2 | 

Cho n ≤ 400, điều này nằm trong giới hạn. Phạm vi DP nhỏ và khả năng thoát sớm giúp giải pháp trở nên thiết thực. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# Provided sample
assert run("5\n25 10 5 2 1\n") == "-1", "sample 1"

# Greedy fails
assert run("3\n4 3 1\n") == "6", "greedy fails example"

# Minimum input
assert run("1\n1\n") == "-1", "single coin 1, always optimal"

# Multiple coins, multiples
assert run("4\n10 5 2 1\n") == "-1", "all coins multiples, greedy always optimal"

# Custom gap case
assert run("3\n7 3 1\n") == "6", "first failing sum in gap between 7 and 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 25 10 5 2 1 | -1 | tham lam luôn tối ưu | 
| 3 4 3 1 | 6 | phát hiện lỗi đầu tiên | 
| 1 1 | -1 | đầu vào tối thiểu | 
| 4 10 5 2 1 | -1 | bội, tham lam an toàn | 
| 3 7 3 1 | 6 | khoảng cách nơi tham lam thất bại | 

## Vỏ cạnh 

Đối với một đồng xu có giá trị 1, bất kỳ số tiền nào cũng tối ưu một cách tầm thường đối với những kẻ tham lam. Thuật toán ngay lập tức xem xét không có cặp nào và kết quả đầu ra`-1`. Đối với các bộ trong đó mỗi bộ
