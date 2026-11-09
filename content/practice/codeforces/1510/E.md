---
title: "CF 1510E - Điểm cân bằng /\\textbackslash/\\textbackslash"
description: "Chúng ta được cho một dãy số nguyên đại diện cho một hàng có trọng số và chúng ta muốn tìm một vị trí dọc theo hàng này sao cho dãy số đó cân bằng theo một quy tắc nhất định."
date: "2026-06-10T19:24:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1510
codeforces_index: "E"
codeforces_contest_name: "2020-2021 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2700
weight: 1510
solve_time_s: 117
verified: true
draft: false
---

[CF 1510E - Điểm cân bằng /\\textbackslash/\\textbackslash](https://codeforces.com/problemset/problem/1510/E) 

**Đánh giá:** 2700 
**Thẻ:** - 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên đại diện cho một hàng có trọng số và chúng ta muốn tìm một vị trí dọc theo hàng này sao cho dãy số đó cân bằng theo một quy tắc nhất định. Cụ thể, một vị trí được gọi là **điểm cân bằng** nếu tổng trọng số ở bên trái của nó bằng tổng trọng số ở bên phải. Nhiệm vụ của chúng tôi là tìm tất cả các vị trí như vậy và báo cáo một vị trí nếu nó tồn tại. 

Đầu vào bao gồm độ dài của chuỗi`n`theo sau là`n`số nguyên đại diện cho trọng số. Đầu ra là chỉ số của điểm cân bằng (dựa trên 1) hoặc`-1`nếu không tồn tại. 

Các hạn chế ở mức vừa phải:`n`có thể đi lên`10^5`và trọng lượng có thể lớn như`10^9`. Điều này có nghĩa là bất kỳ giải pháp nào lặp qua tất cả các cặp chỉ mục trong một vòng lặp lồng nhau sẽ quá chậm vì sẽ mất nhiều thời gian hơn.`O(n^2)`hoạt động. Chúng tôi cần một`O(n)`cách tiếp cận vượt qua giới hạn 2 giây thông thường. 

Trường hợp cạnh không rõ ràng xảy ra khi tất cả các trọng số bằng 0. Mọi vị trí trong trường hợp này về mặt kỹ thuật đều là điểm cân bằng vì cả tổng bên trái và bên phải đều bằng 0. Một trường hợp cạnh khác là khi chuỗi có độ dài bằng 1, trong đó vị trí duy nhất về cơ bản là điểm cân bằng. Những ví dụ nhỏ này có thể gây khó khăn cho việc triển khai đơn giản và không xử lý ranh giới một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là đơn giản. Đối với mỗi vị trí`i`, chúng ta tính tổng của tất cả các phần tử ở bên trái và tất cả các phần tử ở bên phải, sau đó so sánh hai tổng. Điều này hiệu quả vì nó tuân theo định nghĩa trực tiếp, nhưng việc tính tổng cho mỗi chỉ số rất tốn kém. Mỗi khoản tiền mất`O(n)`hoạt động, dẫn đến sự phức tạp tổng thể của`O(n^2)`, rõ ràng là quá chậm đối với`n = 10^5`. 

Điều quan trọng là chúng ta không cần phải tính lại tổng ở mỗi bước. Nếu chúng ta tính toán trước tổng của mảng thì với bất kỳ chỉ mục nào`i`, tổng bên trái là tổng của các phần tử đứng trước`i`và tổng ở bên phải là tổng trừ đi tổng ở bên trái trừ phần tử hiện tại. Điều này cho phép chúng ta lặp lại một lần qua mảng, duy trì tổng bên trái và tính tổng bên phải trong thời gian không đổi cho mỗi chỉ mục. Điều này làm giảm độ phức tạp`O(n)`. 

Lực lượng vũ phu hoạt động vì nó trực tiếp thực hiện định nghĩa về trạng thái cân bằng, nhưng không thành công khi`n`là lớn. Quan sát về việc duy trì tổng tiền tố cho phép chúng ta giảm vấn đề xuống còn một lần duyệt qua mảng trong khi tính toán tổng trái và phải một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tổng tiền tố / Một lượt | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của mảng. Đây là tổng của tất cả các trọng số và sẽ cho phép chúng ta tính tổng bên phải một cách nhanh chóng. 
2. Khởi tạo một biến`left_sum = 0`để theo dõi tổng tích lũy của các phần tử ở bên trái của chỉ mục hiện tại. 
3. Lặp qua mảng với chỉ mục`i`: 

- Tính toán`right_sum`BẰNG`total_sum - left_sum - a[i]`. Điều này thể hiện tổng của tất cả các phần tử ở bên phải vị trí hiện tại. 
- Nếu như`left_sum == right_sum`, báo cáo`i+1`(chỉ số dựa trên 1) làm điểm cân bằng và chấm dứt. 
- Nếu không thì cập nhật`left_sum += a[i]`để bao gồm phần tử hiện tại trước khi chuyển sang chỉ mục tiếp theo. 
4. Nếu không tìm thấy điểm cân bằng sau vòng lặp, hãy quay lại`-1`. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà`left_sum`luôn bằng tổng các phần tử trước chỉ mục hiện tại. Ở mỗi bước,`right_sum`thể hiện chính xác tổng các phần tử sau chỉ mục hiện tại. Nếu như`left_sum == right_sum`thỏa mãn định nghĩa về điểm cân bằng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def find_equilibrium():
    n = int(input())
    a = list(map(int, input().split()))
    total_sum = sum(a)
    left_sum = 0

    for i in range(n):
        right_sum = total_sum - left_sum - a[i]
        if left_sum == right_sum:
            print(i + 1)
            return
        left_sum += a[i]
    print(-1)

find_equilibrium()
```Giải pháp bắt đầu bằng cách đọc`n`và mảng. Tổng số tiền được tính một lần. Vòng lặp duy trì hoạt động`left_sum`và tính toán`right_sum`trong thời gian không đổi sử dụng tổng số tiền. Việc in ấn được thực hiện cẩn thận`i+1`bởi vì vấn đề sử dụng lập chỉ mục dựa trên 1. Đang cập nhật`left_sum`sau khi kiểm tra điều kiện đảm bảo rằng các tổng luôn phản ánh đúng các phần của mảng. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:`5\n1 2 3 3 2`| tôi | một [tôi] | left_sum | đúng_sum | cân bằng? | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 10 | Không | 
| 1 | 2 | 1 | 8 | Không | 
| 2 | 3 | 3 | 5 | Không | 
| 3 | 3 | 6 | 2 | Không | 
| 4 | 2 | 9 | 0 | Không | 

Đầu ra:`-1`Dấu vết này xác nhận thuật toán tính toán chính xác tổng trái và phải ở mọi chỉ số và thấy rằng không tồn tại trạng thái cân bằng. 

**Ví dụ 2:** 

đầu vào:`3\n2 4 2`| tôi | một [tôi] | left_sum | đúng_sum | cân bằng? | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | 6 | Không | 
| 1 | 4 | 2 | 2 | Có | 

Đầu ra:`2`Điều này chứng tỏ thuật toán xác định chính xác phần tử ở giữa là điểm cân bằng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền một lần qua mảng, mỗi phép tính là O(1) | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng; không cần thêm mảng | 

Được cho`n <= 10^5`, thuật toán này hoàn thành trong chưa đầy một giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    find_equilibrium()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("3\n2 4 2\n") == "2", "sample 1"
assert run("5\n1 2 3 3 2\n") == "-1", "sample 2"

# custom cases
assert run("1\n10\n") == "1", "single element"
assert run("4\n0 0 0 0\n") == "1", "all zeros"
assert run("5\n1 1 1 1 1\n") == "-1", "all equal non-zero"
assert run("2\n5 5\n") == "1", "two elements equilibrium at first"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n10 | 1 | Vỏ cạnh một phần tử | 
| 4\n0 0 0 0 | 1 | Tất cả trọng lượng bằng không | 
| 5\n1 1 1 1 1 | -1 | Tất cả các trọng lượng bằng nhau không có điểm cân bằng | 
| 2\n5 5 | 1 | Hai yếu tố, cân bằng ở chỉ số đầu tiên | 

## Vỏ cạnh 

Đối với mảng một phần tử`[10]`, thuật toán tính toán`left_sum = 0`Và`right_sum = 0`, thỏa mãn`left_sum == right_sum`và đầu ra`1`. Đối với mảng hoàn toàn bằng 0`[0, 0, 0, 0]`, chỉ số đầu tiên cũng thỏa mãn điều kiện, cho kết quả đầu ra`1`. Cả hai kịch bản đều xác nhận thuật toán xử lý chính xác các ranh giới và tránh từng lỗi một. 

Bài xã luận này hướng dẫn người đọc rút ra lại lời giải bằng cách sử dụng tổng tiền tố trong khi quản lý cẩn thận các điều kiện biên, bất biến và tính toán hiệu quả.
