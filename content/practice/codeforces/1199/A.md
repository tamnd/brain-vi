---
title: "CF 1199A - Ngày Thành Phố"
description: "Chúng ta được cung cấp một chuỗi lượng mưa hàng ngày trong mùa hè và chúng ta cần tìm ngày sớm nhất “không mưa nhiều”."
date: "2026-06-11T23:55:57+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1199
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 576 (Div. 2)"
rating: 1000
weight: 1199
solve_time_s: 85
verified: true
draft: false
---

[CF 1199A - Ngày Thành phố](https://codeforces.com/problemset/problem/1199/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi lượng mưa hàng ngày trong mùa hè và chúng ta cần tìm ngày sớm nhất “không mưa”. Một ngày được coi là không mưa nếu so với ngày trước`x`ngày và những ngày tiếp theo`y`những ngày (trong thời kỳ mùa hè), trời có lượng mưa ít hơn tất cả những ngày đó. Đầu vào bao gồm`n`ngày, với`x`ngày nhìn lại và`y`ngày nhìn về phía trước, tiếp theo là`n`các số nguyên riêng biệt biểu thị lượng mưa. Đầu ra là chỉ số của ngày đầu tiên thỏa mãn điều kiện này. 

Bởi vì`n`có thể lên tới 100.000 và người dân chỉ xem tối đa 7 ngày trước hoặc sau, mọi giải pháp so sánh ngày này với tất cả các ngày khác trong một cửa sổ lớn sẽ không hiệu quả. Một lực lượng vũ phu ngây thơ sẽ kiểm tra`x + y`các ngày lân cận cho mỗi ngày, dẫn đến tối đa 100.000 * 14 = 1.400.000 thao tác trong trường hợp xấu nhất. Kích thước này đủ nhỏ cho một CPU hiện đại nên ngay cả một giải pháp đơn giản cũng có thể hoạt động được. Sự tinh tế chính đến từ việc xử lý các ranh giới một cách hợp lý: lần đầu tiên`x`ngày, chúng tôi không thể kiểm tra`x`ngày trước đó; lần cuối cùng`y`ngày, chúng tôi không thể kiểm tra`y`ngày sau đó. Ngoài ra, vì tất cả lượng mưa đều khác nhau nên các bất đẳng thức nghiêm ngặt đều an toàn và chúng ta không cần phải xử lý các mối quan hệ. 

Một cách tiếp cận bất cẩn có thể quên giới hạn phạm vi trong`1`ĐẾN`n`. Ví dụ, nếu`x = 2`và chúng tôi đang xem xét ngày`1`, chúng ta không thể nhìn vào ngày`-1`hoặc`0`. Nếu được triển khai một cách ngây thơ, việc truy cập các chỉ mục phủ định có thể tạo ra kết quả không chính xác hoặc lỗi thời gian chạy. Một điều tinh tế nữa là chúng ta phải trả về ngày sớm nhất, vì vậy nếu nhiều ngày thỏa mãn điều kiện thì chúng ta phải chọn ngày có chỉ số nhỏ nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại mỗi ngày, kiểm tra lượng mưa cho`x`những ngày trước đó và`y`ngày tiếp theo và xác định xem ngày hiện tại có nhỏ hơn tất cả các ngày lân cận trong cửa sổ này hay không. Điều này đúng vì nó trực tiếp thực hiện định nghĩa “không mưa nhiều”, nhưng nó có thể không hiệu quả nếu`x`Và`y`đã lớn. Đây,`x`Và`y`được giới hạn bởi 7, vì vậy ngay cả việc so sánh trực tiếp cũng được chấp nhận. 

Cái nhìn sâu sắc quan trọng cho một cách tiếp cận rõ ràng hơn một chút là bởi vì`x`Và`y`rất nhỏ nên chúng tôi không cần bất kỳ cấu trúc dữ liệu nâng cao nào như cây phân đoạn hoặc deques. Chúng ta có thể đơn giản lặp lại từ ngày`1`Hôm nay`n`và mỗi ngày hãy kiểm tra các cửa sổ nhỏ bên trái và bên phải của nó. Ngay khi chúng ta tìm thấy một ngày mà tất cả những người hàng xóm đều vĩ đại hơn, chúng ta có thể trả lại ngày đó ngay lập tức. Điều này tận dụng thực tế là tiêu chí “ngày sớm nhất” cho phép quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra hàng xóm) | O(n * (x + y)) | O(n) | Đã chấp nhận | 
| Tối ưu hóa (cùng logic, thoát sớm) | O(n * (x + y)) | O(n) | Đã chấp nhận | 

Trong thực tế, cả hai đều được chấp nhận với điều kiện nhỏ`x`Và`y`. 

## Hướng dẫn thuật toán 

1. Đọc`n`,`x`, Và`y`, sau đó đọc mảng`a`của lượng mưa. 
2. Lặp lại chỉ mục từng ngày`d`từ`1`ĐẾN`n`. 
3. Cho mỗi ngày`d`, kiểm tra tất cả các ngày trước đó từ`max(1, d - x)`lên đến`d - 1`. Nếu bất kỳ nơi nào trong số này có lượng mưa nhỏ hơn hoặc bằng`a[d]`, rồi ngày`d`không thể không mưa được, nên bỏ qua ngày hôm sau. 
4. Kiểm tra tất cả các ngày tiếp theo từ`d + 1`lên đến`min(n, d + y)`. Nếu bất kỳ nơi nào trong số này có lượng mưa nhỏ hơn hoặc bằng`a[d]`, rồi ngày`d`bị loại. 
5. Nếu cả hai bước kiểm tra đều đạt, hãy in`d`và chấm dứt, vì đó là ngày không mưa sớm nhất. 

Tại sao nó hoạt động: Thuật toán so sánh rõ ràng ngày hiện tại với chính xác những ngày mà công dân quan tâm, tôn trọng các ranh giới. Vì tất cả các giá trị đều khác biệt nên so sánh nghiêm ngặt đảm bảo rằng điều kiện được đánh giá chính xác. Lặp lại tuyến tính đảm bảo ngày hợp lệ đầu tiên được trả về. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, x, y = map(int, input().split())
a = list(map(int, input().split()))

for i in range(n):
    is_valid = True
    for j in range(max(0, i - x), i):
        if a[j] <= a[i]:
            is_valid = False
            break
    if not is_valid:
        continue
    for j in range(i + 1, min(n, i + y + 1)):
        if a[j] <= a[i]:
            is_valid = False
            break
    if is_valid:
        print(i + 1)
        break
```Mã lặp lại từ ngày đầu tiên đến ngày cuối cùng. Mỗi ngày, nó kiểm tra các cửa sổ bên trái và bên phải bằng cách sử dụng`max`Và`min`để đảm bảo chúng tôi không đi quá giới hạn. Ngày đầu tiên vượt qua cả hai séc sẽ được in. các`+1`chiếm chỉ mục dựa trên 1 trong báo cáo vấn đề. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`10 2 2`, lượng mưa:`10 9 6 7 8 3 2 1 4 5`| Ngày (i) | Cửa sổ bên trái | Cửa sổ bên phải | một [tôi] | Kiểm tra kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | - | 9,6 | 10 | 10 > 9? Có, 10 > 6? Có | 
| 2 | 10 | 6,7 | 9 | 9 < 10? Có, 9 < 6? Không | 
| 3 | 10,9 | 7,8 | 6 | 6 < 10 & 6 < 9? Có, 6 < 7 & 6 < 8? Có | 

Thuật toán dừng ở ngày thứ 3 và in`3`. 

### Mẫu 2 

đầu vào:`10 2 2`, lượng mưa:`10 9 6 7 8 3 2 1 4 5`nhưng kiểm tra ngày thứ 8 

Dấu vết cho thấy ngày thứ 8 đã bỏ hàng xóm`3,2`và hàng xóm bên phải`4,5`.`1 < 3,2? Yes`,`1 < 4,5? Yes`, xác nhận đây là một ngày không mưa hợp lệ. Ngày thứ 8 sẽ chỉ được tìm thấy sau khi những ngày trước đó thất bại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * (x + y)) | Mỗi n ngày kiểm tra tới x + y hàng xóm, x, y <= 7, do đó hệ số không đổi | 
| Không gian | O(n) | Lưu trữ mảng lượng mưa | 

Được cho`n`lên tới 10^5 và x+y <= 14, tổng số thao tác ~ 1,4 triệu, chạy thoải mái dưới 1 giây. Việc sử dụng bộ nhớ là tuyến tính trong`n`, dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution code
    n, x, y = map(int, input().split())
    a = list(map(int, input().split()))
    for i in range(n):
        is_valid = True
        for j in range(max(0, i - x), i):
            if a[j] <= a[i]:
                is_valid = False
                break
        if not is_valid:
            continue
        for j in range(i + 1, min(n, i + y + 1)):
            if a[j] <= a[i]:
                is_valid = False
                break
        if is_valid:
            print(i + 1)
            break
    return output.getvalue().strip()

# Provided samples
assert run("10 2 2\n10 9 6 7 8 3 2 1 4 5\n") == "3", "sample 1"
# Minimum size input
assert run("1 0 0\n100\n") == "1", "minimum size"
# Maximum x and y
assert run("5 7 7\n5 4 3 2 1\n") == "5", "x+y > n-1 boundary"
# Edge at start
assert run("5 1 1\n1 2 3 4 5\n") == "1", "first day is earliest"
# Edge at end
assert run("5 1 1\n5 4 3 2 1\n") == "5", "last day is earliest"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 0\n100`| 1 | Đầu vào kích thước tối thiểu | 
|`5 7 7\n5 4 3 2 1`| 5 | Nhìn cửa sổ vượt quá số ngày có sẵn | 
|`5 1 1\n1 2 3 4 5`| 1 | Ngày đầu tiên là ngày đầu tiên | 
|`5 1 1\n5 4 3 2 1`| 5 | Ngày sớm nhất là ngày cuối cùng | 

## Vỏ cạnh
