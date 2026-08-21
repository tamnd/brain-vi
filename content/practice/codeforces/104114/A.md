---
title: "CF 104114A - Nối thêmThêm vào"
description: "Chúng ta được cấp một chuỗi cơ sở s. Mỗi ngày, Momo không sửa đổi nội bộ mà thay vào đó xây dựng một chuỗi dài hơn bằng cách ghép các bản sao của s gốc. Sau ngày thứ nhất, chuỗi chính xác là s. Sau ngày thứ 2, nó trở thành s + s. Sau ngày k, nó sẽ được lặp lại k lần liên tiếp."
date: "2026-07-02T01:58:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104114
codeforces_index: "A"
codeforces_contest_name: "2022 ICPC Southeastern Europe Regional Contest"
rating: 0
weight: 104114
solve_time_s: 45
verified: true
draft: false
---

[CF 104114A - Nối thêmAppendAppend](https://codeforces.com/problemset/problem/104114/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi cơ sở`s`. Mỗi ngày, Momo không sửa đổi nội bộ mà thay vào đó xây dựng một chuỗi dài hơn bằng cách ghép các bản sao của bản gốc`s`. Sau ngày thứ nhất, chuỗi chính xác`s`. Sau ngày thứ 2, nó trở thành`s + s`. Sau ngày k, nó trở thành`s`lặp đi lặp lại k lần liên tiếp. 

Bobo có một chuỗi khác`t`. Câu hỏi đặt ra là xác định số ngày nhỏ nhất k sao cho`t`có thể được tìm thấy dưới dạng một chuỗi con của chuỗi được hình thành bằng cách lặp lại`s`chính xác là k lần. 

Một chuỗi con có nghĩa là chúng ta được phép xóa các ký tự khỏi chuỗi lặp lại mà không thay đổi thứ tự và chúng ta muốn có thể nhận được`t`. 

Khó khăn chính là chúng ta không được hỏi liệu`t`là một dãy con của một chuỗi cố định nhưng có số lần lặp lại nhỏ nhất`s`cần thiết để nó trở thành có thể. 

Các ràng buộc cho phép cả hai`s`Và`t`lên tới 500.000 ký tự. Điều này ngay lập tức loại trừ mọi cách tiếp cận xây dựng chuỗi lặp lại một cách rõ ràng. Ngay cả hai lần lặp lại của chuỗi kích thước tối đa cũng đã vượt quá giới hạn bộ nhớ và việc xây dựng câu trả lời là không thể. 

Chúng ta cũng cần phải cẩn thận về thực tế là`t`có thể yêu cầu quét nhiều lần`s`. Việc kiểm tra trình tự đơn giản mỗi ngày sẽ mô phỏng tối đa k lần vượt qua`s`, đưa ra hành vi bậc hai hoặc tệ hơn. 

Một vài tình huống khó khăn quan trọng. 

Nếu như`t`chứa một ký tự không có trong`s`, thì nói chung là không thể, nhưng câu lệnh đảm bảo có câu trả lời, vì vậy trường hợp này bị loại trừ. 

Nếu như`t`đã là một dãy con của`s`, câu trả lời là 1. 

Nếu ký tự của`t`được phân phối sao cho các lực phù hợp khởi động lại quá trình quét`s`nhiều khi, câu trả lời có thể lớn, nhưng chúng ta phải phát hiện điều này mà không cần xây dựng chuỗi lặp lại một cách rõ ràng. 

## Phương pháp tiếp cận 

Chiến lược bạo lực trực tiếp là mô phỏng từng ngày. Đối với ngày k, về mặt khái niệm, chúng tôi xây dựng`s`lặp lại k lần và kiểm tra xem`t`là một dãy con sử dụng phép quét hai con trỏ. Việc kiểm tra trình tự con này là tuyến tính theo độ dài của chuỗi được xây dựng, vì vậy O(k · |s| + |t|) mỗi ngày. 

Trong trường hợp xấu nhất, bản thân k có thể lớn bằng |t|, chẳng hạn khi mỗi ký tự của`t`chỉ khớp một ký tự trên mỗi bản sao của`s`. Điều này dẫn đến độ phức tạp hoàn toàn xung quanh O(|s| · |t|), quá chậm đối với giới hạn 5 · 10^5. 

Quan sát quan trọng là chúng ta không bao giờ cần xây dựng các chuỗi lặp lại một cách rõ ràng. Thay vào đó, chúng tôi mô phỏng chuỗi con khớp một cách tham lam trong khi duyệt qua`s`. 

Chúng tôi quét`t`từ trái sang phải và duy trì một con trỏ ở`s`. Khi chúng ta không thể khớp ký tự tiếp theo của`t`trong quá trình quét hiện tại của`s`, chúng tôi “quấn quanh” bản sao tiếp theo của`s`, tương ứng chính xác với việc di chuyển đến ranh giới lặp lại của ngày hôm sau. Mỗi lần chúng tôi khởi động lại quá trình quét`s`, chúng tôi tăng bộ đếm ngày. 

Điều này có hiệu quả vì việc so khớp chuỗi con chỉ phụ thuộc vào thứ tự tương đối và việc lặp lại`s`chỉ đơn giản cung cấp cho chúng tôi một cửa sổ quét giống hệt khác. 

Để thực hiện điều này hiệu quả, chúng tôi xử lý trước vị trí của từng ký tự trong`s`để chúng ta có thể chuyển sang lần xuất hiện hợp lệ tiếp theo trong O(log σ) hoặc O(1) bằng cách sử dụng mảng. Tuy nhiên, một thuật toán tham lam O(n + m) thậm chí còn đơn giản hơn bằng cách quét`s`liên tục bằng cách sử dụng thiết lập lại con trỏ. 

Quá trình này tương đương với việc duyệt đi duyệt lại nhiều lần`s`trong khi tiêu thụ`t`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (xây dựng chuỗi mỗi ngày) | O( | s | · | 
| Quét tham lam tối ưu theo chu kỳ`s`| O( | s | + | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng sự phù hợp`t`như một chuỗi con trên các bản sao lặp đi lặp lại của`s`mà không cần xây dựng chúng. 

1. Khởi tạo con trỏ`i = 0`cho chuỗi`t`và một quầy`days = 1`, thể hiện rằng chúng ta hiện đang ở trong bản sao đầu tiên của`s`. 

Chúng tôi cũng duy trì một con trỏ`j = 0`để quét`s`. 
2. Trong khi`i < len(t)`, cố gắng so khớp`t[i]`bằng cách quét`s`bắt đầu từ vị trí`j`. 
3. Di chuyển`j`chuyển tiếp qua`s`cho đến khi`s[j] == t[i]`hoặc chúng ta đi đến cuối`s`. 

Bước này thể hiện việc cố gắng tìm kết quả phù hợp tiếp theo trong lần lặp lại hiện tại. 
4. Nếu chúng tôi tìm thấy sự trùng khớp`s[j] == t[i]`, tiến cả hai con trỏ (`i += 1`,`j += 1`) và tiếp tục. 

Điều này bảo tồn thứ tự tiếp theo. 
5. Nếu chúng ta đến cuối`s`không tìm thấy sự phù hợp, tăng`days`, cài lại`j = 0`và tiếp tục quét`s`ngay từ đầu. 

Điều này tương ứng với việc chuyển sang bản sao lặp lại tiếp theo của`s`. 
6. Lặp lại cho đến khi tất cả các ký tự của`t`được khớp. 

Giá trị cuối cùng của`days`là số lần lặp lại tối thiểu cần thiết. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán mô phỏng một cách hiệu quả việc truyền tải qua một phép nối vô hạn của`s`. Mỗi lần chúng ta kiệt sức`s`không hoàn thiện`t`, chúng ta buộc phải chuyển sang khối giống hệt tiếp theo. Vì mọi khối đều giống hệt nhau nên trạng thái duy nhất quan trọng là vị trí hiện tại bên trong`s`và chỉ số hiện tại trong`t`. Quá trình quét tham lam đảm bảo rằng mỗi ký tự của`t`được khớp ở vị trí sớm nhất có thể trong bản sao hiện tại hoặc tiếp theo của`s`, vì vậy chúng tôi không bao giờ lạm dụng một khối một cách không cần thiết. Điều này đảm bảo số lần đặt lại tối thiểu, tương ứng chính xác với số ngày tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    t = input().strip()

    n = len(s)

    i = 0
    j = 0
    days = 1

    while i < len(t):
        if j == n:
            days += 1
            j = 0

        if s[j] == t[i]:
            i += 1
            j += 1
        else:
            j += 1

    print(days)

if __name__ == "__main__":
    solve()
```Mã duy trì hai con trỏ: một trên`t`và một trên bản sao hiện tại của`s`. Khi quét`s`kết thúc, chúng tôi tăng bộ đếm ngày và bắt đầu quét lại. Chi tiết triển khai chính là chúng tôi không bao giờ tạo các chuỗi lặp lại và con trỏ`j`hoạt động như một vị trí ảo bên trong sự lặp lại hiện tại. 

Một điểm tế nhị là chúng ta phải thiết lập lại`j`chính xác khi nó đạt tới`len(s)`, không phải khi kết quả không khớp, vì các kết quả không khớp vẫn sẽ tiếp tục quá trình quét trong cùng một lần lặp lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

hãy để`s = dwalkcake`,`t = cakewalk`. 

| bước | tôi | j | ngày | hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | quét cho đến khi khớp 'c' | 
| 2 | 0 | 5 | 1 | khớp 'c' | 
| 3 | 1 | 6 | 1 | khớp 'a' | 
| 4 | 2 | 7 | 1 | khớp 'k' | 
| 5 | 3 | 8 | 1 | khớp 'e' | 
| 6 | 4 | 9 | 1 | đã đến cuối, khởi động lại | 
| 7 | 4 | 0 | 2 | ngày mới bắt đầu | 
| 8 | 4 | 1 | 2 | khớp 'w' ... | 

Chu kỳ thứ hai của`s`là cần thiết để kết thúc việc khớp. Điều này chứng tỏ rằng việc tiêu thụ tham lam qua các bản sao là cần thiết. 

### Ví dụ 2 

hãy để`s = abc`,`t = acbac`. 

| bước | tôi | j | ngày | hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | khớp 'a' | 
| 2 | 1 | 1 | 1 | khớp 'c' | 
| 3 | 2 | 2 | 1 | khớp 'b' | 
| 4 | 3 | 3 | 2 | khởi động lại s | 
| 5 | 3 | 0 | 2 | khớp 'a' | 
| 6 | 4 | 1 | 2 | khớp 'c' | 

Chúng tôi thấy rằng sự lặp lại thứ hai là cần thiết bởi vì`t`buộc phải sử dụng lại cấu trúc trước đó sau khi dùng hết một lượt. 

Những dấu vết này cho thấy thuật toán luôn tiến hành một cách tham lam và chỉ tăng số ngày khi quét toàn bộ`s`được tiêu thụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | s | 
| Không gian | O(1) | Chỉ có một vài con trỏ số nguyên được lưu trữ, không cần cấu trúc phụ trợ tỷ lệ với kích thước đầu vào. | 

Hành vi tuyến tính dễ dàng phù hợp với các ràng buộc trong đó cả hai chuỗi có tối đa 5 · 10^5 ký tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf
    import sys

    input = sys.stdin.readline

    def solve():
        s = input().strip()
        t = input().strip()

        n = len(s)
        i = 0
        j = 0
        days = 1

        while i < len(t):
            if j == n:
                days += 1
                j = 0
            if s[j] == t[i]:
                i += 1
                j += 1
            else:
                j += 1

        return str(days)

    return solve()

# provided sample (as described)
assert run("dwalkcake\ncakewalk\n") == "2"

# custom cases
assert run("abc\nabc\n") == "1"
assert run("abc\ncba\n") == "3"
assert run("a\naaaaa\n") == "5"
assert run("ab\nbbbb\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`abc / abc`| 1 | đã có dãy con trong một bản | 
|`abc / cba`| 3 | buộc đặt lại nhiều lần | 
|`a / aaaaa`| 5 | sự lặp lại cực độ của ký tự đơn | 
|`ab / bbbb`| 4 | hành vi bỏ qua trường hợp xấu nhất | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi`t`yêu cầu quét toàn bộ nhiều lần`s`vì các kết quả trùng khớp chỉ xảy ra ở gần cuối mỗi bản sao. 

Ví dụ, nếu`s = abcd`Và`t = dddd`, mỗi bản sao của`s`chỉ đóng góp một trận đấu có thể sử dụng được ở ký tự cuối cùng. Thuật toán quét liên tục đến cuối, tăng dần`days`và tiếp tục, tạo ra số ngày chính xác bằng số lần xuất hiện. 

Một trường hợp cạnh khác là khi`t`rất ngắn so với`s`, chẳng hạn như`s = abcde...`Và`t = single character`. Thuật toán khớp ngay trong lần quét đầu tiên và không bao giờ tăng dần`days`, trả về chính xác 1. 

Cuối cùng, nếu`s`là một ký tự lặp đi lặp lại như`aaaaa`Và`t`cũng là tất cả`a`, thuật toán không bao giờ đặt lại cho đến khi cần thiết và số ngày bằng với độ dài của`t`, phù hợp với yêu cầu lặp lại tối thiểu.
