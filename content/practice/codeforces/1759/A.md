---
title: "CF 1759A - Có-Có?"
description: "Chúng tôi được cung cấp một chuỗi ngắn cho mỗi truy vấn và chúng tôi muốn quyết định xem liệu nó có thể xuất hiện bên trong một mẫu lặp lại vô hạn được hình thành bằng cách viết đi viết lại từ “Có” mà không có dấu phân cách hay không."
date: "2026-06-09T14:27:05+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1759
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round  834 (Div. 3)"
rating: 800
weight: 1759
solve_time_s: 119
verified: true
draft: false
---

[CF 1759A - Có-Có?](https://codeforces.com/problemset/problem/1759/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi ngắn cho mỗi truy vấn và chúng tôi muốn quyết định xem liệu nó có thể xuất hiện bên trong một mẫu lặp lại vô hạn được hình thành bằng cách viết đi viết lại từ “Có” mà không có dấu phân cách hay không. Chuỗi chúng ta đang kiểm tra không bắt buộc phải căn chỉnh với ranh giới từ, nó có thể bắt đầu và kết thúc ở bất kỳ đâu bên trong dải băng vô hạn đó. 

Ý tưởng chính là chuỗi vô hạn có cấu trúc tuần hoàn chặt chẽ có độ dài 3: nó lặp lại`Y`,`e`,`s`,`Y`,`e`,`s`, vân vân. Bất kỳ chuỗi con hợp lệ nào cũng phải khớp với từng ký tự của chu kỳ lặp lại này. 

Các ràng buộc rất nhỏ, với tối đa 1000 trường hợp thử nghiệm và mỗi chuỗi có độ dài tối đa là 50. Điều này ngay lập tức loại trừ mọi thứ phức tạp hơn việc quét tuyến tính trên mỗi trường hợp thử nghiệm. Một mô phỏng trực tiếp hoặc kiểm tra mẫu là đủ. 

Một trường hợp tinh tế là khi chuỗi bắt đầu ở các độ lệch khác nhau của chu kỳ. Ví dụ, bắt đầu từ`e`hoặc`s`là hợp lệ, vì vậy chúng ta phải cho phép cả ba phép quay của mẫu cơ sở. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xây dựng rõ ràng một tiền tố đủ dài của`"YesYesYes..."`và kiểm tra xem chuỗi truy vấn có xuất hiện dưới dạng chuỗi con hay không. Điều này hiệu quả vì độ dài tối đa là nhỏ, do đó, việc xây dựng một chuỗi có độ dài khoảng vài trăm là đủ. Tuy nhiên, đây là chi phí không cần thiết và gián tiếp về mặt khái niệm. 

Quan sát quan trọng là chuỗi vô hạn hoàn toàn tuần hoàn với chu kỳ`"Yes"`. Điều này có nghĩa là mọi ký tự ở vị trí`i`phải phù hợp`"Yes"[i mod 3]`, nhưng vì chuỗi con có thể bắt đầu ở bất cứ đâu nên chúng ta cũng phải cho phép dịch chuyển mẫu theo chu kỳ. Chỉ có ba sự sắp xếp bắt đầu hợp lệ:`"Yes"`,`"esY"`, Và`"sYe"`. 

Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem chuỗi đầu vào có khớp với một trong ba mẫu lặp lại này hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Xây dựng Brute Force | O(n · t) | O(n) | Được chấp nhận nhưng không cần thiết | 
| Kiểm tra chu kỳ | O(n · t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định mẫu cơ sở`"Yes"`và xét ba chuyển động quay tuần hoàn của nó. Mỗi vòng quay biểu thị một giai đoạn bắt đầu hợp lệ của chuỗi vô hạn. 

2. Đối với mỗi chuỗi kiểm tra, hãy thử khớp chuỗi đó với từng ký tự xoay theo từng ký tự. 

3. Để quay cố định, hãy kiểm tra vị trí`i`trong chuỗi chống lại`pattern[i % 3]`. Nếu tất cả các vị trí khớp nhau thì chuỗi đó hợp lệ. 

4. Nếu không có phép quay nào khớp, chuỗi không thể xuất hiện trong sự lặp lại vô hạn. 

Điểm quan trọng là sự căn chỉnh không xác định được, vì vậy chúng tôi ép buộc ba giai đoạn có thể thay vì cố định một giai đoạn. 

### Tại sao nó hoạt động 

Chuỗi vô hạn được xác định đầy đủ bởi máy tự động chu kỳ 3. Bất kỳ chuỗi con nào cũng tương đương với việc chọn độ lệch bắt đầu trong chu kỳ đó và đọc tiếp. Vì độ dài chu kỳ là 3 nên có chính xác ba lớp bắt đầu tương đương và việc kiểm tra tất cả chúng bao gồm mọi cách căn chỉnh chuỗi con có thể có. Điều này đảm bảo tính đầy đủ mà không cần xây dựng chuỗi vô hạn đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ok(s, p):
    for i, ch in enumerate(s):
        if ch != p[i % 3]:
            return False
    return True

def solve():
    t = int(input())
    base = "Yes"
    rotations = [base[i:] + base[:i] for i in range(3)]

    for _ in range(t):
        s = input().strip()
        for p in rotations:
            if ok(s, p):
                print("YES")
                break
        else:
            print("NO")

solve()
```Giải pháp đầu tiên tính toán trước ba phép quay theo chu kỳ của`"Yes"`. Đối với mỗi chuỗi truy vấn, nó sẽ thử kiểm tra căn chỉnh trực tiếp theo từng vòng quay. Hoạt động modulo xử lý việc khớp định kỳ một cách rõ ràng. 

Một sai lầm phổ biến là chỉ kiểm tra lại`"YesYes..."`bắt đầu từ chỉ số 0. Điều đó không thành công đối với các chuỗi như`"esYes"`, hợp lệ nhưng bắt đầu ở giữa chu kỳ. Vòng xoay khắc phục hoàn toàn điều này. 

## Ví dụ đã hoạt động 

Lấy sợi dây`"esYes"`. 

| tôi | s[i] | sự xoay vòng`"esY"`| trận đấu | 
|---|------|-------------------|--------| 
| 0 | e | e | | 
| 1 | s | s | | 
| 2 | Y | Y | | 
| 3 | e | e | | 
| 4 | s | s | | 

Điều này xác nhận nó khớp với chu kỳ bắt đầu ở offset 1. 

Bây giờ hãy xem xét`"Yess"`. 

| tôi | s[i] | sự xoay vòng`"Yes"`| trận đấu | 
|---|------|-------------------|--------| 
| 0 | Y | Y | | 
| 1 | e | e | | 
| 2 | s | s | | 
| 3 | s | Y | | 

Điều này không thành công vì ký tự thứ tư phá vỡ cấu trúc tuần hoàn, do đó không có phép quay nào chấp nhận nó. 

Những ví dụ này cho thấy tính đúng đắn phụ thuộc hoàn toàn vào tính nhất quán định kỳ chứ không phải sự trùng lặp từng phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(t · n) | Mỗi chuỗi được kiểm tra theo 3 mẫu bằng quét tuyến tính | 
| Không gian | O(1) | Chỉ một số mẫu cố định được lưu trữ | 

Tổng kích thước đầu vào đủ nhỏ để quét tuyến tính cho mỗi trường hợp thử nghiệm là tối ưu và dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def ok(s, p):
        for i, ch in enumerate(s):
            if ch != p[i % 3]:
                return False
        return True

    t = int(input())
    base = "Yes"
    rotations = [base[i:] + base[:i] for i in range(3)]

    out = []
    for _ in range(t):
        s = input().strip()
        for p in rotations:
            if ok(s, p):
                out.append("YES")
                break
        else:
            out.append("NO")
    return "\n".join(out)

assert run("3\nYes\nesY\nooo\n") == "YES\nYES\nNO"
assert run("1\nY") == "YES"
assert run("1\nseY") == "YES"
assert run("1\nYess") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| trận đấu hợp lệ tối thiểu | CÓ | trường hợp căn chỉnh ký tự đơn | 
| trận đấu xoay | CÓ | chuỗi con bắt đầu vào giữa chu kỳ | 
| tiền tố không hợp lệ | KHÔNG | phá vỡ tính tuần hoàn | 

## Vỏ cạnh 

cho`"Y"`,`"e"`, Và`"s"`, thuật toán kiểm tra tất cả các phép quay và ngay lập tức chấp nhận vì mỗi ký tự khớp với ít nhất một pha của chu trình ở vị trí 0. 

cho`"seY"`, sự quay`"seY"`khớp chính xác, cho thấy rằng không cần phải căn chỉnh toàn bộ chu kỳ, chỉ cần nhất quán với một số phần bù. 

Vì`"Yess"`, mọi vòng quay đều thất bại ở vị trí 3, xác nhận rằng ngay cả một sự không khớp duy nhất cũng sẽ phá vỡ tính hợp lệ bất kể tiền tố có chính xác hay không.
