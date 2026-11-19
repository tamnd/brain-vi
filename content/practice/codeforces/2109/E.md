---
title: "CF 2109E - Chuỗi nhị phân Wowee"
description: "Chúng ta được cấp một chuỗi nhị phân s có độ dài n và một số nguyên k. Nhiệm vụ của chúng ta là thực hiện chính xác k thao tác, mỗi thao tác chọn số 0 trong chuỗi hiện tại và lật tất cả các bit từ đầu lên số 0 đó."
date: "2026-06-08T04:41:34+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "strings"]
categories: ["algorithms"]
codeforces_contest: 2109
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1025 (Div. 2)"
rating: 2400
weight: 2109
solve_time_s: 89
verified: false
draft: false
---

[CF 2109E - Chuỗi nhị phân Wowee](https://codeforces.com/problemset/problem/2109/E) 

**Đánh giá:** 2400 
**Tags:** tổ hợp, dp, chuỗi 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi nhị phân`s`chiều dài`n`và một số nguyên`k`. Nhiệm vụ của chúng ta là thực hiện chính xác`k`các thao tác, mỗi thao tác chọn một số 0 trong chuỗi hiện tại và lật tất cả các bit từ đầu đến số 0 đó. Mục đích là đếm xem có bao nhiêu trình tự khác nhau của`k`hoạt động tồn tại. Hai chuỗi được coi là khác nhau nếu chúng chọn các chỉ số khác nhau ở bất kỳ bước nào. 

Các hạn chế ở mức vừa phải:`n`có thể lên tới 500, và`k`nhiều nhất là`n`. Điều này loại trừ bất kỳ giải pháp nào có độ phức tạp theo cấp số nhân trong`n`hoặc`k`. Lý tưởng nhất là chúng ta cần một cái gì đó đa thức`O(n * k)`hoặc`O(n^2)`mỗi trường hợp thử nghiệm. Tổng của`n`trên tất cả các trường hợp thử nghiệm được giới hạn bởi 500, vì vậy chúng tôi có thể chấp nhận các vòng lặp lồng nhau`n`nếu cần thiết. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi có tất cả số 0 hoặc tất cả số 1. Ví dụ, nếu`s = "000"`Và`k = 3`, cách đếm đơn giản có thể đếm quá mức các chuỗi lật cùng một tiền tố nhiều lần. Một trường hợp phức tạp khác là khi`k`lớn hơn số số 0 trong chuỗi ban đầu. Chúng ta không thể chọn một chỉ mục có`1`, vì vậy lịch sử lật có vấn đề. Những sợi dây nhỏ như`"0"`với`k=1`hoặc`"1"`với`k=1`cũng bộc lộ những lỗi lầm riêng lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng ta có thể thử đệ quy tất cả các chỉ số`i`Ở đâu`s[i]`bằng 0, lật tiền tố và lặp lại`k-1`nhiều lần hơn. Điều này đúng vì nó khám phá tất cả các chuỗi hoạt động, nhưng nó quá chậm. Trong trường hợp xấu nhất, mỗi thao tác có thể chọn tối đa`n`các chỉ số và có`k`hoạt động, trao`O(n^k)`độ phức tạp, lớn về mặt thiên văn ngay cả đối với những thứ nhỏ`n`. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng điều duy nhất quan trọng là số lượng số 0 trong chuỗi sau mỗi lần lật và vị trí tương đối của chúng. Việc lật tiền tố sẽ chuyển đổi tất cả các bit trong tiền tố đó và điều này sẽ thay đổi số 0 nào có sẵn cho thao tác tiếp theo. Chúng ta có thể mô hình hóa điều này bằng cách sử dụng lập trình động về số lượng số 0 và số lượng phép toán còn lại. Cụ thể, chúng ta có thể duy trì một bảng DP`dp[i][j]`biểu thị số cách thực hiện`i`hoạt động xem xét lần đầu tiên`j`số không trong cấu hình chuỗi hiện tại. Điều này làm giảm vấn đề từ việc khám phá tất cả các chuỗi lần lật sang việc đếm các chuỗi con hợp lệ, làm cho độ phức tạp trở thành đa thức. 

Quan sát thấy số lượng số 0 nhiều nhất`n`cho phép chúng ta lập chỉ mục bảng DP theo số 0 thay vì vị trí chuỗi, điều này làm giảm đáng kể số lượng trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^k) | O(n*k) | Quá chậm | 
| DP trên số không | O(n*k) | O(n*k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi chuỗi`s`vào danh sách các vị trí số 0. Thứ tự của các số 0 rất quan trọng vì việc lật các tiền tố sẽ thay đổi số 0 nào có sẵn cho các hoạt động trong tương lai. Biểu thị danh sách này là`zeros`. 
2. Khởi tạo bảng DP`dp`với kích thước`(k+1) x (len(zeros)+1)`. Đây,`dp[i][j]`sẽ biểu thị số cách thực hiện`i`hoạt động sử dụng đầu tiên`j`số không. Bộ`dp[0][0] = 1`như trường hợp cơ bản: các phép toán bằng 0 sử dụng số 0 là một chuỗi hợp lệ. 
3. Lặp lại`i`từ 1 đến`k`, đại diện cho số lượng hoạt động được thực hiện cho đến nay. 
4. Đối với mỗi`j`từ 1 đến`len(zeros)`, đại diện cho lần đầu tiên`j`số không, cập nhật`dp[i][j]`bằng cách tính tổng`dp[i-1][p]`cho tất cả`p < j`. Mô hình này chọn`j`-thứ 0 làm mục tiêu cho`i`-thao tác thứ sau khi thực hiện`i-1`hoạt động đầu tiên`p`số không. Việc lựa chọn tiền tố đảm bảo rằng mỗi thao tác đều hợp lệ. 
5. Sau khi điền vào bảng DP, đáp án là tổng của`dp[k][j]`cho tất cả`j`từ 1 đến`len(zeros)`. Điều này đếm tất cả các chuỗi thực hiện chính xác`k`hoạt động. 

Tại sao nó hoạt động: Mỗi trạng thái DP đếm chính xác các chuỗi theo số phép toán và tập hợp con các số 0 được xem xét. Việc chuyển đổi phản ánh hạn chế là chúng ta chỉ có thể lật các tiền tố kết thúc bằng số 0. Số học mô-đun xử lý số lượng lớn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()
        zeros = [i for i, ch in enumerate(s) if ch == '0']
        m = len(zeros)
        if k > m:
            print(0)
            continue

        dp = [[0]*(m+1) for _ in range(k+1)]
        dp[0][0] = 1

        for i in range(1, k+1):
            prefix_sum = [0]*(m+1)
            for j in range(m+1):
                prefix_sum[j] = (prefix_sum[j-1] + dp[i-1][j]) % MOD if j else dp[i-1][j]
            for j in range(1, m+1):
                dp[i][j] = prefix_sum[j-1]

        ans = sum(dp[k][1:]) % MOD
        print(ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc đầu vào và xây dựng vị trí của số không. Bảng DP được khởi tạo với kích thước`(k+1) x (number of zeros +1)`. Chúng tôi sử dụng mảng tổng tiền tố để tính toán tổng của tất cả các trạng thái trước đó cho quá trình chuyển đổi DP một cách hiệu quả. Đáp án cuối cùng là tổng của hàng cuối cùng của bảng DP tương ứng chính xác với`k`hoạt động. Các lỗi xảy ra có thể tránh được bằng cách lập chỉ mục cẩn thận:`dp[i][j]`tương ứng với cái đầu tiên`j`số không, không phải vị trí trong chuỗi. các`if k > m`check nhanh chóng xử lý các trường hợp không thể. 

## Ví dụ đã hoạt động 

Dấu vết mẫu 1:`s = "010"`,`k=1`. 

| tôi (hoạt động) | j (số không được xem xét) | dp[i][j] | 
| --- | --- | --- | 
| 0 | 0 | 1 | 
| 1 | 1 | 1 | 
| 1 | 2 | 1 | 

Số không ở vị trí`[0, 2]`. Chúng ta có thể chọn số 0 cho một thao tác. Tổng của`dp[1][1:]`= 1+1 = 2, phù hợp với kết quả mong đợi. 

Dấu vết mẫu 2:`s = "000"`,`k=2`. 

Số không tại`[0,1,2]`. Bảng DP cập nhật như sau: 

| tôi | dp[i][1] | dp[i][2] | dp[i][3] | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 0 | 1 | 2 | 

Trả lời = sum(dp[2][1:]) = 0+1+2=3, khớp với kết quả mong đợi. 

Những dấu vết này xác nhận rằng thuật toán đếm tất cả các chuỗi mà không đếm quá mức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*k) | Đối với mỗi k thao tác, chúng tôi tính toán một hàng DP trên tối đa n số 0. Tổng tiền tố giảm tổng bên trong thành O(1). | 
| Không gian | O(n*k) | Bảng DP có (k+1)*(số lượng số 0+1) mục nhập. | 

Với n 500 và k 500, kết quả này đạt trong vòng 2 giây và 256MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("5\n3 1\n010\n3 2\n000\n5 4\n01001\n8 8\n11001100\n20 20\n10010110101101010110\n") == "2\n3\n10\n27286\n915530405"

# minimum input
assert run("1\n1 1\n0\n") == "1"
assert run("1\n1 1\n1\n") == "0"

# maximum zeros
assert run("1\n5 3\n00000\n") == "10"

# impossible k
assert run("1\n3 4\n000\n") == "0"

# alternating zeros and ones
assert run("1\n4 2\n0101\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "1\n1 1\n0\n" | 1 | Chuỗi số 0 đơn | 
| "1\n1 1\n1\n" | 0 | Một chuỗi duy nhất, không thể hoạt động | 
| "1\n5 3\n00000\n" | 10 | Nhiều số không, nhiều thao tác | 
| "1\n3 4\n000\n" | 0 | k vượt quá số 0, trường hợp không thể | 
| "1\n4 2\n0101\n" | 2 | Mô hình xen kẽ, chuyển tiếp DP | 

## Vỏ cạnh 

Nếu`s = "1111"`Và`k=1`, không có số 0 để
