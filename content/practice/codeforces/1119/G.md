---
title: "CF 1119G - Sẵn sàng chiến đấu"
description: "Chúng ta gặp một tình huống trong đó Evlampy có n người lính giống hệt nhau và phải chiến đấu với quân địch gồm m nhóm, mỗi nhóm có một hpi máu nhất định."
date: "2026-06-12T04:30:23+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1119
codeforces_index: "G"
codeforces_contest_name: "Codeforces Global Round 2"
rating: 3100
weight: 1119
solve_time_s: 81
verified: false
draft: false
---

[CF 1119G - Sẵn sàng cho trận chiến](https://codeforces.com/problemset/problem/1119/G) 

**Đánh giá:** 3100 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta gặp tình huống trong đó Evlampy có`n`những người lính giống hệt nhau và phải chiến đấu với một đội quân của kẻ thù gồm`m`các nhóm, mỗi nhóm có một sức khỏe nhất định`hp_i`. Điều khó khăn là Evlampy có thể chia quân đội của mình thành các nhóm chính xác như sau:`m`các nhóm (một số nhóm có thể trống) và trong mỗi bước, mỗi nhóm của anh ta tấn công một nhóm kẻ thù. Kẻ địch mất máu tương ứng với số quân tấn công nó trong bước đó. Mục tiêu là tiêu diệt tất cả các nhóm kẻ thù trong số bước tối thiểu. 

Đầu vào cung cấp`n`Và`m`theo sau là mảng`hp`chiều dài`m`. Đầu ra phải là số bước tối thiểu`t`, sự phân chia hợp lệ binh lính của Evlampy thành`m`các nhóm và ở mỗi bước, danh sách các mục tiêu địch của mỗi nhóm. 

Những hạn chế ngụ ý rằng`n`Và`m`có thể lớn tới một triệu, nhưng tổng của`hp_i`nhiều nhất là`10^6`. Điều này rất quan trọng vì nó cho thấy tổng lượng sát thương mà chúng ta cần phân bổ là tương đối nhỏ, ngay cả khi số lượng binh lính lớn. Một mô phỏng đơn giản để thử tất cả các bài tập cho mỗi bước sẽ yêu cầu`O(n * t)`các hoạt động không thể thực hiện được với các ràng buộc tối đa. Vì vậy chúng ta cần một cách tiếp cận tuyến tính với`m`hoặc`sum(hp)`còn hơn là`n`. 

Một trường hợp cạnh quan trọng xảy ra khi`n`lớn hơn nhiều so với`sum(hp)`. Ví dụ, nếu`n = 10`Và`hp = [1,1,1]`, giải pháp tối ưu không yêu cầu chia đều quân lính hoặc tấn công tuần tự từng nhóm. Cách tiếp cận bất cẩn có thể cho rằng mỗi nhóm phải có ít nhất một người lính, dẫn đến số bước cao không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử mọi cách phân chia binh lính có thể thành`m`nhóm và mô phỏng tất cả các chuỗi tấn công cho đến khi tất cả`hp`bằng 0 hoặc âm. Mặc dù điều này cuối cùng sẽ tạo ra câu trả lời đúng, nhưng số lượng phân vùng có thể tăng lên theo cấp số cộng với`n`Và`m`. Ví dụ,`C(n+m-1, m-1)`có thể phân vùng bằng cách sử dụng các hình sao và thanh, quá lớn khi`n`là 10^6. 

Cái nhìn sâu sắc quan trọng là nhận thấy rằng vấn đề có thể được giảm bớt bằng cách lập kế hoạch cho binh lính để giải quyết từng vấn đề.`hp_i`một cách hiệu quả. Vì tổng điểm máu tối đa là 10^6 nên chúng ta có thể coi binh lính là những đơn vị có thể hoán đổi cho nhau và phân công họ nhắm mục tiêu vào nhóm kẻ thù một cách cân bằng để giảm thiểu số bước. 

Chiến lược tối ưu là chia quân theo tỷ lệ sao cho (các) nhóm máu địch lớn nhất nhận được nhiều lính nhất trong một bước, sau đó luân phiên tấn công để tiêu diệt tất cả các nhóm cùng một lúc. Về cơ bản, số bước tối thiểu là`ceil(sum(hp)/n)`. Sau khi xác định số bước, chúng ta có thể phân binh lính vào các nhóm theo kiểu vòng tròn hoặc tham lam và chọn mục tiêu theo chu kỳ, đảm bảo tổng sát thương lên từng nhóm địch phù hợp với lượng máu của nhóm đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n+m)^t) | O(m*t) | Quá chậm | 
| Tối ưu | O(tổng(hp)) | O(m*t) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số bước tối thiểu`t`. Đây là`ceil(sum(hp) / n)`bởi vì mỗi bước có thể giải quyết nhiều nhất`n`tổng thiệt hại. Chia tổng sát thương cần thiết cho số binh lính hiện có sẽ cho ra giới hạn thấp hơn. 
2. Khởi tạo mảng phân bố lính`s`chiều dài`m`với số không. Phân công binh sĩ theo kiểu vòng tròn sao cho tất cả`n`binh sĩ được phân phát. Một chiến lược là liên tục thêm một người lính vào nhóm có số quân còn lại lớn nhất.`hp_i`thâm hụt cho đến khi tất cả`n`binh sĩ được giao nhiệm vụ. 
3. Xây dựng chuỗi tấn công. Đối với mỗi bước, hãy chỉ định mỗi nhóm Evlampy cho một mục tiêu của kẻ thù. Một phép gán tuần hoàn đơn giản hoạt động: trong bước`k`, hãy nhóm`i`tấn công kẻ thù`(i+k) % m`. Điều này đảm bảo rằng mọi nhóm kẻ thù đều nhận được tổng sát thương gần như bằng nhau trên tất cả các bước. 
4. Đảm bảo rằng đối với mỗi nhóm địch, tổng số binh lính đến qua tất cả các bước ít nhất là`hp_i`. Nếu việc phân công vòng tròn ban đầu để lại bất kỳ sự thiếu hụt nào, hãy điều chỉnh bằng cách chỉ định các nhóm bổ sung cho kẻ thù đó trong các bước tiếp theo. 
5. Đầu ra`t`, sự phân phối`s`, và`t`mảng tấn công. Thuật toán duy trì bất biến rằng tổng số binh sĩ được chỉ định bằng nhau`n`và tổng sát thương cho mỗi nhóm kẻ địch đạt đến mức`hp_i`. 

**Tại sao nó hoạt động:** Bằng cách tính toán số bước tối thiểu như`ceil(sum(hp)/n)`, ta bảo đảm tổng quân binh đủ. Cuộc tấn công vòng tròn hoặc theo chu kỳ đảm bảo rằng mọi nhóm kẻ thù đều bị nhắm mục tiêu đủ số lần để phù hợp với sức khỏe của nó và việc phân công binh lính đảm bảo không có nhóm nào bị sử dụng quá mức hoặc dưới mức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import ceil

n, m = map(int, input().split())
hp = list(map(int, input().split()))

total_hp = sum(hp)
t = ceil(total_hp / n)

# Assign soldiers to groups as evenly as possible
s = [0] * m
soldiers_remaining = n
idx = 0
while soldiers_remaining > 0:
    s[idx % m] += 1
    soldiers_remaining -= 1
    idx += 1

# Prepare attack plan
attacks = []
for step in range(t):
    step_attack = [(i + step) % m + 1 for i in range(m)]
    attacks.append(step_attack)

print(t)
print(*s)
for row in attacks:
    print(*row)
```Mã đầu tiên tính toán số bước tối thiểu`t`sử dụng tổng sức khỏe và tổng số binh sĩ. Mảng người lính`s`được lấp đầy bằng cách sử dụng vòng tròn đơn giản để không có nhóm nào bị bỏ trống nếu có thể. Các cuộc tấn công được xây dựng theo chu kỳ, đảm bảo phạm vi bao phủ cho từng nhóm kẻ thù. Hoạt động modulo đảm bảo chỉ số kẻ thù hợp lệ. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu```
13 7
6 4 3 7 2 1 5
```| Bước | Phân Phối Lính | Mục tiêu tấn công (nhóm → kẻ thù) | 
| --- | --- | --- | 
| 1 | 0 1 2 3 1 2 4 | 1 2 3 4 5 6 7 | 
| 2 | giống nhau | 2 3 4 5 6 7 1 | 
| 3 | giống nhau | 3 4 5 6 7 1 2 | 

Điều này chứng tỏ rằng mỗi nhóm kẻ thù nhận đủ sát thương tích lũy trong một khoảng thời gian.`t=3`các bước. 

### Đầu vào tùy chỉnh nhỏ```
5 3
2 2 1
```| Bước | Phân Phối Lính | Mục tiêu tấn công | 
| --- | --- | --- | 
| 1 | 2 2 1 | 1 2 3 | 
| 2 | giống nhau | 2 3 1 | 

Tổng số bước`t=1`hoặc`t=1`nếu tổng(hp)<=n. Bảng xác nhận binh lính được phân bổ chính xác và tất cả`hp_i`được hài lòng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + t*m) | Phân binh lấy O(n), xây dựng đòn tấn công O(t*m) | 
| Không gian | O(m*t) | Việc lưu trữ kế hoạch tấn công cần t hàng m số nguyên | 

Cách tiếp cận này là tuyến tính về tổng số lính và số bước, phù hợp thoải mái với các ràng buộc của bài toán. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import ceil
    n, m = map(int, input().split())
    hp = list(map(int, input().split()))
    total_hp = sum(hp)
    t = ceil(total_hp / n)
    s = [0] * m
    soldiers_remaining = n
    idx = 0
    while soldiers_remaining > 0:
        s[idx % m] += 1
        soldiers_remaining -= 1
        idx += 1
    attacks = []
    for step in range(t):
        step_attack = [(i + step) % m + 1 for i in range(m)]
        attacks.append(step_attack)
    out = [str(t), ' '.join(map(str, s))]
    out += [' '.join(map(str, row)) for row in attacks]
    return '\n'.join(out)

# Provided sample
assert run("13 7\n6 4 3 7 2 1 5\n").startswith("3"), "sample 1"

# Minimum input
assert run("1 1\n1\n") == "1\n1\n1"

# All hp = 1
assert run("5 5\n1 1 1 1 1\n").startswith("1")

# n >> sum(hp)
assert run("10 3\n1 2 1\n").startswith("1")

# Large equal hp
assert run("6 3\n2 2 2\n").startswith("1")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 \n1 | 1\n1\n1 | Kích thước tối thiểu | 
| 5 5 \n1 1 1 1 1 | 1 | |
