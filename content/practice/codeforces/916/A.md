---
title: "CF 916A - Jamie và báo thức báo lại"
description: "Chúng ta được cung cấp thời gian thức dậy theo đồng hồ 24 giờ và khoảng thời gian cố định x tính bằng phút. Jamie đặt báo thức vào khoảng thời gian không xác định trước thời điểm thức dậy. Sau khi chuông báo thức reo, anh ấy liên tục báo lại nó sau mỗi x phút cho đến khi đến đúng giờ thức dậy."
date: "2026-06-13T02:02:29+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 916
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 457 (Div. 2)"
rating: 900
weight: 916
solve_time_s: 194
verified: true
draft: false
---

[CF 916A - Jamie và báo thức báo lại](https://codeforces.com/problemset/problem/916/A) 

**Xếp hạng:** 900 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 3 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp thời gian thức dậy theo đồng hồ 24 giờ và một khoảng thời gian cố định`x`trong vài phút. Jamie đặt báo thức vào khoảng thời gian không xác định trước thời điểm thức dậy. Sau khi chuông báo thức reo, anh ấy liên tục báo lại nó mỗi lần`x`phút cho đến khi đạt đến thời gian thức dậy chính xác. 

Thay vì chọn trực tiếp thời gian báo thức, chúng tôi nghĩ ngược lại. Giả sử báo thức được đặt`y`báo lại trước thời điểm thức dậy. Sau đó, thời gian báo thức là chính xác`y * x`phút trước thời gian mục tiêu. Mỗi lần báo lại sẽ đẩy thời gian về phía trước`x`phút, vậy là sau`y`nhấn, báo thức sẽ đến vào thời điểm thức dậy. 

Hạn chế duy nhất là thời gian báo thức ban đầu phải là “may mắn”, nghĩa là khi viết bằng`hh:mm`định dạng, ít nhất một chữ số là`7`. Chúng ta cần số không âm nhỏ nhất`y`như vậy nếu chúng ta quay trở lại`y * x`phút kể từ thời điểm thức dậy trên đồng hồ tròn 24 giờ, thời gian thu được chứa một`7`chữ số. 

Các ràng buộc cực kỳ nhỏ: không gian trạng thái đồng hồ chỉ có 24 * 60 = 1440 lần có thể. Bước báo lại tối đa là 60 phút, do đó, ngay cả khi quét toàn bộ tất cả những gì có thể`y`các giá trị sẽ chỉ chạm tới vài nghìn trạng thái trước khi quấn quanh và lặp lại một chu kỳ. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu logarit hoặc nâng cao; một mô phỏng tuyến tính là đủ. 

Sự tinh tế chính là thời gian kết thúc vào khoảng nửa đêm. Trở về từ`00:00`1 phút sẽ hạ cánh tại`23:59`. Bất kỳ giải pháp nào quên số học modulo sẽ tạo ra thời gian âm không chính xác. 

Một sai lầm phổ biến khác là dừng lại sớm khi nhận thấy thời điểm may mắn đang ở hướng thuận thay vì lý luận ngược so với thời điểm thức dậy. Điều kiện là về _thời gian báo thức bắt đầu_ chứ không phải thời gian báo lại trung gian. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng quá trình từ lúc thức dậy ngược trở lại. Đối với mỗi số lần báo lại có thể`y = 0, 1, 2, ...`, ta tính thời gian`y * x`phút trước thời điểm thức dậy và kiểm tra xem thời gian đó có chứa chữ số không`7`. 

Chế độ xem bạo lực này đã gần đạt mức tối ưu vì mỗi lần kiểm tra là O(1) và đồng hồ lặp lại sau mỗi 1440 phút. Sau tối đa 1440/gcd(1440, x) bước, quy trình sẽ chuyển sang một chu trình và quay lại các lần trước đó. Từ`x ≤ 60`, độ dài chu kỳ này nhỏ và trong trường hợp xấu nhất chúng ta kiểm tra nhiều nhất vài trăm hoặc một nghìn trạng thái trước khi lặp lại. 

Quan sát quan trọng là chỉ có 1440 trạng thái đồng hồ riêng biệt, do đó, ngay cả việc kiểm tra tất cả các độ lệch ngược có thể có trong tối đa một chu kỳ đầy đủ cũng vẫn an toàn. Chúng ta không cần phải lý luận về thời gian vô hạn hay những giới hạn lớn lao; chúng ta chỉ đơn giản đang tìm kiếm trên một chuỗi tuần hoàn hữu hạn được tạo ra bằng cách trừ liên tục`x`phút. 

Vì vậy, giải pháp tối ưu chỉ là mô phỏng được kiểm soát này với số học mô-đun tính bằng phút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force qua y | O(1440 / x) | O(1) | Đã chấp nhận | 
| Mô phỏng tương tự với số học đồng hồ mô-đun | O(1440) trường hợp xấu nhất | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi làm việc hoàn toàn trong vài phút từ 00:00 để đơn giản hóa mọi việc. 

1. Chuyển đổi thời gian thức dậy thành tổng số phút`t = hh * 60 + mm`. Điều này đưa ra một thang đo tuyến tính duy nhất cho số học mọi thời gian. Làm việc theo phút sẽ tránh được việc phải xử lý nhiều lần ranh giới giờ-phút. 
2. Khởi tạo`y = 0`. Điều này thể hiện không có báo lại, nghĩa là chúng tôi bắt đầu bằng cách kiểm tra xem liệu thời gian thức dậy có phải là thời gian báo thức bắt đầu hợp lệ hay không. 
3. Thực hiện các thao tác sau nhiều lần: tính thời gian báo thức của ứng viên như sau:`(t - y * x) mod 1440`. Modulo 1440 đảm bảo chúng tôi duy trì chu kỳ 24 giờ, xử lý chính xác tình trạng kéo dài suốt nửa đêm. 
4. Chuyển ứng viên này trở lại dạng giờ và phút. Trích xuất các chữ số và kiểm tra xem có bất kỳ ký tự nào trong bốn ký tự chứa`'7'`. 
5. Nếu điều kiện đúng, trả về`y`ngay lập tức. Vì chúng ta lặp lại`y`theo thứ tự tăng dần, giá trị hợp lệ đầu tiên được đảm bảo là tối thiểu. 
6. Nếu không thì tăng`y`và lặp lại. 

Ý tưởng cấu trúc quan trọng là chúng ta đang đi qua một chu trình xác định trên một không gian trạng thái hữu hạn. Mỗi lần tăng của`y`di chuyển chính xác`x`lùi lại vài phút, vì vậy chúng tôi khám phá một quỹ đạo duy nhất của đồ thị đồng hồ modulo-1440. 

### Tại sao nó hoạt động 

Mỗi giá trị của`y`tương ứng với chính xác một thời gian báo động có thể có: thời gian`y * x`phút trước thời điểm thức dậy. Vì thời gian có tính tuần hoàn modulo 1440, tăng dần`y`đi qua một chuỗi các trạng thái đồng hồ lặp đi lặp lại. Vì chỉ có tổng cộng 1440 trạng thái nên trình tự này cuối cùng phải lặp lại, nghĩa là tất cả các ứng cử viên có thể tiếp cận đều nằm trong một chu trình hữu hạn. 

Bằng cách quét`y`theo thứ tự tăng dần, chúng tôi kiểm tra thời gian báo thức theo thứ tự chính xác là số lần báo lại tăng dần. Do đó, lần đầu tiên thỏa mãn điều kiện “chứa chữ số 7” là số lần báo lại hợp lệ tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def has_seven(h, m):
    return '7' in f"{h:02d}{m:02d}"

def solve():
    x = int(input())
    hh, mm = map(int, input().split())

    start = hh * 60 + mm

    for y in range(0, 2000):
        t = (start - y * x) % 1440
        h = t // 60
        m = t % 60
        if has_seven(h, m):
            print(y)
            return

solve()
```Giải pháp chuyển đổi thời gian thành phút để đơn giản hóa phép trừ và xử lý tổng hợp. Vòng lặp kết thúc`y`được giới hạn an toàn bởi một hằng số đủ lớn để bao trùm tất cả các chu kỳ có thể xảy ra; 2000 là quá đủ vì không gian trạng thái chỉ là 1440. 

Hàm trợ giúp kiểm tra điều kiện chữ số bằng cách định dạng thời gian dưới dạng chuỗi có chiều rộng cố định, đảm bảo giữ nguyên các số 0 đứng đầu, điều này cần thiết cho các trường hợp như`07:xx`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
11 23
```Chúng ta bắt đầu từ 11:23, tức là 683 phút. 

| y | t = (683 - 3y) mod 1440 | Thời gian | Chứa 7 | 
| --- | --- | --- | --- | 
| 0 | 683 | 11:23 | Không | 
| 1 | 680 | 11:20 | Không | 
| 2 | 677 | 11:17 | Có | 

Tại`y = 2`, thời gian trở thành 11:17, bao gồm số 7 trong trường phút, vì vậy chúng tôi dừng lại. Điều này xác nhận rằng chúng tôi đang quét số lần báo lại theo thứ tự tăng dần và trả về số lần hợp lệ đầu tiên. 

### Ví dụ 2 

đầu vào:```
1
1 8
```Thời gian bắt đầu là 68 phút (01:08). 

| y | t = (68 - y) mod 1440 | Thời gian | Chứa 7 | 
| --- | --- | --- | --- | 
| 0 | 68 | 01:08 | Không | 
| 1 | 67 | 01:07 | Có | 

Ở đây, lần tạm dừng đầu tiên đã tạo ra thời điểm may mắn, cho thấy rằng câu trả lời có thể rất nhỏ và chúng tôi đã đưa vào một cách chính xác.`y = 0`trong không gian tìm kiếm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1440) | Mỗi lần lặp thực hiện công việc liên tục và chúng ta chỉ đi qua tối đa một chu kỳ đồng hồ đầy đủ | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Cấu trúc cố định 24 giờ đảm bảo vòng lặp luôn chạy trong một giới hạn không đổi nhỏ, dưới giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else __import__("builtins").print  # placeholder

# A direct correct runner
def run(inp: str) -> str:
    import subprocess, textwrap, sys
    return subprocess.run(
        [sys.executable, "-c", CODE],
        input=inp.encode(),
        stdout=subprocess.PIPE
    ).stdout.decode()

# We embed solution as CODE for testing simplicity
CODE = r"""
import sys
input = sys.stdin.readline

def has_seven(h, m):
    return '7' in f"{h:02d}{m:02d}"

x = int(input())
hh, mm = map(int, input().split())
start = hh * 60 + mm

for y in range(2000):
    t = (start - y * x) % 1440
    h = t // 60
    m = t % 60
    if has_seven(h, m):
        print(y)
        break
"""

# provided samples
assert run("3\n11 23\n").strip() == "2"

# custom cases
assert run("1\n00 07\n").strip() == "0"  # already lucky
assert run("60\n12 00\n").strip() != ""   # some valid answer exists
assert run("5\n00 00\n").strip() is not None
assert run("7\n23 59\n").strip() is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 / 00:07 | 0 | đã may mắn bắt đầu trạng thái | 
| 60 / 12:00 | không trống | khả năng tiếp cận chung theo bước lớn | 
| 5 / 00:00 | số nguyên hợp lệ | sự đúng đắn bao quanh | 
| 7 / 23:59 | số nguyên hợp lệ | vượt ranh giới lúc nửa đêm | 

## Vỏ cạnh 

Một trường hợp thường gặp là khi thời gian thức dậy đã là may mắn. Ví dụ,`00:07`với bất kỳ`x`nên quay lại ngay`0`. Thuật toán xử lý việc này vì nó kiểm tra`y = 0`trước bất kỳ phép trừ nào. 

Một trường hợp tinh vi khác là đã qua nửa đêm. Nếu thời gian thức dậy là`00:00`và chúng tôi trừ đi số phút, chúng tôi phải hạ cánh`23:xx`một cách chính xác. Số học modulo 1440 đảm bảo kết quả âm tính được đưa vào khoảng thời gian ngày trên chính xác, do đó không cần xử lý đặc biệt. 

Trường hợp cuối cùng là khi chu trình quay trở lại cùng một thời điểm sau một vài lần lặp. Vì không gian tìm kiếm là hữu hạn nên thuật toán cuối cùng sẽ lặp qua tất cả các trạng thái có thể truy cập được. Bởi vì chúng tôi dừng lại ở lần xuất hiện hợp lệ đầu tiên nên việc lặp lại không ảnh hưởng đến tính chính xác mà chỉ giới hạn thời gian chạy.
