---
title: "CF 23A - Bạn được trao một chuỗi..."
description: "Chúng ta được cấp một chuỗi các chữ cái viết thường và nhiệm vụ là tìm độ dài của chuỗi con dài nhất xuất hiện ít nhất hai lần trong chuỗi. Các lần xuất hiện lặp đi lặp lại có thể chồng chéo lên nhau. Đầu vào là một chuỗi có tối đa 100 ký tự."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 23
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 23"
rating: 1200
weight: 23
solve_time_s: 115
verified: true
draft: false
---
[CF 23A - Bạn được cấp một chuỗi...](https://codeforces.com/problemset/problem/23/A) 

**Đánh giá:** 1200 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các chữ cái viết thường và nhiệm vụ là tìm độ dài của chuỗi con dài nhất xuất hiện ít nhất hai lần trong chuỗi. Các lần xuất hiện lặp đi lặp lại có thể chồng chéo lên nhau. Đầu vào là một chuỗi có tối đa 100 ký tự. Đầu ra là một số nguyên biểu thị độ dài tối đa của chuỗi con lặp lại như vậy. 

Vì độ dài chuỗi nhỏ, lên tới 100, nên chúng ta có thể sử dụng các thuật toán bậc hai về độ dài của chuỗi. Điều này làm cho các phương pháp so sánh tất cả các cặp chuỗi con trở nên khả thi. Các trường hợp đặc biệt của vấn đề là các tình huống không có chuỗi con lặp lại, trong đó tất cả các ký tự đều giống nhau hoặc trong đó các chuỗi con trùng nhau hoàn toàn. Ví dụ, chuỗi`aaaa`có chuỗi con lặp lại dài nhất có độ dài 3 (`aaa`), có thể xảy ra sự chồng chéo: vị trí 0-2 và 1-3. Một cách tiếp cận ngây thơ có thể không xử lý được sự chồng chéo này một cách chính xác nếu nó chỉ xem xét các vị trí không chồng chéo. 

Một trường hợp tinh tế khác là một chuỗi có các chữ cái hoàn toàn độc đáo, chẳng hạn như`abcd`. Ở đây, câu trả lời đúng là`0`bởi vì không có chuỗi con nào lặp lại, thậm chí không có chữ cái đơn lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là vũ lực. Chúng ta có thể tạo ra tất cả các chuỗi con có thể có của đầu vào và đếm số lần mỗi chuỗi xuất hiện. Đối với mỗi chuỗi con, chúng tôi trượt một cửa sổ qua chuỗi và so sánh các chuỗi con có cùng độ dài. Nếu chúng tôi tìm thấy một chuỗi con xuất hiện ít nhất hai lần, chúng tôi sẽ ghi lại độ dài của nó và cập nhật mức tối đa. Phương pháp này đúng vì nó kiểm tra rõ ràng mọi chuỗi con ứng cử viên, nhưng độ phức tạp của nó là O(n³) trong trường hợp xấu nhất: O(n²) để tạo tất cả các chuỗi con và O(n) để kiểm tra số lần xuất hiện. Với n = 100, đây là khoảng 1.000.000 thao tác, có thể chấp nhận được với tốc độ CPU hiện đại, nhưng nó chậm hơn và có thể được tối ưu hóa. 

Điểm mấu chốt là chúng ta không cần phải kiểm tra từng chuỗi con riêng lẻ. Thay vào đó, chúng ta có thể lặp lại các độ dài có thể có theo thứ tự giảm dần và kiểm tra xem có chuỗi con nào có độ dài đó lặp lại hay không. Khi chúng tôi tìm thấy độ dài phù hợp, chúng tôi có thể dừng lại. Việc kiểm tra tất cả các chuỗi con có độ dài cố định có thể được thực hiện trong thời gian O(n²) bằng cách sử dụng một bộ hoặc từ điển để theo dõi các chuỗi con đã nhìn thấy. Điều này làm giảm độ phức tạp tổng thể xuống O(n³) trong trường hợp xấu nhất, nhưng trên thực tế, nó nhanh hơn vì chúng ta dừng sớm khi tìm thấy chuỗi con lặp lại dài nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Được chấp nhận với n ≤ 100 | 
| Kiểm tra tập hợp dựa trên độ dài | O(n³) trong trường hợp xấu nhất, trung bình nhanh hơn | O(n²) | Được chấp nhận, thực hiện đơn giản hơn | 

## Hướng dẫn thuật toán 

1. Xác định độ dài của chuỗi đầu vào`n`. Khởi tạo một biến`max_len`về 0. Điều này sẽ lưu trữ độ dài chuỗi con lặp lại dài nhất được tìm thấy cho đến nay. 
2. Lặp lại độ dài chuỗi con ứng viên`l`từ 1 đến`n-1`. Chúng ta không cần kiểm tra độ dài`n`bởi vì một chuỗi con có độ dài`n`chỉ có thể xuất hiện một lần. 
3. Đối với mỗi chiều dài`l`, khởi tạo một tập hợp trống`seen`. Điều này sẽ theo dõi tất cả các chuỗi con có độ dài`l`chúng tôi gặp phải. 
4. Trượt một cửa sổ có chiều dài`l`trên chuỗi từ chỉ số 0 đến`n-l`. Trích xuất chuỗi con tại mỗi vị trí. 
5. Nếu chuỗi con đã có sẵn`seen`, cập nhật`max_len`ĐẾN`l`nếu như`l`lớn hơn hiện tại`max_len`. Thoát khỏi vòng lặp bên trong này vì chúng ta đã tìm thấy chuỗi con lặp lại có độ dài này. 
6. Nếu chuỗi con không có trong`seen`, thêm nó vào bộ và tiếp tục. 
7. Sau khi kiểm tra tất cả các độ dài, xuất ra`max_len`. 

Tại sao nó hoạt động: Thuật toán duy trì một bất biến`max_len`là độ dài lớn nhất mà chuỗi con lặp lại được tìm thấy. Bằng cách kiểm tra các chuỗi con ngắn hơn trước, chúng tôi đảm bảo không bỏ sót chuỗi con dài hơn hợp lệ nào. Việc sử dụng một tập hợp đảm bảo rằng các chuỗi con trùng lặp được phát hiện một cách hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = len(s)
max_len = 0

for l in range(1, n):
    seen = set()
    for i in range(n - l + 1):
        sub = s[i:i+l]
        if sub in seen:
            max_len = l
            break
        seen.add(sub)

print(max_len)
```Giải pháp đọc chuỗi và lặp lại theo độ dài chuỗi con có thể. Vòng lặp bên trong xây dựng từng chuỗi con bằng cách cắt. Việc sử dụng một bộ đảm bảo các bản sao được phát hiện trong thời gian trung bình O(1) cho mỗi lần kiểm tra. Việc ngắt vòng lặp bên trong khi tìm thấy chuỗi con lặp lại sẽ tránh được những công việc không cần thiết. 

## Ví dụ đã hoạt động 

cho`abcd`: 

| tôi | Đã kiểm tra chuỗi con | Đã xem bộ | max_len | 
| --- | --- | --- | --- | 
| 1 | 'a','b','c','d' | {'a','b','c','d'} | 0 | 
| 2 | 'ab','bc','cd' | {'ab','bc','cd'} | 0 | 
| 3 | 'abc','bcd' | {'abc','bcd'} | 0 | 

Không có chuỗi con lặp lại, vì vậy đầu ra là`0`. 

Vì`aaaa`: 

| tôi | Đã kiểm tra chuỗi con | Đã xem bộ | max_len | 
| --- | --- | --- | --- | 
| 1 | 'a','a' | {'a'} | 1 | 
| 2 | 'aa','aa' | {'aa'} | 2 | 
| 3 | 'aaa','aaa' | {'aaa'} | 3 | 

Ở đây chuỗi con lặp lại dài nhất là`aaa`với sự xuất hiện chồng chéo, đưa ra`max_len = 3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n³) trường hợp xấu nhất | Vòng lặp bên ngoài theo độ dài O(n), vòng lặp bên trong trên các vị trí O(n), cắt chuỗi con O(n) | 
| Không gian | O(n²) | Đặt lưu trữ tối đa n chuỗi con có độ dài n trong trường hợp xấu nhất | 

Cho n ≤ 100, giải pháp thực hiện tối đa 1.000.000 lần kiểm tra chuỗi con và vừa vặn trong giới hạn 2 giây. Việc sử dụng bộ nhớ vẫn ở mức tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    n = len(s)
    max_len = 0
    for l in range(1, n):
        seen = set()
        for i in range(n - l + 1):
            sub = s[i:i+l]
            if sub in seen:
                max_len = l
                break
            seen.add(sub)
    return str(max_len)

# provided samples
assert run("abcd\n") == "0", "sample 1"
assert run("aaaa\n") == "3", "overlapping repeats"

# custom cases
assert run("abcabc\n") == "3", "entire repeated sequence"
assert run("a\n") == "0", "single character string"
assert run("ababab\n") == "4", "overlapping pattern abab"
assert run("abcdefg\n") == "0", "all unique letters"
assert run("aaaaa\n") == "4", "all same letters, max overlap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| abcabc | 3 | chuỗi con lặp lại bằng một nửa chuỗi | 
| một | 0 | kích thước đầu vào tối thiểu | 
| ababab | 4 | lặp lại chồng chéo | 
| abcdefg | 0 | không lặp lại | 
| aaa | 4 | chồng chéo tối đa | 

## Vỏ cạnh 

Một chuỗi ký tự đơn như`a`sản xuất`0`vì không có chuỗi con nào có thể lặp lại. Thuật toán lặp lại chính xác theo độ dài bắt đầu từ 1 và không tìm thấy chuỗi con lặp lại. 

Đối với một chuỗi như`aaaaa`, thuật toán tìm các đoạn lặp chồng chéo bằng cách kiểm tra các chuỗi con có độ dài tăng dần. Nó phát hiện chính xác`aaaa`là chuỗi con lặp lại dài nhất vì`aaaa`xảy ra ở vị trí 0-3 và 1-4. Bộ này đảm bảo các bản sao được phát hiện và việc ngắt vòng lặp sau khi tìm thấy sự lặp lại sẽ tránh thiếu các chuỗi con dài hơn.
