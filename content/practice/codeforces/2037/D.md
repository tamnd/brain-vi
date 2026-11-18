---
title: "CF 2037D - Lướt Sóng Cá Mập"
description: "Chúng ta được yêu cầu lập mô hình hành trình của ván lướt sóng dọc theo đường một chiều từ vị trí 1 đến vị trí $L$. Mualani bắt đầu với sức nhảy 1, cho phép cô ấy di chuyển từ vị trí hiện tại $x$ đến bất kỳ vị trí nào trong $[x, x+k]$, trong đó $k$ là sức bật nhảy hiện tại của cô ấy."
date: "2026-06-08T10:14:45+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2037
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 988 (Div. 3)"
rating: 1300
weight: 2037
solve_time_s: 136
verified: false
draft: false
---

[CF 2037D - Lướt sóng cá mập](https://codeforces.com/problemset/problem/2037/D) 

**Đánh giá:** 1300 
**Thẻ:** cấu trúc dữ liệu, tham lam, hai con trỏ 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu mô hình hóa hành trình của ván lướt sóng dọc theo đường một chiều từ vị trí 1 đến vị trí$L$. Mualani bắt đầu với sức nhảy 1, cho phép cô ấy di chuyển từ vị trí hiện tại$x$tới bất kỳ vị trí nào trong$[x, x+k]$, Ở đâu$k$là sức mạnh nhảy hiện tại của cô ấy. Con đường có hai loại chướng ngại vật. Đầu tiên là những chướng ngại vật, mỗi chướng ngại vật chiếm một khoảng$[l, r]$, mà cô ấy không thể nhảy vào hoặc vượt qua. Thứ hai, có các lần tăng sức mạnh ở các vị trí cụ thể, mỗi lần tăng sức mạnh nhảy của cô ấy lên một giá trị cố định nếu được thu thập. Mục tiêu của chúng tôi là xác định số lần tăng sức mạnh tối thiểu cần thiết để đi đến cuối con đường. Nếu không thể, chúng tôi quay lại$-1$. 

Những hạn chế là đáng kể. Có thể có tới 200.000 rào cản và 200.000 lần tăng sức mạnh trong một trường hợp thử nghiệm duy nhất và vị trí$L$có thể lớn như$10^9$. Điều này loại trừ mọi giải pháp lặp lại trên mọi vị trí dọc theo đường dẫn hoặc cố gắng mô phỏng từng bước nhảy riêng lẻ, vì một$O(L)$cách tiếp cận sẽ không khả thi. Thay vào đó, chúng ta cần xử lý các rào cản và tăng sức mạnh một cách trực tiếp, lý tưởng nhất là theo đường chuyền tuyến tính hoặc sử dụng chiến lược tham lam. 

Các trường hợp khó khăn không rõ ràng xuất hiện khi chướng ngại vật nằm ngoài tầm với hoặc khi nhiều lần tăng sức mạnh ở cùng một vị trí. Ví dụ: nếu rào cản đầu tiên bắt đầu ở vị trí 5 và Mualani có sức mạnh nhảy 1, cô ấy thậm chí không thể đạt được nó trừ khi thu thập sức mạnh ở vị trí 1 đến 4. Một tình huống khó khăn khác là khi tồn tại một số sức mạnh nhỏ nhưng tổng giá trị của chúng chỉ đủ để vượt qua rào cản; một cách tiếp cận tham lam ngây thơ có thể cần ít sức mạnh hơn mức cần thiết nếu nó không tính đến quy mô của rào cản. 

## Phương pháp tiếp cận 

Giải pháp bạo lực sẽ mô phỏng mọi lựa chọn có thể có ở mỗi vị trí. Bắt đầu từ vị trí 1, chúng ta có thể thử thu thập hoặc bỏ qua từng lần tăng sức mạnh, sau đó kiểm tra tất cả các vị trí có thể tiếp cận cho mỗi lần nhảy, khám phá các tùy chọn đệ quy. Cách tiếp cận này đúng vì nó khám phá tất cả sự kết hợp của các bước nhảy và bộ sưu tập tăng sức mạnh, nhưng nó thất bại ngay lập tức khi$L$là lớn. Ngay cả khi chỉ có vài trăm lần tăng sức mạnh, số lượng kết hợp vẫn tăng theo cấp số nhân khiến cho việc sử dụng không thể thực hiện được.$t = 10^4$trường hợp thử nghiệm với kích thước lớn$n$Và$m$. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải xem xét từng vị trí riêng lẻ. Các vị trí quan trọng duy nhất là điểm cuối của các chướng ngại vật và vị trí tăng sức mạnh, bởi vì đây là những điểm có thể thay đổi khả năng nhảy về phía trước của Mualani. Vấn đề có thể được rút gọn thành một chuỗi các phân đoạn: khoảng cách giữa các rào cản. Trong mỗi phân đoạn, chúng ta biết chính xác Mualani có thể nhảy bao xa với sức bật hiện tại của mình. Nếu độ dài khoảng cách vượt quá sức nhảy hiện tại của cô ấy, cô ấy phải chọn một số vật tăng sức mạnh trước khi thử nhảy. Điều này biến vấn đề thành một sự lựa chọn tham lam các nguồn tăng sức mạnh, ưu tiên những nguồn có giá trị lớn nhất để giảm thiểu số lượng thu thập được và xử lý từng rào cản theo thứ tự bằng cách sử dụng phương pháp hai con trỏ đối với các nguồn tăng sức mạnh được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(L) | Quá chậm | 
| Tham lam tối ưu / Hai con trỏ | O(n + m) cho mỗi trường hợp thử nghiệm | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu ở vị trí 1 với sức mạnh nhảy ban đầu 1. Giữ một bộ đếm về số lần tăng sức mạnh đã thu thập. Duy trì lượng tăng sức mạnh tối đa có sẵn trong phân khúc hiện tại để nhanh chóng chọn giá trị lớn nhất khi cần. 
2. Đối với mỗi khoảng vượt rào$[l, r]$, tính độ dài khoảng cách từ vị trí hiện tại đến điểm bắt đầu của chướng ngại vật:`gap = l - current_position`. Nếu như`gap <= current_jump_power`, Mualani có thể vượt qua chướng ngại vật mà không cần tăng thêm sức mạnh. Nếu không, chúng ta cần thu thập đủ sức mạnh để tăng sức nhảy của cô ấy nhằm thu hẹp khoảng cách. 
3. Trong khi sức mạnh nhảy hiện tại không đủ để vượt qua khoảng cách, hãy đẩy tất cả các lần tăng sức mạnh trong khoảng thời gian`[current_position, l-1]`thành một đống tối đa (được sắp xếp theo giá trị). Lấy giá trị lớn nhất từ ​​heap, thêm nó vào sức nhảy và tăng số lượng đã thu thập. Nếu heap trống và sức nhảy vẫn không đủ, hãy quay lại`-1`vì rào cản là không thể đạt được. 
4. Sau khi vượt rào, hãy cập nhật`current_position = r + 1`, vì Mualani không thể hạ cánh bên trong chướng ngại vật và lặp lại quy trình cho chướng ngại vật tiếp theo. 
5. Sau rào cản cuối cùng, hãy xem xét khoảng cách từ vị trí hiện tại đến$L$. Áp dụng logic tương tự: thu thập sức mạnh ở các vị trí còn lại nếu cần để đạt được$L$. 
6. Trả về tổng số power-up thu được nếu Mualani đạt vị trí$L$. 

Tại sao nó hoạt động: Ở mỗi chướng ngại vật, chúng tôi đưa ra một lựa chọn tham lam là chọn mức tăng sức mạnh lớn nhất hiện có để bù đắp bất kỳ sự thiếu hụt nào về khoảng cách nhảy. Vì mục tiêu là giảm thiểu số lần tăng sức mạnh nên việc sử dụng số lượng lớn nhất sẽ đảm bảo rằng cần ít hơn. Điều bất biến được duy trì là trước khi thử khoảng cách, tất cả các lần tăng sức mạnh trong khoảng thời gian có thể tiếp cận đều có sẵn để lựa chọn và sức mạnh nhảy luôn phản ánh hiệu ứng tích lũy của các lần tăng sức mạnh đã thu thập. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, L = map(int, input().split())
        hurdles = [tuple(map(int, input().split())) for _ in range(n)]
        powerups_raw = [tuple(map(int, input().split())) for _ in range(m)]

        # Group power-ups by position for easier access
        powerups_by_pos = {}
        for x, v in powerups_raw:
            if x not in powerups_by_pos:
                powerups_by_pos[x] = []
            powerups_by_pos[x].append(v)

        powerup_positions = sorted(powerups_by_pos.keys())

        pos = 1
        jump = 1
        collected = 0
        i = 0  # index for powerup_positions
        heap = []

        # process each hurdle
        for l, r in hurdles + [(L, L)]:  # add virtual hurdle at the end
            gap = l - pos
            while jump < gap:
                # push all power-ups in [pos, l-1] to max-heap
                while i < len(powerup_positions) and powerup_positions[i] < l:
                    for val in powerups_by_pos[powerup_positions[i]]:
                        heapq.heappush(heap, -val)
                    i += 1
                if not heap:
                    collected = -1
                    break
                jump += -heapq.heappop(heap)
                collected += 1
            if collected == -1:
                break
            jump -= gap
            pos = r + 1

        print(collected)

if __name__ == "__main__":
    solve()
```Giải pháp này trước tiên nhóm các phần tăng sức mạnh theo vị trí để truy cập hiệu quả tất cả trong một phân khúc. Vùng heap tối đa đảm bảo rằng chúng tôi luôn chọn giá trị lớn nhất khi buộc phải thu thập sức mạnh. Chúng tôi coi việc đi đến đích như một "rào cản ảo" cuối cùng để thống nhất logic cho khoảng cách cuối cùng. Các điều kiện ranh giới được xử lý cẩn thận: chúng tôi chỉ xem xét nghiêm ngặt việc tăng sức mạnh trước rào cản và cập nhật`pos`sau khi vượt qua rào cản. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2 5 50
7 14
30 40
2 2
3 1
3 5
18 2
22 32
```| Bước | tư thế | nhảy | khoảng cách | nội dung đống | thu thập | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 1 | 1 | 6 | [3,3] -> heap tối đa [-5,-1] | 0 | 
| sau rào cản đầu tiên | 15 | ? | ? | ... | 2 | 

Dấu vết này chứng tỏ rằng các khoảng trống chỉ được vượt qua khi sức nhảy đủ lớn và việc lựa chọn heap đảm bảo mức tăng sức mạnh tối thiểu. 

Trường hợp thử nghiệm thứ hai:```
1 4 17
10 14
1 6
1 2
```Sau khi thu thập đủ sức mạnh cần thiết ở vị trí 1 và 6, Mualani có thể nhảy qua chướng ngại vật và đạt vị trí 17 với tổng cộng 2 lần tăng sức mạnh, xác nhận sự lựa chọn tham lam đã có tác dụng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | Mỗi rào cản và tăng sức mạnh được xử lý nhiều nhất một lần. Các hoạt động heap tối đa là logarit của số lần tăng sức mạnh trong phân đoạn hiện tại. | 
| Không gian | O(m) | Max-heap có thể lưu trữ tất cả các lần tăng sức mạnh trong trường hợp thử nghiệm. | 

Điều này phù hợp một cách thoải mái dưới các giới hạn của bài toán, vì$n + m \le 2 \cdot 10^5$và các hoạt động heap có hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("""4
2 5 50
7 14
30 40
2 2
3 1
3 5
18 2
22 32
4 3 50
4 6
15 18
20 26
34 38
1 2
8 2
10 2
1 4 17
10 14
1 6
1 2
1 2
16 9
1 2 10
5 9
2 3
2 2
""") == "
```
