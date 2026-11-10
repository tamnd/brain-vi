---
title: "CF 1533E - Thành lập đội cờ vua"
description: "Chúng tôi được giao nhiệm vụ thành lập một đội cờ vua bằng cách thêm một người chơi cuối cùng vào danh sách $n$ thành viên hiện có của Polycarp. Sức mạnh của mỗi người chơi là một số nguyên và đội đối phương đã có $n+1$ thành viên."
date: "2026-06-10T16:22:14+07:00"
tags: ["codeforces", "competitive-programming", "*special", "binary-search", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1533
codeforces_index: "E"
codeforces_contest_name: "Kotlin Heroes: Episode 7"
rating: 0
weight: 1533
solve_time_s: 192
verified: true
draft: false
---

[CF 1533E - Thành lập đội cờ vua](https://codeforces.com/problemset/problem/1533/E) 

**Đánh giá:** - 
**Tags:** *đặc biệt, tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 3 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao nhiệm vụ thành lập một đội cờ vua bằng cách thêm một người chơi cuối cùng vào danh sách hiện có của Polycarp.$n$các thành viên. Sức mạnh của mỗi người chơi là một số nguyên và đội đối phương đã có$n+1$các thành viên. Mục tiêu là chọn ra một ứng viên từ$m$các lựa chọn sao cho, sau khi ghép đôi đội của chúng tôi với đội đối phương, sự khác biệt tối đa giữa đối thủ của người chơi và chính họ - độ khó của trò chơi - được giảm thiểu. 

Đầu vào cung cấp cho chúng tôi các kỹ năng của nhóm hiện tại của chúng tôi$a_1 \dots a_n$, đội đối phương$b_1 \dots b_{n+1}$và các lựa chọn ứng viên$c_1 \dots c_m$. Đối với mỗi ứng cử viên, chúng tôi cần tính toán độ khó tối thiểu có thể đạt được bằng cách ghép đôi người chơi một cách tối ưu. 

Những hạn chế là đáng kể:$n$Và$m$có thể đạt được$2 \cdot 10^5$, nghĩa là bất kỳ$O(n \cdot m)$hoặc$O(n^2)$cách tiếp cận sẽ quá chậm. Với 2 giây thực hiện, chúng ta cần một giải pháp gần như$O(n \log n + m \log n)$hoặc tốt hơn. 

Các trường hợp khó khăn mà việc triển khai đơn giản có thể bỏ sót bao gồm các tình huống trong đó kỹ năng của ứng viên nhỏ hơn nhiều hoặc lớn hơn nhiều so với tất cả các thành viên hiện có trong nhóm. Ví dụ, nếu tất cả$a_i = 1$và ứng cử viên là$c_k = 10^9$, việc ghép nối kém có thể tạo ra một khó khăn lớn. Một thuật toán bất cẩn không xem xét việc sắp xếp và thứ tự ghép nối tối ưu sẽ tạo ra kết quả sai trong những trường hợp cực đoan này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét việc thêm từng ứng viên vào nhóm, tạo ra tất cả$n+1$các cặp giai thừa có thể có với đội đối phương và tính toán độ khó tối đa cho mỗi đội. Cách tiếp cận này đúng về nguyên tắc nhưng hoàn toàn không khả thi. Vì$n = 2 \cdot 10^5$, số lượng hoán vị là vô cùng lớn. 

Quan sát quan trọng là việc giảm thiểu sự khác biệt tối đa tương đương với việc ghép các kỹ năng nhỏ nhất với các kỹ năng đối lập nhỏ nhất và lớn nhất với kỹ năng lớn nhất, sau khi sắp xếp. Điều này là do chức năng$f(x) = b_j - a_i$đơn điệu về mặt sắp xếp: việc kết hợp người chơi mạnh với đối thủ mạnh sẽ làm giảm khoảng cách trong trường hợp xấu nhất. Do đó, việc sắp xếp cả đội hiện có và đối thủ cho phép chúng tôi tính toán trước sự khác biệt giữa các thành viên liên tiếp. Chúng ta có thể tính toán hai mảng khác biệt, một mảng biểu thị việc bỏ qua đối thủ đầu tiên và mảng còn lại bỏ qua đối thủ cuối cùng, bao gồm tất cả các điểm chèn cho ứng cử viên mới một cách hiệu quả. Sau đó, đối với mỗi ứng viên, tìm kiếm nhị phân sẽ tìm ra vị trí chính xác trong nhóm đã được sắp xếp và các mảng được tính toán trước sẽ nhanh chóng giảm thiểu độ khó. 

Nhận xét rằng chúng ta chỉ cần xem xét các cặp liền kề sau khi sắp xếp sẽ làm giảm bài toán từ$O((n+1)!)$ĐẾN$O(n \log n + m \log n)$, có thể chấp nhận được đối với kích thước đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n+1)!) | O(n) | Quá chậm | 
| Sắp xếp + Tham lam + Tìm kiếm nhị phân | O(n log n + m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp đội ngũ hiện tại của Polycarp$a$và đội đối phương$b$. Điều này đảm bảo rằng việc ghép nối theo thứ tự sẽ giảm thiểu những khó khăn riêng lẻ. 
2. Tính hai mảng tiền tố:`left_diffs`để ghép nối$a_i$với$b_{i-1}$(bỏ qua đối thủ đầu tiên) và`right_diffs`để ghép nối$a_i$với$b_i$(bỏ qua đối thủ cuối cùng). Mỗi cửa hàng chênh lệch tối đa cho kịch bản đó. Những mảng này đại diện cho hai "khoảng trống" tự nhiên mà ứng viên mới có thể lấp đầy. 
3. Đối với mỗi ứng viên$c_k$, sử dụng tìm kiếm nhị phân để xác định vị trí$c_k$sẽ phù hợp với đội được sắp xếp$a$. Điều này xác định khoảng cách đối thủ mà nó sẽ chiếm giữ. 
4. Tính độ khó bằng cách kết hợp độ khó tối đa của tiền tố ở bên trái, độ khác biệt do thí sinh đưa ra và độ khó tối đa của hậu tố ở bên phải. Bước này sử dụng các mảng được tính toán trước cho$O(1)$truy cập. 
5. Đưa ra độ khó tối thiểu cho mỗi thí sinh. 

Tại sao nó hoạt động: Sắp xếp đảm bảo rằng việc ghép nối theo thứ tự sẽ giảm thiểu sự khác biệt tối đa. Việc tính toán trước mức tối đa của tiền tố cho phép chúng tôi tính toán một cách hiệu quả độ khó tối đa mới cho bất kỳ thao tác chèn ứng viên nào. Tìm kiếm nhị phân đảm bảo ứng viên được đặt ở vị trí tối ưu so với nhóm hiện có. Bằng cách xây dựng, thuật toán luôn đánh giá mức chênh lệch tối đa tối thiểu có thể đạt được đối với mỗi ứng viên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import bisect

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))
m = int(input())
c = list(map(int, input().split()))

a.sort()
b.sort()

left = [0] * n
right = [0] * n

for i in range(n):
    left[i] = b[i] - a[i]
for i in range(n):
    right[i] = b[i+1] - a[i]

pref_left = [0] * (n+1)
pref_right = [0] * (n+1)

for i in range(n):
    pref_left[i+1] = max(pref_left[i], left[i])
for i in range(n-1, -1, -1):
    pref_right[i] = max(pref_right[i+1], right[i])

res = []
for val in c:
    idx = bisect.bisect_left(a, val)
    max_left = pref_left[idx]
    max_right = pref_right[idx]
    max_diff = max(max_left, max_right, b[idx] - val)
    res.append(str(max_diff))

print(' '.join(res))
```Giải pháp này trước tiên sắp xếp các đội, sau đó tính toán trước sự khác biệt khi bỏ qua một trong hai đầu của mảng đối thủ. Tìm kiếm nhị phân xác định điểm chèn tối ưu của ứng viên và độ khó tối đa được tính theo thời gian không đổi cho mỗi ứng viên. Việc xử lý cẩn thận các mảng tiền tố sẽ ngăn chặn tình trạng sai sót
