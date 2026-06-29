---
title: "CF 104619A - Tiến tới khu vực Đào Viên"
description: "Chúng ta có một ngày dương lịch duy nhất vào năm 2023, được viết là YYYY-MM-DD. Ngày này thể hiện thời điểm một cuộc thi lập trình (TOPC) được lên kế hoạch tổ chức."
date: "2026-06-29T17:25:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104619
codeforces_index: "A"
codeforces_contest_name: "2023 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 104619
solve_time_s: 47
verified: true
draft: false
---

[CF 104619A - Tiến tới khu vực Đào Viên](https://codeforces.com/problemset/problem/104619/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một ngày dương lịch duy nhất vào năm 2023, được viết là`YYYY-MM-DD`. Ngày này thể hiện thời điểm một cuộc thi lập trình (TOPC) được lên kế hoạch tổ chức. Nhiệm vụ là quyết định xem ngày này có quá muộn hay không, dựa trên quy tắc giới hạn cố định bắt nguồn từ lịch trình khu vực ICPC Đào Viên. 

Điều kiện chính được gắn với ngày 21 tháng 10 năm 2023. Bất kỳ ngày dự thi TOPC nào cũng phải trước ngày dự thi khu vực đó ít nhất 35 ngày; nếu không thì coi như đã quá muộn. Vì vậy, vấn đề trở thành một so sánh lịch: cho một ngày trong năm 2023, hãy xác định xem ngày đó hoàn toàn sớm hơn hoặc bằng ngày giới hạn (ngày 21 tháng 10 trừ 35 ngày) hay sau ngày đó. 

Các ràng buộc đầu vào đơn giản hóa vấn đề một cách đáng kể. Năm luôn là 2023 và phạm vi ngày tháng nhỏ và bị giới hạn (tháng 1 đến 12, ngày 1 đến 28). Điều này loại bỏ những lo ngại về ngày không hợp lệ hoặc năm nhuận. Chúng tôi chỉ cần số học ngày chính xác trong một năm cố định. 

Một sai lầm ngây thơ là so sánh tháng và ngày theo từ điển dưới dạng chuỗi. Ví dụ, so sánh`"2023-09-30"`Và`"2023-10-01"`như chuỗi hoạt động, nhưng so sánh`"2023-10-01"`và mức cắt được tính toán không chính xác vì các chuỗi không chuẩn hóa có thể thất bại trong lý luận chung. Một trường hợp phức tạp khác là hiểu sai phép trừ 35 ngày: giả định “Tháng 9 an toàn, tháng 10 thì không”, vì ranh giới nằm vào đầu tháng 9. 

Một kịch bản cụ thể: nếu thời điểm cắt vào khoảng đầu tháng 9 năm 2023, thì những ngày như`2023-09-16`vẫn còn hiệu lực, nhưng`2023-10-01`trở nên quá muộn mặc dù thời gian đã gần kề. Điều này cho thấy phương pháp phỏng đoán theo tháng là không đủ. 

Vì vậy, vấn đề về cơ bản là so sánh ngày với một ngày ngưỡng cố định. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận bạo lực sẽ mô phỏng phép trừ theo ngày kể từ ngày 21 tháng 10 năm 2023, quay trở lại 35 ngày. Vì phạm vi ngày nhỏ và cố định nên việc tính toán thủ công hoặc thông qua các vòng lặp đơn giản trên biểu diễn lịch là điều đơn giản. Chi phí là thời gian không đổi trong thực tế, vì năm và phạm vi là cố định. 

Một cách tiếp cận rõ ràng hơn là chuyển đổi mỗi ngày thành một số nguyên biểu thị “số ngày kể từ một nguồn gốc cố định”, sau đó so sánh các số nguyên. Vì tất cả các ngày đều là năm 2023 và các tháng được giới hạn nên chúng ta có thể xác định trước độ dài của tháng và tính toán các giá trị ngày trong năm. 

Khi chúng tôi chuyển đổi ngày đầu vào thành chỉ mục ngày, chúng tôi sẽ tính toán ngày giới hạn tương tự và so sánh. 

Quan sát quan trọng là chúng ta không cần thư viện ngày đầy đủ. Chuyển đổi theo năm cố định là đủ và so sánh trở thành so sánh số nguyên duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lịch Brute Force | O(1) | O(1) | Đã chấp nhận | 
| Chuyển Đổi Ngày Trong Năm | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính ngày giới hạn bằng cách trừ đi 35 ngày kể từ ngày 21 tháng 10 năm 2023. Vì tất cả các ngày đều nằm trong một năm và các tháng đều nhỏ nên chúng tôi tính toán trước số ngày tích lũy cho mỗi tháng. 

Sau đó, chúng tôi chuyển đổi cả ngày đầu vào và ngày hết hạn thành giá trị ngày trong năm. 

### Các bước 

1. Phân tích chuỗi đầu vào`YYYY-MM-DD`thành các số nguyên năm, tháng, ngày. Chúng ta chỉ quan tâm đến tháng và ngày vì năm đã được cố định. 
2. Xây dựng mảng tĩnh biểu thị số ngày trong mỗi tháng của năm 2023. Vì bài toán đảm bảo tính hợp lệ và không bao gồm năm nhuận nên tháng 2 có 28 ngày. 
3. Xây dựng một mảng tổng tiền tố trong đó`prefix[m]`cung cấp tổng số ngày tính đến cuối tháng`m - 1`. Điều này cho phép chuyển đổi nhanh chóng một ngày thành một chỉ số ngày nguyên duy nhất. 
4. Chuyển đổi ngày đầu vào thành giá trị ngày trong năm bằng cách sử dụng`prefix[month] + day`. 
5. Tính ngày hết hạn là ngày 21 tháng 10 năm 2023 rồi trừ đi 35 ngày. Chuyển đổi kết quả đó thành một giá trị ngày khác trong năm. 
6. So sánh hai giá trị. Nếu chỉ số ngày đầu vào lớn hơn chỉ số ngưỡng thì ngày đó quá muộn; nếu không thì nó hợp lệ. 

Lý do đằng sau việc chuyển đổi sang chỉ mục tuyến tính là nó loại bỏ tất cả cấu trúc lịch. Sau khi làm phẳng, vấn đề trở thành một phép so sánh số nguyên đơn giản. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến mà ánh xạ ngày trong năm duy trì thứ tự thời gian: nếu ngày A xảy ra trước ngày B trong lịch thì chỉ số ngày của nó nhỏ hơn. Bởi vì chúng tôi tính toán cả hai giá trị bằng cách sử dụng cùng một cấu trúc độ dài tháng nên không có biến dạng nào được đưa ra. Trừ 35 ngày kể từ một ngày đã biết trong cùng một biểu diễn sẽ mang lại ranh giới chính xác và so sánh trong không gian này tương đương với so sánh ngày thực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def to_day(month, day):
    days_in_month = [0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
    prefix = [0] * 13
    for i in range(1, 13):
        prefix[i] = prefix[i - 1] + days_in_month[i - 1]
    return prefix[month] + day

def solve():
    s = input().strip()
    year, month, day = map(int, s.split("-"))

    # input date as day index
    d = to_day(month, day)

    # cutoff: 2023-10-21 minus 35 days
    cutoff = to_day(10, 21) - 35

    if d > cutoff:
        print("TOO LATE")
    else:
        print("GOOD")

if __name__ == "__main__":
    solve()
```Bước phân tích cú pháp trích xuất trực tiếp tháng và ngày từ chuỗi định dạng ISO. Chúng tôi bỏ qua năm vì nó đã được sửa. 

chức năng`to_day`chuyển đổi một ngày thành một vị trí tuyến tính trong năm bằng cách sử dụng tổng tiền tố của độ dài tháng. Điều này đảm bảo so sánh thời gian liên tục sau khi chuyển đổi. 

Điểm giới hạn được tính bằng cách chuyển đổi ngày 21 tháng 10 thành chỉ số ngày và trừ đi 35, điều này an toàn vì tất cả các phép tính đều nằm trong cùng một không gian lịch được tuyến tính hóa. 

Cuối cùng, chúng tôi so sánh trực tiếp và in nhãn theo yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`2023-09-16`Chúng tôi tính toán các giá trị ngày trong năm. 

| Bước | Tháng | Ngày | Chỉ số ngày | 
| --- | --- | --- | --- | 
| Ngày nhập | 9 | 16 | 31+28+31+30+31+30+31+31 + 16 = 259 | 
| Cơ sở cắt | 10 | 21 | 294 | 
| Cắt sau -35 | - | - | 259 | 

Đầu vào bằng ranh giới giới hạn, vì vậy nó không muộn hơn nó. Đầu ra là`GOOD`. 

Điều này xác nhận rằng sự bình đẳng được coi là hợp lệ, phù hợp với điều kiện “ít nhất 35 ngày trước”. 

### Ví dụ 2 

đầu vào:`2023-10-01`| Bước | Tháng | Ngày | Chỉ số ngày | 
| --- | --- | --- | --- | 
| Ngày nhập | 10 | 1 | 273 | 
| Cắt | - | - | 259 | 

Vì 273 > 259, ngày nằm sau ranh giới giới hạn, nên kết quả đầu ra là`TOO LATE`. 

Điều này cho thấy ngày đầu tháng 10 đã vượt quá khoảng thời gian cho phép mặc dù chúng gần với ngày diễn ra cuộc thi khu vực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Tất cả các tính toán đều liên quan đến mảng có kích thước cố định và số học không đổi | 
| Không gian | O(1) | Chỉ một mảng cố định nhỏ cho độ dài tháng và tổng tiền tố | 

Giải pháp này phù hợp thoải mái trong các ràng buộc vì nó thực hiện tính toán theo thời gian không đổi cho mỗi trường hợp thử nghiệm và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import sys as _sys
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("2023-09-16\n") == "GOOD"
assert run("2023-10-01\n") == "TOO LATE"

# boundary: exactly cutoff
assert run("2023-09-16\n") == "GOOD"

# just after cutoff
assert run("2023-09-17\n") == "TOO LATE"

# earliest date
assert run("2023-01-01\n") == "GOOD"

# late year boundary
assert run("2023-10-28\n") == "TOO LATE"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 16-09-2023 | TỐT | trường hợp đẳng thức biên | 
| 17-09-2023 | QUÁ TRỄ | ngày không hợp lệ đầu tiên sau khi hết hạn | 
| 2023-01-01 | TỐT | xử lý ngày tối thiểu | 
| 28-10-2023 | QUÁ TRỄ | vượt xa giới hạn | 

## Vỏ cạnh 

Một trường hợp tinh vi là ranh giới giới hạn chính xác. Nếu ngày đầu vào chuyển đổi thành chỉ số cùng ngày với ngưỡng được tính toán thì ngày đó vẫn phải được chấp nhận. Thuật toán xử lý việc này một cách chính xác vì nó sử dụng`>`để từ chối, không phải`>=`. 

Một trường hợp khác là ngày cuối tháng mười. Ví dụ,`2023-10-28`chuyển đổi thành chỉ số ngày lớn hơn đáng kể so với ngưỡng, do đó, nó được phân loại chính xác là quá muộn mà không cần logic cụ thể theo tháng. 

Trường hợp cuối cùng là những ngày đầu năm như`2023-01-01`. Những bản đồ này tới các chỉ số ngày rất nhỏ và luôn an toàn trước thời điểm giới hạn, điều này xác nhận rằng cách biểu diễn tổng tiền tố duy trì trật tự trong cả năm.
