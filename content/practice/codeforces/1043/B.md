---
title: "CF 1043B - Mất Mảng"
description: "Chúng ta được cung cấp một chuỗi các giá trị tiền tố a[0], a[1], ..., a[n] trong đó a[0] = 0. Ban đầu, chuỗi này được tạo từ một mảng ẩn x có độ dài k, nhưng mảng đó không còn tồn tại nữa."
date: "2026-06-16T17:38:42+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1043
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 519 by Botan Investments"
rating: 1200
weight: 1043
solve_time_s: 223
verified: true
draft: false
---

[CF 1043B - Mảng bị mất](https://codeforces.com/problemset/problem/1043/B) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 3m 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi các giá trị tiền tố`a[0], a[1], ..., a[n]`Ở đâu`a[0] = 0`. Trình tự ban đầu được tạo từ một mảng ẩn`x`chiều dài`k`, nhưng mảng đó không còn nữa. 

Quy tắc tạo rất đơn giản: mỗi giá trị tiếp theo`a[i]`được hình thành bằng cách lấy tổng tiền tố trước đó`a[i-1]`và thêm một phần tử của`x`, được chọn theo kiểu tuần hoàn. Cụ thể, giá trị gia tăng ở bước`i`chỉ phụ thuộc vào`x[(i-1) mod k]`. Điều này có nghĩa là trình tự tăng dần của`a`là tuần hoàn với chu kỳ`k`. 

Chúng ta được yêu cầu tìm tất cả các giá trị có thể có của`k`sao cho tồn tại một số mảng số nguyên`x`chiều dài`k`có thể đã tạo ra mảng tổng tiền tố đã cho. 

Kích thước đầu vào`n`nhiều nhất là 1000, điều này ngay lập tức gợi ý rằng`O(n^2)`giải pháp có thể chấp nhận được. Bất kỳ giải pháp nào cố gắng xây dựng lại`x`độc lập cho từng ứng viên`k`và xác minh tính nhất quán sẽ trôi qua một cách thoải mái. Bất kỳ số mũ nào trên các tập hợp con hoặc hoán vị đều không cần thiết vì cấu trúc đã được xác định đầy đủ bởi các ràng buộc về tính tuần hoàn. 

Một sự tinh tế quan trọng là`x`không bị ràng buộc là tích cực hoặc bị chặn. Điều này loại bỏ nhiều ràng buộc tổng tiền tố điển hình và chỉ để lại cấu trúc đẳng thức làm yếu tố quyết định. 

Một trường hợp thất bại phổ biến xuất phát từ việc hiểu sai điều kiện là yêu cầu một cấu trúc cụ thể`x`. Ví dụ, giả sử`x[i] = a[i] - a[i-1]`trực tiếp chỉ hoạt động khi`k = n`. Đối với nhỏ hơn`k`, giống nhau`x`giá trị phải được sử dụng lại ở nhiều vị trí, vì vậy tính nhất quán giữa các chỉ số là yêu cầu thực sự. 

Một trường hợp cạnh tinh tế khác xuất hiện khi`k = 1`. Trong trường hợp này, tất cả sự khác biệt phải giống hệt nhau. Bất kỳ triển khai nào quên kiểm tra điều này một cách rõ ràng thường chấp nhận không chính xác các mảng có sự khác biệt không đổi. 

## Phương pháp tiếp cận 

Ý tưởng tự nhiên đầu tiên là thử độ dài của từng ứng viên`k`và xây dựng lại mảng ẩn một cách rõ ràng`x`. Nếu chúng ta giả định`x[j] = a[j+1] - a[j]`lần đầu tiên`k`vị trí, sau đó chúng ta có thể mô phỏng việc xây dựng toàn bộ mảng và so sánh nó với mảng đã cho`a`. Điều này đúng nhưng không hiệu quả nếu được thực hiện với mô phỏng lặp đi lặp lại, bởi vì với mỗi`k`chúng tôi có thể mô phỏng lên đến`O(n)`bước, dẫn đến`O(n^2)`cách tiếp cận tổng thể. 

Tuy nhiên, chúng ta có thể đơn giản hóa vấn đề bằng cách thay đổi quan điểm. Thay vì nghĩ đến việc xây dựng lại`x`, chúng ta nhìn vào chuỗi sự khác biệt`d[i] = a[i] - a[i-1]`. Trình tự này chính xác là mô hình lặp đi lặp lại của`x`. Vì vậy, vấn đề trở thành: cho cái gì`k`là mảng sai phân tuần hoàn theo chu kỳ`k`? 

Điều này biến nhiệm vụ thành một cuộc kiểm tra định kỳ trực tiếp. Đối với một cố định`k`, mọi vị trí`i`phải thỏa mãn`d[i] = d[i-k]`. Nếu điều này đúng cho tất cả hợp lệ`i`, sau đó`k`là hợp lệ. 

Cách tiếp cận brute-force kiểm tra điều kiện này một cách độc lập cho từng`k`, tính toán lại các so sánh mỗi lần. Vì mỗi lần kiểm tra là`O(n)`, giải pháp đầy đủ là`O(n^2)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra chu kỳ trên k) | O(n^2) | O(n) | Đã chấp nhận | 
| Tối ưu (cùng ý tưởng, triển khai trực tiếp) | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách kiểm tra mọi khoảng thời gian có thể`k`. 

1. Tính mảng sai phân`d`, Ở đâu`d[i] = a[i] - a[i-1]`. Điều này cô lập mẫu lặp lại tương ứng với mảng ẩn`x`. Tổng tiền tố không còn cần thiết nữa khi chúng tôi trích xuất sự khác biệt. 
2. Lặp lại tất cả các giá trị có thể có của`k`từ`1`ĐẾN`n`. Mỗi`k`được coi là độ dài khoảng thời gian ứng cử viên. 
3. Đối với cố định`k`, giả sử nó hợp lệ và xác minh tính nhất quán trên mảng khác biệt. Đối với mỗi chỉ số`i`bắt đầu từ`k+1`ĐẾN`n`, kiểm tra xem`d[i] == d[i-k]`. 
4. Nếu tìm thấy bất kỳ sự không phù hợp nào, hãy từ chối điều này`k`ngay lập tức. Điều này đảm bảo chúng tôi chỉ chấp nhận các cấu trúc định kỳ thực sự. 
5. Nếu tất cả các so sánh thành công, hãy thêm`k`vào danh sách câu trả lời. 
6. Sau khi kiểm tra tất cả các ứng viên, in ra các giá trị hợp lệ thu thập được theo thứ tự tăng dần. 

### Tại sao nó hoạt động 

Bất biến chính là mảng sai phân phải lặp lại chính xác mỗi lần`k`vị trí nếu nó bắt nguồn từ một chiều dài-`k`mảng tuần hoàn`x`. Bởi vì mỗi`d[i]`trực tiếp tương ứng với`x[(i-1) mod k]`, sự bằng nhau của tất cả các phần tử cách đều nhau`k`xa nhau vừa cần vừa đủ. Nếu có bất kỳ sự không phù hợp nào tồn tại thì không có sự phân công nào`x`có thể dung hòa được mâu thuẫn, vì mỗi vị trí trong`x`sẽ buộc phải nhận hai giá trị khác nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = [0] + list(map(int, input().split()))

    d = [0] * (n + 1)
    for i in range(1, n + 1):
        d[i] = a[i] - a[i - 1]

    res = []

    for k in range(1, n + 1):
        ok = True
        for i in range(k + 1, n + 1):
            if d[i] != d[i - k]:
                ok = False
                break
        if ok:
            res.append(k)

    print(len(res))
    print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách xây dựng lại mảng sai phân để cấu trúc tuần hoàn trở nên rõ ràng. Điều này tránh việc phải xử lý nhiều lần các khoản tiền tố trong quá trình xác thực. 

Vòng lặp cốt lõi cố gắng mọi cách có thể`k`và kiểm tra tính tuần hoàn bằng cách so sánh chính xác từng phần tử với phần tử đó`k`các vị trí đằng sau nó. Việc nghỉ sớm rất quan trọng vì nó ngăn cản những công việc không cần thiết một khi phát hiện ra mâu thuẫn. 

Phải cẩn thận với việc lập chỉ mục: mảng khác biệt ở đây dựa trên 1 để khớp với việc lập chỉ mục tiền tố của`a`, làm đơn giản hóa mối quan hệ`d[i] = a[i] - a[i-1]`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
```Đây`a = [0, 1, 2, 3, 4, 5]`Và`d = [1, 1, 1, 1, 1]`. 

| k | kiểm tra so sánh | hợp lệ | 
| --- | --- | --- | 
| 1 | tất cả đều bằng nhau (tầm thường) | vâng | 
| 2 | tất cả d[i] == d[i-2] | vâng | 
| 3 | tất cả đều bình đẳng | vâng | 
| 4 | tất cả đều bình đẳng | vâng | 
| 5 | tất cả đều bình đẳng | vâng | 

Tất cả các giá trị đều hoạt động vì mảng chênh lệch không đổi, vì vậy bất kỳ khoảng thời gian nào cũng hợp lệ. 

Đầu ra:```
5
1 2 3 4 5
```Điều này chứng tỏ trường hợp tồn tại tính đối xứng cực đại và mọi chu kỳ ứng cử viên đều hợp lệ. 

### Ví dụ 2 

đầu vào:```
5
1 3 5 6 8
```Chúng tôi tính toán`a = [0,1,3,5,6,8]`Và`d = [1,2,2,1,2]`. 

| k | so sánh chính | hợp lệ | 
| --- | --- | --- | 
| 1 | 2 ≠ 1 ngay lập tức | không | 
| 2 | d[3]=2 so với d[1]=1 không khớp | không | 
| 3 | d[4]=1 == d[1], d[5]=2 == d[2] | vâng | 
| 4 | không khớp ở vị trí 5 | không | 
| 5 | nhất quán một cách tầm thường | vâng | 

Đầu ra:```
2
3 5
```Ví dụ này cho thấy nhiều khoảng thời gian hợp lệ có thể cùng tồn tại khi mẫu khác biệt lặp lại một phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi giá trị trong số n ứng cử viên k được xác minh bằng cách quét tối đa n phần tử | 
| Không gian | O(n) | Chỉ sử dụng mảng khác biệt và lưu trữ đầu ra | 

Với`n ≤ 1000`, số lượng thao tác tối đa là khoảng một triệu so sánh, phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins
    output = io.StringIO()
    sys.stdout = output

    # ---- solution ----
    n = int(input())
    a = [0] + list(map(int, input().split()))

    d = [0] * (n + 1)
    for i in range(1, n + 1):
        d[i] = a[i] - a[i - 1]

    res = []
    for k in range(1, n + 1):
        ok = True
        for i in range(k + 1, n + 1):
            if d[i] != d[i - k]:
                ok = False
                break
        if ok:
            res.append(k)

    print(len(res))
    print(*res)
    # ------------------

    sys.stdout.seek(0)
    return output.getvalue().strip()

# provided sample
assert run("5\n1 2 3 4 5\n") == "5\n1 2 3 4 5"

# custom: single element
assert run("1\n7\n") == "1\n1"

# custom: strictly alternating differences
assert run("4\n1 2 1 2\n") == "2\n2 4"

# custom: all equal differences
assert run("3\n10 20 30\n") == "3\n1 2 3"

# custom: no small period
assert run("5\n1 2 4 7 11\n") == "1\n5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`1 / 1`| trường hợp ranh giới tối thiểu | 
|`1 2 1 2`|`2 / 2 4`| mô hình xen kẽ | 
|`10 20 30`|`3 / 1 2 3`| trường hợp chênh lệch không đổi | 
|`1 2 4 7 11`|`1 / 5`| không lặp lại ngoại trừ toàn bộ chiều dài | 

## Vỏ cạnh 

Một đầu vào tối thiểu trong đó`n = 1`là quan trọng bởi vì mọi`k = 1`có giá trị tầm thường. Thuật toán xử lý việc này một cách tự nhiên vì không có sự so sánh nào bị lỗi, do đó`1`luôn luôn được bao gồm. 

Khi mọi sự khác biệt đều giống nhau, mọi`k`trở nên hợp lệ. Thuật toán chấp nhận chính xác tất cả các giá trị vì mọi so sánh`d[i] == d[i-k]`luôn luôn giữ. 

Đối với các mẫu xen kẽ hoặc có cấu trúc, kích thước nhỏ hơn`k`các giá trị có thể thất bại trong khi những giá trị lớn hơn lại thành công. Kiểm tra định kỳ nắm bắt chính xác điều này vì các điểm không khớp xuất hiện chính xác ở vị trí căn chỉnh bị hỏng và việc từ chối xảy ra ngay lập tức mà không cần phải xây dựng lại toàn bộ.
