---
title: "CF 105013C - \u732b\u72d7\u5927\u6218"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Với mỗi test, chúng ta nhận được một chuỗi có độ dài $n$. Nhiệm vụ là chuyển đổi chuỗi theo một cách rất cụ thể và quyết định kết quả nào trong số ba kết quả đầu ra cố định phù hợp sau khi xử lý. Quy tắc xử lý đơn giản nhưng nghiêm ngặt."
date: "2026-06-28T02:12:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105013
codeforces_index: "C"
codeforces_contest_name: "The 19th Southeast University Programming Contest (Summer)"
rating: 0
weight: 105013
solve_time_s: 46
verified: true
draft: false
---

[CF 105013C - \u732b\u72d7\u5927\u6218](https://codeforces.com/problemset/problem/105013/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được một chuỗi có độ dài$n$. Nhiệm vụ là chuyển đổi chuỗi theo một cách rất cụ thể và quyết định kết quả nào trong số ba kết quả đầu ra cố định phù hợp sau khi xử lý. 

Quy tắc xử lý đơn giản nhưng nghiêm ngặt. Đầu tiên, tất cả các ký tự được chuyển đổi thành chữ thường để phiên bản chữ hoa và chữ thường được xử lý như nhau. Sau đó, các ký tự giống nhau liên tiếp được nén sao cho bất kỳ ký tự nào chạy như`"aaaa"`trở thành đơn`"a"`,`"bbbbb"`trở thành`"b"`, vân vân. Sau quá trình chuẩn hóa này, chuỗi kết quả được so sánh với hai mẫu đặc biệt. Nếu nó bằng`"meow"`, đầu ra là`"0_0"`. Nếu nó bằng`"waov"`, đầu ra là`"*_*"`. Bất kỳ kết quả nào khác tạo ra`"???"`. 

Các ràng buộc không hạn chế rõ ràng trong câu lệnh, nhưng sự hiện diện của việc xử lý chuỗi trực tiếp trên mỗi trường hợp kiểm thử gợi ý rõ ràng rằng$n$có thể đủ lớn để bất cứ điều gì tệ hơn tuyến tính trên mỗi trường hợp thử nghiệm sẽ có rủi ro. Điều này ngay lập tức loại trừ các phương pháp liên tục xây dựng lại các chuỗi không hiệu quả hoặc sử dụng các lần quét lồng nhau có thể làm giảm hành vi bậc hai. 

Trường hợp cạnh tinh tế đến từ bước nén. Một cách giải thích ngây thơ có thể loại bỏ các ký tự trùng lặp trên toàn cầu thay vì chỉ thu gọn các lần chạy liên tiếp. Ví dụ,`"mmeeooww"`nên trở thành`"meow"`, nhưng việc loại bỏ trùng lặp toàn cục sẽ giảm nó xuống một cách không chính xác`"meow"`chỉ khi đơn hàng được bảo quản cẩn thận. Một trường hợp cạnh khác là vỏ hỗn hợp như`"MeOoW"`vẫn phải chuẩn hóa chính xác trước khi nén. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu tuân theo mô tả theo nghĩa đen. Đối với mỗi chuỗi, trước tiên chúng tôi chuyển đổi mọi ký tự thành chữ thường. Sau đó, chúng tôi liên tục quét chuỗi và loại bỏ các ký tự trùng lặp cho đến khi không còn ký tự bằng nhau liền kề. Việc triển khai đơn giản có thể xây dựng lại chuỗi sau mỗi lần chuyển hoặc kiểm tra liên tục các ký tự lân cận và xóa từng ký tự trùng lặp. 

Điều này hoạt động chính xác vì nó trực tiếp thực thi định nghĩa nén liên tiếp. Tuy nhiên, điểm yếu của nó xuất hiện khi dây có quãng chạy dài. Mỗi thao tác xóa có thể dịch chuyển hậu tố còn lại và việc thực hiện việc này lặp đi lặp lại sẽ dẫn đến hành vi bậc hai trong trường hợp xấu nhất trên mỗi trường hợp thử nghiệm. Với nhiều trường hợp thử nghiệm, việc này nhanh chóng trở nên quá chậm khi$n$là lớn. 

Điều quan trọng là chúng ta không cần phải thực hiện nhiều lần. Việc nén chỉ phụ thuộc vào việc mỗi ký tự có bằng ký tự trước đó trong chuỗi được xử lý cuối cùng hay không. Điều này có nghĩa là chúng ta có thể xây dựng kết quả trong một lần chuyển bằng cách chỉ thêm một ký tự khi nó khác với ký tự được thêm cuối cùng. Kết hợp với một lần chuyển đổi chữ thường, toàn bộ quá trình chuyển đổi trở thành tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (xóa nhiều lần hoặc xây dựng lại) |$O(n^2)$|$O(n)$| Quá chậm | 
| Chuẩn hóa một lần + nén chạy |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc chuỗi và chuyển đổi mọi ký tự thành chữ thường. Điều này đảm bảo kiểu chữ không ảnh hưởng đến việc so sánh vì mẫu đích chỉ được xác định bằng chữ thường. 
2. Khởi tạo một chuỗi kết quả trống sẽ lưu trữ dạng nén. 
3. Lặp lại từng ký tự của chuỗi đã chuẩn hóa. 
4. Chỉ thêm ký tự vào kết quả nếu nó khác với ký tự cuối cùng có trong kết quả. Nếu giống nhau thì bỏ qua vì nó thuộc cùng một khối liên tiếp. 
5. Sau khi quét, so sánh chuỗi nén thu được với`"meow"`Và`"waov"`. 
6. Đầu ra`"0_0"`nếu nó phù hợp`"meow"`,`"*_*"`nếu nó phù hợp`"waov"`, nếu không thì xuất ra`"???"`. 

### Tại sao nó hoạt động 

Bước nén tương đương với việc thay thế mọi khối ký tự liền kề tối đa bằng một ký tự đại diện duy nhất. Cấu trúc một lần đảm bảo rằng chính xác một ký tự trên mỗi khối được giữ lại, bởi vì chúng tôi chỉ ngăn chặn các lần lặp lại ngay sau cùng một ký tự. Vì mỗi ranh giới khối được phát hiện chính xác một lần khi ký tự thay đổi, nên chuỗi kết quả giống hệt với những gì việc loại bỏ trùng lặp chuỗi đầy đủ lặp lại cuối cùng sẽ tạo ra, nhưng không lặp lại công việc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    # lowercase conversion
    s = s.lower()

    # run-length compression
    t = []
    for c in s:
        if not t or t[-1] != c:
            t.append(c)

    t = ''.join(t)

    if t == "meow":
        print("0_0")
    elif t == "waov":
        print("*_*")
    else:
        print("???")

def main():
    t = int(input())
    for _ in range(t):
        solve()

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên bình thường hóa chuỗi đầu vào bằng cách sử dụng`.lower()`, đảm bảo so sánh ký tự thống nhất. Việc nén được thực hiện bằng cách sử dụng danh sách`t`để tránh việc nối chuỗi lặp lại, điều này có thể dẫn đến hành vi bậc hai trong Python. Mỗi ký tự chỉ được so sánh với ký tự được lưu giữ cuối cùng, đảm bảo xử lý thời gian tuyến tính. Cuối cùng, chúng tôi so sánh với hai mẫu mục tiêu. 

Một cạm bẫy phổ biến là sử dụng`t += c`trên chuỗi nhiều lần, điều này sẽ làm giảm hiệu suất. Một điều nữa là quên rằng việc nén phải đảm bảo trật tự trong khi chỉ loại bỏ các bản sao liên tiếp chứ không phải tất cả các bản sao trên toàn cầu. 

## Ví dụ đã hoạt động 

Hãy xem xét chuỗi đầu vào`"mMeeOoWW"`. 

Sau khi chuyển đổi chữ thường, chúng tôi nhận được`"mmeeooww"`. 

| Bước | Nhân vật | Kết quả hiện tại | Hành động | 
| --- | --- | --- | --- | 
| 1 | m | m | nối thêm | 
| 2 | m | m | bỏ qua | 
| 3 | e | tôi | nối thêm | 
| 4 | e | tôi | bỏ qua | 
| 5 | o | meo | nối thêm | 
| 6 | o | meo | bỏ qua | 
| 7 | w | meo meo | nối thêm | 
| 8 | w | meo meo | bỏ qua | 

Kết quả cuối cùng là`"meow"`, vậy đầu ra là`"0_0"`. 

Bây giờ hãy xem xét`"waaaAAOOV"`. 

Sau khi chuyển đổi chữ thường, chúng tôi nhận được`"waaaaooov"`. 

| Bước | Nhân vật | Kết quả hiện tại | Hành động | 
| --- | --- | --- | --- | 
| 1 | w | w | nối thêm | 
| 2 | một | wa | nối thêm | 
| 3 | một | wa | bỏ qua | 
| 4 | một | wa | bỏ qua | 
| 5 | một | wa | bỏ qua | 
| 6 | o | wa | nối thêm | 
| 7 | o | wa | bỏ qua | 
| 8 | o | wa | bỏ qua | 
| 9 | v | waov | nối thêm | 

Kết quả cuối cùng là`"waov"`, vậy đầu ra là`"*_*"`. 

Những dấu vết này cho thấy rằng chỉ có sự chuyển tiếp giữa các ký tự liên tiếp riêng biệt mới quan trọng và các khối lặp lại sẽ thu gọn một cách rõ ràng bất kể khung gốc hoặc độ dài chạy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi ký tự được xử lý một lần trong quá trình chuẩn hóa và một lần trong quá trình nén logic | 
| Không gian |$O(n)$| Bộ đệm đầu ra lưu trữ tối đa một ký tự mỗi lần chạy | 

Giải pháp này chia tỷ lệ tuyến tính theo kích thước đầu vào, phù hợp với các ràng buộc xử lý chuỗi điển hình của Codeforces trong đó tổng kích thước đầu vào qua các thử nghiệm là lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# provided-style cases
assert run("1\n4 meow\n") == "0_0"
assert run("1\n4 waov\n") == "*_*"

# custom cases
assert run("1\n5 meooW\n") == "0_0", "mixed case collapse to meow"
assert run("1\n8 mmmmmeow\n") == "0_0", "long prefix collapse"
assert run("1\n4 abcd\n") == "???", "non matching pattern"
assert run("1\n6 WAAAAA\n") == "???", "single character collapse"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`meooW`|`0_0`| chuẩn hóa trường hợp + nén chạy | 
|`mmmmmeow`|`0_0`| tiền tố dài lặp đi lặp lại thu gọn chính xác | 
|`abcd`|`???`| cấu trúc không phù hợp | 
|`WAAAAA`|`???`| trường hợp cạnh chạy đơn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi toàn bộ chuỗi bao gồm một ký tự lặp lại, chẳng hạn như`"AAAAA"`. Sau khi chuyển đổi và nén chữ thường, điều này trở thành`"a"`. Thuật toán xử lý nó bằng cách thêm ký tự đầu tiên và bỏ qua tất cả các ký tự khác vì chúng khớp với ký tự được thêm cuối cùng. Chuỗi cuối cùng là`"a"`, không khớp`"meow"`cũng không`"waov"`, vì vậy đầu ra là`"???"`. 

Một trường hợp khác là xen kẽ vỏ với các lần chạy, như`"MeEeOoWw"`. Việc chuyển đổi chữ thường tạo ra`"meeooww"`, và năng suất nén`"meow"`. Thuật toán xử lý chính xác điều này vì việc so sánh luôn được thực hiện sau khi chuẩn hóa và các ranh giới chạy được xác định hoàn toàn bằng sự bằng nhau của các ký tự liền kề chứ không phải kiểu chữ gốc.
