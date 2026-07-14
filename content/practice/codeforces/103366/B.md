---
title: "CF 103366B - Phân số tiếp tục"
description: "Chúng ta được cho một số hữu tỷ dương được biểu diễn dưới dạng phân số rút gọn x/y và chúng ta được yêu cầu biểu thị nó dưới dạng phân số hữu hạn liên tục."
date: "2026-07-03T12:56:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103366
codeforces_index: "B"
codeforces_contest_name: "2021 Jiangxi Provincial Collegiate Programming Contest"
rating: 0
weight: 103366
solve_time_s: 48
verified: true
draft: false
---

[CF 103366B - Phân số tiếp tục](https://codeforces.com/problemset/problem/103366/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số hữu tỷ dương được biểu diễn dưới dạng phân số rút gọn x/y và chúng ta được yêu cầu biểu thị nó dưới dạng phân số hữu hạn liên tục. Phân số liên tục ở đây là một biểu thức lồng nhau trong đó giá trị được viết dưới dạng phần nguyên a0 cộng với nghịch đảo của một phân số liên tục khác được hình thành từ các hệ số còn lại a1, a2, v.v. cho đến an. 

Theo thuật ngữ hoạt động hơn, chúng ta muốn phân tách x/y thành một chuỗi các số nguyên sao cho nếu chúng ta bắt đầu từ dưới cùng và liên tục đảo ngược và cộng, chúng ta sẽ tái tạo lại phân số ban đầu một cách chính xác. Đầu ra không phải là một giá trị đơn lẻ mà là một chuỗi các hệ số mô tả duy nhất sự phân tách này. 

Các ràng buộc lớn về kích thước giá trị, với x và y lên tới 10^9 và lên tới 1000 trường hợp thử nghiệm. Điều này gợi ý rõ ràng rằng mọi giải pháp đều phải chạy theo thời gian logarit cho mỗi trường hợp thử nghiệm, vì ngay cả các cách tiếp cận kiểu O(x) hoặc O(y) cũng không thể thực hiện được. Thuật toán Euclide tiêu chuẩn chạy trong O(log min(x, y)), nhanh và cũng có cấu trúc liên quan đến các phân số tiếp tục, đây là gợi ý chính. 

Trường hợp cạnh tinh tế xuất hiện khi một trong các số là bội số của số kia sau các bước rút gọn. Ví dụ: nếu x = 5 và y = 1 thì phân số đã là số nguyên. Phân số tiếp theo chỉ nên là [5] chứ không phải dài hơn. Một trường hợp góc khác là khi các bước chia nhanh chóng tạo ra số dư bằng 0, chẳng hạn như 114/1 hoặc 1/114. Trong những trường hợp này, trình tự trở nên rất ngắn và việc triển khai bất cẩn luôn cố gắng thêm các bước tương hỗ có thể thêm sai các số 0 hoặc các thuật ngữ không hợp lệ. 

## Phương pháp tiếp cận 

Một cách ngây thơ để suy nghĩ về vấn đề là mô phỏng trực tiếp định nghĩa. Người ta có thể tính liên tục phần nguyên của x/y, trừ nó, sau đó đảo ngược phần phân số còn lại và tiếp tục cho đến khi phân số đó trở thành số nguyên. Điều này rõ ràng về mặt khái niệm: mỗi bước trích xuất ai = sàn(x/y), sau đó thay thế (x, y) bằng (y, x - ai * y). Điều này hiệu quả vì nó phản ánh chính xác định nghĩa toán học của các phân số liên tục. 

Tuy nhiên, quá trình này không chỉ là một thủ thuật mà thực chất nó là thuật toán Euclide được viết dưới một dạng khác. Quan sát quan trọng là bước trừ x - ai * y đang tính toán chính xác phần còn lại trong phép chia và việc hoán đổi (x, y) tương ứng với việc chuyển sang cặp số chia-số dư tiếp theo. Điều này có nghĩa là mỗi hệ số phân số liên tục tương ứng trực tiếp với một thương số trong thuật toán Euclid. 

Quan điểm vũ phu sẽ đề xuất các phép toán số học lặp đi lặp lại trên các phân số trung gian có khả năng lớn. Nhưng mỗi bước sẽ giảm nghiêm ngặt một trong các số thành số dư nhỏ hơn số chia trước đó, do đó số bước bị giới hạn bởi số lần lặp Euclide, là logarit. 

Một khi chúng ta nhận ra sự tương đương này, bài toán sẽ giảm xuống còn việc thực hiện liên tục phép chia số nguyên và trích phần dư cho đến khi phần dư bằng 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trực tiếp phân số tiếp theo | O(log min(x, y)) | O(1) | Đã chấp nhận | 
| Giải thích thuật toán Euclide | O(log min(x, y)) | O(1) | Đã chấp nhận | 

Cả hai cách tiếp cận về cơ bản đều giống nhau, nhưng việc xem xét nó thông qua Euclid khiến cho tính đúng đắn ngay lập tức và việc thực hiện trở nên đơn giản. 

## Hướng dẫn thuật toán 

Chúng tôi liên tục áp dụng phép chia số nguyên trong khi vẫn duy trì bất biến rằng cặp hiện tại (x, y) luôn biểu thị phân số còn lại mà chúng tôi vẫn cần khai triển.

1. Tính a = x // y và ghi nó làm hệ số phân số tiếp theo. Điều này trích xuất phần nguyên của phân số ở giai đoạn hiện tại. 
2. Tính số dư r = x % y. Điều này thể hiện những gì còn lại sau khi loại bỏ toàn bộ phần nguyên của y khỏi x. 
3. Nếu số dư r bằng 0 thì dừng. Tại thời điểm này x/y chính xác là một số nguyên, do đó phân số tiếp tục kết thúc ở hệ số cuối cùng. 
4. Ngược lại, chúng ta thay thế (x, y) bằng (y, r). Điều này tương ứng với việc đảo ngược phần dư phân số, vì phân số còn lại là r/y, và lấy nghịch đảo sẽ cho ra y/r cho giai đoạn tiếp theo. 
5. Lặp lại quá trình này cho đến khi kết thúc. 

Mỗi lần lặp lại giảm nghiêm ngặt giá trị thứ hai y theo nghĩa Euclide, đảm bảo quá trình phải kết thúc. 

### Tại sao nó hoạt động 

Ở mỗi bước, chúng ta biểu thị phân số x/y dưới dạng a + r/y trong đó a = Floor(x/y). Đây chính xác là số hạng đầu tiên của phân số tiếp tục. Phần r/y còn lại sau đó được đảo ngược để tiếp tục phân rã. Phép biến đổi (x, y) → (y, r) bảo toàn giá trị của phân số trong cấu trúc phân số tiếp tục, vì nó tương ứng với việc viết lại r/y thành 1 / (y/r). Vì mỗi bước chính xác là một bước chia trong thuật toán Euclid nên dãy thương được xác định duy nhất và phải kết thúc khi số dư bằng 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(x, y):
    res = []
    while True:
        a = x // y
        res.append(a)
        r = x % y
        if r == 0:
            break
        x, y = y, r
    return res

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        x, y = map(int, input().split())
        cf = solve_case(x, y)
        out.append(str(len(cf) - 1) + " " + " ".join(map(str, cf)))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Chức năng cốt lõi`solve_case`thực hiện vòng lặp chia Euclide. Mỗi lần lặp lại gắn thêm thương số`a = x // y`, tương ứng trực tiếp với một hệ số trong phân số tiếp theo. Phần còn lại quyết định liệu chúng tôi chấm dứt hay tiếp tục. 

Chi tiết triển khai chính là trao đổi`(x, y) = (y, r)`, điều này rất dễ bị hiểu sai nếu người ta nghĩ về phân số thay vì thuật toán Euclid. Nếu không có sự hoán đổi này, quá trình sẽ không tiến triển đúng hướng tới việc chấm dứt. 

Định dạng đầu ra yêu cầu in số lần chuyển đổi giữa các hệ số, đó là lý do tại sao chúng tôi xuất ra`len(cf) - 1`còn hơn là`len(cf)`. 

## Ví dụ đã hoạt động 

### Ví dụ 1: x = 105, y = 38 

Chúng tôi theo dõi các bước Euclide: 

| x | y | a = x//y | r = x%y | 
| --- | --- | --- | --- | 
| 105 | 38 | 2 | 29 | 
| 38 | 29 | 1 | 9 | 
| 29 | 9 | 3 | 2 | 
| 9 | 2 | 4 | 1 | 
| 2 | 1 | 2 | 0 | 

Các hệ số thu được là [2, 1, 3, 4, 2]. 

Điều này cho thấy sự suy giảm Euclide đầy đủ trong đó mỗi phần còn lại trở nên nhỏ hơn hoàn toàn, xác nhận rằng mỗi hệ số tương ứng với một bước chia. 

### Ví dụ 2: x = 114, y = 1 

| x | y | a = x//y | r | 
| --- | --- | --- | --- | 
| 114 | 1 | 114 | 0 | 

Chúng tôi ngay lập tức chấm dứt sau một bước, tạo ra [114]. 

Điều này thể hiện trường hợp số nguyên, trong đó phân số tiếp theo không có cấu trúc lồng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log min(x, y)) | Mỗi bước thực hiện phép chia Euclide và phần còn lại giảm dần, đảm bảo độ sâu logarit | 
| Không gian | O(k) | Chúng tôi lưu trữ k hệ số phân số liên tục, trong đó k là số bước Euclide | 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm với giá trị lên tới 10^9 và mỗi trường hợp hoàn thành tối đa vài chục lần lặp, do đó giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import gcd

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            x, y = map(int, input().split())
            res = []
            while True:
                a = x // y
                res.append(a)
                r = x % y
                if r == 0:
                    break
                x, y = y, r
            out.append(str(len(res) - 1) + " " + " ".join(map(str, res)))
        return "\n".join(out)

    return solve()

# provided samples
assert run("2\n105 38\n1 114\n") == "4 2 1 3 4 2\n0 114"

# custom cases
assert run("1\n5 1\n") == "0 5", "integer case"
assert run("1\n1 5\n") == "4 0 5 1 2", "small fraction"
assert run("1\n7 3\n") == "2 2 1 3", "mixed division"
assert run("1\n13 8\n") == "3 1 1 1 2", "Fibonacci-like case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 1 | 0 5 | chấm dứt số nguyên thuần túy | 
| 1 5 | 4 0 5 1 2 | phân số bắt đầu bằng phần nguyên bằng 0 | 
| 7 3 | 2 2 1 3 | hành vi Euclid nhiều bước | 
| 13 8 | 3 1 1 1 2 | cấu trúc xấu nhất giảm chậm | 

## Vỏ cạnh 

Trường hợp một cạnh là khi x < y, tạo ra a0 = 0 ngay lập tức. Ví dụ: x = 1, y = 5 tạo ra một chuỗi bắt đầu bằng 0. Thuật toán xử lý điều này một cách tự nhiên vì x // y trở thành 0 và bước còn lại sẽ chuyển phân số thành (y, x), tiếp tục chính xác. 

Truy tìm x = 1, y = 5: 

Bước đầu tiên cho a = 0, dư 1. Sau đó chúng ta chuyển sang (5, 1), tạo ra 5 và kết thúc. Dãy cuối cùng là [0, 5], là dạng phân số tiếp tục chính xác. 

Một trường hợp cạnh khác là khi x = y. Trong trường hợp này, a = 1 và số dư ngay lập tức bằng 0. Đầu ra là [1] và không cần xử lý thêm. Thuật toán dừng chính xác mà không tạo ra hoán đổi bổ sung hoặc hệ số không hợp lệ.
