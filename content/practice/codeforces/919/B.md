---
title: "CF 919B - Số hoàn hảo"
description: "Một số nguyên dương được gọi là số hoàn hảo khi tổng của tất cả các chữ số thập phân của nó chính xác là 10. Chúng ta được cho một số nguyên k và chúng ta phải xuất ra số nguyên dương nhỏ nhất thứ k có tổng các chữ số bằng 10. Thứ tự là thứ tự số thông thường."
date: "2026-06-12T09:48:09+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "dp", "implementation", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 919
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 460 (Div. 2)"
rating: 1100
weight: 919
solve_time_s: 94
verified: true
draft: false
---

[CF 919B - Số hoàn hảo](https://codeforces.com/problemset/problem/919/B) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, vũ lực, dp, thực hiện, lý thuyết số 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một số nguyên dương được gọi là _perfect_ khi tổng của tất cả các chữ số thập phân của nó chính xác là 10. 

Chúng ta được cho một số nguyên`k`, và chúng ta phải xuất ra`k`-số nguyên dương nhỏ nhất có tổng các chữ số bằng 10. Thứ tự là thứ tự số thông thường. Ví dụ: số hoàn hảo đầu tiên là`19`bởi vì`1 + 9 = 10`, thứ hai là`28`, vân vân. 

Hạn chế là rất nhỏ. Chúng ta chỉ cần cái đầu tiên`10,000`những con số hoàn hảo. Điều đó ngay lập tức cho thấy rằng việc tạo ra từng ứng viên một là khả thi. Thách thức không phải là xử lý các kích thước đầu vào khổng lồ mà là tìm ra một cách rõ ràng để liệt kê các số hoàn hảo theo thứ tự tăng dần. 

Một quan sát hữu ích là những số có tổng bằng 10 không phải là hiếm. Trong số vài trăm nghìn số nguyên đầu tiên đã có rất nhiều số nguyên như vậy. Từ`k ≤ 10000`, ngay cả một cuộc tìm kiếm đơn giản cũng sẽ kết thúc một cách thoải mái trong thời gian giới hạn. 

Có một vài trường hợp đáng để kiểm tra. 

Đối với đầu vào```
1
```câu trả lời là```
19
```Một giải pháp bất cẩn bắt đầu tìm kiếm từ`10`và quên kiểm tra mọi số có thể bỏ lỡ giá trị hợp lệ đầu tiên. 

Đối với đầu vào```
2
```câu trả lời là```
28
```Một giải pháp vô tình đếm các số có tổng chữ số _tối đa_ 10 thay vì _chính xác_ 10 sẽ bao gồm không chính xác các giá trị như`10`. 

Đối với các vị trí lớn hơn, việc đếm trùng lặp có thể trở thành vấn đề nếu logic tạo bị lỗi. Ví dụ, cả hai`109`Và`190`có tổng các chữ số là 10. Chúng ta phải đếm mọi số nguyên riêng biệt chính xác một lần và giữ nguyên thứ tự số. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là kiểm tra các số nguyên dương theo thứ tự tăng dần. Với mỗi số nguyên hãy tính tổng các chữ số của nó. Bất cứ khi nào tổng bằng 10, hãy tăng bộ đếm. Khi bộ đếm đạt tới`k`, xuất ra số hiện tại. 

Cách tiếp cận này đúng vì nó kiểm tra các số nguyên theo thứ tự tăng dần và đếm chính xác những số có tổng chữ số là 10. Thời điểm bộ đếm trở thành`k`, chúng tôi đã tìm thấy`k`-số hoàn hảo nhỏ nhất thứ 

Câu hỏi còn lại là liệu nó có đủ nhanh hay không. Câu trả lời là có. Số hoàn hảo thứ 10.000 không lớn lắm và việc tính tổng các chữ số chỉ cần một số thao tác. Ngay cả việc kiểm tra vài trăm nghìn con số cũng là chuyện nhỏ trong giới hạn 2 giây. 

Ngoài ra còn có một quan sát nhỏ xuất hiện thường xuyên trong các lời giải được chấp nhận. Mọi số hoàn hảo đều kết thúc bằng một chữ số nào đó và thêm một dấu`0`không thay đổi tổng các chữ số. Người ta có thể rút ra một phương pháp tạo chuyên biệt hơn từ thực tế này. Tuy nhiên, các ràng buộc quá nhỏ nên giải pháp liệt kê đơn giản đã đầy đủ và dễ lý luận hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê Brute Force | O(N log N) | O(1) | Đã chấp nhận | 
| Quan sát thế hệ trực tiếp | O(k log N) | O(1) | Đã chấp nhận | 

Đây`N`biểu thị số lượng lớn nhất được kiểm tra trước khi tìm ra câu trả lời. 

## Hướng dẫn thuật toán 

1. Đọc`k`. 
2. Khởi tạo bộ đếm`found = 0`. 
3. Bắt đầu kiểm tra các số nguyên theo thứ tự tăng dần, bắt đầu từ`1`. 
4. Với số nguyên hiện tại, hãy tính tổng các chữ số thập phân của nó. 
5. Nếu tổng các chữ số bằng`10`, tăng`found`. 
6. Nếu`found == k`, xuất số nguyên hiện tại và dừng lại. 
7. Nếu không thì tiếp tục đến số nguyên tiếp theo. 

Ý tưởng chính là các con số được xử lý theo thứ tự tăng dần. Mỗi số hợp lệ được tính chính xác một lần, do đó`k`-lần thứ chúng ta gặp một số hợp lệ phải tương ứng với`k`-số hoàn hảo nhỏ nhất thứ 

### Tại sao nó hoạt động 

Ở mỗi bước,`found`bằng số lượng số hoàn hảo gặp trong số tất cả các số nguyên đã được kiểm tra. 

Bất biến này ban đầu đúng vì không có số nguyên nào được xử lý và`found = 0`. Bất cứ khi nào một số nguyên mới được kiểm tra, chúng tôi sẽ tăng`found`chính xác khi số nguyên đó là hoàn hảo. Do đó, bất biến vẫn đúng sau mỗi lần lặp. 

Vì các số nguyên được truy cập theo thứ tự tăng dần nên khi`found`đầu tiên trở thành`k`, chính xác`k`số hoàn hảo đã xuất hiện bằng và bao gồm số nguyên hiện tại. Do đó số nguyên hiện tại chính xác là`k`-số hoàn hảo nhỏ nhất thứ 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def digit_sum(x):
    s = 0
    while x:
        s += x % 10
        x //= 10
    return s

def solve():
    k = int(input())
    
    found = 0
    num = 1
    
    while True:
        if digit_sum(num) == 10:
            found += 1
            if found == k:
                print(num)
                return
        num += 1

solve()
```chức năng`digit_sum`tính tổng các chữ số thập phân bằng phép chia lặp lại cho 10. 

Vòng lặp chính quét các số nguyên theo thứ tự tăng dần. Mỗi khi một số có tổng chữ số là 10, bộ đếm sẽ tăng lên. Ngay khi quầy đến`k`, số hiện tại được in. 

Lỗi phổ biến nhất là sai sót một lỗi trong logic đếm. Việc so sánh phải diễn ra sau khi tăng bộ đếm, vì chính số hiện tại có thể là câu trả lời. 

Số nguyên Python không bị giới hạn, do đó không có vấn đề tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```| Số hiện tại | Tổng chữ số | Đã tìm thấy | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 2 | 0 | 
| ... | ... | ... | 
| 19 | 10 | 1 | 

Khi`19`được xử lý, tổng các chữ số bằng 10. Bộ đếm trở thành 1, khớp với`k`, vậy câu trả lời là`19`. 

### Ví dụ 2 

đầu vào:```
2
```| Số hiện tại | Tổng chữ số | Đã tìm thấy | 
| --- | --- | --- | 
| 19 | 10 | 1 | 
| 20 | 2 | 1 | 
| 21 | 3 | 1 | 
| ... | ... | ... | 
| 28 | 10 | 2 | 

Số hoàn hảo đầu tiên gặp là`19`. Thứ hai là`28`, do đó, thuật toán đầu ra`28`. 

Dấu vết này chứng tỏ rằng các số hợp lệ được tính theo thứ tự số, đây chính xác là thứ tự mà bài toán yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Mỗi số được kiểm tra yêu cầu tính tổng các chữ số của nó, lấy các chữ số O(log N) | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Vì`k ≤ 10000`, phạm vi tìm kiếm được yêu cầu đủ nhỏ để phép liệt kê đơn giản này dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ là không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    k = int(input())

    def digit_sum(x):
        s = 0
        while x:
            s += x % 10
            x //= 10
        return s

    found = 0
    num = 1

    while True:
        if digit_sum(num) == 10:
            found += 1
            if found == k:
                print(num)
                return
        num += 1

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("1\n") == "19", "sample 1"

# custom cases
assert run("2\n") == "28", "second perfect number"
assert run("3\n") == "37", "third perfect number"
assert run("10\n") == "109", "crosses into three digits"
assert run("20\n") == "208", "larger counting check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`19`| Vị trí hợp lệ tối thiểu | 
|`2`|`28`| Đếm liên tiếp | 
|`3`|`37`| Tính chính xác của trình tự sớm | 
|`10`|`109`| Chuyển từ hai chữ số sang ba chữ số | 
|`20`|`208`| Đếm và đặt hàng lớn hơn | 

## Vỏ cạnh 

Hãy xem xét truy vấn nhỏ nhất có thể:```
1
```Thuật toán kiểm tra các số nguyên theo thứ tự. Số đầu tiên có tổng các chữ số bằng 10 là`19`. Bộ đếm trở thành 1 và khớp`k`, do đó đầu ra là:```
19
```Điều này xác nhận rằng việc tìm kiếm bắt đầu đủ sớm và không bỏ qua câu trả lời hợp lệ đầu tiên. 

Coi như:```
2
```Các số hợp lệ gặp phải là`19`, sau đó`28`. Sau khi xử lý`19`, bộ đếm là 1. Sau khi xử lý`28`, bộ đếm trở thành 2 và thuật toán dừng lại. Đầu ra là:```
28
```Điều này xác minh rằng việc đếm dựa trên sự bằng nhau của tổng chữ số chính xác chứ không phải bất kỳ điều kiện yếu hơn nào. 

Coi như:```
10
```Mười số hoàn hảo đầu tiên là:`19, 28, 37, 46, 55, 64, 73, 82, 91, 109`Thuật toán đạt`109`là số hợp lệ thứ mười và kết quả đầu ra:```
109
```Bài tập này thực hiện quá trình chuyển đổi sang số có ba chữ số và xác nhận rằng tổng các chữ số được tính toán chính xác bất kể số chữ số là bao nhiêu.
