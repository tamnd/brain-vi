---
title: "CF 1854B - Kiếm tiền hoặc mở khóa"
description: "Chúng ta được đưa ra một tình huống trong đó người chơi muốn tối đa hóa thu nhập của mình trong khi mở khóa các kỹ năng trong trò chơi. Có n kỹ năng, mỗi kỹ năng yêu cầu một số kỹ năng đã được mở khóa trước đó và cung cấp phần thưởng bằng xu nếu được mở khóa."
date: "2026-06-09T05:11:32+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dp"]
categories: ["algorithms"]
codeforces_contest: 1854
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 889 (Div. 1)"
rating: 2200
weight: 1854
solve_time_s: 52
verified: true
draft: false
---

[CF 1854B - Kiếm tiền hoặc mở khóa](https://codeforces.com/problemset/problem/1854/B) 

**Xếp hạng:** 2200 
**Tags:** bitmasks, vũ phu, dp 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một tình huống trong đó người chơi muốn tối đa hóa thu nhập của mình trong khi mở khóa các kỹ năng trong trò chơi. có`n`kỹ năng, mỗi kỹ năng yêu cầu một số kỹ năng đã được mở khóa trước đó và cung cấp phần thưởng bằng xu nếu được mở khóa. Một số kỹ năng có thể được mở khóa ngay lập tức mà không cần điều kiện tiên quyết, trong khi những kỹ năng khác yêu cầu sự kết hợp của các kỹ năng trước đó. Mục tiêu là tính toán tổng số xu tối đa mà người chơi có thể kiếm được bằng cách chọn thứ tự mở khóa các kỹ năng tôn trọng các điều kiện tiên quyết này. 

Đầu vào mô tả phần thưởng của từng kỹ năng và các điều kiện tiên quyết của nó. Đầu ra là số tiền tối đa có thể đạt được dưới những ràng buộc. 

Các ràng buộc đủ nhỏ để`n`lên tới 20. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào lặp lại trên tất cả các tập hợp con kỹ năng đều khả thi vì`2^20`là khoảng một triệu, vừa vặn trong giới hạn tính toán thông thường. Tuy nhiên, những cách tiếp cận ngây thơ thử tất cả các hoán vị của các lệnh mở khóa sẽ yêu cầu`n!`các hoạt động quá lớn đối với`n=20`. Vì vậy, chúng ta cần xem xét các kỹ thuật như lập trình động trên các tập con hoặc mặt nạ bit. 

Các trường hợp cạnh rất tinh tế. Một là kỹ năng ban đầu có thể mở khóa nhưng không có phần thưởng; một kỹ năng khác đòi hỏi tất cả các kỹ năng khác để mở khóa. Ví dụ: với ba kỹ năng trong đó kỹ năng 3 yêu cầu kỹ năng 1 và 2, thứ tự tối ưu phải xem xét mở khóa 1 và 2 trước 3. Cách tiếp cận tham lam bất cẩn chỉ chọn phần thưởng cao nhất trước tiên có thể thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là xem xét mọi thứ tự mở khóa các kỹ năng. Với mỗi hoán vị của`n`kỹ năng, chúng tôi kiểm tra xem các điều kiện tiên quyết có được đáp ứng ở mỗi bước hay không, tổng hợp phần thưởng của các kỹ năng đã mở khóa. Mặc dù điều này đảm bảo tính chính xác nhưng nó đòi hỏi`n!`trong trường hợp xấu nhất là không thể thực hiện được`n=20`. 

Cách tiếp cận tối ưu tận dụng lập trình động bitmask. Mỗi tập hợp con các kỹ năng có thể được biểu diễn dưới dạng bitmask có độ dài`n`, trong đó một bit được đặt nếu kỹ năng tương ứng được mở khóa. trạng thái DP`dp[mask]`đại diện cho số tiền tối đa có thể đạt được sau khi mở khóa tập hợp con`mask`. Quan sát quan trọng là chúng ta có thể lặp lại tất cả các tập hợp con và đối với mỗi tập hợp con, hãy cân nhắc thêm một kỹ năng mới nếu các điều kiện tiên quyết của nó được đáp ứng trong mặt nạ hiện tại. Điều này làm giảm không gian giải pháp từ`n!`hoán vị để`n * 2^n`chuyển tiếp, có thể chấp nhận được đối với`n ≤ 20`. 

Brute-force hoạt động vì nó xem xét tất cả các khả năng, nhưng thất bại khi`n`là lớn. Quan sát cho thấy vấn đề có thể được biểu thị dưới dạng chuyển đổi giữa các tập hợp con cho phép chúng tôi giảm nó xuống DP trên mặt nạ bit, điều này hiệu quả và tránh được những kiểm tra không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Mặt nạ bit DP | O(n * 2^n) | O(2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng ca kiểm thử, sau đó lặp lại từng trường hợp kiểm thử. 
2. Đối với mỗi kỹ năng, hãy lưu trữ phần thưởng xu và mặt nạ bit của các điều kiện tiên quyết. 
3. Khởi tạo một mảng DP có kích thước`2^n`với âm vô cực, ngoại trừ`dp[0] = 0`vì không có kỹ năng nào được mở khóa mang lại số tiền bằng không. 
4. Lặp lại tất cả các mặt nạ từ`0`ĐẾN`2^n - 1`. Đối với mỗi mặt nạ, hãy xem xét mở khóa một kỹ năng mới`i`vẫn chưa có mặt nạ. 
5. Nếu mặt nạ hiện tại chứa tất cả các điều kiện tiên quyết cho kỹ năng`i`, tính toán mặt nạ mới bằng cách thiết lập bit`i`. Cập nhật`dp[new_mask]`là mức tối đa của giá trị hiện tại của nó và`dp[mask] + coins[i]`. 
6. Sau khi xử lý tất cả các mặt nạ, câu trả lời là giá trị tối đa trong mảng DP, tương ứng với bất kỳ tập hợp con nào của các kỹ năng đã được mở khóa. 

Tính bất biến đó là`dp[mask]`luôn nắm giữ số tiền tối đa có thể đạt được cho bộ kỹ năng đã mở khóa chính xác đó. Chúng tôi không bao giờ bỏ qua bất kỳ quá trình chuyển đổi hợp lệ nào vì chúng tôi khám phá một cách có hệ thống mọi tập hợp con và mọi kỹ năng bổ sung. Không có chuỗi hợp lệ nào bị bỏ sót, vì vậy giá trị tối đa được tính ở cuối được đảm bảo là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        coins = []
        prereq_masks = []
        for _ in range(n):
            parts = list(map(int, input().split()))
            c = parts[0]
            k = parts[1]
            prereqs = parts[2:] if k > 0 else []
            mask = 0
            for p in prereqs:
                mask |= 1 << (p - 1)
            coins.append(c)
            prereq_masks.append(mask)
        
        dp = [-1] * (1 << n)
        dp[0] = 0
        for mask in range(1 << n):
            if dp[mask] == -1:
                continue
            for i in range(n):
                if not (mask & (1 << i)) and (mask & prereq_masks[i]) == prereq_masks[i]:
                    new_mask = mask | (1 << i)
                    dp[new_mask] = max(dp[new_mask], dp[mask] + coins[i])
        
        print(max(dp))

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào một cách hiệu quả và xây dựng mặt nạ cho các điều kiện tiên quyết. Mảng DP theo dõi số tiền tối đa cho mỗi tập hợp con và quá trình chuyển đổi chỉ xảy ra khi các điều kiện tiên quyết được đáp ứng. Các thao tác bitwise cẩn thận đảm bảo chúng tôi không bao giờ mở khóa kỹ năng sớm. sử dụng`-1`đối với các trạng thái chưa được khởi tạo sẽ tránh việc vô tình tính tổng với các tập hợp con không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
10 0
20 1 1
30 2 1 2
```| mặt nạ | dp[mặt nạ] | Ghi chú | 
| --- | --- | --- | 
| 000 | 0 | trường hợp cơ sở | 
| 001 | 10 | mở khóa kỹ năng 1 | 
| 011 | 30 | mở khóa kỹ năng 2 sau 1 | 
| 111 | 60 | mở khóa kỹ năng 3 sau 1 và 2 | 

Dấu vết này xác nhận các điều kiện tiên quyết được thực thi và các bản cập nhật dp tích lũy tiền một cách chính xác. 

### Ví dụ 2 

đầu vào:```
1
2
5 0
10 1 1
```| mặt nạ | dp[mặt nạ] | Ghi chú | 
| --- | --- | --- | 
| 00 | 0 | trường hợp cơ sở | 
| 01 | 5 | mở khóa kỹ năng 1 | 
| 11 | 15 | mở khóa kỹ năng 2 sau 1 | 

Bảng hiển thị DP thu được số tiền tối đa cho mỗi tập hợp con và xử lý số tiền nhỏ`n`đúng cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 2^n) | Đối với mỗi tập hợp con trong số 2^n, chúng tôi xem xét n kỹ năng chuyển tiếp. | 
| Không gian | O(2^n) | Chúng tôi lưu trữ giá trị DP cho mọi tập hợp con kỹ năng. | 

Đối với n 20, 2^20 ≈ 1 triệu và n * 2^n ≈ 20 triệu thao tác, vừa vặn thoải mái trong giới hạn 2 giây thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided sample
assert run("1\n3\n10 0\n20 1 1\n30 2 1 2\n") == "60", "sample 1"

# Custom cases
assert run("1\n2\n5 0\n10 1 1\n") == "15", "simple two skills"
assert run("1\n1\n100 0\n") == "100", "single skill"
assert run("1\n3\n1 0\n1 1 1\n1 2 1 2\n") == "3", "chain unlocks"
assert run("1\n3\n5 0\n0 1 1\n10 1 1\n") == "15", "skill with zero reward"
assert run("1\n4\n1 0\n2 1 1\n3 2 1 2\n4 3 1 2 3\n") == "10", "all skills unlockable in sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 kỹ năng, tuần tự | 15 | điều kiện tiên quyết cơ bản | 
| 1 kỹ năng duy nhất | 100 | kích thước đầu vào tối thiểu | 
| 3 kỹ năng bị xiềng xích | 3 | mở khóa tuần tự với số tiền tối thiểu | 
| kỹ năng không thưởng | 15 | thuật toán xử lý chính xác phần thưởng bằng 0 | 
| Chuỗi 4 kỹ năng đầy đủ | 10 | tích lũy chuỗi đầy đủ | 

## Vỏ cạnh 

Khi một kỹ năng không có phần thưởng nhưng được yêu cầu đối với người khác, DP chỉ đưa nó vào một cách chính xác nếu cần thiết. Ví dụ: 

đầu vào:```
1
3
0 0
5 1 1
10 1 2
```Dấu vết DP: 

| mặt nạ | dp[mặt nạ] | 
| --- | --- | 
| 000 | 0 | 
| 001 | 0 | 
| 011 | 5 | 
| 111 | 15 | 

Thuật toán chọn kỹ năng 1 mặc dù phần thưởng bằng 0 vì nó kích hoạt kỹ năng 2. Điều này xác nhận DP thực thi chính xác các điều kiện tiên quyết ngay cả khi một số phần thưởng bằng 0.
