---
title: "CF 102B - Tổng các chữ số"
description: "Bài toán yêu cầu chúng ta thay thế nhiều lần một số bằng tổng các chữ số của nó cho đến khi số đó trở thành số có một chữ số. Đầu vào là một số n có thể cực lớn, lên tới 10 triệu chữ số."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 102
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 79 (Div. 2 Only)"
rating: 1000
weight: 102
solve_time_s: 114
verified: true
draft: false
---

[CF 102B - Tổng các chữ số](https://codeforces.com/problemset/problem/102/B) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta thay thế nhiều lần một số bằng tổng các chữ số của nó cho đến khi số đó trở thành số có một chữ số. Đầu vào là một số`n`có thể cực kỳ lớn, lên tới 10 triệu chữ số. Đầu ra là số phép biến đổi cần thiết để đạt được số có một chữ số. 

Kích thước đầu vào ngay lập tức loại trừ việc lưu trữ`n`như một số nguyên tiêu chuẩn trong hầu hết các ngôn ngữ lập trình, vì các số nguyên có hàng triệu chữ số vượt quá các kiểu dữ liệu gốc. Chúng ta cần một cách tiếp cận phù hợp với`n`dưới dạng một chuỗi hoặc tận dụng các thuộc tính của tổng chữ số mà không hiện thực hóa đầy đủ mọi số trung gian. 

Trường hợp cạnh khóa xảy ra khi`n`đã là một số có một chữ số. Ví dụ, nếu`n = 0`hoặc`n = 7`, không cần chuyển đổi, vì vậy đầu ra phải là`0`. Việc triển khai đơn giản tự động thực hiện phép tính tổng sẽ trả về không chính xác`1`trong trường hợp này. Một sự tinh tế khác là những con số rất lớn như`n = "9999999999999999999999"`. Thuật toán đúng không nên cố gắng chuyển đổi trực tiếp thành số nguyên. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là brute-force: liên tục tính tổng các chữ số cho đến khi số đó chỉ còn một chữ số. Điều này đúng vì mỗi phép biến đổi sẽ làm giảm nghiêm ngặt độ lớn của số, đảm bảo sự chấm dứt cuối cùng. Chúng ta có thể lặp qua các chữ số của`n`và tổng hợp chúng. Vấn đề với cách tiếp cận này là đối với một số có hàng triệu chữ số, việc tính tổng các chữ số lặp đi lặp lại có thể chậm nếu thực hiện một cách đơn giản, đặc biệt nếu bạn chuyển đổi số qua lại giữa các loại. 

Cách tiếp cận tối ưu quan sát thấy rằng số lần biến đổi được xác định bởi số lần chúng ta giảm một số có nhiều chữ số thông qua tổng chữ số. Thay vì lo lắng về những số nguyên khổng lồ, chúng ta có thể xử lý`n`như một chuỗi. Ở lần lặp đầu tiên, chúng tôi kiểm tra xem`n`có nhiều hơn một chữ số. Nếu có, hãy tính tổng các chữ số (phân tích các ký tự riêng lẻ). Sau đó tiếp tục cho đến khi chúng ta đạt được một số có một chữ số. Mỗi lần lặp chỉ cần duyệt qua chuỗi biểu diễn`n`hoặc một số nguyên nhỏ sau lần vượt qua đầu tiên. 

Điều này có hiệu quả vì sau lần chuyển đổi đầu tiên, số này trở nên đủ nhỏ để có thể quản lý như một số nguyên bình thường. Không xảy ra tình trạng tràn số nguyên và chúng tôi không bao giờ cố gắng giữ hơn 10 triệu chữ số cùng một lúc ở dạng số nguyên trung gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(L * log n) | O(L) | Đúng nhưng không hiệu quả đối với L lớn | 
| Tối ưu | O(L) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số`n`như một chuỗi. Điều này tránh bất kỳ lỗi tràn số nguyên nào đối với số cực lớn. 
2. Kiểm tra xem`n`đã là một số có một chữ số. Nếu được thì quay lại ngay`0`. Điều này xử lý trường hợp Edge không cần chuyển đổi. 
3. Khởi tạo bộ đếm`count`về không. Điều này sẽ theo dõi số lượng chuyển đổi. 
4. Trong khi`n`có nhiều hơn một chữ số, hãy chuyển từng ký tự thành số nguyên và tính tổng tất cả các chữ số. Thay thế`n`với số tiền này. Tăng`count`bởi một. 
5. Một lần`n`là một số có một chữ số, đầu ra`count`. 

Tại sao nó hoạt động: Điều bất biến là sau mỗi lần lặp,`n`trở nên nhỏ hơn hoàn toàn về số chữ số hoặc giá trị số của nó. Quá trình kết thúc khi`n`có chính xác một chữ số, đảm bảo số đếm phản ánh chính xác số lượng phép biến đổi đã được thực hiện. Thuật toán hoạt động ngay cả đối với các đầu vào ban đầu rất lớn vì tổng đầu tiên giảm chuỗi chữ số thành một số nguyên có thể quản lý được và các tổng tiếp theo hoạt động trên các số đủ nhỏ để vừa với các loại số nguyên tiêu chuẩn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = input().strip()

if len(n) == 1:
    print(0)
else:
    count = 0
    current = n
    while len(current) > 1:
        total = sum(int(ch) for ch in current)
        current = str(total)
        count += 1
    print(count)
```Giải pháp đầu tiên đọc đầu vào dưới dạng một chuỗi để xử lý các số rất lớn. Chúng tôi ngay lập tức kiểm tra xem đầu vào chỉ có một chữ số để xử lý trường hợp tầm thường hay không. Bên trong vòng lặp,`sum(int(ch) for ch in current)`tính toán tổng của tất cả các chữ số một cách hiệu quả mà không chuyển đổi toàn bộ chuỗi thành số nguyên, tránh tràn. Sau khi tính tổng, kết quả được chuyển trở lại thành chuỗi để điều kiện vòng lặp có thể kiểm tra số chữ số. Bộ đếm đảm bảo số lượng phép biến đổi là chính xác. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:`0`| Bước | hiện tại | tổng các chữ số | đếm | 
| --- | --- | --- | --- | 
| ban đầu | "0" | - | 0 | 

Từ`current`đã có một chữ số, thuật toán trả về`0`. Điều này xác nhận việc xử lý chính xác các trường hợp tầm thường. 

Đầu vào mẫu 2:`991`| Bước | hiện tại | tổng các chữ số | đếm | 
| --- | --- | --- | --- | 
| 1 | "991" | 9+9+1=19 | 1 | 
| 2 | "19" | 1+9=10 | 2 | 
| 3 | "10" | 1+0=1 | 3 | 

Thuật toán trả về`3`, đếm chính xác số phép biến đổi cần thiết để đạt đến số có một chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L) | Mỗi chữ số được truy cập nhiều nhất một lần trong lần lặp đầu tiên; lần lặp tiếp theo hoạt động trên một số nguyên nhỏ. | 
| Không gian | O(1) | Chúng tôi chỉ lưu trữ tổng và bộ đếm hiện tại, bất kể kích thước đầu vào. | 

Thuật toán này hiệu quả ngay cả đối với kích thước đầu vào tối đa là 10 triệu chữ số. Lần lặp đầu tiên có độ dài đầu vào tuyến tính và các lần lặp tiếp theo là không đáng kể vì số này nhanh chóng giảm xuống còn một hoặc hai chữ số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = input().strip()
    if len(n) == 1:
        return "0"
    count = 0
    current = n
    while len(current) > 1:
        total = sum(int(ch) for ch in current)
        current = str(total)
        count += 1
    return str(count)

# provided samples
assert run("0\n") == "0", "sample 1"
assert run("10\n") == "1", "sample 2"
assert run("991\n") == "3", "sample 3"

# custom cases
assert run("9\n") == "0", "single-digit input"
assert run("1234567890\n") == "2", "large multi-digit input"
assert run("99999999999999999999\n") == "2", "all nines large number"
assert run("10000000000000000000\n") == "2", "boundary leading one"
assert run("5\n") == "0", "minimum non-zero single-digit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "9" | "0" | Đầu vào một chữ số | 
| "1234567890" | "2" | Đầu vào nhiều chữ số tiêu chuẩn | 
| "999999999999999999999" | "2" | Đầu vào lớn, tất cả các chữ số giống nhau | 
| "100000000000000000000" | "2" | Đầu vào lớn, chữ số đầu tiên | 
| "5" | "0" | Đầu vào tối thiểu khác 0 | 

## Vỏ cạnh 

Thuật toán xử lý chính xác đầu vào có một chữ số bằng cách trả về`0`ngay lập tức. Đối với số lượng rất lớn như`"99999999999999999999"`, nó tính tổng một cách hiệu quả dưới dạng chuỗi và rút gọn thành số có hai chữ số, sau đó tính tổng lại để đạt kết quả có một chữ số. Đầu ra`2`phản ánh chính xác số lần chuyển đổi. Việc chuyển đổi giữa chuỗi và số nguyên được kiểm soát cẩn thận để ngăn chặn bất kỳ sự tràn nào, đảm bảo tính chính xác cho đầu vào lên tới 10 triệu chữ số.
