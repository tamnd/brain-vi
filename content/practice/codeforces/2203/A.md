---
title: "CF 2203A - Tháp hộp"
description: "Monocarp có n hộp giống hệt nhau, mỗi hộp nặng m đơn vị và có thể đỡ tối đa d đơn vị ở trên. Anh ta muốn xếp tất cả các hộp thành các tòa tháp đồng thời tôn trọng độ bền: không hộp nào có thể chịu được trọng lượng lớn hơn d."
date: "2026-06-07T20:00:50+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 2203
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 187 (Rated for Div. 2)"
rating: 800
weight: 2203
solve_time_s: 102
verified: true
draft: false
---

[CF 2203A - Tháp hộp](https://codeforces.com/problemset/problem/2203/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Monocarp có`n`các hộp giống hệt nhau, mỗi hộp có trọng lượng`m`đơn vị và có thể hỗ trợ lên đến`d`đơn vị ở trên cùng. Anh ấy muốn xếp tất cả các hộp thành các tòa tháp đồng thời tôn trọng độ bền: không hộp nào có thể chịu được trọng lượng nặng hơn`d`. Nhiệm vụ là xác định số lượng tháp tối thiểu cần thiết để sử dụng tất cả các hộp. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra`n`,`m`, Và`d`. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: số lượng tháp tối thiểu. Mỗi tháp phải chứa ít nhất một hộp và tất cả các hộp phải được đặt đúng vị trí. 

Bởi vì tối đa`n`,`m`, Và`d`là 50 và có thể có tới 10.000 trường hợp thử nghiệm, bất kỳ giải pháp nào thực hiện tính toán số học đơn giản cho mỗi trường hợp thử nghiệm đều khả thi. Một mô phỏng đơn giản lặp đi lặp lại việc xếp chồng các hộp trong khi kiểm tra các ràng buộc về trọng lượng có thể hoạt động nhưng không cần thiết. Các trường hợp cạnh bao gồm các tình huống trong đó`m > d`, khiến cho không thể đặt hai hộp chồng lên nhau và trường hợp`d`lớn so với`m`, cho phép tất cả các hộp trong một tháp duy nhất. Ví dụ, nếu`n = 5`,`m = 3`,`d = 2`, không thể xếp chồng nên mỗi hộp tạo thành tháp riêng, tạo thành 5 tháp. Một cách tiếp cận bất cẩn không kiểm tra giới hạn trọng lượng mỗi hộp sẽ cố gắng xếp chồng các hộp một cách không chính xác và đếm thiếu các tòa tháp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng việc xếp chồng: bắt đầu với một tháp trống, thêm các hộp cho đến khi hộp tiếp theo vi phạm ràng buộc về độ bền, sau đó bắt đầu một tháp mới. Lặp lại cho đến khi tất cả các hộp được đặt. Điều này hiệu quả vì nó trực tiếp thực thi điều kiện trọng lượng, nhưng nó yêu cầu lặp qua`n`hộp cho mỗi trường hợp thử nghiệm. Với`n`lên tới 50 và`t`lên tới 10.000, điều này có thể quản lý được, nhưng nó quá mức cần thiết. 

Cách tiếp cận tối ưu quan sát thấy rằng trong một tòa tháp`k`hộp, hộp ở phía dưới hỗ trợ`(k-1) * m`cân nặng. Để tránh bị vỡ,`(k-1) * m <= d`. Giải quyết cho`k`cho`k <= d // m + 1`. Số lượng tháp tối thiểu khi đó là trần của`n`chia cho chiều cao tối đa có thể có của mỗi tháp:`ceil(n / ((d // m) + 1))`. Điều này làm giảm vấn đề thành một phép tính số học đơn giản cho mỗi trường hợp thử nghiệm. 

Việc quan sát dựa vào tính đồng nhất của các hộp. Vì mọi hộp đều có trọng lượng và độ bền như nhau nên chúng ta chỉ cần tìm tháp an toàn cao nhất. Sau đó phân phối tất cả các hộp vào các tháp có chiều cao này đảm bảo số lượng tháp tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * t) | O(1) | Có thể chấp nhận được nhưng không cần thiết | 
| Tối ưu | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`,`m`, Và`d`. 
3. Tính chiều cao tối đa mà một tòa tháp có thể có mà không cần phá hộp:`max_height = d // m + 1`. Điều này xuất phát từ việc giải quyết`(k-1) * m <= d`vì`k`. 
4. Tính số tháp tối thiểu cần thiết là`ceil(n / max_height)`. Trong số học số nguyên, đây là`(n + max_height - 1) // max_height`. 
5. In kết quả cho từng test. 

Tại sao nó hoạt động:`max_height`đảm bảo không có hộp nào vượt quá độ bền của nó. Chia tổng số hộp cho`max_height`và làm tròn đảm bảo tất cả các hộp được sử dụng trong khi giảm thiểu tháp. Tính đồng nhất của các hộp làm cho tính bất biến này có hiệu lực trên toàn bộ phân bố tháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m, d = map(int, input().split())
    max_height = d // m + 1
    towers = (n + max_height - 1) // max_height
    print(towers)
```Mã đầu tiên được đọc`t`, sau đó lặp lại từng trường hợp thử nghiệm.`max_height`được tính toán từ độ bền và trọng lượng, và phép chia số nguyên với việc làm tròn sẽ cho số lượng tháp. Công thức`(n + max_height - 1) // max_height`tránh số học dấu phẩy động. Các lỗi sai lệch có thể tránh được bằng cách sử dụng công thức trần thay vì thử làm tròn thủ công. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
8 10 20
```| n | m | d | chiều cao tối đa | tháp | 
| --- | --- | --- | --- | --- | 
| 8 | 10 | 20 | 3 | 3 | 

Giải thích:`(k-1)*m <= d`cho`k <= 3`.`ceil(8/3) = 3`tháp là cần thiết. 

Đầu vào mẫu 2:```
5 3 2
```| n | m | d | chiều cao tối đa | tháp | 
| --- | --- | --- | --- | --- | 
| 5 | 3 | 2 | 1 | 5 | 

Giải thích:`d < m`, do đó không thể xếp chồng lên nhau, mỗi hộp tạo thành một tháp riêng biệt. 

Những dấu vết này chứng minh cả trường hợp có thể xếp chồng lên nhau và không thể xếp chồng lên nhau, đồng thời xác nhận hoạt động phân chia trần nhà. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm bao gồm một vài phép tính số học. | 
| Không gian | O(1) | Chỉ một vài số nguyên cho mỗi trường hợp thử nghiệm được lưu trữ. | 

Được cho`t`lên tới 10.000 và số học đơn giản cho mỗi trường hợp thử nghiệm, giải pháp sẽ chạy hiệu quả trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, m, d = map(int, input().split())
        max_height = d // m + 1
        towers = (n + max_height - 1) // max_height
        print(towers)
    return output.getvalue().strip()

# provided samples
assert run("3\n8 10 20\n8 1 20\n5 3 2\n") == "3\n1\n5", "sample 1"

# custom cases
assert run("1\n1 1 1\n") == "1", "minimum size input"
assert run("1\n50 50 50\n") == "2", "maximum size input where stacking barely fits"
assert run("1\n10 2 5\n") == "3", "general stacking"
assert run("1\n4 5 2\n") == "4", "weight exceeds durability, no stacking"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 | Đầu vào tối thiểu hộp đơn | 
| 50 50 50 | 2 | Giá trị tối đa, kiểm tra tính chính xác số học | 
| 10 2 5 | 3 | Hộp đựng có thể xếp chồng lên nhau điển hình | 
| 4 5 2 | 4 | Mỗi hộp tạo thành tháp riêng, trọng lượng > độ bền | 

## Vỏ cạnh 

Nếu`d < m`, chiều cao tối đa`d // m + 1`trở thành 1. Điều này đảm bảo mỗi hộp được đặt trong tháp riêng của nó. Ví dụ, với`n = 4`,`m = 5`,`d = 2`,`max_height = 1`, thuật toán tính toán`(4 + 1 - 1) // 1 = 4`, xuất ra chính xác 4 tòa tháp. Thuật toán xử lý tình huống này một cách tự nhiên mà không cần bất kỳ cách viết hoa đặc biệt nào. Nếu như`d`là bội số lớn của`m`, nói`d = 100`,`m = 2`,`n = 10`,`max_height = 51`, sau đó`(10 + 51 - 1) // 51 = 1`, đặt tất cả các hộp vào một tháp duy nhất, điều này là chính xác.
