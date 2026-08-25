---
title: "CF 104313C - \u042f \u043a\u0430\u043b\u0435\u043d\u0434\u0430\u0440\u044c"
description: "Chúng ta có một tháng duy nhất biết được ngày trong tuần của một ngày cụ thể. Điểm neo đã biết đó bao gồm số ngày từ 1 đến 31 và tên ngày trong tuần như Thứ Hai hoặc Chủ Nhật. Sử dụng mỏ neo này, chúng ta phải xác định ngày trong tuần của một ngày khác trong cùng tháng."
date: "2026-07-01T19:45:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104313
codeforces_index: "C"
codeforces_contest_name: "II \u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u042e\u041c\u0428 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 104313
solve_time_s: 45
verified: true
draft: false
---

[CF 104313C - \u042f \u043a\u0430\u043b\u0435\u043d\u0434\u0430\u0440\u044c](https://codeforces.com/problemset/problem/104313/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tháng duy nhất biết được ngày trong tuần của một ngày cụ thể. Điểm neo đã biết đó bao gồm số ngày từ 1 đến 31 và tên ngày trong tuần như Thứ Hai hoặc Chủ Nhật. Sử dụng mỏ neo này, chúng ta phải xác định ngày trong tuần của một ngày khác trong cùng tháng. 

Cấu trúc của bài toán về cơ bản là một lịch tuyến tính: mỗi ngày sẽ tiến lên ngày trong tuần một bước trong chu kỳ bảy. Việc tiến hoặc lùi giữa hai ngày chỉ phụ thuộc vào sự khác biệt về số ngày của chúng chứ không phụ thuộc vào bất kỳ đặc điểm cụ thể nào của tháng như độ dài hoặc năm nhuận khác nhau. Thao tác duy nhất chúng ta cần là dịch một phần bù số thành một phép dịch theo chuỗi tuần hoàn có độ dài bảy. 

Các ràng buộc là cực kỳ nhỏ, với số ngày được giới hạn bởi 31. Điều này ngay lập tức loại trừ mọi nhu cầu xử lý trước phức tạp, mô phỏng trên phạm vi lớn hoặc cấu trúc dữ liệu. Bất kỳ giải pháp nào tính toán sự khác biệt giữa hai số nguyên và áp dụng số học mô-đun đều đủ trong thời gian không đổi. 

Trường hợp cạnh chính phát sinh từ hành vi bao quanh trong chu kỳ ngày trong tuần. Ví dụ: nếu ngày đã biết là gần cuối tuần và ngày mục tiêu là sớm hơn trong tháng thì sự thay đổi sẽ trở thành số âm. Việc triển khai đơn giản mà quên chuẩn hóa modulo 7 có thể tạo ra việc lập chỉ mục không chính xác hoặc thậm chí truy cập mảng âm. Một vấn đề tế nhị khác là việc đặt hàng nhất quán vào các ngày trong tuần. Nếu ánh xạ giữa tên và chỉ mục ngày trong tuần không nhất quán giữa mã hóa và giải mã, kết quả sẽ bị dịch chuyển không chính xác bởi một độ lệch cố định. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ mô phỏng từng ngày bắt đầu từ ngày đã biết. Chúng ta sẽ gán ngày trong tuần đã biết cho ngày a, sau đó lặp lại tiến hoặc lùi qua tất cả các ngày cho đến b, tăng hoặc giảm ngày trong tuần mỗi lần. Điều này hiệu quả vì mỗi bước đều mang tính quyết định nhưng trong trường hợp xấu nhất, chúng tôi di chuyển trong suốt thời gian của tháng, tối đa 31 bước. Mặc dù điều này vẫn còn tầm thường, nhưng về mặt khái niệm, nó là chi phí không cần thiết và trở nên dễ hỏng nếu giới hạn lớn hơn. 

Quan sát quan trọng là các ngày trong tuần tạo thành một chu kỳ có kích thước bảy. Thay vì mô phỏng từng ngày trung gian, chúng ta chỉ cần độ dịch chuyển ròng giữa b và a. Sự dịch chuyển đó chỉ đơn giản là (b − a) và độ dịch chuyển ngày trong tuần là giá trị theo modulo 7. Khi chúng ta chuyển đổi các ngày trong tuần thành số nguyên, câu trả lời sẽ trở thành một biểu thức số học duy nhất. 

Điều này làm giảm toàn bộ vấn đề thành ánh xạ thời gian không đổi và số học mô-đun, loại bỏ hoàn toàn mọi phép lặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O( | b − a | ) | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách chuyển đổi các ngày trong tuần thành hệ thống số tuần hoàn và áp dụng phép dịch chuyển. 

1. Ấn định mỗi ngày trong tuần một số từ 0 đến 6 theo thứ tự thời gian bắt đầu từ thứ Hai. Điều này tạo ra sự trình bày theo chu kỳ nhất quán trong tuần. 
2. Đọc ngày neo đã biết a và ngày trong tuần của nó, rồi chuyển s thành dạng số cur. 
3. Đọc ngày mục tiêu b. 
4. Tính chênh lệch diff = b − a, biểu thị số ngày chúng ta tiến lên (hoặc lùi lại nếu âm). 
5. Chuyển đổi chênh lệch này thành ca ngày trong tuần bằng cách sử dụng diff mod 7, đảm bảo nó luôn nằm trong phạm vi từ 0 đến 6. 
6. Thêm sự dịch chuyển này vào cur và lại lấy modulo 7 để duy trì trong chu kỳ ngày trong tuần. 
7. Chuyển số kết quả trở lại thành chuỗi ngày trong tuần và xuất ra. 

### Tại sao nó hoạt động

Hệ thống ngày trong tuần là một chu trình mô-đun thuần túy có độ dài bảy. Mỗi gia số của một ngày dương lịch tương ứng với việc thêm một modulo 7 vào không gian ngày trong tuần. Do đó, việc chuyển từ ngày a sang ngày b tương ứng chính xác với việc áp dụng (b − a) các mức tăng như vậy. Việc giảm mô-đun dịch chuyển 7 này sẽ duy trì các lớp ca tương đương, do đó mọi chu kỳ đầy đủ trong bảy ngày sẽ bị hủy bỏ mà không ảnh hưởng đến ngày cuối cùng trong tuần. Ánh xạ giữa các chuỗi và chỉ mục là tính từ, do đó không có sự mơ hồ nào được đưa ra ở bất kỳ giai đoạn nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

days = {
    "Monday": 0,
    "Tuesday": 1,
    "Wednesday": 2,
    "Thursday": 3,
    "Friday": 4,
    "Saturday": 5,
    "Sunday": 6
}

rev = {v: k for k, v in days.items()}

def solve():
    parts = input().split()
    a = int(parts[0])
    s = parts[1].strip()

    b = int(input().strip())

    cur = days[s]
    shift = (b - a) % 7
    ans = (cur + shift) % 7

    print(rev[ans])

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng sự phân biệt cố định giữa tên ngày trong tuần và số nguyên, điều này rất cần thiết để thực hiện số học. Đầu vào được phân tích cú pháp theo hai bước vì dòng đầu tiên chứa cả số và chuỗi. Tính toán dịch chuyển sử dụng modulo 7 để đảm bảo tính chính xác cho cả chuyển động tiến và lùi trong chu kỳ tuần. Việc chuyển đổi cuối cùng trở lại chuỗi hoàn tất việc ánh xạ. 

Một lỗi triển khai phổ biến là quên bình thường hóa những khác biệt tiêu cực. Trong Python, modulo âm vẫn hoạt động chính xác, nhưng trong các ngôn ngữ khác, điều này có thể dẫn đến chỉ số không chính xác trừ khi được điều chỉnh rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 Wednesday
5
```Chúng tôi ánh xạ các ngày trong tuần từ Thứ Hai = 0 đến Chủ Nhật = 6. 

| Bước | Giá trị | 
| --- | --- | 
| một | 3 | 
| b | 5 | 
| s | Thứ Tư = 2 | 
| khác biệt | 5 − 3 = 2 | 
| ca | 2 mod 7 = 2 | 
| chỉ số kết quả | (2 + 2) mod 7 = 4 | 
| đầu ra | Thứ sáu | 

Điều này xác nhận việc dịch chuyển về phía trước diễn ra như mong đợi, diễn ra trước hai ngày kể từ thứ Tư. 

### Ví dụ 2 

đầu vào:```
1 Monday
7
```| Bước | Giá trị | 
| --- | --- | 
| một | 1 | 
| b | 7 | 
| s | Thứ Hai = 0 | 
| khác biệt | 6 | 
| ca | 6 mod 7 = 6 | 
| chỉ số kết quả | (0 + 6) mod 7 = 6 | 
| đầu ra | chủ nhật | 

Điều này cho thấy hành vi bao quanh chính xác khi vượt qua chu kỳ cuối tuần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ việc tra cứu từ điển và các phép tính số học theo thời gian cố định mới được thực hiện | 
| Không gian | O(1) | Đã sửa lỗi ánh xạ bảy ngày trong tuần | 

Giải pháp này phù hợp một cách tầm thường trong các ràng buộc vì tất cả các hoạt động đều có thời gian không đổi và mức sử dụng bộ nhớ được cố định bất kể đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue().strip() if False else _run(inp)

def _run(inp: str) -> str:
    import sys
    from io import StringIO

    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = StringIO(inp)
    sys.stdout = StringIO()

    solve()

    out = sys.stdout.getvalue().strip()
    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out

# provided sample
assert _run("3 Wednesday\n5\n") == "Friday"

# minimum distance
assert _run("1 Monday\n1\n") == "Monday"

# wrap forward across week
assert _run("7 Sunday\n8\n") == "Monday"

# wrap backward logic
assert _run("2 Tuesday\n1\n") == "Monday"

# large jump multiple cycles
assert _run("10 Friday\n100\n") == "Thursday"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 Thứ Hai / 1 | Thứ hai | dịch chuyển không | 
| 7 Chủ nhật / 8 | Thứ hai | bao bọc phía trước | 
| 2 Thứ Ba / 1 | Thứ hai | chuyển động lùi | 
| 10 Thứ Sáu / 100 | Thứ năm | nhiều chu kỳ đầy đủ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi b nhỏ hơn a, tạo ra hiệu âm. Ví dụ: 

đầu vào:```
10 Friday
8
```Ở đây, Thứ Sáu tương ứng với chỉ số 4. Hiệu là 8 − 10 = −2. Áp dụng modulo 7 cho 5, vì vậy chúng tôi tiến thêm năm bước kể từ thứ Sáu: 

Thứ sáu (4) → Thứ bảy (5) → Chủ nhật (6) → Thứ hai (0) → Thứ ba (1) → Thứ tư (2) 

Đầu ra là thứ Tư, phù hợp với việc đếm ngược trực tiếp. 

Một trường hợp khác là khi hiệu chính xác là bội số của 7: 

đầu vào:```
15 Sunday
29
```Hiệu là 14 và 14 mod 7 = 0. Ngày trong tuần không thay đổi. Điều này xác nhận rằng các chu kỳ cả tuần bị hủy bỏ hoàn toàn, duy trì ngày cố định trong tuần bất kể đã trải qua bao nhiêu tuần.
