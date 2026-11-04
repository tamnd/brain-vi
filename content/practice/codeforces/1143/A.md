---
title: "CF 1143A - Cánh Cửa"
description: "Vấn đề có thể được trình bày lại như sau. Ông Đen có một ngôi nhà có hai bộ cửa, mỗi bộ dẫn đến một lối ra riêng: trái và phải. Mỗi cánh cửa ban đầu đều đóng và chúng ta biết chính xác trình tự ông Black mở chúng."
date: "2026-06-12T03:35:26+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1143
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 549 (Div. 2)"
rating: 800
weight: 1143
solve_time_s: 71
verified: true
draft: false
---

[CF 1143A - Những cánh cửa](https://codeforces.com/problemset/problem/1143/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề có thể được trình bày lại như sau. Ông Đen có một ngôi nhà có hai bộ cửa, mỗi bộ dẫn đến một lối ra riêng: trái và phải. Mỗi cánh cửa ban đầu đều đóng và chúng ta biết chính xác trình tự ông Black mở chúng. Khi tất cả các cửa ở ít nhất một lối ra đều mở, anh ta có thể rời khỏi nhà. Nhiệm vụ là tìm ra điểm sớm nhất trong chuỗi của anh ta khi điều này có thể thực hiện được. 

Đầu vào bao gồm một số nguyên`n`đại diện cho tổng số cửa, theo sau là danh sách`n`số nguyên trong đó mỗi số nguyên là`0`nếu cửa thuộc lối ra bên trái, hoặc`1`nếu nó thuộc về lối ra bên phải. Chúng ta cần xác định tiền tố nhỏ nhất của chuỗi này để mở tất cả các cửa ở một trong các lối ra. Đầu ra là độ dài tiền tố này`k`. 

Các ràng buộc cho phép lên đến`n = 200,000`cửa. Điều này loại trừ bất kỳ cách tiếp cận nào có độ phức tạp thời gian bậc hai, bởi vì`n^2`hoạt động sẽ đạt 40 tỷ, vượt xa giới hạn thời gian 1 giây. Các giải pháp tuyến tính hoặc gần tuyến tính đều khả thi. Trường hợp chính cần xem xét là khi một lối ra chỉ có một cửa; nếu cánh cửa đó được mở lần cuối, phương pháp kiểm tra tiền tố ngây thơ có thể báo cáo không chính xác cơ hội thoát ra đầu tiên. 

Một ví dụ nhỏ nhưng minh họa là`n = 3`với trình tự`1 0 0`. Lối ra bên trái có hai cửa, lối ra bên phải có một. Sau khi mở cánh cửa đầu tiên (lối ra bên phải) thì ông Đen đã có thể rời đi rồi nên kết quả đúng là`1`. Cách tiếp cận bất cẩn chỉ đếm tổng số cửa đã mở hoặc quét từ lối ra bên trái trước có thể tạo ra kết quả không chính xác.`3`. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: với mỗi tiền tố của chuỗi, hãy kiểm tra xem tất cả các cửa của lối ra bên trái đều mở hay tất cả các cửa của lối ra bên phải đều mở. Để thực hiện điều này, chúng tôi sẽ duy trì hai bộ: một bộ dành cho các cửa vẫn đóng ở lối ra bên trái và một bộ dành cho lối ra bên phải. Đối với mỗi cánh cửa được mở, hãy loại bỏ nó khỏi bộ tương ứng. Khi một trong hai bộ trở nên trống, hãy xuất chỉ mục hiện tại. Điều này đúng, nhưng việc xây dựng các tập hợp và kiểm tra tính trống ở mỗi bước liên quan đến`O(n)`hoạt động trên mỗi tiền tố, đưa ra`O(n^2)`tổng cộng. Với`n`lên tới 200.000, cách tiếp cận này quá chậm. 

Cách tiếp cận tối ưu dựa vào việc đếm. Đầu tiên, chúng ta đếm xem có bao nhiêu cửa thuộc lối ra bên trái và bao nhiêu cửa ở bên phải. Sau đó, chúng tôi duy trì số lượng cửa được mở ở mỗi lối ra khi chúng tôi lặp lại trình tự. Khi một trong hai bộ đếm đạt đến tổng số cửa ở lối ra đó, chúng ta biết rằng tất cả các cửa ở lối ra đó đều mở và chúng ta có thể trả về ngay vị trí hiện tại làm câu trả lời. Điều này hoạt động trong thời gian tuyến tính`O(n)`bởi vì mỗi cửa được xử lý chính xác một lần và chỉ cần thêm không gian liên tục cho các quầy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai biến`left_total`Và`right_total`để đếm số cửa ở mỗi lối ra. Lặp lại thông tin đầu vào một lần để điền các số đếm này. Điều này là cần thiết để biết số lượng mục tiêu khi tất cả các cửa ở lối ra đều mở. 
2. Khởi tạo hai bộ đếm`left_opened`Và`right_opened`về không. Chúng sẽ theo dõi có bao nhiêu cửa ở mỗi lối ra đã được mở cho đến nay. 
3. Lặp lại trình tự các cửa bằng một lần duy nhất. Đối với mỗi cửa, tăng bộ đếm tương ứng (`left_opened`cho một`0`Và`right_opened`cho một`1`). 
4. Sau khi cập nhật bộ đếm, hãy kiểm tra xem nó có bằng tổng số cửa của lối ra đó không (`left_opened == left_total`hoặc`right_opened == right_total`). Lần đầu tiên điều kiện này được giữ, xuất chỉ mục hiện tại (dựa trên 1) và chấm dứt. 
5. Vì bài toán đảm bảo rằng mọi cánh cửa cuối cùng đều được mở nên chúng ta đảm bảo sẽ tìm ra giải pháp trước khi đi đến cuối chuỗi. 

Tại sao nó hoạt động: ở mỗi bước,`left_opened`Và`right_opened`thể hiện chính xác số lượng cửa hiện đang mở ở mỗi lối ra. Khi một trong số này khớp với tổng số cửa ở lối ra đó, điều đó có nghĩa là tất cả các cửa ở lối ra đó đều mở, thỏa mãn điều kiện thoát ra. Bởi vì chúng ta quay lại ngay lập tức ở lần xuất hiện đầu tiên nên chúng ta đảm bảo tìm được giá trị nhỏ nhất`k`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
doors = list(map(int, input().split()))

left_total = doors.count(0)
right_total = n - left_total

left_opened = 0
right_opened = 0

for i, door in enumerate(doors, start=1):
    if door == 0:
        left_opened += 1
    else:
        right_opened += 1
    if left_opened == left_total or right_opened == right_total:
        print(i)
        break
```Giải pháp bắt đầu bằng cách đếm xem mỗi lối ra có bao nhiêu cửa. Sau đó, bộ đếm cho các cửa đã mở sẽ được cập nhật khi chúng tôi lặp lại trình tự. các`enumerate`chức năng được sử dụng với`start=1`để phù hợp với đầu ra dựa trên 1 được yêu cầu. Kiểm tra có điều kiện đảm bảo rằng khoảnh khắc đầu tiên lối ra được mở hoàn toàn sẽ được trả về ngay lập tức. sử dụng`doors.count(0)`là một đường truyền tuyến tính đơn giản không đáng kể so với vòng lặp chính, giữ cho độ phức tạp tổng thể là tuyến tính. Các lỗi xảy ra có thể tránh được bằng cách lập chỉ mục dựa trên 1 trong`enumerate`. 

## Ví dụ đã hoạt động 

Mẫu 1:`5`, sự liên tiếp`0 0 1 0 0`. 

| tôi | cửa | left_opened | đúng_opened | Tình trạng | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | sai | 
| 2 | 0 | 2 | 0 | sai | 
| 3 | 1 | 2 | 1 | đúng | 

Sau khi mở cửa thứ 3 thì tất cả các cửa ở lối ra bên phải (chỉ có một cửa) đều mở nên kết quả đầu ra là`3`. 

Mẫu 2:`6`, sự liên tiếp`1 0 1 0 0 1`. 

| tôi | cửa | left_opened | đúng_opened | Tình trạng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | sai | 
| 2 | 0 | 1 | 1 | sai | 
| 3 | 1 | 1 | 2 | sai | 
| 4 | 0 | 2 | 2 | sai | 
| 5 | 0 | 3 | 2 | đúng | 

Tất cả các cửa bên trái (tổng cộng 3) đều mở sau cánh cửa thứ năm, vì vậy câu trả lời là`5`. Dấu vết này xác nhận rằng thuật toán theo dõi các bộ đếm một cách chính xác và xác định tiền tố tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc đếm các cửa và lặp qua chuỗi đều là tuyến tính trong`n`| 
| Không gian | O(1) | Chỉ có bốn bộ đếm số nguyên được sử dụng, độc lập với`n`| 

Thuật toán dễ dàng xử lý kích thước đầu vào tối đa 200.000 cửa trong chưa đầy một giây. Việc sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    doors = list(map(int, input().split()))
    left_total = doors.count(0)
    right_total = n - left_total
    left_opened = right_opened = 0
    for i, door in enumerate(doors, start=1):
        if door == 0:
            left_opened += 1
        else:
            right_opened += 1
        if left_opened == left_total or right_opened == right_total:
            return str(i)

# provided samples
assert run("5\n0 0 1 0 0\n") == "3", "sample 1"
assert run("6\n1 0 1 0 0 1\n") == "5", "sample 2"

# custom cases
assert run("2\n0 1\n") == "1", "minimum size, either door first opens an exit"
assert run("4\n1 1 0 0\n") == "2", "first exit fully opened immediately"
assert run("5\n0 0 0 1 1\n") == "3", "left exit completes before right"
assert run("5\n1 1 1 0 0\n") == "3", "right exit completes before left"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n0 1 | 1 | số lượng cửa tối thiểu, thoát ngay lập tức | 
| 4\n1 1 0 0 | 2 | lối ra đầu tiên kết thúc sớm theo trình tự | 
| 5\n0 0 0 1 1 | 3 | lối ra bên trái hoàn thành trước, lối ra bên phải chưa hoàn thành | 
| 5\n1 1 1 0 0 | 3 | lối ra bên phải hoàn thành trước, bên trái chưa hoàn thành | 

## Vỏ cạnh 

Đối với lối ra một cửa, thuật toán sẽ ngay lập tức phát hiện lối ra khi cánh cửa đó được mở. đầu vào`2\n0 1\n`cho thấy việc mở cửa đầu tiên (lối ra bên phải) cho phép ông Black rời đi mặc dù lối ra bên trái vẫn còn
