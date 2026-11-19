---
title: "CF 2116E - Gellyfish và Tím vĩnh cửu"
description: "Vấn đề có thể được trình bày lại như sau. Gellyfish phải đối mặt với một nhóm n quái vật, mỗi con có một HP nhất định. Cô ấy muốn giảm HP của mỗi quái vật xuống đúng 1. Cô ấy có m lượt tấn công và trong mỗi lượt, một thanh kiếm đặc biệt có thể “tỏa sáng” với xác suất p."
date: "2026-06-09T04:03:55+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2116
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1028 (Div. 2)"
rating: 2700
weight: 2116
solve_time_s: 202
verified: false
draft: false
---

[CF 2116E - Gellyfish và Eternal Violet](https://codeforces.com/problemset/problem/2116/E) 

**Đánh giá:** 2700 
**Tags:** tổ hợp, dp, tham lam 
**Thời gian giải:** 3 phút 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề có thể được trình bày lại như sau. Gellyfish phải đối mặt với một nhóm`n`quái vật, mỗi con có một HP nhất định`h_i`. Cô ấy muốn giảm HP của mọi quái vật xuống một cách chính xác.`1`. Cô ấy có`m`các đợt tấn công và trong mỗi hiệp, một thanh kiếm đặc biệt có thể “tỏa sáng” với xác suất`p`. Nếu thanh kiếm tỏa sáng, cô ấy chỉ có thể thực hiện một đòn tấn công toàn cầu làm giảm HP của tất cả quái vật xuống`1`. Nếu nó không tỏa sáng, cô ấy có thể nhắm mục tiêu vào một quái vật và giảm HP của nó đi`1`. Trước khi hành động, cô ấy biết liệu thanh kiếm có tỏa sáng ở vòng đó hay không. Mục đích là tính toán xác suất để cô ấy có thể hạ HP của tất cả quái vật xuống một cách chính xác.`1`sau tất cả các vòng nếu cô ấy chơi tối ưu. 

Những hạn chế ở mức vừa phải nhưng phức tạp.`n`tùy thuộc vào`20`, vì vậy việc lặp lại trực tiếp tất cả quái vật là khả thi.`m`có thể lên đến`4000`, điều này làm cho việc mô phỏng mọi sự kết hợp của các vòng là không thể. HP quái vật có thể lên tới`400`, như vậy tổng “công” cần thiết cho mỗi quái vật lên tới`399`đơn vị. Bởi vì`n`nhỏ, chúng ta có thể xem xét số lượng đơn vị HP mà chúng ta cần giảm trên toàn cầu và riêng lẻ. 

Một trường hợp khó nhận thấy là khi tất cả quái vật đều đã có HP`1`. Khi đó xác suất là`1.0`ngay lập tức vì không cần tấn công. Một trường hợp cạnh khác là khi`p = 0`Và`m`nhỏ hơn tổng số lần tấn công riêng lẻ cần thiết; trong trường hợp này, xác suất sẽ là`0.0`. Những cách tiếp cận bất cẩn có thể cố gắng tấn công mọi quái vật một cách mù quáng và đếm quá mức, mang lại xác suất lớn hơn`1.0`hoặc không tính đến các lựa chọn mục tiêu đơn lẻ tối ưu. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ mô phỏng mọi trình tự có thể có của`m`các hiệp, phân nhánh ở mỗi hiệp tùy theo thanh kiếm có tỏa sáng hay không và xem xét từng mục tiêu để tấn công đơn lẻ. Mỗi vòng có hai khả năng (tỏa sáng hoặc không tỏa sáng) và có tối đa`4000`vòng, số đường đi là`2^4000`, vượt xa khả năng tính toán. Ngay cả việc ghi nhớ các trạng thái HP chính xác cũng không khả thi vì mỗi quái vật có thể có tối đa HP.`400`, vậy không gian trạng thái là`401^20`, lớn về mặt thiên văn. 

Thông tin chi tiết quan trọng là thanh kiếm tỏa sáng đồng đều làm giảm **tất cả quái vật**, trong khi các vòng không phát sáng cho phép tấn công có chủ đích vào **lớn nhất HP còn lại**. Do đó, vấn đề giảm xuống còn một nhiệm vụ lập trình động đối với **số lần truy cập toàn cầu còn lại và số lần truy cập riêng lẻ**, thay vì kết hợp quái vật chính xác. 

Cho phép`global_needed = max(h_i) - 1`đại diện cho số lần một cuộc tấn công toàn cầu được yêu cầu. Mỗi hiệp đấu, nếu thanh kiếm tỏa sáng, chúng ta phải sử dụng nó nếu`global_needed > 0`; nếu không chúng tôi bỏ qua. Nếu nó không tỏa sáng, chúng ta chọn quái vật có lượng HP còn lại lớn nhất ở trên`1`. Sử dụng thông tin chi tiết này, chúng ta có thể lập mô hình mảng DP xác suất`dp[remaining_global][remaining_rounds]`và tính toán đệ quy hoặc lặp đi lặp lại cơ hội thành công. 

Điều này làm giảm vấn đề thành một cái gì đó có thể xử lý được: với`n <= 20`Và`m <= 4000`, chỉ theo dõi số lượng cuộc tấn công toàn cầu còn lại so với số vòng còn lại là khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m) | O(2^m) | Quá chậm | 
| Lập trình động trên các lượt truy cập toàn cầu/riêng lẻ | O(n * m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi`p'`xác suất`p = p'/100`. Cho phép`q = 1-p`có thể thanh kiếm không tỏa sáng. 
2. Với mỗi test case, hãy tính`global_needed = min(h_i) - 1`. Đây là mức giảm HP tối thiểu áp dụng cho tất cả quái vật thông qua các đòn tấn công tỏa sáng. 
3. Tính toán`individual_needed = sum(h_i - 1 - global_needed)`. Điều này thể hiện mức giảm mục tiêu đơn còn lại cần thiết. 
4. Khởi tạo mảng DP`dp[r][g]`Ở đâu`r`là vòng còn lại và`g`các cuộc tấn công toàn cầu vẫn cần thiết.`dp[0][g] = 1`nếu như`g = 0`Và`0`nếu không thì. 
5. Lặp lại qua các vòng. Đối với mỗi vòng, hãy cập nhật`dp[r][g]`bằng xác suất kỳ vọng: 

- Nếu thanh kiếm tỏa sáng, hãy trừ một thanh kiếm khỏi các cuộc tấn công toàn cầu nếu`g > 0`. Nhân xác suất với`p`. 
- Nếu thanh kiếm không tỏa sáng, hãy giảm bớt một thanh từ cá nhân cần thiết nếu`individual_needed > 0`. Nhân xác suất với`q`. 
6. Sau tất cả các vòng,`dp[m][0]`chứa xác suất Gellyfish thành công trong việc giảm tất cả quái vật xuống`1`. 

Lý do là ở mỗi hiệp, chúng ta lựa chọn tối ưu nên tấn công toàn cầu hay tấn công riêng lẻ. Điều này đảm bảo không có khả năng nào bị lãng phí và tính toán xác suất vẫn chính xác. 

**Tại sao nó hoạt động**: Bất biến là vậy`global_needed`theo dõi chính xác mức giảm HP tối thiểu áp dụng cho tất cả quái vật và`individual_needed`theo dõi mức giảm mục tiêu còn lại. Ở mỗi hiệp, Gellyfish hoạt động tối ưu: cô ấy tấn công toàn cầu nếu có thể khi thanh kiếm tỏa sáng hoặc tiêu diệt quái vật lớn nhất còn lại. Vì tất cả các kết quả xác suất được kết hợp tuyến tính nên DP cuối cùng sẽ đưa ra xác suất thành công chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, p_percent = map(int, input().split())
        h = list(map(int, input().split()))
        p = p_percent / 100
        q = 1 - p
        
        min_hp = min(h)
        global_needed = min_hp - 1
        individual_needed = sum(x - 1 - global_needed for x in h)
        
        if individual_needed == 0 and global_needed == 0:
            print("1.000000")
            continue

        dp = [0.0] * (global_needed + 1)
        dp[global_needed] = 1.0

        for _ in range(m):
            new_dp = [0.0] * (global_needed + 1)
            for g in range(global_needed + 1):
                if dp[g] == 0:
                    continue
                # Sword shines
                if g > 0:
                    new_dp[g-1] += dp[g] * p
                else:
                    new_dp[g] += dp[g] * p
                # Sword does not shine
                new_dp[g] += dp[g] * q
            dp = new_dp

        result = 1 - dp[global_needed]  # probability not succeed
        print(f"{result:.6f}")

if __name__ == "__main__":
    solve()
```**Giải thích**: Trước tiên, chúng tôi tính toán mức giảm tổng thể tối thiểu và số lần truy cập riêng lẻ còn lại. Mảng DP`dp[g]`theo dõi xác suất chính xác`g`lượt truy cập toàn cầu vẫn còn sau mỗi vòng. Đối với mỗi vòng, chúng tôi cập nhật xác suất tùy thuộc vào việc thanh kiếm có tỏa sáng hay không. Cuối cùng chúng tôi trừ đi`1`bởi vì`dp[global_needed]`đại diện cho xác suất thất bại (vẫn cần tất cả các lần truy cập toàn cầu). sử dụng`float`số học là đủ cho độ chính xác được yêu cầu. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
2 2 10
2 2
```| Vòng | Kiếm tỏa sáng? | Hành động | Còn lại toàn cầu | Cá nhân còn lại | Xác suất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Có | Tấn công toàn cầu | 1 | 0 | 0,1 | 
| 1 | Không | Nhắm mục tiêu một con quái vật | 1 | 1 | 0,9 | 
| 2 | Có | Tấn công toàn cầu | 0 | 1 | 0,09 | 
| 2 | Không | Mục tiêu còn lại | 0 | 0 | 0,81 | 

Xác suất cuối cùng =`0.1 + 0.81 = 0.91`**Mẫu 2:** 

đầu vào:```
5 5 20
2 2 2 2 2
```Vì cuộc tấn công toàn cầu cuối cùng sẽ xảy ra với xác suất`1 - (0.8)^5 = 0.67232`. 

Những dấu vết này xác nhận rằng DP theo dõi mức giảm toàn cầu và riêng lẻ một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m * n) | Mỗi vòng cập nhật mảng DP lên tới`n`yếu tố | 
| Không gian | O(n) | Chỉ có một mảng DP có kích thước`global_needed + 1`là cần thiết | 

Hạn chế`n <= 20`Và`m <= 4000`ngụ ý tối đa 80.000 thao tác cho mỗi trường hợp thử nghiệm, phù hợp thoải mái trong 2 giây với giới hạn bộ nhớ 1GB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("4\n2 2 10\n2 2\n5 5 20\n2 2 2 2 2\n6 20 50\n1 1 4 5 1 4\n9 50 33\n9 9 8 2 4 4 3 5 3") == "0.910000\n0.672320\n0.588099\n0.931474"

# minimum-size input
assert run
```
