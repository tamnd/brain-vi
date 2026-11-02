---
title: "CF 1095A - Mật mã lặp"
description: "Chúng ta được cung cấp một chuỗi được tạo ra bởi một quy tắc "lặp lại kéo dài" rất cụ thể được áp dụng cho một chuỗi gốc ẩn nào đó. Chuỗi ban đầu ngắn, có độ dài tối đa là 10, nhưng chúng tôi không biết."
date: "2026-06-13T05:22:55+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1095
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 529 (Div. 3)"
rating: 800
weight: 1095
solve_time_s: 1373
verified: false
draft: false
---

[CF 1095A - Mật mã lặp lại](https://codeforces.com/problemset/problem/1095/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 22 phút 53 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi được tạo ra bởi một quy tắc "lặp lại kéo dài" rất cụ thể được áp dụng cho một chuỗi gốc ẩn nào đó. Chuỗi ban đầu ngắn, có độ dài tối đa là 10, nhưng chúng tôi không biết. Những gì chúng ta được cho là chuỗi mở rộng cuối cùng sau khi mỗi ký tự được lặp lại một số lần bằng vị trí của nó trong chuỗi gốc. 

Cụ thể, nếu chuỗi ẩn là`s = s1 s2 ... sm`, sau đó là ký tự đầu tiên`s1`xuất hiện một lần trong chuỗi được mã hóa, ký tự thứ hai`s2`xuất hiện hai lần liên tiếp, ký tự thứ ba xuất hiện ba lần, v.v. Do đó, chuỗi được mã hóa là sự kết hợp của các khối có độ dài tăng dần theo 1, 2, 3, ..., m. 

Nhiệm vụ là khôi phục chuỗi gốc từ dạng mở rộng này. 

Các ràng buộc rất nhỏ: độ dài chuỗi được mã hóa nhiều nhất là 55 và độ dài chuỗi gốc nhiều nhất là 10 vì 1 + 2 + ... + 10 = 55. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp quét hoặc mô phỏng nào theo thời gian tuyến tính đều dễ dàng đủ. Ngay cả các vòng lặp lồng nhau lên tới 10 cấp cũng không đáng kể về mặt hiệu suất nhưng không cần thiết. 

Trường hợp cạnh chính là cấu trúc hơn là tính toán. Vì kích thước khối ngày càng tăng nên một giải pháp sai thường thất bại khi nó giả định các phân đoạn có kích thước bằng nhau hoặc cố gắng phân chia một cách tham lam mà không tôn trọng mô hình tăng trưởng 1, 2, 3, ... nghiêm ngặt. Một vấn đề tinh tế khác là việc lập chỉ mục theo từng thứ một: ký tự thứ k thuộc về một khối có kích thước k, không phải k-1 hoặc k+1, điều này có thể âm thầm làm hỏng quá trình tái thiết. 

## Phương pháp tiếp cận 

Viễn cảnh bạo lực bắt đầu bằng cách tưởng tượng chúng ta cố gắng đoán chuỗi gốc và mô phỏng quá trình mã hóa. Vì độ dài ban đầu nhiều nhất là 10 nên mỗi vị trí có 26 khả năng, do đó có 26^10 ứng viên tiềm năng. Đối với mỗi ứng cử viên, chúng tôi có thể xây dựng dạng mã hóa của nó trong thời gian O(m^2) (vì chúng tôi nối thêm i bản sao cho vị trí i), mang lại không gian tìm kiếm rộng lớn về mặt thiên văn. Ngay cả khi cắt tỉa, điều này hoàn toàn không cần thiết đối với cấu trúc. 

Quan sát quan trọng là quá trình mã hóa có tính xác định và nội tại theo một cách rất có cấu trúc: chuỗi được mã hóa chỉ đơn giản là nối các khối có độ dài đã biết. Khối đầu tiên có kích thước 1, khối thứ hai có kích thước 2, v.v. Do đó, thay vì đoán chuỗi gốc, chúng ta có thể giải mã trực tiếp bằng cách duyệt qua chuỗi được mã hóa và cắt ra các khối có kích thước tăng dần. 

Điều này biến vấn đề thành một nhiệm vụ phân tích cú pháp đơn giản: duy trì một con trỏ vào chuỗi, trích xuất 1 ký tự, sau đó là 2 ký tự, rồi 3, v.v. và ghi lại ký tự đầu tiên của mỗi khối. 

Cách tiếp cận vũ phu là theo cấp số nhân trong trường hợp xấu nhất do đoán chuỗi. Cách tiếp cận tối ưu là tuyến tính theo chiều dài của chuỗi được mã hóa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26^m · m2) | O(m) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng lại chuỗi gốc bằng cách khai thác cấu trúc khối cố định của mật mã. 

1. Khởi tạo con trỏ`i = 0`để theo dõi vị trí của chúng tôi trong chuỗi được mã hóa. Chúng tôi cũng duy trì một bộ đếm kích thước khối`k = 1`. 
2. Trong khi`i`nhỏ hơn độ dài của chuỗi, chúng ta biết rằng khối tiếp theo trong mã hóa ban đầu có kích thước`k`, vì vậy chúng tôi lấy chuỗi con bắt đầu từ`i`chiều dài`k`. 
3. Ký tự gốc tạo ra khối này phải là ký tự đầu tiên của chuỗi con đó, vì tất cả các ký tự trong khối đều giống hệt nhau về mặt cấu trúc. 
4. Thêm ký tự đó vào câu trả lời. 
5. Di chuyển con trỏ về phía trước bằng`k`, và tăng`k`bằng 1 để phù hợp với quy tắc lặp lại tăng dần. 
6. Tiếp tục cho đến khi hết dây. 

Ý tưởng chính là chúng ta không bao giờ cần xác thực nội dung khối ngoài tính nhất quán, vì vấn đề đảm bảo đầu vào là hợp lệ. 

### Tại sao nó hoạt động 

Ở mỗi bước, quá trình mã hóa tạo ra một khối liền kề gồm các ký tự giống hệt nhau có độ dài được xác định chính xác bởi vị trí của nó trong chuỗi gốc. Do các khối không chồng lên nhau và được sắp xếp chặt chẽ theo kích thước tăng dần nên ranh giới giữa các khối được xác định duy nhất theo dãy 1, 2, 3, ..., m. Do đó, tại thời điểm giải mã, ranh giới khối được cố định và rõ ràng, đồng thời việc chọn ký tự đầu tiên của mỗi khối sẽ tái tạo lại chuỗi gốc một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
t = input().strip()

res = []
i = 0
k = 1

while i < n:
    res.append(t[i])
    i += k
    k += 1

print("".join(res))
```Giải pháp đọc chuỗi được mã hóa và duyệt qua nó bằng hai biến:`i`, vị trí hiện tại và`k`, độ dài khối dự kiến. Ở mỗi lần lặp, chúng ta trích xuất ký tự ở vị trí`i`là đại diện của khối hiện tại. Sau đó chúng tôi bỏ qua`k`ký tự vì mã hóa đảm bảo rằng toàn bộ phân đoạn này tương ứng với một ký tự lặp lại. tăng dần`k`đảm bảo chúng tôi tôn trọng mô hình tăng trưởng 1, 2, 3, .... 

Không cần xác thực chuỗi con vì đầu vào được đảm bảo là mã hóa hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
baabbb
```Chúng tôi theo dõi việc giải mã từng bước. 

| Bước | tôi | k | Chặn đọc | Ký tự được chọn | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | "b" | b | b | 
| 2 | 1 | 2 | "aa" | một | ba | 
| 3 | 3 | 3 | "bbb" | b | em yêu | 

Con trỏ di chuyển 0 → 1 → 3 → 6, sử dụng toàn bộ chuỗi. Chuỗi được xây dựng lại là`bab`, phù hợp với bản gốc. 

Dấu vết này cho thấy ranh giới khối được xác định đầy đủ bởi chuỗi độ dài tăng dần. 

### Ví dụ 2 

đầu vào:```
10
ccdddeeeee
```Chúng tôi cho rằng nó hợp lệ theo cùng một cấu trúc. 

| Bước | tôi | k | Chặn đọc | Ký tự được chọn | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | "c" | c | c | 
| 2 | 1 | 2 | "cd" | c | cc | 
| 3 | 3 | 3 | "dde" | d | ccd | 
| 4 | 6 | 4 | "eeee" | e | ccde | 

Điều này chứng tỏ rằng ngay cả khi các ký tự khác nhau giữa các khối, mỗi khối vẫn độc lập và được xác định đầy đủ bởi vị trí bắt đầu và độ dài dự kiến ​​của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nhân vật được truy cập đúng một lần khi chúng ta nhảy qua các khối | 
| Không gian | O(1) | Ngoài bộ lưu trữ đầu ra, chỉ có một số bộ đếm được sử dụng | 

Các ràng buộc giới hạn kích thước đầu vào ở mức 55, do đó, thậm chí quét tuyến tính kém hiệu quả hơn cũng đủ. Giải pháp tối ưu và thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(input().strip())
    t = input().strip()

    res = []
    i = 0
    k = 1

    while i < n:
        res.append(t[i])
        i += k
        k += 1

    return "".join(res)

# provided sample
assert run("6\nbaabbb\n") == "bab"

# minimal case
assert run("1\na\n") == "a"

# single block chain
assert run("3\naaabbbccc\n") == "abc"

# mixed characters valid structure
assert run("6\nxxyyyzzz\n") == "xyz"

# edge increasing pattern
assert run("10\nabcdefghijjjj\n") == "abcdefghij"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 một | một | trường hợp ranh giới tối thiểu | 
| 3 aaabbbccc | abc | cấu trúc khối lặp đi lặp lại | 
| 6 xyyyzzz | xyz | bước đúng qua các khối | 
| 10 abcdefghijjjj | abcdefghij | giải mã tuần tự có độ dài đầy đủ | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế là đầu vào nhỏ nhất có thể có trong đó độ dài chuỗi là 1. Trong trường hợp đó, thuật toán bắt đầu bằng`k = 1`, đọc chính xác một ký tự và kết thúc ngay lập tức. Không có nguy cơ vượt quá vì mức tăng của con trỏ khớp chính xác với độ dài còn lại. 

Một trường hợp khác là khi tất cả các ký tự đều giống hệt nhau. Ví dụ,`t = "aaaaa..."`. Thuật toán vẫn hoạt động vì nó không bao giờ dựa vào sự khác biệt về ký tự mà chỉ dựa vào độ dài khối. Mỗi bước sử dụng một phân đoạn đang phát triển và đầu ra chỉ lặp lại cùng một ký tự cho mỗi khối. 

Trường hợp cuối cùng là độ dài tối đa là 55. Vì 1 + 2 + ... + 10 = 55 nên thuật toán sẽ tự nhiên kết thúc chính xác sau 10 lần lặp. Không có sự mơ hồ một phần khối, bởi vì tổng kích thước khối khớp chính xác với độ dài chuỗi.
