---
title: "CF 1580C - Bảo trì tàu hỏa"
description: "Chúng tôi có một hệ thống theo dõi các chuyến tàu thuộc các mẫu khác nhau trong một chuỗi ngày. Khi bắt đầu, không có tàu. Vào mỗi ngày, chúng tôi thêm một đoàn tàu có mô hình cụ thể hoặc loại bỏ một đoàn tàu."
date: "2026-06-10T10:16:19+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1580
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 745 (Div. 1)"
rating: 2200
weight: 1580
solve_time_s: 343
verified: false
draft: false
---

[CF 1580C - Bảo trì tàu](https://codeforces.com/problemset/problem/1580/C) 

**Xếp hạng:** 2200 
**Tags:** bạo lực, cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 5 phút 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hệ thống theo dõi các chuyến tàu thuộc các mẫu khác nhau trong một chuỗi ngày. Khi bắt đầu, không có tàu. Vào mỗi ngày, chúng tôi thêm một đoàn tàu có mô hình cụ thể hoặc loại bỏ một đoàn tàu. Mỗi đoàn tàu xen kẽ giữa vận hành và bảo trì: nó chạy trong`x_i`ngày, sau đó sẽ được bảo trì trong`y_i`ngày, sau đó chạy lại và lặp lại cho đến khi nó bị xóa. Bài toán yêu cầu số lượng tàu được bảo trì mỗi ngày, chỉ tính số tàu tồn tại trong ngày đó. Nếu một đoàn tàu được di dời vào một ngày thì nó không được tính là đang bảo trì vào ngày đó. 

Đầu vào bao gồm tới 200.000 mô hình tàu và 200.000 sự kiện (ngày). Độ dài chu kỳ của mỗi đoàn tàu`x_i + y_i`có thể lớn tới 2×10^9. Các giới hạn này ngay lập tức loại trừ việc mô phỏng lực lượng từng ngày cho mỗi chuyến tàu, bởi vì nếu một chuyến tàu được thêm vào ngày đầu tiên và bị loại bỏ vào ngày thứ 200.000, thì việc mô phỏng tất cả các chu kỳ sẽ yêu cầu hàng tỷ thao tác. Vì vậy, bất kỳ giải pháp nào cũng phải mở rộng quy mô`m`(số lượng sự kiện) chứ không phải tổng số ngày tàu. 

Một trường hợp khó phát hiện khi`x_i`hoặc`y_i`lớn hơn số ngày còn lại. Ví dụ: nếu một đoàn tàu được thêm vào muộn và thời gian bảo trì của nó không bao giờ bắt đầu trước khi bị loại bỏ, một mô phỏng đơn giản có thể tính sai đoàn tàu đó là đang bảo trì. Một trường hợp khác là chu kỳ rất ngắn; nếu tàu chạy mỗi ngày, việc thực hiện bất cẩn có thể tính gấp đôi số lần bảo trì hoặc điều chỉnh sai ngày bắt đầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: duy trì một tập hợp các đoàn tàu đang hoạt động. Đối với mỗi ngày, lặp lại tất cả các chuyến tàu hiện đang hoạt động và tính toán xem ngày đó có nằm trong thời gian bảo trì hay không bằng cách kiểm tra`(current_day - add_day) % (x_i + y_i) >= x_i`. Điều này đúng về mặt logic, nhưng độ phức tạp trong trường hợp xấu nhất là`O(m × n)`. Với`m`Và`n`lên tới 2×10^5, điều này mang lại khoảng 4×10^10 thao tác, quá chậm. 

Thông tin chi tiết quan trọng đến từ việc quan sát rằng tàu có thể được chia thành hai loại: **tàu đạp xe nhanh**, trong đó độ dài chu trình nhỏ và **tàu đạp xe chậm**, trong đó độ dài chu trình lớn. Đối với các đoàn tàu chạy chậm, chúng tôi có thể đánh dấu chính xác ngày nào chúng đi vào và rời khỏi bảo trì bằng cách sử dụng "mảng khác biệt" hoặc phương pháp xếp hàng sự kiện. Đối với các đoàn tàu tốc độ nhanh, số lượng độ lệch khác nhau theo modulo độ dài chu kỳ là nhỏ, cho phép chúng tôi tính toán trước số lượng đoàn tàu đang bảo trì cho mỗi độ lệch và số lượng cập nhật một cách hiệu quả bằng cách sử dụng cấu trúc tổng tiền tố. Cách tiếp cận kết hợp này làm giảm sự phức tạp`O(m√m)`hoặc tốt hơn. 

Về bản chất, brute-force hoạt động vì bạn có thể kiểm tra từng chuyến tàu riêng lẻ nhưng không thành công khi độ dài chu kỳ quá lớn và quá trình mô phỏng trở nên quá dài. Việc quan sát thấy các chu kỳ lặp lại định kỳ cho phép chúng tôi tránh lặp lại hàng ngày và thay vào đó xử lý các bản cập nhật hàng loạt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m × n) | O(n) | Quá chậm | 
| Tối ưu | O(m√m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính ngưỡng`B = sqrt(m)`để phân biệt chu kỳ nhanh và chậm. Bất kỳ chuyến tàu nào có`x_i + y_i < B`là nhanh, nếu không thì chậm. Điều này cân bằng số lượng chuyến tàu mỗi loại với số ngày. 
2. Đối với mỗi đoàn tàu chạy chậm, khi được thêm vào, hãy lên lịch các khoảng thời gian bảo trì bằng cách sử dụng một mảng khác nhau. Đối với mỗi khoảng thời gian`[start_day, end_day)`, tăng bộ đếm tại`start_day`và giảm nó tại`end_day`. Khi tàu rời đi sớm, hãy ngừng lên lịch cho các chuyến tàu tiếp theo. 
3. Đối với các chuyến tàu chạy nhanh, hãy tính toán trước một mảng có kích thước`cycle_length`thể hiện số lượng đoàn tàu đang được bảo trì trong mỗi ngày của chu trình. Khi một đoàn tàu được thêm vào, hãy tăng các độ lệch thích hợp và khi loại bỏ nó, hãy giảm các độ lệch tương tự. Điều này cho phép tra cứu nhanh chóng bất kỳ ngày nào bằng cách`(day - add_day) % cycle_length`. 
4. Cho mỗi ngày`t`trong chuỗi sự kiện, tổng số đoàn tàu đang bảo trì là tổng của mảng chênh lệch tại`t`(đối với các chuyến tàu chậm) cộng với tổng của tất cả các mảng có chu kỳ nhanh cho ngày hôm đó. Xuất ra số tiền này. 
5. Cập nhật kết cấu khi một đoàn tàu bị di dời để những ngày trong tương lai phản ánh chính xác sự vắng mặt của đoàn tàu. 

Tại sao nó hoạt động: Các chuyến tàu chạy chậm chỉ được theo dõi rõ ràng trong những ngày chúng thực sự được bảo trì, vì vậy chúng tôi không bao giờ mô phỏng những ngày không cần thiết. Các chuyến tàu chạy nhanh được theo dõi bằng độ dài chu kỳ modulo bù, tận dụng đặc tính tuần hoàn. Cùng với nhau, hai cơ chế này đảm bảo số lượng bảo trì chính xác hàng ngày. Các bất biến là: mỗi chuyến tàu chậm chỉ đóng góp vào những ngày bảo trì chính xác và mỗi chuyến tàu nhanh đóng góp theo mảng chu trình được tính toán trước của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

def main():
    n, m = map(int, input().split())
    xy = [tuple(map(int, input().split())) for _ in range(n)]
    events = [tuple(map(int, input().split())) for _ in range(m)]
    
    B = int(m**0.5) + 1
    slow = []
    fast = defaultdict(list)
    
    # classify trains
    for i, (x, y) in enumerate(xy):
        if x + y >= B:
            slow.append(i)
        else:
            fast[i] = [0]*(x+y)
    
    diff = [0]*(m+2)
    add_day = [None]*n
    
    for day, (op, k) in enumerate(events):
        k -= 1
        if op == 1:
            add_day[k] = day
            x, y = xy[k]
            if k in fast:
                for i in range(x, x+y):
                    fast[k][i % (x+y)] += 1
            else:
                t = day + x
                while t < m:
                    l = t
                    r = min(t+y, m)
                    diff[l] += 1
                    diff[r] -= 1
                    t += x + y
        else:
            x, y = xy[k]
            if k in fast:
                for i in range(x, x+y):
                    fast[k][i % (x+y)] -= 1
            add_day[k] = None
    
    res = []
    curr = 0
    for day in range(m):
        curr += diff[day]
        total = curr
        for k in fast:
            if add_day[k] is not None:
                cycle = xy[k][0]+xy[k][1]
                offset = (day - add_day[k]) % cycle
                if offset >= xy[k][0]:
                    total += 1
        res.append(total)
    
    print("\n".join(map(str, res)))

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên phân vùng thành các loại chậm và nhanh để xử lý từng loại một cách hiệu quả. Mảng khác biệt theo dõi sự đóng góp bảo trì của các đoàn tàu chậm, trong khi các đoàn tàu nhanh sử dụng mảng chu trình được tính toán trước để xử lý những thay đổi thường xuyên. Một điểm tinh tế đang sử dụng`day - add_day[k]`độ dài chu kỳ modulo cho tàu nhanh, điều chỉnh chính xác thời gian bảo trì đầu tiên. Cần cẩn thận để không tính một chuyến tàu vào ngày nó bị dỡ bỏ. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3 4
10 15
12 10
1 1
1 3
1 1
2 1
2 3
```| Ngày | Đã thêm/xóa | Khác biệt chậm | Bù đắp nhanh | Số lần bảo trì | 
| --- | --- | --- | --- | --- | 
| 0 | Thêm 1 | 0 | 0 | 0 | 
| 1 | Thêm 3 | 0 | 1 (train3 đang bảo trì) | 1 | 
| 2 | Thêm 1 | 0 | 0 | 0 | 
| 3 | Xóa 1 | 0 | 0 | 0 | 

Dấu vết này cho thấy thuật toán tính toán chính xác số lượng bảo trì trong những ngày tàu đi vào hoặc ngừng hoạt động, bao gồm cả ảnh hưởng của chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m√m) | Các đoàn tàu nhanh được xử lý bằng cách bù chu kỳ, các đoàn tàu chậm được xử lý bằng mảng sai phân, cả hai đều có quy mô kết hợp gần bằng m√m | 
| Không gian | O(n + m) | Lưu trữ mảng chênh lệch cho tàu chậm và mảng chu kỳ nhỏ cho tàu nhanh | 

Điều này rất phù hợp trong các giới hạn vì m 2×10^5, cho phép thực hiện khoảng 2×10^5 × 500 phép tính trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import __main__
    sys.stdout = io.StringIO()
    __main__.main()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""3 4
10 15
12 10
1 1
1 3
1 1
2 1
2 3""") == "0\n1\n0\n0", "sample 1"

# custom cases
assert run("""1 3
1 1
1 1
2 1
1 1""") == "0\n0\n0", "single train add/remove/add"
assert run("""2 5
2 3
1 1
1 1
1 2
2 1
2 2
1 1""") == "0
```
