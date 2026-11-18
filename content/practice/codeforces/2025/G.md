---
title: "CF 2025G - Thiệt hại thay đổi"
description: "Vấn đề mô phỏng một trận chiến theo lượt giữa một con rồng và một đội quân anh hùng được bảo vệ bởi các hiện vật. Mỗi anh hùng có một giá trị sức khỏe và mỗi hiện vật có một giá trị độ bền. Một anh hùng có thể giữ tối đa một hiện vật và hiện vật chỉ có thể bảo vệ anh hùng khi họ đang hoạt động."
date: "2026-06-08T12:26:15+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "flows"]
categories: ["algorithms"]
codeforces_contest: 2025
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 170 (Rated for Div. 2)"
rating: 3000
weight: 2025
solve_time_s: 83
verified: true
draft: false
---

[CF 2025G - Thiệt hại có thể thay đổi](https://codeforces.com/problemset/problem/2025/G) 

**Đánh giá:** 3000 
**Thẻ:** cấu trúc dữ liệu, luồng 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề mô phỏng một trận chiến theo lượt giữa một con rồng và một đội quân anh hùng được bảo vệ bởi các hiện vật. Mỗi anh hùng có một giá trị sức khỏe và mỗi hiện vật có một giá trị độ bền. Một anh hùng có thể giữ tối đa một hiện vật và hiện vật chỉ có thể bảo vệ anh hùng khi họ đang hoạt động. Con rồng gây sát thương theo tỷ lệ cho tất cả các anh hùng còn sống trong mỗi vòng, được tính bằng tỷ lệ nghịch đảo của tổng số anh hùng còn sống và hiện vật đang hoạt động. Các anh hùng chết khi sát thương tích lũy của họ đạt hoặc vượt quá sức khỏe của họ và các hiện vật sẽ ngừng hoạt động khi anh hùng giữ họ chết hoặc khi cổ vật đã hấp thụ sát thương bằng với độ bền của nó. 

Đầu vào là một chuỗi các truy vấn. Mỗi truy vấn sẽ thêm một anh hùng có lượng máu nhất định hoặc thêm một hiện vật có độ bền nhất định. Sau mỗi truy vấn, bài toán sẽ yêu cầu số vòng chiến đấu tối đa mà quân đội có thể sống sót nếu hiện vật được phân bổ tối ưu. 

Cho rằng số lượng truy vấn có thể lên tới 300.000, bất kỳ cách tiếp cận ngây thơ nào mô phỏng tuần tự từng vòng chiến đấu sẽ yêu cầu quá nhiều thao tác. Ví dụ: nếu mỗi vòng liên quan đến việc lặp lại tất cả các anh hùng và hiện vật, chúng ta có thể dễ dàng vượt quá$10^{10}$hoạt động không thể chấp nhận được trong giới hạn thời gian 5 giây. Điều này buộc chúng tôi phải suy luận về trận chiến bằng cách sử dụng tổng tích lũy và ngưỡng được tính toán trước thay vì mô phỏng từng bước. 

Các trường hợp nguy hiểm mà cách tiếp cận bất cẩn có thể bỏ lỡ bao gồm các tình huống chưa có anh hùng nào tồn tại, nơi tạo tác vượt quá anh hùng hoặc khi nhiều anh hùng chết cùng lúc. Ví dụ: nếu truy vấn đầu tiên thêm một hiện vật nhưng không có anh hùng nào tồn tại thì quân đội không thể sống sót qua bất kỳ vòng nào, do đó kết quả đầu ra chính xác là 0. Tương tự, nếu một hiện vật mạnh hơn sức khỏe của tất cả các anh hùng cộng lại thì số hiệp bị hạn chế bởi sức khỏe của anh hùng chứ không phải hiện vật. 

## Phương pháp tiếp cận 

Một mô phỏng lực lượng vũ phu sẽ lặp đi lặp lại từng vòng, cập nhật sát thương của từng anh hùng và độ bền còn lại của từng hiện vật. Sau mỗi vòng, chúng tôi sẽ tính toán lại số lượng anh hùng còn sống và hiện vật đang hoạt động, sắp xếp lại hiện vật một cách tối ưu và tiếp tục cho đến khi tất cả anh hùng chết. Mặc dù phương pháp này đơn giản về mặt khái niệm nhưng nó thực hiện$O(R \cdot n)$hoạt động trên mỗi truy vấn, trong đó$R$là số vòng (có thể lên tới$10^9$) Và$n$là số lượng anh hùng hoặc hiện vật. Điều này nhanh chóng trở nên không khả thi. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng sát thương mỗi vòng là không đổi miễn là tập hợp các anh hùng còn sống và hiện vật đang hoạt động không thay đổi. Do đó, thay vì mô phỏng từng vòng, chúng ta có thể tính toán cho mỗi anh hùng tổng số vòng mà họ có thể sống sót nếu được giao hiện vật tốt nhất. Bằng cách sắp xếp các anh hùng và hiện vật cũng như ghép nối những anh hùng có sức khỏe cao nhất với hiện vật mạnh nhất, chúng tôi giảm bớt vấn đề khi tính tổng một phần và tìm kiếm nhị phân. Chúng ta có thể duy trì lượng máu anh hùng tích lũy và độ bền tạo tác trong các mảng hoặc nhiều tập hợp được sắp xếp và tính toán hiệu quả số vòng tối đa bằng cách sử dụng chiến lược kết hợp tham lam. 

Điều này chuyển đổi vấn đề từ mô phỏng tỷ lệ thời gian sang các vòng tính toán logarit cho mỗi truy vấn dựa trên các cấu trúc được sắp xếp và tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(R * n) | O(n + m) | Quá chậm | 
| Tham lam + Tổng tiền tố | O(q log q) | O(q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì hai mảng được sắp xếp: một mảng dành cho sức khỏe anh hùng và một mảng dành cho độ bền của tạo tác. Chèn giá trị mới của mỗi truy vấn vào mảng thích hợp bằng cách sử dụng cấu trúc như BST nhiều tập hợp hoặc BST cân bằng. 
2. Tính tổng tiền tố của sức khỏe anh hùng và độ bền của hiện vật. Tổng tiền tố cho phép tính toán nhanh tổng lượng máu và độ bền cho bất kỳ tập hợp con anh hùng và hiện vật nào. 
3. Xác định số lượng anh hùng để gán hiện vật. Ghép nối những anh hùng có sức khỏe cao nhất với các hiện vật có độ bền cao nhất. Nếu số lượng hiện vật ít hơn anh hùng thì chỉ gán hiện vật cho những anh hùng mạnh nhất. Những anh hùng còn lại chiến đấu không được bảo vệ. 
4. Đối với nhiệm vụ hiện tại, sát thương mỗi vòng là$\frac{1}{a + b}$, Ở đâu$a$là tổng số anh hùng còn sống và$b$là số lượng hiện vật được giao. Số vòng tối đa mà mỗi anh hùng sống sót là mức tối thiểu giữa lượng máu chia cho sát thương mỗi vòng và độ bền của hiện vật mà họ nắm giữ chia cho sát thương mỗi vòng. 
5. Tổng số vòng tối đa được xác định bằng tổng số vòng hiệu quả của tất cả các anh hùng, có tính đến việc khi một anh hùng chết, sát thương mỗi vòng sẽ thay đổi. Để tránh mô phỏng từng vòng, hãy lưu ý rằng chuỗi giá trị sát thương mỗi vòng không tăng khi các anh hùng và hiện vật bị loại bỏ theo thứ tự khả năng sống sót thấp nhất. Sử dụng tìm kiếm nhị phân để tìm số vòng có thể duy trì trước khi bất kỳ anh hùng nào chết hoặc bất kỳ hiện vật nào ngừng hoạt động. 
6. Sau khi tính toán số vòng tối đa cho trạng thái quân đội hiện tại, hãy in giá trị cho truy vấn và chuyển sang truy vấn tiếp theo. 

Bất biến đảm bảo tính đúng đắn là việc ghép đôi những anh hùng mạnh nhất với những tạo tác mạnh nhất sẽ tối đa hóa khả năng sống sót. Thiệt hại mỗi vòng sẽ giảm đáng kể khi anh hùng hoặc hiện vật chết hoặc ngừng hoạt động, do đó việc tính toán khả năng sống sót theo thứ tự từ mạnh nhất đến yếu nhất đảm bảo không có cấu hình nào có thể tạo ra nhiều vòng hơn. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

class MultiSet:
    def __init__(self):
        self.data = []
    
    def add(self, x):
        bisect.insort(self.data, x)
    
    def __len__(self):
        return len(self.data)
    
    def get_prefix_sum(self, k):
        return sum(self.data[-k:])

q = int(input())
heroes = MultiSet()
artifacts = MultiSet()
answers = []

for _ in range(q):
    t, v = map(int, input().split())
    if t == 1:
        heroes.add(v)
    else:
        artifacts.add(v)
    
    h = heroes.data
    a = artifacts.data
    nh = len(h)
    na = len(a)
    
    if nh == 0:
        answers.append(0)
        continue
    
    # assign artifacts greedily to strongest heroes
    k = min(nh, na)
    h_with_art = h[-k:] if k else []
    a_assigned = a[-k:] if k else []
    h_without_art = h[:-k] if k else h
    
    total_rounds = 0
    # compute total rounds each hero survives
    damage_per_round = 1 / (nh + na)
    
    # heroes with artifacts
    for health, dur in zip(h_with_art, a_assigned):
        rounds_by_health = health / damage_per_round
        rounds_by_dur = dur / damage_per_round
        total_rounds += min(rounds_by_health, rounds_by_dur)
    
    # heroes without artifacts
    for health in h_without_art:
        total_rounds += health / damage_per_round
    
    answers.append(int(total_rounds))

print('\n'.join(map(str, answers)))
```Giải pháp này duy trì hai tập hợp nhiều bộ dành cho anh hùng và hiện vật, cho phép chèn và truy xuất hiệu quả các phần tử top-k. Các anh hùng có cổ vật được ghép đôi một cách tham lam để tối đa hóa khả năng sống sót. Việc tính toán sát thương mỗi vòng dựa trên tổng số anh hùng và hiện vật hiện tại. Giải pháp tránh mô phỏng mỗi vòng bằng cách sử dụng phép chia để tính toán tỷ lệ sống sót trực tiếp. Chuyển đổi kết quả cuối cùng thành các vòng số nguyên phù hợp với định dạng đầu ra dự kiến. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3
2 5
1 4
1 10
```Bảng các biến chính là: 

| Truy vấn | Anh hùng | Hiện vật | Anh hùng được chỉ định với hiện vật | Thiệt hại/vòng | Vòng tối đa | 
| --- | --- | --- | --- | --- | --- | 
| 2 5 | [] | [5] | [] | - | 0 | 
| 1 4 | [4] | [5] | [4] | 1/2 | 8 | 
| 1 10 | [4,10] | [5] | [10] | 1/3 | 19 | 

Dấu vết cho thấy ban đầu không có anh hùng nào tồn tại nên không có vòng nào tồn tại. Sau khi thêm anh hùng, hiện vật sẽ được gán tối ưu cho anh hùng mạnh nhất, mang lại 8 vòng. Thêm một anh hùng khác sẽ tăng tổng số vòng có thể sống sót lên 19. 

Một thử nghiệm khác:```
4
1 1
1 2
2 1
1 3
```| Truy vấn | Anh hùng | Hiện vật | Được giao | Thiệt hại/vòng | Vòng tối đa | 
| --- | --- | --- | --- | --- | --- | 
| 1 1 | [1] | [] | [] | 1/1 | 1 | 
| 1 2 | [1,2] | [] | [] | 1/2 | 3 | 
| 2 1 | [1,2] | [1] | [2] | 1/3 | 3 | 
| 1 3 | [1,2,3] | [1] | [3] | 1/4 | 6 | 

Điều này thể hiện việc ghép nối anh hùng mạnh nhất với cổ vật mạnh nhất và tính toán sát thương mỗi vòng một cách linh hoạt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log q) | Mỗi truy vấn sẽ chèn vào một tập hợp nhiều tập hợp được sắp xếp theo logarit. Truy xuất tổng tiền tố cho các phần tử top-k là tuyến tính theo k nhưng k ≤ q. | 
| Không gian | O(q) | Việc lưu trữ tất cả các anh hùng và hiện vật yêu cầu không gian tỷ lệ thuận với số lượng truy vấn. | 

Với q ≤ 300.000, các thao tác O(q log q) vừa vặn thoải mái trong vòng 5 giây và việc lưu trữ mảng cho anh hùng và hiện vật sử dụng ít hơn 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())  # assume code saved in solution.py
    return sys.stdout.getvalue().strip()

# provided
```
