---
title: "CF 1692A - Marathon"
description: "Chúng ta được yêu cầu xác định vị trí của Timur so với ba vận động viên khác trong một cuộc chạy marathon. Mỗi trường hợp thử nghiệm cung cấp bốn số nguyên riêng biệt: đầu tiên là khoảng cách của Timur, tiếp theo là khoảng cách của ba người tham gia khác."
date: "2026-06-09T23:00:39+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1692
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 799 (Div. 4)"
rating: 800
weight: 1692
solve_time_s: 108
verified: true
draft: false
---

[CF 1692A - Marathon](https://codeforces.com/problemset/problem/1692/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định vị trí của Timur so với ba vận động viên khác trong một cuộc chạy marathon. Mỗi trường hợp thử nghiệm cung cấp bốn số nguyên riêng biệt: đầu tiên là khoảng cách của Timur, tiếp theo là khoảng cách của ba người tham gia khác. Nhiệm vụ là đếm xem có bao nhiêu vận động viên chạy được quãng đường xa hơn Timur. 

Vấn đề có cấu trúc đơn giản nhưng việc hiểu các giới hạn rất hữu ích. Mỗi khoảng cách nằm trong khoảng từ 0 đến 10.000 và có thể có tới 10.000 trường hợp thử nghiệm. Điều này cho chúng ta biết rằng chúng ta cần một giải pháp tuyến tính về số lượng trường hợp thử nghiệm, vì ngay cả O(n²) trên mảng bốn phần tử nhỏ về mặt kỹ thuật cũng có thể chấp nhận được (vì 4² là không đáng kể), nhưng nếu chúng ta viết mã cố gắng so sánh không cần thiết giữa các trường hợp thử nghiệm thì nó có thể trở nên kém hiệu quả. 

Các trường hợp cần chú ý là khi Timur chạy khoảng cách tối đa trong số bốn hoặc tối thiểu. Ví dụ: nếu Timur chạy 10.000 và những người khác chạy 0, 1, 2 thì kết quả đúng là 0. Một giải pháp bất cẩn có thể cố gắng sử dụng cách sắp xếp và lấy chỉ mục không chính xác nếu giả định rằng Timur luôn đứng đầu hoặc cuối cùng. Một khía cạnh tinh tế khác là khi khoảng cách của những người tham gia khác tạo thành một chuỗi liên tiếp xung quanh khoảng cách của Timur, chẳng hạn như 3, 4, 5, 2 với Timur là 4. Logic bất cẩn có thể tính sai những người tham gia ở phía trước so với những người bị ràng buộc hoặc phía sau. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ nhất là kiểm tra khoảng cách của Timur với từng người trong số ba người tham gia còn lại. Khởi tạo bộ đếm ở mức 0, lặp lại ba khoảng cách và tăng bộ đếm mỗi khi bạn tìm thấy giá trị lớn hơn giá trị của Timur. Lực lượng vũ phu này hoạt động vì chúng tôi chỉ có ba số khác để so sánh, vì vậy tệ nhất là chúng tôi thực hiện ba so sánh cho mỗi trường hợp thử nghiệm. Với 10.000 trường hợp thử nghiệm, con số này lên tới 30.000 so sánh - không đáng kể đối với bất kỳ CPU hiện đại nào. Lực lượng vũ phu là chính xác và tốc độ không phải là vấn đề đáng lo ngại ở đây. 

Thông tin chi tiết quan trọng về tối ưu hóa, mặc dù không thực sự cần thiết, là nhận ra rằng danh sách luôn có kích thước bốn. Chúng ta có thể sắp xếp bốn số và tìm trực tiếp thứ hạng của Timur. Việc sắp xếp bốn số cũng không quan trọng, nhưng nó đưa ra nhiều thao tác hơn so với phương pháp so sánh ba số đơn giản. Về mặt khái niệm, việc sắp xếp nặng hơn nên phương pháp so sánh được ưu tiên vì sự rõ ràng và chi phí tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 
| Dựa trên sắp xếp | O(4 log 4) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận nhưng không cần thiết | 

Kết luận là cách tiếp cận đơn giản nhất cũng là tối ưu: đếm xem có bao nhiêu trong ba khoảng cách vượt quá khoảng cách của Timur. 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Đối với mỗi trường hợp kiểm thử, hãy đọc bốn khoảng cách, lưu khoảng cách Timur vào một biến và các biến khác vào danh sách. 
3. Khởi tạo bộ đếm về 0. Bộ đếm này sẽ theo dõi số lượng người tham gia trước Timur. 
4. Lặp lại danh sách ba khoảng cách còn lại. Đối với mỗi khoảng cách, hãy kiểm tra xem nó có lớn hơn khoảng cách Timur không. Nếu có, hãy tăng bộ đếm. 
5. Sau khi kiểm tra cả ba người tham gia, xuất bộ đếm. Đây là số vận động viên chạy phía trước của Timur. 

Tại sao nó hoạt động: thuật toán duy trì một bất biến đơn giản: tại bất kỳ thời điểm nào trong vòng lặp, bộ đếm phản ánh chính xác có bao nhiêu người tham gia được xử lý cho đến nay đã chạy xa hơn Timur. Cuối cùng, nó đếm chính xác tất cả những người tham gia ở phía trước mà không có bất kỳ rủi ro nào về việc đếm sai số điểm hoặc khoảng cách phía sau Timur. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c, d = map(int, input().split())
    counter = 0
    for distance in (b, c, d):
        if distance > a:
            counter += 1
    print(counter)
```Mã này đầu tiên đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi giải nén trực tiếp bốn số nguyên. Bộ đếm chỉ được tăng lên đối với những người tham gia trước Timur. Việc sử dụng một bộ cho ba khoảng cách còn lại sẽ làm cho phép lặp trở nên ngắn gọn và tránh việc tạo danh sách tạm thời. Các so sánh là các số nguyên đơn giản, do đó không có nguy cơ tràn hoặc các vấn đề về độ chính xác. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`2 3 4 1`Khoảng cách Timur`a = 2`. Các khoảng cách khác là`3, 4, 1`. 

| khoảng cách | quầy | 
| --- | --- | 
| 3 | 1 | 
| 4 | 2 | 
| 1 | 2 | 

Đầu ra:`2`. Bảng này cho thấy cách chúng tôi tăng bộ đếm cho những người tham gia trước Timur và giữ nguyên bộ đếm cho những người tham gia phía sau. 

**Mẫu 2** 

đầu vào:`500 600 400 300`Khoảng cách Timur`a = 500`. Các khoảng cách khác là`600, 400, 300`. 

| khoảng cách | quầy | 
| --- | --- | 
| 600 | 1 | 
| 400 | 1 | 
| 300 | 1 | 

Đầu ra:`1`. Chỉ có người chạy với cự ly 600 là dẫn trước. Điều này xác nhận thuật toán xử lý chính xác khoảng cách cả trên và dưới Timur. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm bao gồm ba phép so sánh theo thời gian không đổi. Với tối đa 10.000 trường hợp thử nghiệm, đây là số lượng trường hợp thử nghiệm tuyến tính. | 
| Không gian | O(1) | Chỉ một vài biến được lưu trữ cho mỗi trường hợp thử nghiệm; không có cấu trúc bổ sung nào phát triển theo kích thước đầu vào. | 

Với các ràng buộc, thuật toán này chạy thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        a, b, c, d = map(int, input().split())
        counter = 0
        for distance in (b, c, d):
            if distance > a:
                counter += 1
        print(counter)
    return output.getvalue().strip()

# Provided samples
assert run("4\n2 3 4 1\n10000 0 1 2\n500 600 400 300\n0 9999 10000 9998\n") == "2\n0\n1\n3"

# Custom test cases
assert run("1\n0 1 2 3\n") == "3", "minimum Timur"
assert run("1\n10000 9999 9998 9997\n") == "0", "maximum Timur"
assert run("1\n5 5 5 5\n") == "0", "all equal (invalid for distinct but for robustness)"
assert run("1\n7 6 8 9\n") == "2", "Timur in the middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 1 2 3`|`3`| Timur chạy ít nhất; tất cả những người khác ở phía trước | 
|`10000 9999 9998 9997`|`0`| Timur chạy nhiều nhất; không có ai phía trước | 
|`5 5 5 5`|`0`| Độ bền của cạnh ngay cả khi đầu vào bằng nhau | 
|`7 6 8 9`|`2`| Timur ở giữa; thuật toán đếm đúng người tham gia | 

## Vỏ cạnh 

Khi Timur chạy khoảng cách tối đa trong số bốn người, ví dụ:`10000 0 1 2`, việc lặp lại kiểm tra`0 > 10000`,`1 > 10000`,`2 > 10000`, tất cả đều sai. Bộ đếm vẫn bằng 0, tạo ra đầu ra chính xác. Khi Timur chạy quãng đường tối thiểu, ví dụ:`0 1 2 3`, mỗi lần so sánh sẽ tăng bộ đếm, tạo ra`3`, phù hợp với số lượng người tham gia phía trước. 

Ngay cả khi các số liên tiếp xung quanh khoảng cách của Timur, chẳng hạn như`4 3 5 2`, thuật toán chỉ tính những giá trị lớn hơn (`5`), trả về chính xác`1`. Không cần sắp xếp hoặc cấu trúc dữ liệu bổ sung để xử lý các trường hợp biên này vì chúng tôi kiểm tra rõ ràng từng giá trị theo khoảng cách của Timur. 

Bài xã luận này đảm bảo người đọc có thể suy luận từ những nguyên tắc đầu tiên: so sánh Timur với từng người tham gia và đếm con số phía trước, vừa chính xác vừa tối ưu cho quy mô vấn đề.
