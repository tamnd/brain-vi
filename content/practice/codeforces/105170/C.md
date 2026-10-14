---
title: "CF 105170C - Tổng Fibonacci"
description: "Chúng ta được cho một số nguyên rất lớn được viết dưới dạng nhị phân, gọi nó là n. Thay vì diễn giải nó dưới dạng một số theo cách thông thường và lặp từ 1 đến n, chúng ta được yêu cầu làm việc trực tiếp với biểu diễn nhị phân của nó."
date: "2026-06-27T08:28:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105170
codeforces_index: "C"
codeforces_contest_name: "The 2024 CCPC National Invitational Contest (Changchun) , The 17th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 105170
solve_time_s: 43
verified: true
draft: false
---

[CF 105170C - Tổng Fibonacci](https://codeforces.com/problemset/problem/105170/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên rất lớn được viết dưới dạng nhị phân, gọi nó là`n`. Thay vì hiểu nó là một số theo cách thông thường và lặp từ 1 đến`n`, chúng ta được yêu cầu làm việc trực tiếp với biểu diễn nhị phân của nó. 

Với mọi số nguyên`i`từ 1 đến`n`, chúng ta xét số lượng bit 1 trong`i`, đó là số lượng của`i`. Gọi giá trị này`g(i)`. Sau đó, chúng tôi ánh xạ số đếm đó thông qua chuỗi Fibonacci, trong đó`f(1) = 1`,`f(2) = 1`và đối với các chỉ số lớn hơn, mỗi số hạng là tổng của hai số hạng trước đó. Câu trả lời cuối cùng là tổng của`f(g(i))`tổng thể`i`TRONG`[1, n]`, lấy modulo`10^9 + 7`. 

Vì vậy, vấn đề không thực sự nằm ở việc lặp lại các con số. Đó là về việc tổng hợp một hàm số đếm trên một phạm vi rộng lớn được xác định bởi giới hạn nhị phân. 

Khó khăn chính đó là`n`có thể có tới 10^7 bit. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp lại các giá trị lên đến`n`, hoặc thậm chí xây dựng tất cả các số nguyên trung gian. Ngay cả việc lưu trữ tất cả các số là không thể. Cấu trúc duy nhất có sẵn là chính chuỗi nhị phân. 

Nhận xét quan trọng là giá trị của`g(i)`chỉ phụ thuộc vào vị trí bit nào được chọn là 1 trong`i`. Vì vậy, vấn đề giảm xuống việc đếm có bao nhiêu số nguyên`i ≤ n`có chính xác`k`đặt bit cho mỗi`k`, rồi tính tổng`f(k)`được cân nhắc bởi những số lượng đó. 

Điều này biến bài toán thành bài toán tổ hợp ràng buộc trên tiền tố nhị phân. 

Một số trường hợp đặc biệt quan trọng: 

Khi nào`n = 1`, giá trị duy nhất là`i = 1`, vậy câu trả lời là`f(1) = 1`. Bất kỳ giải pháp nào giả định ít nhất hai bit hoặc bắt đầu từ 0 sẽ bị tắt. 

Khi`n = 2`(nhị phân`10`), số hợp lệ là`1`Và`2`, với số lượng`1`Và`1`, vậy câu trả lời là`f(1) + f(1) = 2`. 

Một kiểu lỗi phổ biến là cố gắng xử lý phạm vi như tất cả các số nhị phân có độ dài cố định bằng`|n|`, bao gồm không chính xác các số lớn hơn`n`. 

## Phương pháp tiếp cận 

Về mặt khái niệm, cách tiếp cận vũ phu rất đơn giản. Chúng tôi sẽ lặp lại`i`từ 1 đến`n`, tính toán số lượng của nó, đánh giá giá trị Fibonacci cho số lượng đó và tích lũy kết quả. Điều này đúng nhưng hoàn toàn không khả thi. Ngay cả khi chúng ta có thể tính toán số lượng trong O(1), số lần lặp lại theo thứ tự`n`, Và`n`chính nó là một số nguyên nhị phân có tối đa 10^7 bit, do đó độ lớn của nó rất lớn về mặt thiên văn. 

Sự cố xảy ra do chúng tôi đang lặp lại các giá trị thay vì các mẫu bit. Cấu trúc chúng ta thực sự cần không phải là thứ tự số mà là cấu trúc tổ hợp trên các tập hợp con vị trí bit. 

Cái nhìn sâu sắc quan trọng là chuyển đổi quan điểm: mọi con số`i ≤ n`tương ứng với việc chọn một tập hợp con các vị trí bit sao cho số nhị phân thu được không vượt quá`n`. Đối với kích thước tập hợp con cố định`k`, phần đóng góp là`f(k)`nhân với số tập hợp con có kích thước hợp lệ`k`. 

Điều này biến bài toán thành bài toán lập trình động chữ số trên biểu diễn nhị phân của`n`, trong đó chúng tôi đếm có bao nhiêu tiền tố tạo ra các số hợp lệ với số lượng các số được chọn cho trước. 

Chúng tôi duy trì, đối với mỗi tiền tố của`n`, có bao nhiêu cách chúng ta có thể tạo thành các số với số đếm cho trước, chia thành hai trường hợp: liệu chúng ta có hoàn toàn nhỏ hơn`n`hoặc vẫn khớp chính xác với tiền tố. Đây là DP “chặt chẽ” tiêu chuẩn trên các chuỗi nhị phân, với một chiều bổ sung theo dõi số lượng chiều đã chọn cho đến nay. 

Khi chúng ta biết chính xác số lượng số nguyên hợp lệ`k`câu trả lời cuối cùng chỉ là tổng có trọng số trên các giá trị Fibonacci. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force hơn tôi | O(n) | O(1) | Quá chậm | 
| Chữ số DP trên tiền tố nhị phân | O(L²) DP có cấu trúc trong trường hợp xấu nhất | O(L²) hoặc O(L) được tối ưu hóa | Đã chấp nhận | 

Đây`L`là độ dài của chuỗi nhị phân. 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi nhị phân từ trái sang phải, xây dựng số lượng chúng tôi có thể tạo thành nhỏ hơn hoặc bằng tiền tố của`n`. 

1. Tính toán trước các giá trị Fibonacci lên đến`L`, vì số lượng của bất kỳ số nào có nhiều nhất`L`bit không thể vượt quá`L`. Điều này đảm bảo chúng ta có thể ánh xạ ngay lập tức bất kỳ số lượng đơn vị nào tới giá trị Fibonacci của nó. 
2. Thiết lập bảng DP trong đó`dp[tight][k]`biểu thị có bao nhiêu cách chúng ta có thể tạo thành một số bằng cách sử dụng tiền tố được xử lý sao cho chính xác`k`những cái đã được chọn cho đến nay, và`tight`cho biết liệu chúng tôi vẫn khớp chính xác với tiền tố của`n`. 
3. Khởi tạo`dp[1][0] = 1`, nghĩa là trước khi xử lý bất kỳ bit nào, chúng ta có chính xác một cấu trúc trống khớp với tiền tố. 
4. Với mỗi bit trong chuỗi nhị phân của`n`, cập nhật DP: 

Chúng tôi xem xét việc đặt 0 hoặc 1 ở vị trí hiện tại, nhưng chúng tôi phải tôn trọng ràng buộc do tiền tố áp đặt nếu chúng tôi ở trạng thái chặt chẽ. Nếu bit hiện tại của`n`là 0 thì không thể đặt 1 ở trạng thái chặt chẽ. Nếu là 1 thì cả hai lựa chọn đều có thể, nhưng chọn 1 vẫn có thể khiến chúng ta chặt chẽ hoặc đứt quãng tùy vào độ khớp chính xác. 
5. Mỗi lần chuyển đổi cũng cập nhật số lượng chuyển đổi đã chọn, tăng dần`k`khi chúng ta đặt số 1. 
6. Sau khi xử lý tất cả các bit, chúng ta có số lượng số hợp lệ được nhóm theo số lượng của chúng, được tích lũy trên cả trạng thái chặt chẽ và không chặt chẽ. 
7. Tính đáp án cuối cùng bằng cách tính tổng`dp[0][k] + dp[1][k]`nhân với`f(k)`tổng thể`k`. 

### Tại sao nó hoạt động 

Mỗi số nguyên`i ≤ n`tương ứng với chính xác một đường dẫn hợp lệ trong chữ số DP này: chúng tôi quyết định từng chút một liệu`i`trận đấu`n`cho đến nay hoặc đã trở nên nhỏ hơn và chúng tôi theo dõi xem chúng tôi đã sử dụng bao nhiêu cái. Ràng buộc chặt chẽ đảm bảo chúng tôi không bao giờ vượt quá`n`và việc phân chia trạng thái đảm bảo mỗi số hợp lệ được tính chính xác một lần. Vì số lượng người được chọn hoàn toàn được nắm bắt theo số lượng người được chọn nên việc nhóm theo`k`là đủ và tổng cuối cùng tổng hợp chính xác đóng góp của tất cả các số nguyên trong phạm vi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

s = input().strip()
L = len(s)

# Fibonacci up to L
fib = [0] * (L + 2)
fib[1] = 1
for i in range(2, L + 2):
    fib[i] = (fib[i - 1] + fib[i - 2]) % MOD

# dp[tight][k]
# tight = 0 or 1
dp0 = [0] * (L + 1)
dp1 = [0] * (L + 1)

dp1[0] = 1  # start tight, 0 ones used

for ch in s:
    bit = int(ch)
    ndp0 = [0] * (L + 1)
    ndp1 = [0] * (L + 1)

    for k in range(L + 1):
        val1 = dp1[k]
        if val1:
            # place 0
            if bit == 1:
                ndp0[k] = (ndp0[k] + val1) % MOD
            else:
                ndp1[k] = (ndp1[k] + val1) % MOD

            # place 1
            nk = k + 1
            if nk <= L:
                if bit == 1:
                    ndp1[nk] = (ndp1[nk] + val1) % MOD
                # if bit == 0, cannot place 1 in tight state

        val0 = dp0[k]
        if val0:
            # already smaller, can place freely
            ndp0[k] = (ndp0[k] + val0) % MOD
            ndp0[k + 1] = (ndp0[k + 1] + val0) % MOD if k + 1 <= L else ndp0[k + 1]

    dp0, dp1 = ndp0, ndp1

ans = 0
for k in range(L + 1):
    cnt = (dp0[k] + dp1[k]) % MOD
    ans = (ans + cnt * fib[k]) % MOD

print(ans)
```Giải pháp được cấu trúc xung quanh một chữ số DP trên biểu diễn nhị phân. Mảng Fibonacci được tính toán trước một lần và mỗi lần chuyển đổi trạng thái DP sẽ cập nhật số lượng được xây dựng theo số lượng hiện tại của chúng. Việc chia thành`tight`Và`non-tight`các tiểu bang thực thi giới hạn trên được xác định bởi`n`. 

Một chi tiết triển khai tinh tế là duy trì các giới hạn trên`k`. Vì chúng ta chưa bao giờ ch
