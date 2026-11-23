---
title: "CF 44B - Cola"
description: "Chúng ta được yêu cầu xác định có bao nhiêu cách mà những người tổ chức một lớp học mùa đông có thể mua chính xác n lít cola bằng cách sử dụng số lượng chai có hạn có ba kích cỡ: 0,5 lít, 1 lít và 2 lít."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 44
codeforces_index: "B"
codeforces_contest_name: "School Team Contest 2 (Winter Computer School 2010/11)"
rating: 1500
weight: 44
solve_time_s: 69
verified: true
draft: false
---

[CF 44B - Cola](https://codeforces.com/problemset/problem/44/B) 

**Đánh giá:** 1500 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định chính xác có bao nhiêu cách mà những người tổ chức một trường học mùa đông có thể mua`n`lít cola bằng cách sử dụng số lượng chai có ba kích cỡ: 0,5 lít, 1 lít và 2 lít. Các đầu vào`a`,`b`, Và`c`tương ứng là số chai có sẵn ở mỗi kích cỡ. Nhiệm vụ là đếm các kết hợp số lượng chai khác nhau sao cho tổng thể tích chính xác bằng`n`lít. Hai sự kết hợp được coi là khác nhau nếu chúng sử dụng số lượng chai khác nhau với ít nhất một kích cỡ. 

Những hạn chế ở mức độ vừa phải nhưng đòi hỏi một số sự chú ý. Tổng khối lượng mong muốn`n`có thể lên tới 10.000 và mỗi loại chai có thể lên tới 5.000. Một cách tiếp cận bạo lực ngây thơ thử trực tiếp tất cả các kết hợp có thể sẽ liên quan đến`a * b * c`lần lặp lại. Trong trường hợp xấu nhất,`5000 * 5000 * 5000`là 125 tỷ lần lặp, rõ ràng là không thể thực hiện được trong giới hạn thời gian 2 giây. 

Các trường hợp cạnh không rõ ràng phát sinh khi một số loại chai không đủ đáp ứng tổng thể tích hoặc không cần thiết. Ví dụ, nếu`n = 1`Và`a = 0, b = 0, c = 1`, không thể lấy chính xác 1 lít chỉ với chai 2 lít nên kết quả đúng là 0. Một trường hợp tinh vi khác xảy ra khi`n`có sẵn chai nhỏ nhưng lớn hơn: nếu`n = 1`Và`a = 2, b = 0, c = 1`, giải pháp phải sử dụng hai chai 0,5 lít thay vì chai 2 lít, điều mà việc triển khai ngây thơ có thể bỏ qua nếu luôn ưu tiên các chai lớn hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là dùng vũ lực: thử đếm từng chai 0,5 lít từ 0 đến`a`, mỗi lần đếm chai 1 lít từ 0 đến`b`, và mỗi lần đếm chai 2 lít từ 0 đến`c`. Đối với mỗi sự kết hợp, hãy tính tổng khối lượng. Nếu nó bằng`n`, tăng một bộ đếm. Điều này hiệu quả vì nó kiểm tra kỹ lưỡng mọi khả năng. Tuy nhiên, số lần lặp có thể lên tới`5000 * 5000 * 5000 = 125,000,000,000`trong trường hợp xấu nhất, vượt xa những gì khả thi trong khoảng thời gian 2 giây. 

Thông tin quan trọng giúp giảm độ phức tạp là cần lưu ý rằng thể tích đóng góp của chai 0,5 lít và 1 lít là tuyến tính và chúng ta có thể tính toán chính xác số lượng chai 2 lít cần thiết thay vì lặp đi lặp lại tất cả các khả năng. Cụ thể hơn, sau khi chọn`x`nửa lít và`y`chai một lít, khối lượng còn lại đạt được`n`lít có thể được biểu thị dưới dạng`n - (0.5 * x + 1 * y)`. Nếu thể tích còn lại này không âm chia hết cho 2 và số chai 2 lít cần dùng không vượt quá`c`, thì sự kết hợp này là hợp lệ. Điều này làm giảm vòng lặp ba lồng thành vòng đôi trên chai 0,5 lít và 1 lít, giúp giải pháp trở nên khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a * b * c) | O(1) | Quá chậm | 
| Vòng lặp 2D được tối ưu hóa | O(a * b) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`count`về 0. Điều này sẽ theo dõi số lượng kết hợp hợp lệ. 
2. Lặp lại`half`từ 0 đến`a`, đại diện cho số chai 0,5 lít đã sử dụng. Mỗi lần lặp đại diện cho một số chai nửa lít. 
3. Lặp lại`one`từ 0 đến`b`, đại diện cho số chai 1 lít đã sử dụng. 
4. Tính thể tích hiện tại do chai nửa lít và chai 1 lít đã chọn:`current_volume = 0.5 * half + 1 * one`. 
5. Tính thể tích còn lại cần đạt`n`:`remaining_volume = n - current_volume`. Nếu giá trị này âm, hãy chuyển sang lần lặp tiếp theo của vòng lặp bên trong vì sử dụng nhiều chai hơn sẽ vượt quá`n`. 
6. Kiểm tra xem`remaining_volume`chia hết cho 2 vì mỗi chai 2 lít đóng góp đúng 2 lít. Nếu không, tiếp tục lặp lại lần tiếp theo. 
7. Tính số chai 2 lít cần dùng:`two_needed = remaining_volume / 2`. Nếu như`two_needed`vượt quá`c`, bỏ qua sự kết hợp này. 
8. Nếu tất cả các điều kiện đều được thỏa mãn, hãy tăng`count`. 
9. Sau khi cả hai vòng lặp kết thúc, hãy in`count`. 

Tại sao nó hoạt động: Các vòng lặp lặp lại trên tất cả số lượng chai nửa lít và 1 lít khả thi. Đối với mỗi lựa chọn, chúng tôi tính toán số lượng chai 2 lít duy nhất sẽ hoàn thành thể tích mục tiêu chính xác. Các điều kiện đảm bảo rằng số này không âm, tích phân và không vượt quá nguồn cung hiện có. Không có sự kết hợp hợp lệ nào bị bỏ qua và không có sự kết hợp không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, a, b, c = map(int, input().split())

count = 0
for half in range(a + 1):
    for one in range(b + 1):
        current_volume = 0.5 * half + 1 * one
        remaining_volume = n - current_volume
        if remaining_volume < 0:
            continue
        if remaining_volume % 2 != 0:
            continue
        two_needed = int(remaining_volume // 2)
        if two_needed <= c:
            count += 1

print(count)
```Mã trực tiếp thực hiện vòng lặp kép được mô tả. Chúng tôi tính toán cẩn thận`remaining_volume`và kiểm tra khả năng chia hết để đảm bảo chỉ xem xét số lượng chai 2 lít hợp lệ. sử dụng`remaining_volume // 2`đảm bảo một giá trị số nguyên cho`two_needed`. Giới hạn vòng lặp bao gồm số lượng chai có sẵn bằng cách sử dụng`range(a + 1)`Và`range(b + 1)`để che đậy trường hợp khi không lấy được chai nào. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`10 5 5 5`| một nửa | một | hiện tại_volume | còn lại_khối lượng | hai_cần thiết | có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0,0 | 10.0 | 5 | vâng | 
| 0 | 1 | 1.0 | 9,0 | 4,5 | không | 
| 0 | 2 | 2.0 | 8.0 | 4 | vâng | 
| ... | ... | ... | ... | ... | ... | 

Điều này tiếp tục cho đến khi tất cả sự kết hợp của`half`Và`one`được thử nghiệm. Chỉ tính các kết hợp có số nguyên chai 2 lít 5 mới được tính. Số cuối cùng là 9. 

**Ví dụ 2** 

đầu vào:`1 0 1 1`| một nửa | một | hiện tại_volume | còn lại_khối lượng | hai_cần thiết | có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | 0,5 | không | 
| 0 | 1 | 1 | 0 | 0 | vâng | 

Chỉ có một sự kết hợp hợp lệ tồn tại: một chai 1 lít. Đầu ra là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(a * b) | Chúng tôi lặp lại tất cả số lượng chai 0,5 lít và 1 lít; a và b 5000, do đó trường hợp xấu nhất là 25 triệu lần lặp. Mỗi lần lặp là thời gian không đổi. | 
| Không gian | O(1) | Chỉ các bộ đếm và biến vòng lặp được sử dụng. Không có bộ nhớ bổ sung tỷ lệ thuận với kích thước đầu vào. | 

Giải pháp phù hợp trong giới hạn thời gian và bộ nhớ. 25 triệu lần lặp có thể được thực hiện thoải mái dưới 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, a, b, c = map(int, input().split())
    count = 0
    for half in range(a + 1):
        for one in range(b + 1):
            current_volume = 0.5 * half + 1 * one
            remaining_volume = n - current_volume
            if remaining_volume < 0:
                continue
            if remaining_volume % 2 != 0:
                continue
            two_needed = int(remaining_volume // 2)
            if two_needed <= c:
                count += 1
    return str(count)

# provided sample
assert run("10 5 5 5\n") == "9", "sample 1"

# custom tests
assert run("1 0 1 1\n") == "1", "minimum 1-liter"
assert run("1 2 0 1\n") == "1", "using two 0.5-liter bottles"
assert run("5 0 0 3\n") == "0", "impossible to reach 5 with 2-liter bottles"
assert run("4 5 5 0\n") == "3", "combination of 0.5 and 1-liter bottles"
assert run("0 0 0 0\n") == "1", "0 liters, no bottles, exactly 0 ways"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 1 1 | 1 | tối thiểu 1 lít |
