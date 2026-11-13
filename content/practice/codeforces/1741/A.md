---
title: "CF 1741A - So sánh kích cỡ áo thun"
description: "Chúng tôi được yêu cầu so sánh hai kích cỡ áo phông được biểu thị dưới dạng dây. Mỗi chuỗi kết thúc bằng một trong ba chữ cái: S cho nhỏ, M cho trung bình hoặc L cho lớn. Ngoài ra, có thể có một chuỗi X trước S hoặc L, biểu thị cực nhỏ hoặc cực lớn."
date: "2026-06-09T16:25:33+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1741
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 826 (Div. 3)"
rating: 800
weight: 1741
solve_time_s: 159
verified: true
draft: false
---

[CF 1741A - So sánh kích cỡ áo phông](https://codeforces.com/problemset/problem/1741/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu so sánh hai kích cỡ áo phông được biểu thị dưới dạng dây. Mỗi chuỗi kết thúc bằng một trong ba chữ cái: S cho nhỏ, M cho trung bình hoặc L cho lớn. Ngoài ra, có thể có một chuỗi X trước S hoặc L, biểu thị cực nhỏ hoặc cực lớn. Phương tiện không bao giờ có X nên M được cố định. Đầu vào cung cấp nhiều cặp kích thước và với mỗi cặp, chúng ta phải in xem cặp đầu tiên nhỏ hơn, lớn hơn hay bằng cặp thứ hai. 

Điều quan trọng là hiểu được thứ tự. Cái nhỏ thì nhỏ hơn vừa hoặc lớn, và cái lớn thì lớn hơn vừa hoặc nhỏ. Trong số các số nhỏ, nhiều X hơn sẽ làm cho áo nhỏ hơn, ví dụ: XXXS < XS. Trong số các kích thước lớn, càng nhiều X thì nó càng lớn, ví dụ: XXXL > XL. Trung bình luôn nằm giữa tất cả những cái nhỏ và tất cả những cái lớn. 

Các ràng buộc cho chúng ta biết rằng có thể có tới 10.000 trường hợp thử nghiệm và mỗi chuỗi kích thước tối đa là 50 ký tự. Một cách tiếp cận đơn giản liên quan đến bất kỳ vòng lặp lồng nhau nào trên các ký tự chuỗi vẫn có thể hoạt động vì 50 × 10.000 = 500.000 thao tác, có thể chấp nhận được trong giới hạn 1 giây. Tuy nhiên, cần phải phân tích chuỗi cẩn thận để xử lý số lượng X và chữ cái cuối cùng khác nhau. 

Các trường hợp cạnh phát sinh khi so sánh kích thước cực lớn hoặc sự kết hợp bất thường. Ví dụ: XXXXXS so với M sẽ trả về`<`, bởi vì bất kỳ kích thước nhỏ nào, bất kể chiều dài, đều nhỏ hơn kích thước trung bình. Tương tự, L so với XXXL sẽ trả về`<`, vì XXXL là một số lớn và L là một số lớn, nên XXXL > L. Việc so sánh chuỗi từ điển đơn giản sẽ không thành công ở đây, vì "XXXXXS" > "M" dưới dạng chuỗi, mặc dù kích thước nhỏ hơn. 

## Phương pháp tiếp cận 

Phương pháp brute-force là mã hóa từng kích cỡ áo phông dưới dạng một số nguyên biểu thị thứ hạng tuyệt đối của nó và sau đó so sánh các số nguyên này. Đối với số nhỏ, chúng ta có thể gán số âm giảm dần theo số X; đối với mức trung bình, bằng không; đối với số lớn, số dương tăng theo số X. Điều này hiệu quả vì tất cả các kích thước đều theo thứ tự tuyến tính và việc mã hóa chúng cho phép so sánh số đơn giản. Cách tiếp cận này đúng nhưng gây ra việc ghi sổ không cần thiết nếu chúng ta chỉ cần so sánh hai chuỗi cùng một lúc. 

Cách tiếp cận tối ưu tận dụng thực tế là các quy tắc đặt hàng rất đơn giản và được bản địa hóa: ký tự cuối cùng xác định danh mục (S, M, L) và số lượng X trước đó xác định thứ tự tương đối trong S và L. Trước tiên, chúng ta có thể kiểm tra ký tự cuối cùng. Nếu chúng khác nhau, việc so sánh diễn ra ngay lập tức: S < M < L. Nếu các ký tự cuối cùng bằng nhau, chúng ta so sánh số lượng X: đối với S, nhiều X hơn có nghĩa là nhỏ hơn; đối với L, nhiều X hơn có nghĩa là lớn hơn. Điều này yêu cầu tối đa đếm các ký tự trên mỗi chuỗi, tuyến tính theo độ dài chuỗi hoặc O(1) vì độ dài ≤ 50. Không cần cấu trúc dữ liệu bổ sung và chúng tôi có thể xử lý tất cả các trường hợp thử nghiệm một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mã hóa thứ hạng) | O(t * n) | O(1) | Được chấp nhận nhưng phức tạp hơn một chút | 
| Tối ưu (so sánh dựa trên ký tự) | O(t * n) | O(1) | Được chấp nhận và đơn giản hơn | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test, đọc hai dòng size áo thun`a`Và`b`. 
3. Trích xuất ký tự cuối cùng của mỗi chuỗi để xác định loại của nó: nhỏ, trung bình hoặc lớn. 
4. Nếu các loại khác nhau, hãy so sánh trực tiếp theo thứ tự S < M < L. In`<`nếu như`a`nhỏ hơn,`>`nếu lớn hơn hoặc tiếp tục nếu bằng nhau. 
5. Nếu các loại giống nhau, hãy đếm số X trong mỗi chuỗi. Đối với kích thước nhỏ, nhiều X hơn có nghĩa là kích thước nhỏ hơn; đối với kích thước lớn, nhiều X hơn có nghĩa là kích thước lớn hơn. Phương tiện không có X và luôn bằng một phương tiện khác. 
6. So sánh số lượng theo loại và hình in`<`,`>`, hoặc`=`sao cho phù hợp. 

Tại sao nó hoạt động: Thuật toán dựa trên hai thuộc tính. Đầu tiên, ký tự cuối cùng mã hóa danh mục chính, danh mục này đã sắp xếp một phần tất cả các kích cỡ. Thứ hai, số lượng X sẽ điều chỉnh thứ tự trong các danh mục S hoặc L. Bằng cách xử lý rõ ràng hai trường hợp này, thuật toán đảm bảo so sánh chính xác cho tất cả các chuỗi có kích thước hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b = input().split()
    type_a, type_b = a[-1], b[-1]
    
    if type_a != type_b:
        if type_a == 'S':
            print('<')
        elif type_a == 'M':
            if type_b == 'S':
                print('>')
            else:
                print('<')
        else:  # type_a == 'L'
            print('>' if type_b != 'L' else '=')
    else:
        count_a, count_b = a.count('X'), b.count('X')
        if type_a == 'S':
            print('<' if count_a > count_b else '>' if count_a < count_b else '=')
        elif type_a == 'L':
            print('>' if count_a > count_b else '<' if count_a < count_b else '=')
        else:
            print('=')
```Đầu tiên, mã sẽ đọc số lượng trường hợp kiểm thử và lặp qua chúng. Ký tự cuối cùng xác định loại chính, cho phép chúng ta rút ngắn việc so sánh khi các loại khác nhau. Nếu các loại khớp nhau, chúng tôi đếm X và điều chỉnh so sánh tùy theo đó là S hay L. Thứ tự bị đảo ngược đối với S vì càng nhiều X thì S càng nhỏ. Phương tiện được xử lý như một sự bình đẳng đơn giản. 

## Ví dụ đã hoạt động 

Đối với đầu vào`XXXS XS`: 

| một | b | loại_a | loại_b | đếm_a | đếm_b | So sánh | 
| --- | --- | --- | --- | --- | --- | --- | 
| XXXS | XS | S | S | 3 | 1 | đếm_a > đếm_b →`<`| 

Chúng ta thấy rằng XXXS nhỏ hơn XS vì ba X làm cho nó nhỏ hơn. 

Đối với đầu vào`XL M`: 

| một | b | loại_a | loại_b | đếm_a | đếm_b | So sánh | 
| --- | --- | --- | --- | --- | --- | --- | 
| XL | M | L | M | 1 | 0 | gõ_a > gõ_b →`>`| 

XL lớn và M vừa, nên XL > M. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Mỗi test case quét tối đa 50 ký tự để đếm X, lặp lại t lần | 
| Không gian | O(1) | Chỉ có một vài biến để đếm và so sánh kiểu | 

Với t 10.000 và n ≤ 50, thuật toán thực hiện tối đa 500.000 thao tác, nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        t = int(input())
        for _ in range(t):
            a, b = input().split()
            type_a, type_b = a[-1], b[-1]
            
            if type_a != type_b:
                if type_a == 'S':
                    print('<')
                elif type_a == 'M':
                    print('>' if type_b == 'S' else '<')
                else:  # type_a == 'L'
                    print('>' if type_b != 'L' else '=')
            else:
                count_a, count_b = a.count('X'), b.count('X')
                if type_a == 'S':
                    print('<' if count_a > count_b else '>' if count_a < count_b else '=')
                elif type_a == 'L':
                    print('>' if count_a > count_b else '<' if count_a < count_b else '=')
                else:
                    print('=')
    return out.getvalue().strip()

# provided samples
assert run("6\nXXXS XS\nXXXL XL\nXL M\nXXL XXL\nXXXXXS M\nL M\n") == "<\n>\n>\n=\n<\n>", "sample 1"

# custom cases
assert run("3\nM M\nS M\nXXXL L\n") == "=\n<\n>", "medium equality and extremes"
assert run("2\nXS XXS\nXL XXL\n") == ">\n<", "small and large X counts"
assert run("1\nXXXXXXS XXXXXS\n") == "<", "multiple Xs for small"
assert run("1\nXXXL XXXL\n") == "=", "identical large sizes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| M M | = | Bình đẳng trung bình | 
| S M | < | Nhỏ và vừa | 
| XXXL L | > | Lớn có thêm X | 
| XS XXS | > | Đặt hàng số X nhỏ | 
| XL XXL | < | Đặt hàng số X lớn | 
| XXXXXXS XXXXXS | < | Kích thước cực nhỏ | 
| XXXL XXXL | = | Kích thước lớn giống hệt nhau | 

## Vỏ cạnh 

Đối với đầu vào`XXXXXS M`, so sánh ký tự cuối cùng ngay lập tức xác định M lớn hơn bất kỳ ký tự nhỏ nào. Thuật toán in`<`mà không cần đếm X. Vì`XXXL L`, cả hai đều lớn nên số lượng X được so sánh: 3 X > 1 X, dẫn đến`>`. Đối với `
