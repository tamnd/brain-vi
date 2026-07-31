---
title: "CF 102770A - 2020"
description: "Chúng ta cần đếm xem có bao nhiêu ngày theo lịch trong một khoảng thời gian nhất định có đặc tính là biểu diễn YYYYMMDD của chúng chứa ba chữ số liên tiếp 202. Một ngày không được so sánh dưới dạng số có thuộc tính. Việc biểu diễn tám ký tự thực tế rất quan trọng."
date: "2026-07-30T04:31:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102770
codeforces_index: "A"
codeforces_contest_name: "The 17th Zhejiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102770
solve_time_s: 90
verified: true
draft: false
---

[CF 102770A - AD 2020](https://codeforces.com/problemset/problem/102770/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm xem có bao nhiêu ngày dương lịch trong một khoảng thời gian nhất định có đặc tính`YYYYMMDD`biểu diễn chứa ba chữ số liên tiếp`202`. 

Ngày không được so sánh dưới dạng số với thuộc tính. Việc biểu diễn tám ký tự thực tế rất quan trọng. Ví dụ,`22020101`chứa`202`bởi vì chuỗi con bắt đầu ở chữ số thứ hai của năm, trong khi`20190202`chứa nó vì chuỗi con chuyển từ năm sang tháng. 

Đầu vào chứa tối đa`10^5`khoảng ngày độc lập. Năm được giới hạn trong`2000`bởi vì`9999`, vậy chỉ có 8000 năm có thể xảy ra. Điều này loại trừ việc quét hàng ngày cho mọi truy vấn vì toàn bộ phạm vi chứa gần ba triệu ngày và thực hiện việc đó trong`10^5`trường hợp sẽ tiếp cận`3 * 10^11`kiểm tra ngày. Phạm vi năm cố định nhỏ gợi ý xử lý trước tất cả các năm một lần và trả lời từng truy vấn bằng cách sử dụng tổng tiền tố. 

Phần khó khăn là ranh giới và những nơi khác nhau nơi`202`có thể xuất hiện. Một giải pháp bất cẩn chỉ có thể kiểm tra xem năm đó có chứa`202`và bỏ lỡ các trường hợp liên quan đến tháng và ngày. Ví dụ, khoảng`2111 02 01`ĐẾN`2111 02 03`chỉ có một ngày hợp lệ,`21110202`, vì chuỗi con được hình thành bởi phần tháng và ngày. Một sai lầm khác là loại trừ ngày kết thúc. Vì`2020 01 01`ĐẾN`2020 01 02`, cả hai ngày đều hợp lệ vì cả hai chuỗi đầy đủ đều chứa`202`, vậy câu trả lời là`2`. 

Năm nhuận cũng quan trọng. Ví dụ, khoảng`2200 02 28`ĐẾN`2200 03 01`chứa hai ngày chứ không phải ba, vì năm 2200 không phải là năm nhuận. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là lặp qua từng ngày trong khoảng thời gian đó, chuyển đổi nó thành`YYYYMMDD`, và kiểm tra xem`"202"`xuất hiện. Điều này dễ chứng minh là đúng vì mỗi ngày được kiểm tra đúng một lần. Tuy nhiên, khoảng thời gian lớn nhất có thể kéo dài từ năm 2000 đến năm 9999, trong đó có khoảng 2,9 triệu ngày. Lặp lại công việc đó cho`10^5`truy vấn vượt xa giới hạn. 

Quan sát quan trọng là phạm vi lịch rất nhỏ so với số lượng truy vấn. Chỉ có 8000 năm có thể. Chúng ta có thể xử lý trước mỗi năm bằng cách đếm xem có bao nhiêu ngày hợp lệ bên trong nó chứa`202`và cũng lưu trữ thông tin tiền tố trong năm. 

Đối với một truy vấn, câu trả lời sẽ trở thành:`number of valid days in years before Y2`cộng thêm`number of valid days from January 1 to the ending date`, trừ đi giá trị tương tự được tính cho ngày trước ngày bắt đầu. 

Điều này biến mọi truy vấn thành một lượng công việc không đổi sau khi xử lý trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số ngày trong phạm vi) cho mỗi truy vấn | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi truy vấn sau quá trình tiền xử lý O(8000 * 366) | O(8000 * 13 * 32) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước thông tin cho mỗi năm từ 2000 đến 9999. Đối với mỗi năm, mô phỏng tất cả các tháng và ngày hợp lệ, tạo chuỗi ngày tám chữ số và đếm xem nó có chứa`202`. 

Số năm là cố định và nhỏ, do đó quá trình tiền xử lý này rẻ và loại bỏ công việc lặp lại khỏi các truy vấn. 
2. Lưu trữ một mảng tiền tố ở đâu`prefix[y]`là số ngày hợp lệ từ năm 2000 đến hết năm`y`. 

Điều này cho phép hoàn thành nhiều năm trước khi ranh giới truy vấn được trả lời ngay lập tức. 
3. Đối với mỗi năm, cũng lưu trữ số lượng tích lũy theo tháng và ngày. Điều này cho phép chúng tôi chỉ tính phần đầu của năm khi truy vấn kết thúc vào giữa năm đó. 
4. Xác định hàm trả về số ngày hợp lệ từ`2000-01-01`đến bất kỳ ngày nào. 

Nó kết hợp các năm đầy đủ được tính toán trước với thông tin một phần năm. 
5. Trả lời từng câu hỏi bằng:`count(end_date) - count(day_before_start_date)`Trừ ngày hôm trước xử lý khoảng thời gian bao gồm một cách chính xác. 

Tại sao nó hoạt động: 

Hàm tiền tố luôn đếm chính xác số ngày từ điểm bắt đầu cố định đến ngày được yêu cầu. Đối với khoảng thời gian truy vấn, việc xóa mọi thứ trước ngày đầu tiên sẽ để lại chính xác những ngày bên trong khoảng thời gian đó. Vì mỗi ngày đều được tính theo mức độ đầy đủ của nó`YYYYMMDD`trình bày trong quá trình tiền xử lý, không thể xảy ra sự cố`202`bị bỏ lỡ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def leap(y):
    return y % 400 == 0 or (y % 4 == 0 and y % 100 != 0)

days_in_month = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]

year_pref = [0] * 10001
inside = {}

for y in range(2000, 10000):
    cur = [[0] * 32 for _ in range(13)]
    total = 0
    for m in range(1, 13):
        limit = days_in_month[m - 1]
        if m == 2 and leap(y):
            limit += 1
        for d in range(1, limit + 1):
            if "202" in f"{y:04d}{m:02d}{d:02d}":
                total += 1
            cur[m][d] = total
    inside[y] = cur
    year_pref[y] = year_pref[y - 1] + total

def count_to(y, m, d):
    if y < 2000:
        return 0
    ans = year_pref[y - 1]
    if m:
        ans += inside[y][m][d]
    return ans

def previous_day(y, m, d):
    d -= 1
    if d == 0:
        m -= 1
        if m == 0:
            return y - 1, 12, 31
        d = days_in_month[m - 1]
        if m == 2 and leap(y):
            d += 1
    return y, m, d

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        y1, m1, d1, y2, m2, d2 = map(int, input().split())
        py, pm, pd = previous_day(y1, m1, d1)
        out.append(str(count_to(y2, m2, d2) - count_to(py, pm, pd)))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng tiền xử lý xây dựng hai loại thông tin.`year_pref`cửa hàng đã hoàn thành năm, trong khi`inside[y]`lưu trữ các câu trả lời tích lũy trong một năm cụ thể. Cách thứ hai tránh lặp lại nhiều tháng và nhiều ngày trong mỗi truy vấn. 

Trước tiên, hàm truy vấn sẽ lấy tất cả các năm đầy đủ trước năm mục tiêu, sau đó cộng tổng số một phần bên trong năm mục tiêu. Hàm ngày trước được sử dụng vì hàm tiền tố có tính chất bao hàm. Nếu không có nó, ngày bắt đầu sẽ được tính hai lần. 

Số nguyên Python không tràn ở đây vì tổng số ngày trong toàn bộ phạm vi được hỗ trợ chỉ có vài triệu. Chi tiết triển khai chính cần tránh là xử lý tháng Hai một cách chính xác khi di chuyển lùi qua ranh giới tháng. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:`2111 02 01`ĐẾN`2111 02 03`| Bước | Ngày | Kết quả tiền tố | 
| --- | --- | --- | 
| Kết thúc ranh giới | 2111-02-03 | Đếm tất cả các ngày hợp lệ trước và bao gồm cả ngày này | 
| Bắt đầu trừ một ngày | 2111-01-31 | Xóa tất cả các ngày trước đó | 
| Sự khác biệt | 2111-02-01 đến 2111-02-03 | 1 | 

Ngày duy nhất được tính là`21110202`, trong đó có chứa`202`. 

Đối với mẫu thứ hai:`2202 01 01`ĐẾN`2202 12 31`| Bước | Ngày | Kết quả tiền tố | 
| --- | --- | --- | 
| Kết thúc ranh giới | 2202-12-31 | Bao gồm cả năm | 
| Bắt đầu trừ một ngày | 2201-12-31 | Loại trừ mọi thứ trước 2202 | 
| Sự khác biệt | Cả năm 2202 | 365 | 

Mỗi ngày trong năm 2202 đều chứa`202`trong phần năm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(8000 * 366 + T) | Quá trình tiền xử lý sẽ kiểm tra mỗi ngày được hỗ trợ một lần, sau đó mỗi truy vấn có thời gian không đổi | 
| Không gian | O(8000 * 13 * 32) | Lưu trữ thông tin tích lũy cho mỗi năm | 

Chi phí tiền xử lý là khoảng ba triệu lần kiểm tra ngày, một con số rất nhỏ. Sau đó, thậm chí`10^5`truy vấn chỉ yêu cầu các phép tính số học đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

# paste the solution above here and expose solve()

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    result = sys.stdout.getvalue()
    sys.stdin = old
    return result

assert run("""3
2111 2 1 2111 2 3
2202 1 1 2202 12 31
2000 1 1 9999 12 31
""") == "1\n365\n44294\n"

assert run("""1
2000 1 1 2000 1 1
""") == "0\n"

assert run("""1
2020 1 1 2020 1 2
""") == "2\n"

assert run("""1
2202 2 28 2202 3 1
""") == "2\n"

assert run("""1
9999 12 31 9999 12 31
""") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2000-01-01`ĐẾN`2000-01-01`|`0`| Ranh giới tối thiểu và ngày không có`202`| 
|`2020-01-01`ĐẾN`2020-01-02`|`2`| Phát hiện chuỗi con năm và phạm vi bao gồm | 
|`2202-02-28`ĐẾN`2202-03-01`|`2`| Xử lý chuyển đổi năm nhuận | 
|`9999-12-31`ĐẾN`9999-12-31`|`0`| Ranh giới năm tối đa | 

## Vỏ cạnh 

Một chuỗi con có thể vượt qua ranh giới năm thành tháng hoặc ngày. Đối với đầu vào`2111 02 01 2111 02 03`, thuật toán không chỉ kiểm tra năm. Trong quá trình tiền xử lý, nó sẽ kiểm tra chuỗi hoàn chỉnh`21110202`, vậy đáp án đúng`1`. 

Một năm chứa đựng`202`làm cho mỗi ngày trong năm đó có giá trị. Vì`2202 01 01 2202 12 31`, chuỗi con xuất hiện trong chính các chữ số của năm. Giá trị tiền xử lý cho năm 2202 là số ngày trong năm đó, tức là`365`và truy vấn trả về số năm đầy đủ đó. 

Năm nhuận ảnh hưởng đến số ngày có thể tồn tại. Vì`2200 02 28 2200 03 01`, cách tính ngày lùi sử dụng quy tắc Gregorian và nhận ra rằng năm 2200 không phải là năm nhuận. Tháng 2 có 28 ngày nên không có ngày 29 tháng 2 không hợp lệ được đưa ra.
