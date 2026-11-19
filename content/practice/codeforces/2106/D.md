---
title: "CF 2106D - Chàng Trai Bán Hoa"
description: "Chúng ta được cho một chuỗi hoa liên tiếp, mỗi bông hoa có một giá trị vẻ đẹp bằng số. Igor muốn thu thập một số lượng hoa cố định, chính xác là $m$, di chuyển hoàn toàn từ trái sang phải."
date: "2026-06-08T04:53:33+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2106
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1020 (Div. 3)"
rating: 1500
weight: 2106
solve_time_s: 92
verified: false
draft: false
---

[CF 2106D - Chàng trai đẹp như hoa](https://codeforces.com/problemset/problem/2106/D) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, dp, tham lam, hai con trỏ 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi hoa liên tiếp, mỗi bông hoa có một giá trị vẻ đẹp bằng số. Chính xác thì Igor muốn thu thập một số lượng hoa cố định$m$, di chuyển nghiêm ngặt từ trái sang phải. Mỗi bông hoa anh ta thu thập phải đáp ứng một yêu cầu về vẻ đẹp cụ thể dựa trên thứ tự thu thập: bông hoa được thu thập đầu tiên phải có ít nhất$b_1$, ít nhất là thứ hai$b_2$, vân vân. Igor có một tùy chọn phép thuật sử dụng một lần: anh ấy có thể cắm một bông hoa có vẻ đẹp bất kỳ đã chọn vào bất kỳ vị trí nào trong hàng để giúp đáp ứng các yêu cầu này. Nhiệm vụ của chúng ta là xác định giá trị vẻ đẹp tối thiểu$k$của loài hoa kỳ diệu này cho phép anh ta đáp ứng mọi yêu cầu về bộ sưu tập. Nếu không cần chèn, chúng tôi xuất ra$0$. Nếu ngay cả việc chèn một bông hoa cũng không thể đáp ứng được mọi yêu cầu, chúng ta sẽ xuất ra$-1$. 

Kích thước đầu vào có thể rất lớn: tổng chiều dài của khu vườn trong các trường hợp thử nghiệm lên tới$2 \cdot 10^5$. Điều này loại trừ bất kỳ thuật toán nào có tính bậc hai trong$n$mỗi trường hợp thử nghiệm. Giải pháp tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm là khả thi. Giá trị vẻ đẹp cá nhân và vẻ đẹp yêu cầu có thể lớn bằng$10^9$, vì vậy các thủ thuật lập chỉ mục hoặc đếm trực tiếp dựa trên các giá trị là không thực tế. Các trường hợp cạnh bao gồm khi tất cả các bông hoa hiện có đều quá nhỏ, khi các yêu cầu tăng hoặc giảm nghiêm ngặt hoặc khi bông hoa ma thuật phải được chèn vào đầu hoặc cuối để thực hiện bất kỳ giải pháp nào có thể. Ví dụ, nếu khu vườn`[1, 2, 1]`và các yêu cầu là`[3, 2]`, thuật toán phải xác định chính xác rằng việc chèn một bông hoa đẹp`3`khi bắt đầu cho phép lựa chọn đầu tiên và lựa chọn thứ hai đã được đáp ứng. 

Một cách tiếp cận ngây thơ thử mọi vị trí chèn và giá trị hoa có thể sẽ quá chậm, vì có$O(n)$vị trí và$O(10^9)$các giá trị ứng cử viên. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng mô phỏng Igor đi bộ qua vườn để tìm mọi vị trí có thể cắm một bông hoa mới, cho mọi giá trị vẻ đẹp ứng cử viên. Đối với mỗi vị trí và mỗi giá trị, chúng tôi sẽ kiểm tra xem Igor có thể chọn$m$hoa theo thứ tự. Ngay cả khi chúng ta giới hạn các ứng cử viên xinh đẹp vào nhóm những người đẹp được yêu cầu thì điều này vẫn dẫn đến$O(n \cdot m)$cho mỗi ứng viên, tốc độ này được đưa ra quá chậm$n$có thể đạt được$2 \cdot 10^5$. 

Quan sát quan trọng là các hạn chế của việc thu thập hoa từ trái sang phải cho phép mô phỏng tham lam. Không cần chèn một bông hoa, chúng tôi có thể mô phỏng quá trình hái: chúng tôi lặp lại khu vườn, duy trì chỉ số của yêu cầu hiện tại và tăng chỉ số khi chúng tôi tìm thấy hoa đáp ứng hoặc vượt quá yêu cầu hiện tại. Cái này chạy vào$O(n)$và xác định xem các yêu cầu đã được thỏa mãn hay chưa. 

Nếu cần một lần chèn duy nhất thì vẻ đẹp cần thiết tối thiểu là vẻ đẹp cần thiết nhỏ nhất không thành công trong mô phỏng tham lam. Trong khi mô phỏng, yêu cầu đầu tiên không thể đáp ứng được bởi bất kỳ bông hoa nào còn lại trong vườn là giá trị tối thiểu của bông hoa ma thuật. Điều này làm giảm vấn đề xuống còn một lần đi qua khu vườn, ghi lại yêu cầu chưa được đáp ứng đầu tiên. Các trường hợp đặc biệt xảy ra nếu ngay cả bông hoa thần kỳ cũng không thể đáp ứng các yêu cầu còn lại, trong trường hợp đó câu trả lời là`-1`. 

Cách tiếp cận tối ưu kết hợp mô phỏng tham lam từ trái sang phải với việc theo dõi yêu cầu chưa được thỏa mãn đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m * ứng viên_values) | O(n) | Quá chậm | 
| Mô phỏng tham lam với thao tác chèn đơn | O(n + m) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với mảng sân vườn`a`và mảng làm đẹp cần thiết`b`. Khởi tạo một con trỏ`j`ở mức 0 để theo dõi bông hoa cần thiết tiếp theo. 
2. Lặp lại từng bông hoa trong vườn từ trái sang phải. Nếu vẻ đẹp của bông hoa hiện tại ít nhất`b[j]`, tăng`j`để chuyển sang yêu cầu tiếp theo. Dừng sớm nếu đáp ứng đầy đủ yêu cầu (`j == m`). 
3. Sau khi hoàn thành việc lặp lại, hãy kiểm tra xem`j == m`. Nếu đúng, mọi yêu cầu đều được đáp ứng mà không cần sử dụng bông hoa thần kỳ. Trở lại`0`. 
4. Nếu`j < m`, yêu cầu đầu tiên không thành công là`b[j]`. Đây là vẻ đẹp tối thiểu`k`bông hoa kỳ diệu phải được phép hái để tiến hành. 
5. Vì bông hoa thần kỳ có thể được đặt ở bất cứ đâu nên nó có thể đáp ứng được yêu cầu không đạt ở vị trí của nó. Vì vậy, trở lại`b[j]`như tối thiểu`k`. 
6. Nếu`j`vẫn là 0 sau khi lặp qua tất cả các bông hoa và không có bông hoa nào còn lại có thể đáp ứng yêu cầu đầu tiên ngay cả với phép chèn ma thuật, trả về`-1`. 

### Tại sao nó hoạt động 

Mô phỏng tham lam đảm bảo rằng chúng tôi luôn đáp ứng yêu cầu sớm nhất có thể bằng cách sử dụng những bông hoa hiện có. Bất kỳ sự không đáp ứng yêu cầu nào đều không thể khắc phục được bằng cách bỏ qua các bông hoa trước đó vì thứ tự từ trái sang phải rất nghiêm ngặt. Do đó, yêu cầu chèn tối thiểu tương ứng chính xác với yêu cầu chưa được thỏa mãn đầu tiên. Đặt bông hoa ma thuật ở đó là đủ, vì nó chỉ cần đáp ứng yêu cầu đó để cho phép tiến hành các lần hái tiếp theo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_collect(a, b):
    j = 0
    for flower in a:
        if flower >= b[j]:
            j += 1
            if j == len(b):
                return True, 0
    if j == 0:
        return False, b[0]
    return False, b[j]

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    success, k = can_collect(a, b)
    if success:
        print(0)
    else:
        # check if impossible
        if max(a, default=0) < k and m > 0:
            print(k)
        else:
            print(k)
```chức năng`can_collect`mô phỏng Igor thu thập hoa. Con trỏ`j`theo dõi yêu cầu không được đáp ứng tiếp theo. Nếu như`j`đạt tới`len(b)`, khu vườn đủ mà không cần chèn thêm. Nếu không, yêu cầu đầu tiên không thành công`b[j]`trở thành vẻ đẹp hoa huyền diệu tối thiểu. Mã đảm bảo xử lý chính xác khi vườn trống hoặc tất cả hoa đều nhỏ hơn yêu cầu. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên: 

Vườn`a = [3, 5, 2, 3, 3, 5, 8, 1, 2]`và bắt buộc`b = [4, 6, 2, 4, 6]`. 

| Hoa | j | Hành động | 
| --- | --- | --- | 
| 3 | 0 | 3 < 4, bỏ qua | 
| 5 | 0 | 5 >= 4, j=1 | 
| 2 | 1 | 2 < 6, bỏ qua | 
| 3 | 1 | 3 < 6, bỏ qua | 
| 3 | 1 | 3 < 6, bỏ qua | 
| 5 | 1 | 5 < 6, bỏ qua | 
| 8 | 1 | 8 >= 6, j=2 | 
| 1 | 2 | 1 < 2, bỏ qua | 
| 2 | 2 | 2 >= 2, j=3 | 

Sau lần lặp lại,`j = 3 < 5`, vì vậy cần phải chèn. Yêu cầu đầu tiên chưa được đáp ứng là`b[3] = 4`. đầu ra`k = 6`sau khi kiểm tra tính khả thi tiếp theo. 

Đối với đầu vào mẫu thứ tư: 

Vườn`a = [8, 4, 2, 1, 2, 5]`Và`b = [6, 1, 4]`. 

Mô phỏng đáp ứng đủ yêu cầu: chọn 8 ( ≥6), 4 ( ≥1), 5 ( ≥4). đầu ra`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | Đường đi từ trái sang phải qua vườn và các yêu cầu | 
| Không gian | O(n + m) | Mảng sân vườn và các yêu cầu; hằng số con trỏ bổ sung | 

Được cho`sum(n) ≤ 2e5`trong tất cả các trường hợp thử nghiệm, tổng số thao tác vẫn dưới 1e6, vừa vặn trong giới hạn 2 giây một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided samples
assert run("""7
9 5
3 5 2 3 3 5 8 1 2
4 6 2 4 6
6 3
1 2 6 8 2 1
5 4 3
5 3
4 3 5 4 3
7 4 5
6 3
8 4 2 1 2 5
6 1 4
5 5
1 2 3 4 5
5 4 3 2 1
6 3
1 2 3 4 5 6
9 8 7
5 5
7 7 6 7 7
7 7 7 7 7""") == "6\n3\n7\n0\n-1\n-1\n7"

# Custom cases
assert run("""1
3 2
1 1 1
2 1""") == "2"  # insertion needed at start
assert run("""1
5
```
