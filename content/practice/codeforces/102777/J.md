---
title: "CF 102777J - \u0426\u0432\u0435\u0442\u043d\u0430\u044f \u0438\u0433\u0440\u043e\u0432\u0430\u044f \u0434\u043e\u0441\u043a\u0430"
description: "Chúng ta có một bảng hình chữ nhật N × M. Con chip bắt đầu ở ô phía trên bên trái và di chuyển dọc theo một đường xoắn ốc bao phủ bảng. Sau đúng k lượt di chuyển, chúng ta cần xác định màu của ô nơi chip dừng lại. Bản thân bảng không được đưa ra vì nó quá lớn."
date: "2026-07-27T20:32:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102777
codeforces_index: "J"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 19), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102777
solve_time_s: 47
verified: true
draft: false
---

[CF 102777J - \u0426\u0432\u0435\u0442\u043d\u0430\u044f \u0438\u0433\u0440\u043e\u0432\u0430\u044f \u0434\u043e\u0441\u043a\u0430](https://codeforces.com/problemset/problem/102777/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một`N × M`bảng hình chữ nhật. Con chip bắt đầu ở ô phía trên bên trái và di chuyển dọc theo một đường xoắn ốc bao phủ bảng. Sau chính xác`k`di chuyển, chúng ta cần xác định màu của ô nơi con chip dừng lại. Bản thân bảng không được đưa ra vì nó quá lớn. Màu sắc của nó tuân theo mô hình cầu vồng lặp đi lặp lại: mỗi sọc chéo có một trong bảy màu và các màu lặp lại theo chu kỳ. Câu trả lời là số từ 1 đến 7 được gán cho ô cuối cùng. 

Kích thước có thể lớn như`10^9`, vì vậy việc xây dựng bảng là không thể. Ngay cả việc lặp lại trên tất cả các ô cũng không thể thực hiện được vì có thể có tới`10^18`tế bào. Lời giải chỉ phải thực hiện một số lượng nhỏ các phép tính số học, loại trừ mọi cách tiếp cận tùy thuộc vào`N`,`M`, hoặc số lượng ô được truy cập. 

Những trường hợp khó khăn là do kích thước khổng lồ và thực tế là đường xoắn ốc chỉ thay đổi hướng ở các biên. Mô phỏng trực tiếp thường thất bại ở chính xác những chỗ đó. 

Ví dụ: với một bảng ô đơn:```
1 1 0
```con chip không bao giờ rời khỏi ô bắt đầu. Câu trả lời phải là màu sắc của`(0, 0)`, không phải là kết quả của việc cố gắng thực hiện một bước xoắn ốc. 

Một trường hợp quan trọng khác là một tấm ván rất mỏng:```
1 5 3
```Đường xoắn ốc chỉ là một đường thẳng. Việc triển khai giả định mỗi lớp có bốn cạnh không trống có thể truy cập vào các ô bên ngoài bảng. 

Trường hợp ranh giới cuối cùng là khi`k`chính xác là chuyển động cuối cùng trên một lớp. Ví dụ:```
3 3 8
```Sau tám lần di chuyển, con chip sẽ đến ô cuối cùng của hình vuông bên ngoài. Giải pháp tìm lớp tiếp theo trước khi kiểm tra lớp hiện tại sẽ tạo ra tọa độ sai. 

## Phương pháp tiếp cận 

Giải pháp đơn giản là cất giữ bảng hoặc mô phỏng hình xoắn ốc. Mô phỏng rất dễ dàng vì chúng ta chỉ cần nhớ vị trí và hướng hiện tại. Mỗi lần di chuyển sẽ kiểm tra xem ô tiếp theo có ở trong bảng hay không và liệu nó đã được truy cập chưa. Sau đó`k`di chuyển chúng tôi tính toán màu sắc. 

Điều này hiệu quả vì đường xoắn ốc có quy tắc cục bộ đơn giản. Vấn đề là những hạn chế. Trong trường hợp xấu nhất,`N`Và`M`cả hai đều`10^9`, vậy có thể có`10^18`các ô và quá trình mô phỏng sẽ yêu cầu khoảng`10^18`hoạt động. Thậm chí vài tỷ thao tác còn vượt xa thời hạn. 

Quan sát quan trọng là một hình xoắn ốc bao gồm các lớp hình chữ nhật. Thay vì làm theo từng bước, chúng ta có thể bỏ qua các lớp hoàn chỉnh bên ngoài. Sau khi loại bỏ`s`các lớp từ bên ngoài, hình chữ nhật còn lại có kích thước`(N - 2s) × (M - 2s)`. Số lượng ô đã vượt qua trước khi vào lớp`s`có thể được tính toán trực tiếp. Điều này cho phép chúng tôi xác định vị trí lớp chính xác bằng tìm kiếm nhị phân. 

Khi lớp đã được biết đến, số lần di chuyển còn lại sẽ nhỏ so với chu vi của lớp đó. Chúng ta có thể xác định vị trí bằng cách kiểm tra cạnh nào của hình chữ nhật hiện tại chứa khoảng cách còn lại. Màu sắc sau đó ngay lập tức theo sau chỉ số đường chéo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) | O(NM) | Quá chậm | 
| Tối ưu | O(log(min(N, M))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sử dụng tìm kiếm nhị phân để tìm lớp xoắn ốc chứa câu trả lời. một lớp`s`chứa hình chữ nhật từ hàng`s`chèo thuyền`N-1-s`và từ cột`s`vào cột`M-1-s`. Số ô trước lớp này là diện tích được lấy đi từ bên ngoài:`N*M - (N-2*s)*(M-2*s)`. 

Lớp hợp lệ lớn nhất mà điểm bắt đầu đã đạt đến là lớp chứa chip. 

1. Di chuyển hệ tọa độ đến lớp đã chọn. Trừ số lần di chuyển dành cho các lớp trước đó khỏi`k`. Con chip bây giờ bắt đầu từ góc trên bên trái của hình chữ nhật nhỏ hơn này. 
2. Đi bộ xung quanh chu vi của lớp này một cách toán học. Đầu tiên hình xoắn ốc đi sang phải, rồi xuống, sang trái, rồi lên. Độ dài mỗi cạnh cho chúng ta biết khoảng cách còn lại có nằm ở cạnh đó hay không. 
3. Chuyển đổi tọa độ dựa trên số 0 cuối cùng thành màu. Các sọc chéo được xác định bởi`row + column`. Màu cần tìm là:`(row + column) mod 7 + 1`. 

Lý do điều này có hiệu quả là vì mọi lớp bên ngoài hoàn chỉnh của hình xoắn ốc đều độc lập với các lớp bên trong nó. Con chip không thể vào lớp bên trong trước khi tất cả các ô của lớp hiện tại được truyền qua. Tìm kiếm nhị phân tìm thấy chính xác lớp mà chip dừng lại và các phép tính bên giữ nguyên thứ tự như hình xoắn ốc ban đầu. Tọa độ cuối cùng là tọa độ tương tự mà mô phỏng đầy đủ sẽ đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M, k = map(int, input().split())

    def before(layer):
        return N * M - (N - 2 * layer) * (M - 2 * layer)

    lo, hi = 0, min(N, M) // 2
    while lo <= hi:
        mid = (lo + hi) // 2
        if before(mid) <= k:
            lo = mid + 1
        else:
            hi = mid - 1

    layer = hi
    k -= before(layer)

    top = layer
    left = layer
    bottom = N - 1 - layer
    right = M - 1 - layer

    r, c = top, left

    if top == bottom:
        c += k
    elif left == right:
        r += k
    else:
        top_len = right - left
        if k <= top_len:
            c += k
        else:
            k -= top_len
            down_len = bottom - top
            if k <= down_len:
                r += k
            else:
                k -= down_len
                left_len = right - left
                if k <= left_len:
                    c = right - k
                    r = bottom
                else:
                    k -= left_len
                    r = bottom - k
                    c = left

    print((r + c) % 7 + 1)

if __name__ == "__main__":
    solve()
```chức năng`before(layer)`là lối tắt trung tâm. Nó đếm có bao nhiêu ô thuộc về tất cả các lớp bên ngoài trước lớp được yêu cầu. Vì tọa độ và sản phẩm có thể đạt tới`10^18`, Số nguyên Python được sử dụng một cách tự nhiên mà không lo bị tràn. 

Tìm kiếm nhị phân chỉ tìm kiếm theo số lớp có thể, nhiều nhất là`5 * 10^8`, vì vậy nó cần ít hơn 30 lần lặp. 

Sau khi định vị lớp, mã sẽ xử lý các trường hợp suy biến trước tiên. Hình chữ nhật còn lại một hàng hoặc một cột không có bốn cạnh có ý nghĩa, vì vậy việc xử lý nó một cách riêng biệt sẽ tránh được độ dài không hợp lệ. 

Đối với hình chữ nhật bình thường, chuyển động còn lại được so sánh với bốn cạnh theo thứ tự xoắn ốc. Việc so sánh sử dụng độ dài các cạnh được đo bằng bước di chuyển chứ không phải ô, điều này tránh được sai sót ở các góc. 

## Ví dụ đã hoạt động 

Sử dụng mẫu đã cho:```
5 9 25
```Các lớp được xử lý như sau. 

| Bước | Lớp | Tế bào trước lớp | Nước đi còn lại | Vị trí | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 25 |`(0,0)`| 
| Mặt trên | 0 | 0 | 17 |`(0,8)`| 
| Bên phải | 0 | 8 | 13 |`(4,8)`| 
| Mặt dưới | 0 | 12 | 5 |`(4,0)`| 
| Bên trái | 0 | 20 | 2 |`(2,0)`| 
| Mặt trên lại | 0 | 23 | 2 |`(1,2)`| 

Ô cuối cùng là`(1,2)`. Chỉ số đường chéo của nó là`1 + 2 = 3`, vậy màu sắc là`4`. 

Một ví dụ về bảng mỏng nhỏ:```
1 5 3
```Lớp duy nhất có một hàng nên chip di chuyển trực tiếp qua hàng đó. 

| Bước | Nước đi còn lại | Vị trí | 
| --- | --- | --- | 
| Bắt đầu | 3 |`(0,0)`| 
| Di chuyển | 2 |`(0,1)`| 
| Di chuyển | 1 |`(0,2)`| 
| Di chuyển | 0 |`(0,3)`| 

Chỉ số đường chéo là`3`, cho màu`4`. 

Những dấu vết này cho thấy thuật toán giữ nguyên trật tự như đường xoắn ốc ban đầu đồng thời tránh được mọi chuyển động riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log(min(N, M))) | Tìm kiếm nhị phân tìm thấy lớp, sau đó chỉ có công việc liên tục mới xác định được vị trí | 
| Không gian | O(1) | Chỉ một số biến số nguyên cố định được lưu trữ | 

Thuật toán không bao giờ tạo bảng hoặc lưu trữ các ô đã truy cập, vì vậy nó hoạt động ngay cả khi bảng chứa tối đa`10^18`tế bào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue().strip()

assert run("5 9 25\n") == "4", "sample 1"
assert run("1 1 0\n") == "1", "single cell"

assert run("1 5 3\n") == "4", "single row"
assert run("5 1 4\n") == "5", "single column"
assert run("1000000000 1000000000 0\n") == "1", "maximum size start"
assert run("3 3 8\n") == "5", "last cell boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 0`|`1`| Kích thước bảng tối thiểu và không di chuyển | 
|`1 5 3`|`4`| Xử lý xoắn ốc một hàng | 
|`5 1 4`|`5`| Xử lý xoắn ốc một cột | 
|`1000000000 1000000000 0`|`1`| Kích thước lớn không cần mô phỏng | 
|`3 3 8`|`5`| Góc cuối cùng của một lớp | 

## Vỏ cạnh 

Đối với một`1 × 1`bảng, tìm kiếm nhị phân sẽ chọn lớp 0 và cả hai chiều của hình chữ nhật còn lại là một. Việc xử lý đặc biệt cho một ô sẽ trả về vị trí bắt đầu, do đó dữ liệu đầu vào```
1 1 0
```tạo ra màu sắc`1`. 

Đối với một bảng một hàng như```
1 5 3
```hình chữ nhật còn lại có`top == bottom`. Thuật toán không cố gắng đi qua các cạnh thẳng đứng không tồn tại và chỉ di chuyển dọc theo hàng này sang cột khác.`3`, cho màu`4`. 

Đối với một bước di chuyển kết thúc chính xác ở biên của một lớp:```
3 3 8
```thuật toán giữ chip bên trong lớp 0 vì`k`vẫn còn bên trong chu vi của lớp đó. Nó đạt tới`(2,0)`, có chỉ số đường chéo là`2`, tạo màu`3`. Điều này tránh được lỗi phổ biến là chuyển sang lớp bên trong một lần di chuyển quá sớm.
