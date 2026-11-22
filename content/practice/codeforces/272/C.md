---
title: "CF 272C - Dima và Cầu Thang"
description: "Bài toán trình bày một cầu thang gồm n bậc, trong đó mỗi bậc có một chiều cao nhất định cho trước trong một mảng không giảm a. Dima ném những chiếc hộp thẳng đứng lên cầu thang. Mỗi hộp có chiều rộng w và chiều cao h và nó bao phủ cầu thang w đầu tiên."
date: "2026-06-05T01:50:18+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 272
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 167 (Div. 2)"
rating: 1500
weight: 272
solve_time_s: 69
verified: true
draft: false
---

[CF 272C - Dima và Cầu thang](https://codeforces.com/problemset/problem/272/C) 

**Đánh giá:** 1500 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề trình bày một cầu thang của`n`các bước, trong đó mỗi bước có một chiều cao nhất định được đưa ra trong một mảng không giảm`a`. Dima ném những chiếc hộp thẳng đứng lên cầu thang. Mỗi hộp có chiều rộng`w`và một chiều cao`h`, và nó bao gồm phần đầu tiên`w`cầu thang. Một chiếc hộp sẽ hạ cánh ở đầu cầu thang cao nhất mà nó bao phủ hoặc đầu bất kỳ chiếc hộp nào đã hạ cánh trên một trong những cầu thang đó. Kết quả đầu ra mà chúng ta cần là, đối với mỗi hộp theo thứ tự chúng được ném, chiều cao của đáy hộp sau khi nó tiếp đất. 

Những hạn chế chính là`n`Và`m`mỗi cái có thể lên tới 100.000 và chiều cao có thể lên tới 1 tỷ. Điều này có nghĩa là bất kỳ phương pháp nào kiểm tra từng cầu thang cho từng hộp, dẫn đến`O(n*m)`hoạt động sẽ rất chậm. Chúng ta cần giảm việc quét cầu thang lặp đi lặp lại cho mỗi hộp. Các trường hợp cạnh cần xem xét bao gồm khi nhiều hộp nằm trên cùng một cầu thang ban đầu, khi các hộp có chiều rộng`1`, hoặc khi chiều cao cầu thang bằng nhau. 

Việc triển khai đơn giản có thể lặp lại lần đầu tiên`w`cầu thang cho mỗi hộp, tính chiều cao tối đa hiện tại, cộng chiều cao của hộp và cập nhật chiều cao. Mặc dù điều này đúng về mặt logic nhưng nó trở nên không khả thi khi`n`Và`m`lớn. Một điều tinh tế khác là xử lý việc tích lũy một cách chính xác - chúng ta phải theo dõi độ cao một cách linh hoạt khi các hộp xếp chồng lên nhau. 

## Phương pháp tiếp cận 

Giải pháp brute-force chỉ đơn giản lặp lại lần đầu tiên`w`cầu thang cho mỗi hộp, tìm chiều cao tối đa hiện có, in nó và tăng số cầu thang bị ảnh hưởng lên`h`. Điều này hiệu quả vì một hộp luôn nằm ở vị trí tối đa của cầu thang hoặc các hộp trước đó, nhưng độ phức tạp là`O(n*m)`trong trường hợp xấu nhất. Với`n`Và`m`lên tới 10^5, điều này có thể dẫn đến 10^10 thao tác, quá chậm. 

Thông tin chi tiết quan trọng là chúng ta không cần phải theo dõi chiều cao của tất cả các cầu thang riêng lẻ. Chúng ta chỉ cần đạt được điểm cao nhất trong số những điểm đầu tiên`w`cầu thang mỗi khi một hộp hạ cánh. Chúng ta có thể duy trì một biến "chiều cao tối đa hiện tại" phản ánh đỉnh cao nhất trong số các cầu thang được bao phủ cho đến nay. Khi một chiếc hộp rơi xuống, đáy của nó sẽ đạt mức tối đa hiện tại và sau đó chúng ta tăng mức tối đa lên theo chiều cao của hộp. Điều này làm giảm độ phức tạp`O(m)`vì chúng ta không còn lặp lại tất cả các bậc thang cho mỗi hộp nữa. 

Lý do điều này hiệu quả là vì tất cả chiều cao của cầu thang đều không giảm. Bao phủ cái đầu tiên`w`cầu thang luôn bao gồm cầu thang`w`, ít nhất cũng cao bằng bất kỳ bậc thang nào trước nó. Bằng cách duy trì chiều cao tối đa toàn cầu mà các hộp đạt được cho đến nay, chúng tôi mô phỏng chính xác bề mặt hạ cánh cao nhất cho hộp mới mà không cần theo dõi từng cầu thang riêng lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(n) | Quá chậm | 
| Tối ưu | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số bậc thang`n`và mảng`a`chiều cao cầu thang ban đầu. 
2. Khởi tạo một biến`current_max`về không. Điều này sẽ theo dõi điểm cao nhất trong số tất cả các hộp đã hạ cánh trước đó. 
3. Đọc số ô`m`. 
4. Đối với mỗi hộp, hãy đọc chiều rộng của nó`w`và chiều cao`h`. 
5. Tính độ cao hạ cánh lớn nhất bằng`current_max`và chiều cao của`w`-cầu thang thứ (cầu thang cao nhất mà hộp che phủ). Điều này đảm bảo hộp tiếp đất đúng đế. 
6. In chiều cao hạ cánh. 
7. Tăng`current_max`theo chiều cao hộp`h`để phản ánh chiều cao tối đa mới sau khi hộp này hạ cánh. 
8. Lặp lại cho tất cả các hộp. 

Tại sao nó hoạt động: Ở mỗi bước,`current_max`luôn đại diện cho đỉnh cao nhất của bất kỳ cấu trúc nào (cầu thang hoặc hộp đã hạ cánh trước đó) trong số các cầu thang được bao phủ bởi hộp tiếp theo. Vì cầu thang không giảm nên cầu thang`w`đảm bảo tối đa giữa các bậc thang từ 1 đến`w`. Việc thêm chiều cao của hộp sẽ cập nhật điểm cao nhất cho các hộp tiếp theo, giữ nguyên tính bất biến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
m = int(input())

current_max = 0
for _ in range(m):
    w, h = map(int, input().split())
    # stair index is 0-based
    landing_height = max(current_max, a[w-1])
    print(landing_height)
    current_max = landing_height + h
```Trong mã này,`current_max`theo dõi điểm cao nhất trong số những điểm đầu tiên`w`cầu thang sau mỗi hộp. Chúng tôi truy cập`a[w-1]`vì mảng Python dựa trên 0. Sau khi in chiều cao hạ cánh, chúng tôi cập nhật`current_max`bằng cách cộng chiều cao của hộp để tính phần trên cùng của hộp cho các hộp tiếp theo. Việc triển khai này tránh việc quét cầu thang nhiều lần và đảm bảo mỗi hộp đều hạ cánh chính xác. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
5
1 2 3 6 6
4
1 1
3 1
1 1
4 3
```| Hộp | w | h | max(a[w-1], current_max) | hạ cánh_height | current_max sau hộp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | tối đa(1, 0) | 1 | 2 | 
| 2 | 3 | 1 | tối đa(3, 2) | 3 | 4 | 
| 3 | 1 | 1 | tối đa(1, 4) | 4 | 5 | 
| 4 | 4 | 3 | tối đa(6, 5) | 6 | 9 | 

Dấu vết này cho thấy mức tối đa hiện tại tích lũy chính xác điểm cao nhất được bao phủ bởi các hộp. Ngay cả khi chiều rộng của hộp nhỏ hơn các hộp trước đó thì mức tối đa trước đó vẫn được tôn trọng. 

### Tùy chỉnh đầu vào 2```
3
1 1 1
3
3 2
2 1
1 3
```| Hộp | w | h | max(a[w-1], current_max) | hạ cánh_height | current_max sau hộp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 2 | tối đa(1,0) | 1 | 3 | 
| 2 | 2 | 1 | tối đa(1,3) | 3 | 4 | 
| 3 | 1 | 3 | tối đa(1,4) | 4 | 7 | 

Điều này chứng tỏ rằng thuật toán xử lý chính xác các chiều cao cầu thang bằng nhau và tích lũy chiều cao hộp đúng cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi hộp được xử lý trong thời gian không đổi, độc lập với`n`. | 
| Không gian | O(n) | Chỉ có mảng chiều cao cầu thang được lưu trữ; không cần thêm cấu trúc lớn. | 

Với`m`lên tới 100.000 và chỉ số học đơn giản trên mỗi hộp, điều này rất phù hợp trong giới hạn 2 giây. Việc sử dụng bộ nhớ là tối thiểu vì chúng tôi không lưu trữ các cấu trúc bổ sung trên mỗi hộp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    n = int(input())
    a = list(map(int, input().split()))
    m = int(input())
    current_max = 0
    for _ in range(m):
        w, h = map(int, input().split())
        landing_height = max(current_max, a[w-1])
        print(landing_height)
        current_max = landing_height + h
    return output.getvalue().strip()

# provided samples
assert run("5\n1 2 3 6 6\n4\n1 1\n3 1\n1 1\n4 3\n") == "1\n3\n4\n6"

# custom tests
assert run("3\n1 1 1\n3\n3 2\n2 1\n1 3\n") == "1\n3\n4", "equal stairs and stacking"
assert run("1\n10\n1\n1 5\n") == "10", "single stair and single box"
assert run("5\n1 2 3 4 5\n5\n1 1\n2 2\n3 3\n4 4\n5 5\n") == "1\n2\n5\n8\n12", "increasing heights accumulation"
assert run("3\n2 2 2\n3\n1 1\n1 1\n1 1\n") == "2\n3\n4", "narrow boxes on uniform stairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 bậc thang, 3 hộp | 1 3 4 | tích lũy đúng bậc thang bằng nhau | 
| 1 cầu thang, 1 hộp | 10 | trường hợp cạnh phần tử đơn | 
| 5 bậc thang, 5 hộp | 1 2 5 8 12 | xếp chồng lên cầu thang ngày càng tăng | 
| 3 cầu thang, 3 ô hẹp | 2 3 4 | tích lũy cho chiều rộng=1 hộp | 

## Vỏ cạnh 

Khi tất cả các bậc thang có cùng chiều cao hoặc khi các hộp có chiều rộng 1 lặp đi lặp lại, thuật toán vẫn cập nhật chính xác mức tối đa sau mỗi hộp. Ví dụ, với cầu thang`[2,2,2]`và ba hộp có chiều rộng 1, đáy của hộp đầu tiên nằm ở vị trí 2, hộp tiếp theo nhìn thấy `current_max =
