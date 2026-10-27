---
title: "CF 106124C - Cuộc thi đan móc"
description: "Chúng tôi nhận được hai dấu thời gian mô tả thời điểm cuộc thi móc bắt đầu và thời điểm kết thúc. Mỗi dấu thời gian chứa một ngày trong tuần cùng với thời gian trên đồng hồ tính bằng giờ và phút. Mục tiêu là tính toán thời gian cuộc thi kéo dài, tính bằng ngày, giờ và phút."
date: "2026-06-19T20:02:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106124
codeforces_index: "C"
codeforces_contest_name: "2025-2026 ICPC Nordic Collegiate Programming Contest (NCPC 2025)"
rating: 0
weight: 106124
solve_time_s: 50
verified: true
draft: false
---

[CF 106124C - Cuộc thi đan móc](https://codeforces.com/problemset/problem/106124/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được hai dấu thời gian mô tả thời điểm cuộc thi móc bắt đầu và thời điểm kết thúc. Mỗi dấu thời gian chứa một ngày trong tuần cùng với thời gian trên đồng hồ tính bằng giờ và phút. Mục tiêu là tính toán thời gian cuộc thi kéo dài, tính bằng ngày, giờ và phút. 

Điều tinh tế là thời gian có tính tuần hoàn trong nhiều tuần. Nếu thời gian kết thúc xuất hiện sớm hơn thời gian bắt đầu trong tuần thì cuộc thi phải diễn ra vào tuần tiếp theo hoặc thậm chí vài tuần sau đó. Quy tắc đặc biệt duy nhất là nếu hai dấu thời gian giống hệt nhau thì thời lượng được xác định là đúng một tuần. 

Đầu ra phải thể hiện sự khác biệt ở dạng chuẩn hóa: ngày, giờ và phút, bỏ qua mọi thành phần có giá trị bằng 0 và định dạng các thành phần còn lại theo kiểu tiếng Anh tự nhiên. 

Kích thước đầu vào không đổi nên các ràng buộc là cực kỳ nhỏ. Điều này ngay lập tức loại trừ bất kỳ mối lo ngại phức tạp nào ngoài việc phân tích cú pháp và số học theo thời gian không đổi. Khó khăn chính không phải là hiệu suất mà là việc chuẩn hóa chính xác những khác biệt về thời gian trong một chu kỳ lặp lại hàng tuần. 

Những lỗi phổ biến nhất xảy ra khi xử lý các ngày trong tuần một cách độc lập với thời gian trên đồng hồ. Ví dụ: diễn giải “23:00 Thứ Hai đến 01:00 Thứ Hai” dưới dạng khoảng thời gian âm thay vì gói sang tuần tiếp theo sẽ dẫn đến kết quả không chính xác. Một trường hợp khác là dấu thời gian giống hệt nhau, phải tạo ra chính xác 7 ngày thay vì thời lượng bằng 0. 

## Phương pháp tiếp cận 

Tư duy bạo lực sẽ cố gắng mô phỏng thời gian từng phút một, tăng dần từ dấu thời gian bắt đầu cho đến dấu thời gian kết thúc. Điều này đúng về mặt khái niệm vì thời gian tính bằng phút và mỗi bước tiến lên một phút. Tuy nhiên, trong trường hợp xấu nhất, khoảng cách có thể kéo dài nhiều tuần, nghĩa là lặp lại không cần thiết lên tới khoảng 10.000 phút hoặc hơn. Mặc dù trên thực tế vẫn còn nhỏ nhưng cách tiếp cận này không cần thiết và che giấu cấu trúc thực sự của vấn đề. 

Quan sát quan trọng là cả hai dấu thời gian đều tồn tại trong một chu kỳ định kỳ duy nhất là 7 ngày × 24 giờ × 60 phút. Nếu chúng ta ánh xạ từng dấu thời gian vào một chỉ số phút tuyệt đối trong vòng một tuần, chúng ta có thể tính toán sự khác biệt một cách trực tiếp. Sau khi cả hai thời gian được chuyển đổi thành “phút kể từ đầu tuần”, điều mơ hồ duy nhất là liệu điểm cuối có nằm trước điểm bắt đầu trong không gian mô-đun đó hay không. Nếu đúng như vậy, chúng tôi chỉ cần thêm một tuần vào thời gian kết thúc. Điều này chuyển đổi vấn đề thành một phép trừ đơn giản. 

Từ đó, nhiệm vụ còn lại là phân tích tổng chênh lệch thành ngày, giờ và phút bằng phép chia số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng phút Brute Force | O(D) trong đó D là thời lượng tính bằng phút | O(1) | Được chấp nhận nhưng không cần thiết | 
| Chuyển đổi mô-đun | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi dấu thời gian là độ lệch phút tuyệt đối so với đầu tuần tham chiếu, chẳng hạn như 00:00 Thứ Hai. 

1. Ánh xạ các ngày trong tuần thành các số nguyên từ 0 đến 6, trong đó Thứ Hai là 0 và Chủ Nhật là 6. Điều này mang lại thứ tự nhất quán cho các ngày trong tuần. 
2. Chuyển đổi từng dấu thời gian thành tổng số phút kể từ đầu tuần bằng công thức:`total = weekday * 1440 + hour * 60 + minute`. 

Điều này hiệu quả vì mỗi ngày có 1440 phút. 
3. So sánh hai giá trị. Nếu giá trị cuối hoàn toàn nhỏ hơn giá trị bắt đầu, chúng tôi hiểu giá trị này là gói vào tuần tiếp theo, vì vậy chúng tôi thêm 7 × 1440 phút vào giá trị cuối. 
4. Nếu cả hai dấu thời gian giống hệt nhau, hãy trả về đúng một tuần, tức là 7 × 1440 phút mà không cần phân tách thêm. Quy tắc này ghi đè phép trừ thông thường. 
5. Tính chênh lệch theo phút:`diff = end - start`. 
6. Phân tách kết quả thành ngày, giờ, phút:`days = diff // 1440`,`hours = (diff % 1440) // 60`,`minutes = diff % 60`. 
7. Xây dựng chuỗi đầu ra bằng cách chỉ bao gồm các thành phần khác 0 theo thứ tự ngày, giờ, phút và định dạng chúng ở dạng số ít hoặc số nhiều chính xác. 

### Tại sao nó hoạt động 

Tính chính xác đến từ việc biểu diễn tất cả các dấu thời gian theo trục thời gian được tuyến tính hóa trong một khoảng thời gian cố định. Bất kỳ dấu thời gian trong thế giới thực nào trong bài toán này đều tương đương với một vị trí duy nhất trong chu kỳ 10080 phút. Bằng cách chuyển đổi mọi thứ thành không gian tuyến tính này và chỉ áp dụng một hiệu chỉnh bao bọc duy nhất khi cần thiết, chúng tôi duy trì trật tự thời gian thực sự. Bước phân tách là chính xác vì phép chia số nguyên trên các kích thước đơn vị cố định sẽ phân chia rõ ràng số phút thành ngày, giờ và phút mà không bị chồng chéo hoặc mơ hồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

days_map = {
    "Mon": 0,
    "Tue": 1,
    "Wed": 2,
    "Thu": 3,
    "Fri": 4,
    "Sat": 5,
    "Sun": 6
}

def parse(line):
    d, t = line.strip().split()
    h, m = map(int, t.split(":"))
    return days_map[d] * 1440 + h * 60 + m

a = parse(input())
b = parse(input())

WEEK = 7 * 1440

if a == b:
    diff = WEEK
else:
    if b < a:
        b += WEEK
    diff = b - a

days = diff // 1440
diff %= 1440
hours = diff // 60
minutes = diff % 60

parts = []
if days:
    parts.append(f"{days} day" + ("s" if days != 1 else ""))
if hours:
    parts.append(f"{hours} hour" + ("s" if hours != 1 else ""))
if minutes:
    parts.append(f"{minutes} minute" + ("s" if minutes != 1 else ""))

print(", ".join(parts))
```Bước phân tích cú pháp chuyển đổi ngày trong tuần và thời gian trên đồng hồ thành một số nguyên duy nhất biểu thị số phút kể từ đầu tuần. Việc xử lý gói là rất quan trọng: khi thời gian kết thúc sớm hơn trong tuần, chúng tôi sẽ dịch chuyển nó về phía trước một tuần một cách rõ ràng để phép trừ vẫn hợp lệ. 

Logic định dạng sẽ xây dựng đầu ra tăng dần và đảm bảo rằng không có thành phần nào bị loại bỏ hoàn toàn, điều này được yêu cầu bởi quy tắc đầu ra của câu lệnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

Thứ 2 08:00 

15:00 thứ 2 

Chúng tôi ánh xạ cả hai thời gian thành phút: 

| Bước | Bắt đầu | Kết thúc | 
| --- | --- | --- | 
| Chỉ số ngày trong tuần | 0 | 0 | 
| Chuyển đổi giờ/phút | 480 | 900 | 
| Tổng số phút | 480 | 900 | 

Sự khác biệt là 420 phút. 

| Bước | Giá trị | 
| --- | --- | 
| khác biệt | 420 | 
| ngày | 0 | 
| giờ | 7 | 
| phút | 0 | 

Đầu ra trở thành: 

7 giờ 

Điều này xác nhận thuật toán xử lý chính xác những khác biệt trong cùng ngày mà không cần bất kỳ logic bao bọc nào. 

### Ví dụ 2 

đầu vào: 

10:00 thứ 2 

Thứ Tư 08:59 

| Bước | Bắt đầu | Kết thúc | 
| --- | --- | --- | 
| Chỉ số ngày trong tuần | 0 | 2 | 
| Chuyển đổi giờ/phút | 600 | 2×1440 + 539 = 3419 | 
| Tổng số phút | 600 | 3419 | 

Không cần bọc kể từ khi kết thúc ≥ bắt đầu. 

| Bước | Giá trị | 
| --- | --- | 
| khác biệt | 2819 | 
| ngày | 1 | 
| giờ | 22 | 
| phút | 59 | 

Đầu ra: 

1 ngày, 22 giờ, 59 phút 

Điều này cho thấy sự phân tách chính xác trên nhiều đơn vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ phân tích cú pháp và số học theo thời gian không đổi trên hai dấu thời gian | 
| Không gian | O(1) | Đã sửa lỗi ánh xạ và một vài số nguyên | 

Việc tính toán hoàn toàn là số học và không phụ thuộc vào bất kỳ sự tăng trưởng kích thước đầu vào nào. Nó thoải mái phù hợp trong bất kỳ ràng buộc hợp lý. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import *
    # assume solution is wrapped in main logic below
    return _sys.stdout.getvalue().strip()

# Since the solution is script-style, we instead test logic directly:

def solve(inp: str) -> str:
    import sys
    from io import StringIO
    backup = sys.stdin
    sys.stdin = StringIO(inp)

    days_map = {"Mon":0,"Tue":1,"Wed":2,"Thu":3,"Fri":4,"Sat":5,"Sun":6}

    def parse(line):
        d,t=line.strip().split()
        h,m=map(int,t.split(":"))
        return days_map[d]*1440+h*60+m

    a=parse(sys.stdin.readline())
    b=parse(sys.stdin.readline())

    WEEK=7*1440

    if a==b:
        diff=WEEK
    else:
        if b<a:
            b+=WEEK
        diff=b-a

    d=diff//1440
    diff%=1440
    h=diff//60
    m=diff%60

    parts=[]
    if d:
        parts.append(f"{d} day"+("s" if d!=1 else ""))
    if h:
        parts.append(f"{h} hour"+("s" if h!=1 else ""))
    if m:
        parts.append(f"{m} minute"+("s" if m!=1 else ""))

    sys.stdin = backup
    return ", ".join(parts)

# provided samples
assert solve("Mon 08:00\nMon 15:00\n") == "7 hours"
assert solve("Mon 10:00\nWed 08:59\n") == "1 day, 22 hours, 59 minutes"

# custom cases
assert solve("Mon 00:00\nMon 00:00\n") == "7 days", "exact same time"
assert solve("Sun 23:59\nMon 00:00\n") == "1 minute", "wrap across week boundary"
assert solve("Fri 10:00\nFri 10:01\n") == "1 minute", "minute precision"
assert solve("Sat 12:00\nSun 11:00\n") == "23 hours", "less than a day"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thứ Hai 00:00 → Thứ Hai 00:00 | 7 ngày | quy tắc dấu thời gian giống hệt nhau | 
| Chủ Nhật 23:59 → Thứ Hai 00:00 | 1 phút | xử lý gói tuần | 
| Thứ sáu 10:00 → Thứ sáu 10:01 | 1 phút | độ chính xác ở mức độ phút | 
| Thứ bảy 12:00 → Chủ nhật 11:00 | 23 giờ | trường hợp ranh giới không ngày | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi dấu thời gian giống hệt nhau. Ví dụ: 

đầu vào: 

Thứ Hai 00:00 

Thứ Hai 00:00 

Thuật toán phát hiện sự bằng nhau trước bất kỳ số học nào. Nó trực tiếp chỉ định một tuần đầy đủ 10080 phút. Điều này tránh việc tạo ra khoảng thời gian bằng 0 không chính xác, điều này sẽ vi phạm đặc tả vấn đề. 

Một trường hợp khác sẽ kết thúc vào cuối tuần: 

đầu vào: 

CN 23:59 

Thứ Hai 00:00 

Ở đây dấu thời gian kết thúc chuyển đổi thành giá trị số nhỏ hơn thời gian bắt đầu. Thuật toán cộng thêm 10080 phút vào cuối, tạo ra chênh lệch 1 phút. Điều này chứng tỏ rằng thứ tự chỉ được duy trì sau khi chuẩn hóa thành chu kỳ tuyến tính hàng tuần. 

Trường hợp tinh vi cuối cùng là khi thời lượng ít hơn một giờ hoặc một ngày. Bước phân tách xử lý việc này một cách rõ ràng vì phép chia số nguyên tự nhiên tạo ra các thành phần cao hơn bằng 0, sau đó bị bỏ qua trong quá trình định dạng.
