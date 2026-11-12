---
title: "CF 1633C - Tiêu diệt quái vật"
description: "Vấn đề là mô phỏng chiến đấu theo lượt giữa nhân vật Monocarp và một con quái vật. Mỗi người đều có sức khỏe và đòn tấn công ban đầu."
date: "2026-06-10T04:48:20+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 1633
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 122 (Rated for Div. 2)"
rating: 1100
weight: 1633
solve_time_s: 77
verified: true
draft: false
---

[CF 1633C - Tiêu diệt quái vật](https://codeforces.com/problemset/problem/1633/C) 

**Đánh giá:** 1100 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề là mô phỏng chiến đấu theo lượt giữa nhân vật Monocarp và một con quái vật. Mỗi người đều có sức khỏe và đòn tấn công ban đầu. Trong mỗi lượt, nhân vật tấn công trước, làm giảm sức khỏe của quái vật bằng giá trị tấn công hiện tại của anh ta, sau đó quái vật trả đũa, làm giảm sức khỏe của nhân vật bởi đòn tấn công của quái vật. Điều này luân phiên cho đến khi một trong số chúng đạt đến mức 0 hoặc sức khỏe âm. Mục tiêu là để xác định xem Monocarp có thể đảm bảo chiến thắng hay không bằng cách chi tới một số xu nhất định để nâng cấp sức tấn công và/hoặc sức khỏe cho nhân vật của mình. 

Đầu vào chỉ định số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp sức khỏe và đòn tấn công của nhân vật, sức khỏe và đòn tấn công của quái vật cũng như số xu có sẵn cùng với lợi ích cho mỗi xu để nâng cấp sức khỏe và tấn công. Đầu ra là CÓ hoặc KHÔNG đơn giản cho biết liệu bạn có thể đạt được chiến thắng hay không. 

Các ràng buộc cho phép số lượng rất lớn: sức khỏe của nhân vật và quái vật có thể lên tới$10^{15}$, đồng xu lên tới$2 \cdot 10^5$và nâng cấp lớn như$10^{10}$. Vì số lượng ca kiểm thử có thể$5 \cdot 10^4$, một giải pháp phải xử lý từng trường hợp thử nghiệm một cách hiệu quả, trong khoảng$O(k)$hoặc tốt hơn, bởi vì mô phỏng ngây thơ của mỗi lượt sẽ vượt quá giới hạn thời gian. 

Các trường hợp có lợi thế không rõ ràng bao gồm các tình huống trong đó nhân vật đã chiến thắng mà không cần nâng cấp, trong đó tất cả các đồng xu sẽ được dùng để tấn công hoặc lấy máu một cách tối ưu và khi số lượng xu bằng 0. Ví dụ, nếu$h_C = 1$,$d_C = 1$,$h_M = 1$,$d_M = 10$, Và$k = 0$, Monocarp không thể sống sót dù chỉ một đòn, vì vậy kết quả đúng là KHÔNG. Một cách tiếp cận ngây thơ luôn thử nâng cấp mà không xem xét số liệu thống kê hiện tại có thể báo cáo CÓ không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là thử mọi cách để phân phối tiền giữa các nâng cấp sức khỏe và tấn công. Đối với mỗi lần phân phối, chúng tôi sẽ mô phỏng cuộc chiến bằng cách lặp lại các lượt cho đến khi một bên chết. Điều này hiệu quả vì mỗi sự kết hợp nâng cấp đều hợp lệ và mô phỏng phản ánh chính xác các quy tắc. Tuy nhiên, trong trường hợp xấu nhất, có$k+1$phân phối để thử và mô phỏng một cuộc chiến với tối đa$10^{15}$sức khỏe là không thể thực hiện được. Ngay cả việc tính toán số lượt một cách ngây thơ cũng sẽ quá chậm. Vì vậy, vũ lực là không thực tế. 

Thông tin chi tiết quan trọng là số lượt mỗi bên sống sót có thể được tính toán bằng toán học mà không cần mô phỏng từng cuộc tấn công. Nhân vật tiêu diệt quái vật trong$\lceil h_M / d_C' \rceil$rẽ, ở đâu$d_C' = d_C + w \cdot x$Và$x$là số xu dùng để tấn công. Con quái vật giết chết nhân vật trong$\lceil h_C' / d_M \rceil$rẽ, ở đâu$h_C' = h_C + a \cdot (k - x)$cho những đồng tiền chi cho sức khỏe. Nếu số lượt của nhân vật nhỏ hơn hoặc bằng số lượt của quái vật, Monocarp sẽ thắng. Điều này làm giảm vấn đề kiểm tra$k+1$tính toán đơn giản cho mỗi trường hợp thử nghiệm. 

Do đó, chúng tôi có thể lặp lại tất cả các lần phân chia xu có thể có giữa đòn tấn công và sức khỏe, tính toán số lượt để đánh bại mỗi bên bằng cách sử dụng số học số nguyên và kiểm tra xem liệu có ít nhất một lần phân chia đảm bảo chiến thắng hay không. Từ$k$nhiều nhất là$2 \cdot 10^5$và tổng của$k$trên tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$, cách tiếp cận này có hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(k * max(h_M, h_C)) | O(1) | Quá chậm | 
| Toán học tối ưu | O(k) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy đọc chỉ số ban đầu của nhân vật và quái vật cũng như số lượng xu và lợi ích của chúng. 
2. Lặp lại tất cả các cách có thể để phân phối xu giữa đòn tấn công và máu, từ 0 xu để tấn công cho đến$k$. 
3. Với mỗi lần chia, hãy tính đòn tấn công mới$d_C' = d_C + w \cdot x$và sức khỏe mới$h_C' = h_C + a \cdot (k - x)$. 
4. Tính số lượt nhân vật cần để đánh bại quái vật$\text{turns}_C = \lceil h_M / d_C' \rceil$. 
5. Tính số lượt quái vật cần để đánh bại nhân vật$\text{turns}_M = \lceil h_C' / d_M \rceil$. 
6. Nếu$\text{turns}_C \le \text{turns}_M$đối với bất kỳ sự phân chia nào, ngay lập tức kết luận CÓ cho trường hợp thử nghiệm này. Ngược lại, sau khi kiểm tra tất cả các phần tách, hãy kết luận KHÔNG. 
7. In CÓ hoặc KHÔNG cho mỗi test. 

Tại sao nó hoạt động: Bằng cách lặp lại tất cả các lần phân phối tiền khả thi và tính toán thời gian tồn tại theo phương pháp toán học, chúng tôi đảm bảo rằng tất cả các nâng cấp tối ưu đều được xem xét. Vì lượt chiến đấu mang tính quyết định và giảm dần nên việc so sánh số lượt đánh sẽ xác định chính xác kết quả. Vòng lặp kết thúc$k+1$các khả năng là đủ để nắm bắt bất kỳ sự kết hợp tối ưu nào, do đó không bỏ sót phân phối chiến thắng hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

t = int(input())
for _ in range(t):
    hC, dC = map(int, input().split())
    hM, dM = map(int, input().split())
    k, w, a = map(int, input().split())
    
    possible = False
    for coins_attack in range(k + 1):
        coins_health = k - coins_attack
        new_attack = dC + coins_attack * w
        new_health = hC + coins_health * a
        
        turns_to_kill_monster = (hM + new_attack - 1) // new_attack
        turns_to_survive = (new_health + dM - 1) // dM
        
        if turns_to_kill_monster <= turns_to_survive:
            possible = True
            break
    
    print("YES" if possible else "NO")
```Giải pháp đầu tiên đọc đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Đối với mỗi trường hợp thử nghiệm, nó lặp lại tất cả các lần phân phối xu, tính toán đòn tấn công và lượng máu thu được, đồng thời tính số lượt bằng cách sử dụng phép chia trần thông qua số học số nguyên. Vòng lặp sẽ ngắt ngay lập tức khi tìm thấy phân phối chiến thắng để tránh các tính toán không cần thiết. Các trường hợp khó khăn như không có xu, không có giá trị nâng cấp hoặc đã có đủ số liệu thống kê đều được xử lý một cách tự nhiên vì tất cả các bản phân phối, kể cả không có bản nâng cấp, đều được kiểm tra. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
25 4
9 20
1 1 10
```| Tiền xu để tấn công | Đồng xu cho sức khỏe | Cuộc tấn công mới | Sức khỏe mới | Quay lại giết quái vật | Biến để tồn tại | Thắng? | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 4 | 35 | 3 | 2 | Không | 
| 1 | 0 | 5 | 25 | 2 | 2 | Có | 

Ở đây, việc bỏ ra 1 xu để tấn công cho phép nhân vật tiêu diệt quái vật trong 2 lượt, sống sót sau 2 lượt nên có thể chiến thắng. 

Một ví dụ khác:```
25 4
12 20
1 1 10
```| Tiền xu để tấn công | Đồng xu cho sức khỏe | Cuộc tấn công mới | Sức khỏe mới | Quay lại giết quái vật | Biến để tồn tại | Thắng? | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 4 | 35 | 3 | 2 | Không | 
| 1 | 0 | 5 | 25 | 3 | 2 | Không | 

Ở đây, không có sự phân bổ nào cho phép giết quái vật trước khi chết nên câu trả lời là KHÔNG. 

Những dấu vết này chứng minh rằng thuật toán tính toán chính xác số lượt và đánh giá tất cả các lần phân bổ tiền xu có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) cho mỗi trường hợp thử nghiệm | Chúng tôi lặp lại tất cả các lần phân chia k xu có thể có và tính toán các phép toán số học số nguyên cho mỗi lần phân chia. Tổng k trên tất cả các trường hợp thử nghiệm 2_10^5, do đó tổng số phép tính 2_10^5. | 
| Không gian | O(1) | Chỉ một vài biến số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm. | 

Với những hạn chế, giải pháp này phù hợp với giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())  # assuming solution is in solution.py
    return output.getvalue().strip()

# provided samples
assert run("4\n25 4\n9 20\n1 1 10\n25 4\n12 20\n1 1 10\n100 1\n45 2\n0 4 10\n9 2\n69 2\n4 2 7\n") == \
       "YES\nNO\nYES\nYES", "sample 1"

# custom cases
assert run("1\n1 1\n10 1\n0 1 1\n") == "NO", "minimum coins, cannot survive"
assert run("1\n10 5\n10 5\n10 1 1\n") == "YES", "already winning without upgrades"
assert run("1\n5 5\n20 10\n5 2 5\n") == "YES",
```
