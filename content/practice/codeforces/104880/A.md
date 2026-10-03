---
title: "CF 104880A - Nghỉ ngơi tốt"
description: "Chúng ta được cung cấp một lịch trình cố định được mã hóa dưới dạng chuỗi nhị phân có độ dài 24. Mỗi vị trí tương ứng với một giờ trong ngày, bắt đầu từ giờ 1 đến giờ 24. Ký tự 1 có nghĩa là bạn đang làm việc trong giờ đó, còn ký tự 0 có nghĩa là bạn đang nghỉ ngơi."
date: "2026-06-28T09:21:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104880
codeforces_index: "A"
codeforces_contest_name: "The 18-th Beihang University Collegiate Programming Contest (BCPC 2023) - Preliminary"
rating: 0
weight: 104880
solve_time_s: 42
verified: true
draft: false
---

[CF 104880A - Nghỉ ngơi tốt](https://codeforces.com/problemset/problem/104880/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lịch trình cố định được mã hóa dưới dạng chuỗi nhị phân có độ dài 24. Mỗi vị trí tương ứng với một giờ trong ngày, bắt đầu từ giờ 1 đến giờ 24. Một ký tự`1`có nghĩa là bạn đang làm việc trong giờ đó, trong khi`0`có nghĩa là bạn đang nghỉ ngơi. 

Điều kiện duy nhất khiến lịch trình không hợp lệ là sự hiện diện của một khối sáu giờ làm việc liên tục. Nếu bất cứ nơi nào trong ngày tồn tại một chuỗi con liền kề bao gồm toàn bộ sáu`1`ký tự, lịch trình được coi là có hại. Nếu không thì có thể chấp nhận được. 

Nhiệm vụ chỉ đơn giản là quyết định xem khối cấm như vậy có tồn tại hay không. 

Mặc dù kích thước đầu vào được cố định ở mức 24, nhưng cấu trúc của bài toán gợi ý về một nhiệm vụ nhận dạng mẫu chung trong một chuỗi ngắn. Bất kỳ giải pháp nào kiểm tra mọi đoạn liền kề có thể có độ dài 6 sẽ chạy trong thời gian không đổi ở đây, vì chỉ có 19 đoạn như vậy trong chuỗi 24 độ dài. 

Một trường hợp phổ biến xuất phát từ logic quét từng cái một. Ví dụ, đưa ra`111110111111`, một cách triển khai bất cẩn chỉ kiểm tra các khối rời rạc như`[1..6], [7..12]`sẽ bỏ lỡ một đường chạy vượt qua ranh giới một cách không chính xác. Một vấn đề tế nhị khác là quên rằng các lần chạy có thể xuất hiện ở bất cứ đâu, không nhất thiết phải căn chỉnh theo bội số của sáu. 

Xử lý đúng yêu cầu kiểm tra tất cả các cửa sổ chồng chéo có kích thước 6. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để giải quyết vấn đề là kiểm tra mọi chuỗi con liền kề có thể có độ dài 6 và xác minh xem tất cả các ký tự trong chuỗi con đó có phải là`1`. Vì độ dài chuỗi là 24 nên có 24 − 6 + 1 = 19 chuỗi con như vậy. Đối với mỗi chuỗi con, việc kiểm tra tính hợp lệ của nó yêu cầu kiểm tra 6 ký tự nên tổng công việc không đổi và cực kỳ nhỏ. 

Phương pháp vũ lực này đã tối ưu trong cài đặt này vì kích thước đầu vào cố định và nhỏ. Tuy nhiên, sẽ hữu ích về mặt khái niệm khi nghĩ đến một phiên bản tổng quát hơn trong đó độ dài chuỗi là`n`. Trong trường hợp đó, cách tiếp cận đơn giản sẽ kiểm tra tất cả các cửa sổ có kích thước 6, dẫn đến kiểm tra tổng O(n) cửa sổ và O(6n), đơn giản hóa thành O(n). 

Quan sát quan trọng là chúng ta không tính toán một hàm phức tạp trên các chuỗi con mà chỉ đơn giản phát hiện xem có tồn tại bất kỳ chuỗi nào có độ dài ít nhất 6 hay không. Điều này làm cho vấn đề tương đương với việc theo dõi một chuỗi liên tiếp hiện tại`1`S. Thay vì quét lại các chuỗi con liên tục, chúng ta có thể duy trì bộ đếm đang chạy và đặt lại nó bất cứ khi nào`0`đang gặp phải. Khi đồng hồ đếm đến số 6, chúng ta có thể dừng lại ngay lập tức. 

Điều này làm giảm logic từ việc kiểm tra cửa sổ trượt xuống một lượt duy nhất với bộ nhớ không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Cửa sổ trượt Brute Force | O(n) | O(1) | Đã chấp nhận | 
| Bộ đếm một lần | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`run`về không. Biến này biểu thị độ dài hiện tại của liên tiếp`1`s chúng ta đã thấy cho đến nay. 
2. Lặp lại từng ký tự trong chuỗi từ trái sang phải. 
3. Nếu ký tự hiện tại là`1`, tăng`run`bằng 1 vì chuỗi vẫn tiếp tục. 
4. Nếu ký tự hiện tại là`0`, cài lại`run`về 0 vì bất kỳ chuỗi liên tiếp nào cũng bị hỏng. 
5. Sau khi cập nhật`run`, kiểm tra xem nó đã đạt đến 6 chưa. Nếu có, ngay lập tức kết luận lịch trình không hợp lệ và ngừng xử lý. 
6. Nếu vòng lặp kết thúc mà chưa đạt đến số 6 thì lịch trình là hợp lệ. 

Ý tưởng chính là chúng ta không bao giờ cần nhớ nhiều hơn chuỗi hiện tại bởi vì bất kỳ khối hợp lệ nào trong tương lai đều phải liền kề và bất kỳ sự ngắt quãng nào cũng sẽ làm mất hiệu lực tính liên tục trước đó. 

### Tại sao nó hoạt động 

Tại bất kỳ chỉ mục nào trong chuỗi, giá trị của`run`chính xác bằng độ dài của hậu tố dài nhất kết thúc ở vị trí đó chỉ bao gồm`1`S. Điều này có nghĩa là mọi khối liền kề có thể có của`1`s được ngầm biểu diễn dưới dạng giá trị của`run`tại một thời điểm nào đó trong quá trình quét. Nếu một khối có độ dài 6 tồn tại ở bất kỳ đâu thì khi chúng ta đạt đến ký tự cuối cùng của nó,`run`phải có ít nhất 6, kích hoạt phát hiện. Ngược lại, nếu không có khối như vậy tồn tại,`run`không bao giờ đạt đến 6, do đó thuật toán chấp nhận lịch trình một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

run = 0
for ch in s:
    if ch == '1':
        run += 1
        if run >= 6:
            print("NO")
            break
    else:
        run = 0
else:
    print("YES")
```Giải pháp đọc lịch trình gồm 24 ký tự và duy trì một bộ đếm số nguyên duy nhất. Vòng lặp sử dụng Python`for-else`cấu trúc sao cho “CÓ” chỉ được in nếu không xảy ra ngắt sớm. Việc thoát sớm rất quan trọng vì khi phát hiện thấy vi phạm hợp lệ thì việc quét thêm là không cần thiết. 

Một lỗi thực hiện phổ biến là quên đặt lại bộ đếm khi gặp phải`0`, điều này sẽ hợp nhất không chính xác các phần chạy riêng biệt thành một phân đoạn liên tục. Một vấn đề nhỏ khác là chỉ kiểm tra sau vòng lặp thay vì trong quá trình lặp, điều này sẽ ngăn chặn việc chấm dứt sớm nhưng vẫn đúng do kích thước đầu vào nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
111110111111111111111111
```Chúng tôi theo dõi độ dài chạy như sau: 

| Chỉ mục | Char | Chạy sau khi cập nhật | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | tiếp tục | 
| 2 | 1 | 2 | tiếp tục | 
| 3 | 1 | 3 | tiếp tục | 
| 4 | 1 | 4 | tiếp tục | 
| 5 | 1 | 5 | tiếp tục | 
| 6 | 0 | 0 | đặt lại | 
| 7 | 1 | 1 | tiếp tục | 
| 8 | 1 | 2 | tiếp tục | 

Lần chạy này sau đó đạt 6 trong đoạn thứ hai liên tiếp`1`s, do đó thuật toán sẽ xuất ra`NO`. 

Dấu vết này cho thấy cách đặt lại ngăn chặn sự tích lũy khoảng cách chéo và đảm bảo chỉ tính các chuỗi liền kề. 

### Ví dụ 2 

đầu vào:```
111011101110111011101110
```| Chỉ mục | Char | Chạy sau khi cập nhật | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | tiếp tục | 
| 2 | 1 | 2 | tiếp tục | 
| 3 | 1 | 3 | tiếp tục | 
| 4 | 0 | 0 | đặt lại | 
| 5 | 1 | 1 | tiếp tục | 
| 6 | 1 | 2 | tiếp tục | 
| 7 | 1 | 3 | tiếp tục | 
| 8 | 0 | 0 | đặt lại | 

Lần chạy không bao giờ vượt quá 3 trong mẫu này, do đó không tìm thấy vi phạm nào và đầu ra là`YES`. 

Điều này xác nhận rằng nhiều cụm công việc ngắn riêng biệt sẽ an toàn miễn là không có cụm công việc đơn lẻ nào đạt đến độ dài 6. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua chuỗi 24 ký tự, công việc không đổi trên mỗi ký tự | 
| Không gian | O(1) | Chỉ có một bộ đếm số nguyên được lưu trữ | 

Cho rằng`n = 24`, đây thực sự là thời gian không đổi trong thực tế và thỏa mãn mọi ràng buộc một cách tầm thường. 

Giải pháp này nằm trong giới hạn vì nó thực hiện tối đa 24 lần lặp và một vài phép tính số nguyên trên mỗi bước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        s = input().strip()
        run_cnt = 0
        for ch in s:
            if ch == '1':
                run_cnt += 1
                if run_cnt >= 6:
                    print("NO")
                    break
            else:
                run_cnt = 0
        else:
            print("YES")
    return out.getvalue().strip()

# provided samples (illustrative, since exact samples are not fully specified)
assert run("000000000000000000000000") == "YES"
assert run("111111000000000000000000") == "NO"

# custom cases
assert run("111110111110111110111110") == "YES", "no run reaches 6"
assert run("111111000000000000000000") == "NO", "exact boundary 6"
assert run("011111101111110000000000") == "NO", "run across reset"
assert run("101010101010101010101010") == "YES", "alternating pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | CÓ | lịch làm việc trống là hợp lệ | 
| chính xác sáu 1 giây | KHÔNG | điều kiện biên vi phạm | 
| chạy tách biệt | CÓ | đặt lại ngăn chặn việc hợp nhất | 
| mô hình xen kẽ | CÓ | không có đoạn dài liên tiếp | 

## Vỏ cạnh 

Trường hợp cạnh then chốt là khi có đúng sáu lần liên tiếp`1`s xuất hiện ở đầu chuỗi. Đối với đầu vào`111111000000000000000000`, bộ đếm đạt 6 ở chỉ số 6 và thuật toán in ngay lập tức`NO`. Điều này xác nhận rằng việc phát hiện không phụ thuộc vào vị trí và hoạt động chính xác ở ranh giới. 

Một trường hợp khác là khi hai lần chạy năm`1`s được cách nhau bởi một`0`, chẳng hạn như`111110111110...`. Việc đặt lại ở mức 0 đảm bảo lần chạy thứ hai bắt đầu mới và không đạt đến độ dài 6, do đó đầu ra vẫn giữ nguyên`YES`. 

Trường hợp cuối cùng liên quan đến các ký tự xen kẽ như`101010...`. Bộ đếm liên tục đặt lại về 1, không bao giờ tích lũy, do đó thuật toán trả về an toàn`YES`không có nguy cơ dương tính giả.
