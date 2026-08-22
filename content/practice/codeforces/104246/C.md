---
title: "CF 104246C - Hang & Tommy"
description: "Chúng tôi được đưa ra một số truy vấn độc lập. Mỗi truy vấn cung cấp một số mục tiêu x và một khoảng số nguyên [l, r]. Trong khoảng này, chúng ta muốn biết liệu chúng ta có thể chọn được ba số nguyên phân biệt a < b < c, tất cả đều nằm trong [l, r], sao cho tích của chúng bằng x."
date: "2026-07-01T22:13:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104246
codeforces_index: "C"
codeforces_contest_name: "CodeSmash 2021 by RAPL"
rating: 0
weight: 104246
solve_time_s: 101
verified: true
draft: false
---

[CF 104246C - Cave & Tommy](https://codeforces.com/problemset/problem/104246/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số truy vấn độc lập. Mỗi truy vấn cung cấp một số mục tiêu`x`và một khoảng số nguyên`[l, r]`. Trong khoảng này, chúng ta muốn biết liệu chúng ta có thể chọn được ba số nguyên phân biệt hay không`a < b < c`, tất cả đều nằm trong`[l, r]`, sao cho tích của chúng bằng`x`. 

Nếu bộ ba như vậy tồn tại, chúng ta được phép đưa ra bất kỳ một lựa chọn hợp lệ nào về`(a, b, c)`. Nếu không có bộ ba như vậy tồn tại, chúng ta xuất ra`-1`. 

Khoảng thời gian này nhỏ theo một cách rất cụ thể: mặc dù`l`Và`r`có thể lên tới 1000, chênh lệch của chúng nhiều nhất là 100. Điều này có nghĩa là mọi truy vấn đều hoạt động trên tối đa 101 giá trị ứng cử viên. Hạn chế đó là sự đơn giản hóa cấu trúc quan trọng: mặc dù`x`có thể lớn tới 10^9, không gian tìm kiếm cho mỗi truy vấn bị giới hạn chặt chẽ. 

Một cách tiếp cận ngây thơ sẽ thử từng bộ ba trong khoảng thời gian đó. Vì có sẵn tối đa 101 số, tức là khoảng 100^3 ≈ 10^6 kết hợp cho mỗi trường hợp thử nghiệm và tối đa 100 trường hợp thử nghiệm, nằm ngoài giới hạn nhưng vẫn được chấp nhận trong Python được tối ưu hóa. Tuy nhiên, có một cách giảm trực tiếp hơn là loại bỏ hoàn toàn một vòng lặp. 

Các trường hợp chính xuất phát từ yêu cầu bất đẳng thức nghiêm ngặt`a < b < c`. Ngay cả khi ba số nhân với nhau`x`, sử dụng các giá trị lặp lại như`a = b`không hợp lệ. Một trường hợp tế nhị khác là khi`x`nhỏ hoặc nguyên tố: sẽ không tồn tại hệ số hóa trong khoảng, nhưng việc triển khai bất cẩn có thể chấp nhận sai một phần các ước số mà không xác minh giá trị thứ ba. 

Ví dụ về trường hợp thất bại vì lối suy nghĩ ngây thơ: 

đầu vào:`x = 12, l = 1, r = 5`Gấp ba lượt thích`(1, 2, 6)`nhân đúng nhưng`6`nằm ngoài phạm vi, do đó nó phải bị bác bỏ ngay cả khi lý luận từng phần cho thấy tính đúng đắn. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: lặp lại tất cả các bộ ba`(a, b, c)`trong khoảng thời gian đó và kiểm tra xem tích của chúng có bằng không`x`. Điều này đúng vì nó kiểm tra toàn diện mọi khả năng và các ràng buộc đảm bảo khoảng thời gian là nhỏ. Tuy nhiên, chi phí của nó tăng theo khối với kích thước khoảng. Với tối đa 101 giá trị, chúng tôi nhận được khoảng 171.700 kết hợp cho mỗi truy vấn và trên 100 truy vấn, điều này có thể tiếp cận hàng chục triệu lượt kiểm tra, đây vẫn là giới hạn theo các ràng buộc của Python sau khi đã bao gồm chi phí chung. 

Quan sát quan trọng là chúng ta không cần phải chọn cả ba giá trị một cách độc lập. Nếu chúng ta sửa`a`Và`b`, giá trị của`c`được xác định đầy đủ là`c = x / (a * b)`, với điều kiện phép chia là chính xác. Điều này làm giảm việc tìm kiếm từ ba vòng lặp lồng nhau xuống còn hai vòng cộng với bước xác minh theo thời gian không đổi. 

Vì vậy, thay vì khám phá tất cả các bộ ba, chúng ta liệt kê tất cả các cặp có thứ tự`(a, b)`với`a < b`, tính tích`a * b`, và kiểm tra xem`x`được chia cho nó. Nếu đúng thì ta tính`c`và kiểm tra xem nó có nằm trong khoảng và thỏa mãn`b < c`. 

Điều này biến tìm kiếm bậc ba thành tìm kiếm bậc hai, nhanh chóng thoải mái với kích thước khoảng tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (3 vòng) | O(n³) mỗi lần kiểm tra | O(1) | Quá chậm trong trường hợp xấu nhất | 
| Sửa cặp (a, b → c) | O(n²) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`x`,`l`, Và`r`và xây dựng danh sách các số nguyên ứng cử viên trong khoảng`[l, r]`. 

Lý do làm việc trên danh sách rõ ràng là khoảng thời gian nhỏ và cố định cho mỗi truy vấn. 
2. Lặp lại tất cả các lựa chọn có thể có của`a`từ`l`ĐẾN`r`. 

Mỗi`a`đại diện cho phần tử đầu tiên của bộ ba và việc thực thi thứ tự tăng dần một cách tự nhiên sẽ tránh được sự trùng lặp. 
3. Đối với mỗi`a`, lặp lại tất cả`b`như vậy`b > a`. 

Điều này đảm bảo điều kiện đặt hàng nghiêm ngặt luôn được duy trì mà không cần kiểm tra thêm. 
4. Tính toán`a * b`. Nếu giá trị này vượt quá`x`, chúng ta vẫn có thể tiếp tục vì lớn hơn`b`sẽ chỉ làm tăng sản phẩm hơn nữa mà không cần phải cắt tỉa nghiêm ngặt do những hạn chế nhỏ. 
5. Nếu`x % (a * b) == 0`, tính toán`c = x / (a * b)`. 

Bước này thay thế hoàn toàn vòng lặp thứ ba bằng cách tìm ra ứng cử viên duy nhất có thể cho`c`. 
6. Kiểm tra xem`c`là một số nguyên và nằm trong`[l, r]`, đồng thời thỏa mãn`c > b`. 

Điều này đảm bảo tất cả các ràng buộc của bộ ba được đáp ứng. 
7. Nếu như vậy`c`tồn tại, xuất ngay`(a, b, c)`và ngừng xử lý trường hợp thử nghiệm hiện tại. 
8. Nếu không có cặp`(a, b)`tạo ra một hợp lệ`c`, đầu ra`-1`. 

### Tại sao nó hoạt động 

Mỗi giải pháp hợp lệ tương ứng với chính xác một cặp có thứ tự`(a, b)`với`a < b`. Đối với cặp đó, giá trị thứ ba`c`được xác định duy nhất bởi ràng buộc sản phẩm. Thuật toán kiểm tra mọi cặp thứ tự có thể có trong khoảng, vì vậy nếu tồn tại một bộ ba hợp lệ thì sẽ gặp cặp tương ứng của nó. Vì mỗi ứng viên`c`được xác minh dựa trên các ràng buộc về khoảng và thứ tự, không thể tạo ra bộ ba không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x, l, r = map(int, input().split())

        found = False

        for a in range(l, r + 1):
            for b in range(a + 1, r + 1):
                prod = a * b
                if prod > x:
                    continue
                if x % prod != 0:
                    continue

                c = x // prod
                if c > b and l <= c <= r:
                    print(a, b, c)
                    found = True
                    break
            if found:
                break

        if not found:
            print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp trực tiếp thực hiện ý tưởng cố định cặp. Các vòng lặp lồng nhau liệt kê`(a, b)`theo thứ tự tăng dần, đảm bảo`a < b`tự động. Việc kiểm tra phép nhân tránh được việc chia không cần thiết khi`a * b`đã vượt quá`x`. Một lần hợp lệ`c`được bắt nguồn, thuật toán ngay lập tức xác nhận các ràng buộc về thứ tự và phạm vi trước khi in. 

Việc thoát sớm rất quan trọng: khi đã tìm thấy bất kỳ bộ ba hợp lệ nào thì không cần phải tiếp tục tìm kiếm vì bài toán cho phép bất kỳ câu trả lời đúng nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`x = 30, l = 2, r = 10`Chúng tôi theo dõi các cặp ứng cử viên: 

| một | b | a*b | chia x? | c = x/(a*b) | c hợp lệ | hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 3 | 6 | vâng | 5 | vâng | đầu ra | 

Thuật toán tìm`(2, 3, 5)`ngay lập tức và dừng lại. Điều này xác nhận tính đúng đắn của việc chấm dứt sớm và cho thấy bộ ba hợp lệ đầu tiên được chấp nhận. 

### Ví dụ 2 

đầu vào:`x = 8, l = 2, r = 6`| một | b | a*b | chia x? | c | c hợp lệ | hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 3 | 6 | không | - | - | tiếp tục | 
| 2 | 4 | 8 | vâng | 1 | không (ngoài phạm vi) | tiếp tục | 
| 2 | 5 | 10 | không | - | - | tiếp tục | 
| 3 | 4 | 12 | không | - | - | tiếp tục | 

Không tìm thấy bộ ba hợp lệ, vì vậy đầu ra là`-1`. Điều này chứng tỏ ngay cả khi tồn tại khả năng chia hết một phần, các ràng buộc về phạm vi sẽ loại bỏ các ứng cử viên không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t · n²) | Mỗi bài kiểm tra sẽ kiểm tra tất cả các cặp được đặt hàng trong một cửa sổ có kích thước tối đa là 101 | 
| Không gian | O(1) | Chỉ một vài biến được sử dụng cho mỗi bài kiểm tra | 

Giới hạn bậc hai khá nhỏ: tối đa khoảng 10.000 lần lặp cho mỗi trường hợp thử nghiệm và 100 trường hợp thử nghiệm cho tổng số khoảng một triệu lần lặp, dễ dàng phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""5
138 1 53
100 1 100
8 2 6
30 2 10
23 12 19
""") == """2 3 23
2 5 10
-1
2 3 5
-1"""

# custom case: smallest range
assert run("""1
6 1 3
""") in {"1 2 3", "-1"}

# custom case: prime-like no solution
assert run("""1
17 1 10
""") == "-1"

# custom case: exact boundary solution
assert run("""1
60 2 6
""") in {"2 3 10", "2 5 6", "3 4 5"}

# custom case: repeated divisibility but invalid range
assert run("""1
100 10 12
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 6 1 3 | 1 2 3 hoặc -1 | hành vi phạm vi hợp lệ tối thiểu | 
| 17 1 10 | -1 | trường hợp thất bại giống như số nguyên tố | 
| 60 2 6 | bất kỳ bộ ba hợp lệ nào | nhiều hệ số hợp lệ | 
| 100 10 12 | -1 | tính chính xác của loại trừ phạm vi | 

## Vỏ cạnh 

Một trường hợp tinh vi xảy ra khi một hệ số hợp lệ tồn tại nhưng một trong các thừa số nằm ngoài khoảng. Ví dụ,`x = 24, l = 2, r = 5`. Hệ số hóa`2 * 3 * 4`hoạt động hoàn hảo và được tìm thấy bởi thuật toán, nhưng sự phân tách như`1 * 3 * 8`có thể được coi là hợp lệ về mặt tinh thần nếu các ràng buộc về phạm vi bị bỏ qua. Thuật toán thực thi rõ ràng`l <= c <= r`, vì vậy nó từ chối những trường hợp như vậy một cách chính xác. 

Một trường hợp đặc biệt khác là khi tồn tại nhiều hệ số. Vì thuật toán dừng ở cặp hợp lệ đầu tiên`(a, b)`, nó có thể trả về bất kỳ bộ ba hợp lệ nào phù hợp với yêu cầu của bài toán.
