---
title: "CF 146B - Mặt nạ may mắn"
description: "Chúng ta được cho hai số nguyên. Số đầu tiên a là tùy ý. Số thứ hai, b, được đảm bảo là số may mắn, nghĩa là mỗi chữ số của b là 4 hoặc 7. Đối với bất kỳ số nguyên dương nào, \"mặt nạ\" của nó được hình thành bằng cách chỉ lấy các chữ số 4 và 7 từ trái sang phải rồi ghép chúng lại."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 146
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 104 (Div. 2)"
rating: 1300
weight: 146
solve_time_s: 90
verified: true
draft: false
---

[CF 146B - Mặt nạ may mắn](https://codeforces.com/problemset/problem/146/B) 

**Đánh giá:** 1300 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên. Số đầu tiên,`a`, là tùy ý. Số thứ hai,`b`, được đảm bảo là may mắn, nghĩa là mọi chữ số của`b`là một trong hai`4`hoặc`7`. 

Đối với bất kỳ số nguyên dương nào, "mặt nạ" của nó được hình thành bằng cách chỉ lấy các chữ số`4`Và`7`từ trái sang phải và nối chúng lại. Ví dụ, mặt nạ của`72174994`là`7744`vì đó là những chữ số may mắn xuất hiện theo thứ tự. 

Nhiệm vụ là tìm số nguyên nhỏ nhất`c`như vậy`c > a`và mặt nạ của`c`bằng`b`. 

Những hạn chế là nhỏ. Cả hai số nhiều nhất là`100000`, do đó ngay cả việc mô phỏng trực tiếp trên nhiều số ứng cử viên cũng hoàn toàn thực tế. Giới hạn hai giây với các giá trị xung quanh`10^5`thường cho phép vài triệu thao tác đơn giản một cách thoải mái. Điều đó có nghĩa là ngay cả việc kiểm tra từng số một cũng có thể chấp nhận được nếu mỗi tờ séc rẻ. 

Phần khó khăn không phải là hiệu suất, mà là trích xuất mặt nạ một cách chính xác. 

Một sai lầm dễ mắc phải là quên rằng các chữ số không may mắn sẽ bị bỏ qua thay vì dừng quá trình. 

Ví dụ: 

đầu vào:```
1 47
```Câu trả lời đúng là:```
47
```Một giải pháp bất cẩn có thể từ chối`47`bởi vì nó chứa các chữ số khác trước hoặc sau các chữ số may mắn ở một số ứng cử viên sau này, nhưng mặt nạ chỉ phụ thuộc vào dãy con của các chữ số may mắn. 

Một trường hợp tế nhị khác xảy ra khi bản thân con số đó là may mắn. 

đầu vào:```
1 7
```Câu trả lời đúng là:```
7
```Mặt nạ của`7`vẫn còn`7`. Việc triển khai có lỗi loại bỏ tất cả các chữ số trước và vô tình coi mặt nạ là trống sẽ không thành công ở đây. 

Ngoài ra còn có một điều kiện biên liên quan đến các số không có chữ số may mắn nào cả. 

đầu vào:```
40 4
```Câu trả lời đúng là:```
44
```số`41`có mặt nạ`4`, vì vậy thực tế câu trả lời hợp lệ nhỏ nhất là:```
41
```Việc thực hiện bất cẩn mà chỉ kiểm tra các số hoàn toàn may mắn sẽ bỏ qua nhiều câu trả lời hợp lệ một cách sai lầm. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là vũ lực. Bắt đầu từ`a + 1`, tính mặt nạ của mỗi số và dừng khi mặt nạ bằng`b`. 

Để tính mặt nạ, quét các chữ số từ trái sang phải và chỉ giữ lại`4`Và`7`. Nếu trình tự kết quả khớp`b`, chúng tôi đã tìm thấy câu trả lời. 

Điều này hoạt động vì các ràng buộc rất nhỏ. Ngay cả khi chúng ta kiểm tra hàng chục hay hàng trăm nghìn số thì mỗi phép tính mặt nạ cũng chỉ chạm đến một vài chữ số. Một con số lên tới`10^5`có nhiều nhất là sáu chữ số nên tổng công việc rất nhỏ. 

Phương pháp vũ phu đã trôi qua một cách thoải mái. 

Quan sát quan trọng là không gian tìm kiếm bị giới hạn một cách tự nhiên. Từ`b`bản thân nó có tối đa sáu chữ số và chỉ những chữ số may mắn mới quan trọng, chúng ta không cần bất kỳ tổ hợp nâng cao hoặc lập trình động nào. Mô phỏng đơn giản nhất đã đủ tối ưu. 

Một số người cố gắng xây dựng câu trả lời theo từng chữ số, nhưng điều đó tạo ra sự phức tạp không cần thiết. Điều kiện mặt nạ cực kỳ rẻ để kiểm tra trực tiếp nên tìm kiếm tuần tự là giải pháp rõ ràng nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k · d) | O(d) | Đã chấp nhận | 
| Tối ưu | O(k · d) | O(d) | Đã chấp nhận | 

Đây,`k`là số số nguyên được kiểm tra và`d`là số chữ số trên mỗi số nguyên. Từ`d ≤ 6`, thời gian chạy thực tế là tuyến tính trong phạm vi tìm kiếm. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`a`Và`b`. 
2. Chuyển đổi`b`thành một chuỗi vì mặt nạ dễ so sánh nhất dưới dạng chuỗi. 
3. Bắt đầu lặp từ`a + 1`trở lên. 
4. Đối với mỗi số ứng cử viên, hãy chuyển nó thành một chuỗi và tạo mặt nạ của nó. 

Quét từng chữ số từ trái sang phải. Bất cứ khi nào chữ số là`4`hoặc`7`, gắn nó vào mặt nạ. 
5. So sánh mặt nạ đã tạo với`b`. 

Nếu chúng bằng nhau thì ứng viên này là câu trả lời hợp lệ nhỏ nhất vì chúng ta đang kiểm tra các số theo thứ tự tăng dần. 
6. In ứng viên và chấm dứt. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra mọi số nguyên lớn hơn`a`theo thứ tự tăng dần chặt chẽ. Với mỗi số nguyên, nó tính toán chính xác định nghĩa của mặt nạ bằng cách chỉ giữ lại các chữ số may mắn theo thứ tự. 

Số đầu tiên có mặt nạ bằng`b`phải là câu trả lời hợp lệ tối thiểu vì không tồn tại số nào nhỏ hơn không được kiểm tra. Vì mọi ứng viên đều được kiểm tra chính xác theo định nghĩa mặt nạ nên thuật toán không thể bỏ sót câu trả lời hoặc trả về số không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b = input().split()
    a = int(a)

    x = a + 1

    while True:
        mask = []

        for ch in str(x):
            if ch == '4' or ch == '7':
                mask.append(ch)

        if ''.join(mask) == b:
            print(x)
            return

        x += 1

solve()
```Giải pháp trực tiếp theo thuật toán. 

Đầu vào được chia thành hai chuỗi vì`b`được so sánh một cách tự nhiên nhất dưới dạng văn bản. Chúng tôi chỉ chuyển đổi`a`thành một số nguyên vì chúng ta cần số học trên đó. 

Vòng lặp bắt đầu từ`a + 1`bởi vì câu trả lời phải lớn hơn`a`. 

Đối với mỗi số ứng viên`x`, chúng tôi quét từng ký tự biểu diễn thập phân của nó. Bất cứ khi nào chúng ta gặp phải`4`hoặc`7`, chúng tôi thêm nó vào danh sách mặt nạ. Việc tham gia danh sách sẽ tạo ra chuỗi mặt nạ chính xác. 

Việc sử dụng chuỗi sẽ tránh được các lỗi liên quan đến logic nối đầu hoặc số học số nguyên. Vì số lượng rất nhỏ nên cách tiếp cận này vừa đơn giản vừa hiệu quả. 

Thứ tự kiểm tra có vấn đề. Chúng tôi kiểm tra các ứng viên một cách tuần tự, vì vậy ứng viên thành công đầu tiên đương nhiên là rất ít. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 7
```| Ứng viên`x`| Chữ số được quét | Mặt nạ được xây dựng | Trận đấu`7`| 
| --- | --- | --- | --- | 
| 2 | 2 | "" | Không | 
| 3 | 3 | "" | Không | 
| 4 | 4 | "4" | Không | 
| 5 | 5 | "" | Không | 
| 6 | 6 | "" | Không | 
| 7 | 7 | "7" | Có | 

Đầu ra:```
7
```Dấu vết này cho thấy những con số không có chữ số may mắn chỉ tạo ra một mặt nạ trống. Thuật toán tiếp tục quét cho đến khi mặt nạ khớp chính xác với mục tiêu. 

### Ví dụ 2 

đầu vào:```
40 4
```| Ứng viên`x`| Chữ số được quét | Mặt nạ được xây dựng | Trận đấu`4`| 
| --- | --- | --- | --- | 
| 41 | 4, 1 | "4" | Có | 

Đầu ra:```
41
```Ví dụ này chứng tỏ tại sao chúng ta không thể hạn chế bản thân ở những con số hoàn toàn may mắn. Câu trả lời có thể chứa các chữ số không may mắn tùy ý miễn là các chữ số may mắn được trích xuất tạo thành mặt nạ được yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k · d) | Chúng tôi kiểm tra`k`số lượng ứng cử viên, mỗi số có nhiều nhất`d`chữ số | 
| Không gian | O(d) | Mặt nạ lưu trữ tối đa tất cả các chữ số của một số | 

Đây,`d`nhiều nhất là 6 vì mọi số đều nhỏ. Ngay cả khi chúng tôi quét nhiều ứng viên, tổng số thao tác chữ số vẫn rất nhỏ so với thời hạn. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    a, b = input().split()
    a = int(a)

    x = a + 1

    while True:
        mask = []

        for ch in str(x):
            if ch == '4' or ch == '7':
                mask.append(ch)

        if ''.join(mask) == b:
            print(x)
            return

        x += 1

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue()

# provided sample
assert run("1 7\n") == "7\n", "sample 1"

# minimum-style case
assert run("1 4\n") == "4\n", "smallest lucky target"

# mask appears inside non-lucky number
assert run("40 4\n") == "41\n", "non-lucky digits ignored"

# multiple lucky digits
assert run("100 47\n") == "147\n", "mixed digits produce target mask"

# answer is fully lucky
assert run("46 47\n") == "47\n", "direct lucky number"

# larger boundary-style case
assert run("99999 4\n") == "100004\n", "crossing digit length boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 4`|`4`| Câu trả lời hợp lệ đơn giản nhỏ nhất | 
|`40 4`|`41`| Những chữ số không may mắn phải được bỏ qua | 
|`100 47`|`147`| Mặt nạ có thể xuất hiện bên trong với số lượng lớn hơn | 
|`46 47`|`47`| Những con số may mắn hoàn toàn hợp lệ | 
|`99999 4`|`100004`| Xử lý đúng sau khi tăng số chữ số | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
40 4
```Thuật toán bắt đầu lúc`41`. 

Vì`41`, các chữ số được quét từ trái sang phải: 

-`4`là may mắn nên nó được thêm vào mặt nạ. 
-`1`bị bỏ qua. 

Mặt nạ được xây dựng trở thành`"4"`, phù hợp với mục tiêu ngay lập tức. Thuật toán in`41`. 

Trường hợp này xác nhận rằng các chữ số tùy ý giữa các chữ số may mắn không thành vấn đề. 

Bây giờ hãy xem xét:```
1 7
```Thuật toán kiểm tra các số một cách tuần tự: 

-`2`,`3`,`5`, Và`6`sản xuất mặt nạ trống rỗng. 
-`4`sản xuất mặt nạ`"4"`. 
-`7`sản xuất mặt nạ`"7"`. 

Trận đầu tiên là`7`, do đó kết quả đầu ra là chính xác. 

Điều này xác minh rằng một chữ số may mắn duy nhất hoạt động chính xác và mặt nạ trống được xử lý an toàn. 

Cuối cùng, hãy xem xét:```
99999 4
```Thuật toán tiếp tục chuyển các số có năm chữ số thành số có sáu chữ số. 

Ứng cử viên đầu tiên đeo mặt nạ`"4"`là:```
100004
```Các chữ số của nó chứa đúng một chữ số may mắn,`4`, vậy mặt nạ là`"4"`. 

Điều này xác nhận rằng việc triển khai xử lý việc mở rộng độ dài chữ số một cách chính xác và không dựa vào các giả định về chiều rộng cố định.
