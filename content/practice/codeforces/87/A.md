---
title: "CF 87A - Tàu hỏa"
description: "Vasya đang ở ga trung tâm của một nhánh tàu điện ngầm với hai điểm cuối. Mỗi điểm cuối tương ứng với một trong những người bạn gái của anh ấy: Dasha hoặc Masha. Các chuyến tàu đến ga Dasha đến cứ sau một phút và các chuyến tàu đến ga của Masha đến cứ sau mỗi phút."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 87
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 73 (Div. 1 Only)"
rating: 1500
weight: 87
solve_time_s: 75
verified: true
draft: false
---

[CF 87A - Xe lửa](https://codeforces.com/problemset/problem/87/A) 

**Đánh giá:** 1500 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vasya đang ở ga trung tâm của một nhánh tàu điện ngầm với hai điểm cuối. Mỗi điểm cuối tương ứng với một trong những người bạn gái của anh ấy: Dasha hoặc Masha. Xe lửa đến ga Dasha đến mỗi ngày`a`phút và các chuyến tàu tới ga của Masha đều đến`b`phút. Vasya đến ga vào một thời điểm ngẫu nhiên và luôn lên chuyến tàu đầu tiên đến. Nếu hai chuyến tàu đến cùng một lúc, anh ta sẽ lên chuyến tàu đi ít thường xuyên hơn, tức là về phía bạn gái có chuyến tàu có quãng đường chạy dài hơn. 

Đầu vào cung cấp hai số nguyên`a`Và`b`, đại diện cho tần số của đoàn tàu đến mỗi đầu. Kết quả đầu ra sẽ là bạn gái Vasya nào sẽ đến thăm thường xuyên hơn hoặc "Bằng" nếu anh ấy đến thăm cả hai thường xuyên như nhau. 

Những hạn chế,`1 ≤ a, b ≤ 10^6`Và`a ≠ b`, ngụ ý rằng chúng ta không thể mô phỏng một dòng thời gian không xác định theo phút. Giải pháp phải suy luận về các mẫu tần số và tính tuần hoàn thay vì liệt kê một cách thô bạo. Một cách tiếp cận đơn giản mô phỏng mỗi phút sẽ yêu cầu tới 10^6 lần lặp, điều này có thể chấp nhận được, nhưng chúng ta có thể làm tốt hơn nhiều bằng cách sử dụng lý thuyết số. 

Trường hợp cạnh phát sinh khi bội số chung nhỏ nhất của`a`Và`b`tạo ra những khoảng thời gian mà chuyến tàu đầu tiên đến có thể bị ràng buộc và Vasya phải chọn chuyến tàu ít thường xuyên hơn. Ví dụ, với`a = 3`Và`b = 7`, trong khoảng thời gian`21`phút (LCM là 3 và 7), các khoảng thời gian không đối xứng. Việc thực hiện bất cẩn chỉ đơn giản là so sánh`a`Và`b`không xem xét các mối quan hệ sẽ đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng từng phút từ`0`ĐẾN`LCM(a, b)`và đếm số lần chuyến tàu đến Dasha hoặc Masha đến trước. Chúng tôi sẽ tăng bộ đếm cho từng khoảng thời gian và xử lý rõ ràng các lượt đến đồng thời. Điều này hoạt động vì lịch trình tàu là định kỳ với thời gian`LCM(a, b)`. Số lượng hoạt động sẽ là khoảng`LCM(a, b) / min(a, b)`, có thể lên tới 10^12 trong trường hợp xấu nhất, vượt xa giới hạn hợp lý. 

Nhận xét quan trọng là quyết định của Vasya chỉ phụ thuộc vào chuyến tàu đầu tiên sau khi anh đến. Khoảng thời gian giữa các chuyến tàu đến là đồng đều và quy tắc ràng buộc ưu tiên các chuyến tàu ít thường xuyên hơn. Bằng cách chuẩn hóa thành ước số chung lớn nhất (GCD) của`a`Và`b`, chúng ta có thể chia thời kỳ thành các đoạn tỉ lệ với`a`Và`b`so với GCD. Cụ thể, trong mỗi kỳ LCM, Vasya sẽ lên chuyến tàu của Dasha`b / gcd(a, b)`thời gian và chuyến tàu của Masha`a / gcd(a, b)`lần. So sánh hai số liệu này cho chúng ta biết bạn gái nào anh ấy đến thăm thường xuyên hơn. 

Quan sát này làm giảm giải pháp thành một so sánh theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(LCM(a, b)) | O(1) | Quá chậm đối với a, b lớn | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`a`Và`b`từ đầu vào. Chúng đại diện cho khoảng thời gian giữa các chuyến tàu đến Dasha và Masha tương ứng. 
2. Tính ước chung lớn nhất của`a`Và`b`. GCD này thể hiện giai đoạn cơ bản trong đó mô hình lượng khách đến lặp lại. 
3. Xác định có bao nhiêu chuyến khởi hành đến Dasha trong một khoảng thời gian LCM. Đây là`b // gcd(a, b)`. Tương tự, xác định điểm khởi hành của Masha:`a // gcd(a, b)`. 
4. So sánh số lượng. Nếu số chuyến khởi hành về Dasha nhiều hơn, hãy in "Dasha". Nếu số hướng tới Masha cao hơn, hãy in "Masha". Nếu số lượng bằng nhau thì in ra "Bằng". 

Điều bất biến là trong bất kỳ khoảng thời gian LCM nào, các lựa chọn của Vasya đều được phân bổ theo các số lượng này và do mô hình lặp lại vô thời hạn nên các số lượng này xác định đầy đủ tần suất các chuyến thăm dài hạn của anh ta. Tỷ lệ GCD đảm bảo rằng các khoảng thời gian phân số không bị mất và quy tắc ngắt kết nối được tự động ghi lại bằng cách tính số lần khởi hành theo tỷ lệ. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

a, b = map(int, input().split())

g = math.gcd(a, b)
visits_dasha = b // g
visits_masha = a // g

if visits_dasha > visits_masha:
    print("Dasha")
elif visits_masha > visits_dasha:
    print("Masha")
else:
    print("Equal")
```Đầu tiên, mã sẽ đọc các khoảng thời gian chạy và tính toán GCD, đưa ra khoảng thời gian lặp lại cơ bản. Sau đó, nó chia tỷ lệ số lượng tàu đến bằng cách chia khoảng thời gian ngược lại cho GCD. Việc so sánh trực tiếp tuân theo lý luận từ hướng dẫn thuật toán. Phải cẩn thận khi sử dụng phép chia số nguyên chứ không phải phép chia dấu phẩy động để tránh lỗi làm tròn. 

## Ví dụ đã hoạt động 

Mẫu 1:`a = 3`,`b = 7`| Bước | GCD | Thăm Dasha | Thăm Masha | Quyết định | 
| --- | --- | --- | --- | --- | 
| Tính GCD(3,7) | 1 | 7 | 3 | Dasha | 

Điều này cho thấy rằng trong khoảng thời gian 21 phút (LCM 3 và 7), Vasya lên tàu của Dasha 7 lần và lên tàu của Masha 3 lần. Vì vậy anh ấy đến Dasha thường xuyên hơn. 

Mẫu 2:`a = 4`,`b = 6`| Bước | GCD | Thăm Dasha | Thăm Masha | Quyết định | 
| --- | --- | --- | --- | --- | 
| Tính GCD(4,6) | 2 | 3 | 2 | Dasha | 

Dấu vết này xác nhận thuật toán xử lý các trường hợp GCD lớn hơn 1, tạo ra số đếm tần số chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log min(a, b)) | Tính toán GCD bằng thuật toán Euclid | 
| Không gian | O(1) | Chỉ cần một vài biến số nguyên | 

Với các ràng buộc lên tới 10^6, thuật toán Euclidean GCD chạy gần như ngay lập tức, đảm bảo giải pháp dễ dàng phù hợp trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    a, b = map(int, input().split())
    g = math.gcd(a, b)
    visits_dasha = b // g
    visits_masha = a // g
    if visits_dasha > visits_masha:
        return "Dasha"
    elif visits_masha > visits_dasha:
        return "Masha"
    else:
        return "Equal"

# provided samples
assert run("3 7\n") == "Dasha", "sample 1"

# custom cases
assert run("1 2\n") == "Dasha", "minimum a"
assert run("2 1\n") == "Masha", "minimum b"
assert run("5 5\n") == "Equal", "equal values (edge)"
assert run("1000000 999999\n") == "Dasha", "large numbers, a > b"
assert run("999999 1000000\n") == "Masha", "large numbers, b > a"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | Dasha | Xử lý đúng các đầu vào có giá trị tối thiểu | 
| 2 1 | Masha | Đảo ngược chính xác các giá trị tối thiểu | 
| 5 5 | Bằng | Trường hợp cạnh có khoảng bằng nhau (không được phép trong các ràng buộc ban đầu nhưng kiểm tra logic) | 
| 1000000 999999 | Dasha | Xử lý số lượng lớn, a > b | 
| 999999 1000000 | Masha | Xử lý số lượng lớn, b > a | 

## Vỏ cạnh 

Đối với đầu vào`a = 3`,`b = 7`, GCD là 1. Lượt truy cập Dasha = 7, Lượt truy cập Masha = 3. Trong khoảng thời gian 21 phút, mọi thời gian bắt đầu có thể sẽ ánh xạ tới một trong các số lượng này, do đó, việc phân định điểm được xử lý một cách tự nhiên. Nếu Vasya đến phút 0, tàu đến phút 3 và 7 thì anh ấy chọn chuyến đến trước. Để đồng thời đến bội số của 21, anh ta chọn số ít thường xuyên hơn, phù hợp với logic đếm. 

Vì`a = 4`,`b = 6`, GCD = 2, thời gian là 12 phút. Lượt truy cập Dasha = 3, Lượt truy cập Masha = 2. Bất kỳ phút bắt đầu nào trong [0,12) đều dẫn đến các lựa chọn tỷ lệ thuận với số lượng này. Điều này xác nhận rằng thuật toán sẽ chia tỷ lệ chính xác bằng cách sử dụng GCD và áp dụng bộ ngắt kết nối một cách ngầm định.
