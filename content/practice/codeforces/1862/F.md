---
title: "CF 1862F - Phép Thuật Sẽ Cứu Thế Giới"
description: "Chúng ta được yêu cầu tính thời gian tối thiểu Vika, một nữ phù thủy, cần để đánh bại một chuỗi quái vật. Mỗi quái vật có một sức mạnh và Vika có hai loại ma thuật là nước và lửa. Trong một giây, cô tạo ra một lượng mana nước và lửa cố định."
date: "2026-06-09T00:14:51+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "brute-force", "dp"]
categories: ["algorithms"]
codeforces_contest: 1862
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 894 (Div. 3)"
rating: 1800
weight: 1862
solve_time_s: 88
verified: true
draft: false
---

[CF 1862F - Phép thuật sẽ cứu thế giới](https://codeforces.com/problemset/problem/1862/F) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, mặt nạ bit, lực lượng vũ phu, dp 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính thời gian tối thiểu Vika, một nữ phù thủy, cần để đánh bại một chuỗi quái vật. Mỗi quái vật có một sức mạnh và Vika có hai loại ma thuật là nước và lửa. Trong một giây, cô tạo ra một lượng mana nước và lửa cố định. Để đánh bại một con quái vật, cô ấy phải tiêu tốn ít nhất lượng mana trong một câu thần chú bằng sức mạnh của con quái vật đó, sử dụng một trong hai loại phép thuật. Cô ấy có thể sử dụng nhiều phép thuật mỗi giây nếu có đủ năng lượng tích lũy. Vấn đề yêu cầu, đối với nhiều trường hợp thử nghiệm, tìm số giây tối thiểu cần thiết để đánh bại tất cả quái vật. 

Đầu vào cung cấp cho mỗi trường hợp thử nghiệm tốc độ tạo năng lượng và sức mạnh của quái vật. Các ràng buộc rất nhỏ: số lượng quái vật trong mỗi trường hợp thử nghiệm tối đa là 100 và tổng số trên tất cả các trường hợp thử nghiệm cũng tối đa là 100. Sức mạnh của mỗi quái vật tối đa là$10^4$, trong khi lượng năng lượng tạo ra mỗi giây có thể rất lớn, lên tới$10^9$. Điều này cho thấy rằng chúng ta có thể khám phá các giải pháp có quy mô theo cấp số nhân về số lượng quái vật, bởi vì$2^{100}$là quá lớn, nhưng việc sử dụng thông minh các tổng tập hợp con hoặc DP trên các tổng được giới hạn bởi tổng cường độ (nhiều nhất là$100 \times 10^4 = 10^6$) là khả thi. 

Trường hợp khó khăn xảy ra khi một loại mana lớn hơn nhiều so với loại kia. Ví dụ: nếu Vika có thể tạo ra 1 đơn vị nước nhưng 100 đơn vị lửa mỗi giây và tất cả quái vật có sức mạnh 50, cô ấy nên tập trung vào phép thuật lửa. Một cách tiếp cận bất cẩn luôn chia đều mana hoặc bỏ qua việc phân bổ tối ưu quái vật vào loại ma thuật sẽ thất bại. Một trường hợp khác là khi tổng sức mạnh của quái vật khớp chính xác với bội số năng lượng hoặc khi chỉ có một quái vật; những điều này đòi hỏi tính toán chính xác hơn là xấp xỉ. 

## Phương pháp tiếp cận 

Giải pháp vũ phu xem xét tất cả các nhiệm vụ có thể có của quái vật đối với phép thuật nước hoặc lửa. Mỗi quái vật có hai lựa chọn, vì vậy có$2^n$tổng số bài tập. Đối với mỗi nhiệm vụ, chúng tôi tính toán tổng năng lượng nước và lửa cần thiết, chia cho lượng năng lượng được tạo ra mỗi giây (làm tròn) và lấy mức tối đa. Điều này đảm bảo tính chính xác vì nó thực sự kiểm tra mọi cách để phân bổ quái vật giữa hai loại phép thuật. Tuy nhiên, với$n$lên tới 100,$2^{100}$khả năng là không thể thực hiện được. 

Điều quan trọng cần lưu ý là đây là bài toán tổng tập con hai chiều cổ điển. Chúng ta có thể tập trung vào việc phân bổ mana nước, vì khi đó lửa sẽ được xác định tự động. Nếu chúng ta có thể tìm được một tập hợp con quái vật để gán cho nước sao cho tổng lượng mana nước cần thiết là$W$, thì những quái vật còn lại yêu cầu$F$mana và thời gian tối thiểu là$\max(\lceil W / w \rceil, \lceil F / f \rceil)$. Tổng sức mạnh của tất cả quái vật nhiều nhất là$10^6$, vì vậy chúng ta có thể thực hiện lập trình động theo tổng lượng nước có thể đạt được. Cụ thể, chúng tôi duy trì một mảng DP boolean trong đó`dp[s]`đúng nếu tồn tại một tập hợp con quái vật có tổng sức mạnh nước chính xác bằng$s$. Sau khi có DP, chúng tôi lặp lại tất cả các khoản tiền có thể đạt được`s`và tính thời gian như$\max(\lceil s / w \rceil, \lceil (total - s) / f \rceil)$, giữ ở mức tối thiểu 

Điều này làm giảm vấn đề từ thời gian hàm mũ sang thời gian giả đa thức. Kích thước DP tối đa là tổng của tất cả sức mạnh của quái vật, tối đa là$10^6$và đối với mỗi quái vật, chúng tôi cập nhật mảng DP. Với$n \le 100$, điều này hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| DP trên tổng tập hợp con | O(n * S) trong đó S=tổng sức mạnh | O(S) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng sức mạnh của quái vật. Cho phép`total = sum(s_i)`. 
2. Khởi tạo một mảng DP boolean có kích thước`total + 1`, với`dp[0] = True`bởi vì tổng số 0 có thể đạt được khi không có quái vật. 
3. Đối với mỗi sức mạnh quái vật`s_i`, lặp lại mảng DP từ`total`xuống`s_i`. Nếu như`dp[j - s_i]`là Đúng thì đặt`dp[j] = True`. Điều này cập nhật tất cả số tiền nước có thể đạt được. 
4. Khởi tạo`ans`như vô cùng. Lặp lại tất cả các chỉ số`w_sum`trong mảng DP. Đối với mỗi`w_sum`điều đó có thể đạt được (`dp[w_sum] = True`), tính tổng lửa còn lại`f_sum = total - w_sum`. Tính thời gian tích lũy mana:`time = max((w_sum + w - 1) // w, (f_sum + f - 1) // f)`. Cập nhật`ans = min(ans, time)`. 
5. Đầu ra`ans`theo số giây tối thiểu được yêu cầu. 

Tại sao nó hoạt động: DP đảm bảo rằng chúng tôi xem xét mọi khả năng phân chia quái vật giữa nước và lửa. Mỗi`w_sum`tương ứng với sự phân bổ hợp lệ của một số quái vật cho phép thuật nước. Phần bổ sung tự động phân bổ lửa. Việc tính toán của`max(ceil(W/w), ceil(F/f))`mô hình chính xác thời gian để tích lũy đủ mana của cả hai loại. Bằng cách lặp lại tất cả lượng nước có thể đạt được, chúng tôi đảm bảo tìm thấy tổng thời gian tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        w, f = map(int, input().split())
        n = int(input())
        s = list(map(int, input().split()))
        total = sum(s)
        dp = [False] * (total + 1)
        dp[0] = True
        for strength in s:
            for j in range(total, strength - 1, -1):
                if dp[j - strength]:
                    dp[j] = True
        ans = float('inf')
        for w_sum in range(total + 1):
            if dp[w_sum]:
                f_sum = total - w_sum
                time = max((w_sum + w - 1) // w, (f_sum + f - 1) // f)
                if time < ans:
                    ans = time
        print(ans)

if __name__ == "__main__":
    solve()
```Phần DP lặp lại ngược lại để tránh việc đếm hai lần một quái vật nhiều lần trong cùng một tập hợp con. Sự tính toán`(w_sum + w - 1) // w`làm tròn phép chia số nguyên, tương đương với`ceil(w_sum / w)`, điều này đảm bảo tích lũy mana một phần được tính là một giây đầy đủ. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
w=2, f=3, s=[2,6,7]
```| Tập hợp con quái vật | Tổng nước | Tổng lửa | Đến giờ uống nước | Đã đến lúc cháy | Thời gian tối đa | 
| --- | --- | --- | --- | --- | --- | 
| [2,6,7] nước | 15 | 0 | 8 | 0 | 8 | 
| [2] nước | 2 | 13 | 1 | 5 | 5 | 
| [2,7] nước | 9 | 6 | 5 | 2 | 5 | 
| [6,7] nước | 13 | 2 | 7 | 1 | 7 | 
| [6] nước | 6 | 9 | 3 | 3 | 3 | 
| [7] nước | 7 | 8 | 4 | 3 | 4 | 
| [] nước | 0 | 15 | 0 | 5 | 5 | 

Thời gian tối đa tối thiểu là 3 giây, đạt được bằng cách chỉ định quái vật 6 sức mạnh ở nước và những con khác bắn. 

**Đầu vào mẫu 2:**```
w=37, f=58, s=[93]
```| Tập hợp con | Nước | Lửa | Nước thời gian | Ngọn lửa thời gian | Tối đa | 
| --- | --- | --- | --- | --- | --- | 
| [93] nước | 93 | 0 | 3 | 0 | 3 | 
| [] nước | 0 | 93 | 0 | 2 | 2 | 

Nhiệm vụ tối ưu là niệm phép lửa, thời gian là 2 giây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * tổng(s_i)) | DP trên tổng số nước có thể đạt được, lặp lại cho từng quái vật | 
| Không gian | O(tổng(s_i)) | Mảng Boolean DP lưu trữ tổng lượng nước có thể đạt được | 

Cho rằng tổng điểm mạnh ≤ 100 * 10^4 = 10^6 và n ≤ 100, giải pháp phù hợp thoải mái trong giới hạn thời gian 4 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("4\n2 3\n3\n2 6 7\n37 58\n1\n93\n190 90\n2\n23 97\n13 4\n4\n10 10 2 45\n") == "3\n2\n1\n5", "sample 1"

# Custom tests
assert run("1\n1 1\n1\n1\n") == "1", "single monster, equal mana"
assert run("1\n10 100\n2\n50 50\n") == "1", "fire should handle both"
assert run("1\n5
```
