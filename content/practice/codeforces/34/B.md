---
title: "CF 34B - Giảm giá"
description: "Bob đang trong buổi giảm giá bộ sưu tập TV cũ, mỗi chiếc có một mức giá. Một số TV miễn phí, một số có giá dương, thậm chí một số có giá âm, nghĩa là người chủ sẵn sàng trả tiền cho Bob để mang chúng đi."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 34
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 34 (Div. 2)"
rating: 900
weight: 34
solve_time_s: 79
verified: true
draft: false
---
[CF 34B - Giảm giá](https://codeforces.com/problemset/problem/34/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bob đang trong buổi giảm giá bộ sưu tập TV cũ, mỗi chiếc có một mức giá. Một số TV miễn phí, một số có giá dương, thậm chí một số có giá âm, nghĩa là người chủ sẵn sàng trả tiền cho Bob để mang chúng đi. Bob có thể mang tối đa _m_ TV và anh ấy muốn tối đa hóa tổng số tiền anh ấy có thể kiếm được. Đầu vào cung cấp số lượng TV _n_, giới hạn mang theo _m_ và dãy giá TV. Đầu ra là số tiền tối đa mà Bob có thể kiếm được bằng cách chọn tối đa _m_ TV. 

Các ràng buộc rất nhỏ: _n_ và _m_ nhiều nhất là 100 và giá TV nằm trong khoảng từ -1000 đến 1000. Điều này có nghĩa là chúng ta có thể sử dụng các thuật toán có độ phức tạp về thời gian lên đến khoảng O(n log n) hoặc thậm chí O(n²) mà không phải lo lắng về hiệu quả. 

Một điểm tế nhị là Bob chỉ được hưởng lợi từ những chiếc TV có giá âm. Việc chọn một chiếc TV có giá dương sẽ làm giảm tổng lợi nhuận của anh ta. Một cách triển khai ngây thơ tính tổng các TV rẻ nhất một cách mù quáng mà không kiểm tra dấu hiệu của chúng sẽ bao gồm các giá dương một cách không chính xác và làm giảm tổng số tiền. Ví dụ: nếu _n = 5_, _m = 3_ và giá là`-5 2 4 1 3`, câu trả lời đúng là`5`(chỉ chọn`-5`), không`2 + 4 + 1 = 7`điều mà một cách tiếp cận bất cẩn có thể xảy ra nếu nó chỉ sắp xếp tăng dần và tính tổng ba phần đầu tiên. 

Một trường hợp khó khăn khác là khi có ít hơn _m_ TV có giá âm. Trong trường hợp đó, Bob chỉ nên lấy giá âm sẵn có và bỏ qua phần còn lại, vì việc lấy TV miễn phí hoặc dương không làm tăng lợi nhuận của anh ấy. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ xem xét mọi tập hợp con TV có kích thước tối đa _m_ và tính tổng số tiền cho mỗi tập hợp con. Với mỗi sự kết hợp, Bob tính tổng các giá âm. Điều này đúng vì nó kiểm tra tất cả các khả năng, nhưng nó có độ phức tạp theo cấp số nhân: có các tập con O(2ⁿ). Với _n = 100_, điều này rõ ràng là không khả thi. 

Thông tin chi tiết quan trọng là những chiếc TV duy nhất đóng góp tích cực vào tổng doanh thu của Bob là những chiếc có giá âm. Vì vậy, chúng ta chỉ cần chọn giá âm nhỏ nhất, tối đa _m_ TV. Sắp xếp mảng theo thứ tự tăng dần đảm bảo rằng tất cả các giá âm đều được ưu tiên trước. Sau đó, chúng ta có thể lặp qua các phần tử _m_ đầu tiên, chỉ tính tổng các giá trị âm. Điều này làm giảm vấn đề từ việc xem xét tất cả các tập hợp con thành một sắp xếp đơn giản sau đó quét tuyến tính. 

Cách tiếp cận bạo lực có hiệu quả vì nó khám phá mọi khả năng, nhưng nó thất bại khi _n_ phát triển do sự kết hợp theo cấp số nhân. Nhận xét rằng chỉ có giá âm mới quan trọng cho phép chúng ta giảm vấn đề xuống việc sắp xếp và tính tổng một tiền tố nhỏ của mảng, đạt được giải pháp O(n log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(1) hoặc O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng tivi`n`và giới hạn mang theo`m`, cùng với dãy giá cả. 
2. Sắp xếp mảng giá theo thứ tự tăng dần. Điều này mang lại tất cả các mức giá tiêu cực ở phía trước. 
3. Khởi tạo một biến`total`về 0. Điều này sẽ tích lũy thu nhập của Bob. 
4. Lặp lại lần đầu tiên`m`các phần tử của mảng đã được sắp xếp. 
5. Đối với mỗi mức giá, nếu âm thì cộng giá trị tuyệt đối của nó (hoặc trừ đi một cách tương đương) thành`total`. Nếu nó bằng 0 hoặc dương, hãy dừng việc lặp lại vì các TV tiếp theo không làm tăng tổng số. 
6. Xuất kết quả tích lũy`total`. 

Điều này hoạt động vì sau khi sắp xếp, đầu tiên`m`các phần tử là nhỏ nhất và chỉ những số âm mới góp phần tích cực vào lợi ích của Bob. Điều bất biến là tại bất kỳ bước nào,`total`bằng tổng giá trị tuyệt đối của các mức giá âm mà Bob đã chọn cho đến nay. Không có số dương nào sẽ làm tăng tổng, vì vậy việc bỏ qua chúng là an toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
prices = list(map(int, input().split()))

prices.sort()
total = 0

for i in range(min(m, n)):
    if prices[i] < 0:
        total += -prices[i]
    else:
        break

print(total)
```Mã đầu tiên được đọc`n`Và`m`và bảng giá. Việc sắp xếp đảm bảo rằng giá âm ở phía trước. Vòng lặp lặp lại nhiều nhất`m`nhiều lần, dừng sớm nếu gặp giá không âm. Thêm`-prices[i]`chuyển đổi chi phí âm thành tiền dương kiếm được. các`min(m, n)`đảm bảo chúng tôi không bao giờ đi quá giới hạn nếu`m > n`, mặc dù vấn đề đảm bảo`m ≤ n`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`5 3`và giá cả`-6 0 35 -2 4`| Bước | Sắp xếp giá | tôi | Giá | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | -6 -2 0 4 35 | 0 | -6 | 6 | 
| 1 | | 1 | -2 | 8 | 
| 2 | | 2 | 0 | 8 | 

Bob chọn -6 và -2, thu được 6 + 2 = 8. Quá trình lặp dừng ở 0 vì không kiếm được tiền từ các giá không âm. 

### Ví dụ 2 

đầu vào:`4 2`và giá cả`1 -3 -2 0`| Bước | Sắp xếp giá | tôi | Giá | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | -3 -2 0 1 | 0 | -3 | 3 | 
| 1 | | 1 | -2 | 5 | 

Bob chọn -3 và -2, được 5. Đã đạt đến giới hạn hai TV. 

Những dấu vết này cho thấy thuật toán luôn chọn những mức giá âm nhất đến mức giới hạn thực hiện, tối đa hóa thu nhập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế trong thời gian chạy, việc lặp lại tối đa m phần tử là O(n) trong trường hợp xấu nhất | 
| Không gian | O(1) hoặc O(n) | Việc sắp xếp có thể yêu cầu không gian bổ sung O(n) tùy thuộc vào việc triển khai, nếu không, chúng tôi chỉ sử dụng một biến duy nhất`total`| 

Với n 100, các thao tác O(n log n) là khoảng 700 bước, thấp hơn nhiều so với giới hạn 2 giây. Việc sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    prices = list(map(int, input().split()))
    prices.sort()
    total = 0
    for i in range(min(m, n)):
        if prices[i] < 0:
            total += -prices[i]
        else:
            break
    return str(total)

# provided sample
assert run("5 3\n-6 0 35 -2 4\n") == "8", "sample 1"

# custom test cases
assert run("5 3\n-5 2 4 1 3\n") == "5", "only negative TV picked"
assert run("3 3\n1 2 3\n") == "0", "all positive, pick none"
assert run("4 5\n-1 -2 -3 -4\n") == "10", "m > number of TVs negative"
assert run("1 1\n-1000\n") == "1000", "single negative TV"
assert run("2 1\n0 -1\n") == "1", "zero and negative, pick negative"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 3, -5 2 4 1 3 | 5 | Chỉ truyền hình tiêu cực mới đóng góp | 
| 3 3, 1 2 3 | 0 | Tất cả đều tích cực, không kiếm được gì | 
| 4 5, -1 -2 -3 -4 | 10 | m lớn hơn số lượng TV âm | 
| 1 1, -1000 | 1000 | Tivi đơn giá âm lớn | 
| 2 1, 0 -1 | 1 | Không và âm, chọn âm | 

## Vỏ cạnh 

Nếu có ít giá âm hơn`m`, thuật toán vẫn hoạt động. Đối với đầu vào`4 5`và giá cả`-1 -2 -3 -4`, mảng được sắp xếp là`-4 -3 -2 -1`. Chúng tôi lặp lại tất cả bốn mức giá âm, tính tổng các giá trị tuyệt đối của chúng để có được`10`, và dừng lại vì không còn TV nữa. Mặc dù`m = 5`, chúng tôi không bao giờ truy cập phần tử ngoài giới hạn. Điều này xác nhận thuật toán xử lý trường hợp`m`vượt quá số lượng giá âm một cách chính xác. 

Khi tất cả các giá đều dương, chẳng hạn như`1 2 3`với`m = 3`, phần tử được sắp xếp đầu tiên là 1, không âm. Vòng lặp bị ngắt ngay lập tức và tổng số là`0`. Điều này cho thấy thuật toán tránh được việc chọn TV có thể làm giảm thu nhập của Bob một cách chính xác.
