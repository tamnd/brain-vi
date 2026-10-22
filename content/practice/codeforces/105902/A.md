---
title: "CF 105902A - Người ta phải tưởng tượng thời gian thật chặt chẽ,"
description: "Chúng ta được cung cấp ba lộ trình hoặc kế hoạch ứng viên để hoàn thành cùng một nhiệm vụ, mỗi lộ trình có liên quan đến chi phí thời gian. Mục tiêu chỉ đơn giản là chọn tùy chọn nhanh nhất trong số ba tùy chọn và đưa ra thời gian của nó. Mỗi đầu vào bao gồm chính xác ba số nguyên."
date: "2026-06-21T12:15:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105902
codeforces_index: "A"
codeforces_contest_name: "2025 Fujian Normal University Programming Contest"
rating: 0
weight: 105902
solve_time_s: 47
verified: true
draft: false
---

[CF 105902A - Người ta phải tưởng tượng về thời gian ngắn ngủi](https://codeforces.com/problemset/problem/105902/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp ba lộ trình hoặc kế hoạch ứng viên để hoàn thành cùng một nhiệm vụ, mỗi lộ trình có liên quan đến chi phí thời gian. Mục tiêu chỉ đơn giản là chọn tùy chọn nhanh nhất trong số ba tùy chọn và đưa ra thời gian của nó. 

Mỗi đầu vào bao gồm chính xác ba số nguyên. Bạn có thể coi chúng như ba chiến lược cạnh tranh để giải cùng một bài toán con, trong đó mỗi chiến lược đều có một chi phí cố định đã biết. Nhiệm vụ giảm xuống còn việc chọn giá trị nhỏ nhất trong ba giá trị này và in nó. 

Các ràng buộc cực kỳ nhỏ, với mỗi giá trị từ 1 đến 60. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào, ngay cả thứ gì đó nặng không cần thiết, vẫn sẽ chạy trong thời gian không đổi. Không cần tiền xử lý, sắp xếp các cấu trúc lớn hoặc tính toán lặp đi lặp lại. Bất kỳ thuật toán nào thậm chí kiểm tra từng giá trị một lần đều đã tối ưu. 

Không có trường hợp cạnh cấu trúc phức tạp nào liên quan đến chuỗi hoặc phạm vi, nhưng có một mối lo ngại tinh vi về tính đúng đắn thường xuất hiện trong các vấn đề thuộc loại này: đảm bảo rằng sự bình đẳng không ảnh hưởng đến tính đúng đắn. Nếu hai hoặc nhiều giá trị bằng nhau và tối thiểu thì bất kỳ giá trị nào trong số đó đều hợp lệ, do đó việc triển khai không được loại bỏ sai các ứng cử viên tối thiểu hợp lệ do so sánh nghiêm ngặt hoặc thoát sớm giả định tính duy nhất. 

Ví dụ: nếu đầu vào là`1 1 45`, cả hai phương án đầu đều tối ưu. Một giải pháp đúng vẫn phải xuất ra`1`. Cách tiếp cận có lỗi có thể nhầm lẫn khi giả định mức tối thiểu duy nhất và bỏ qua các giá trị bằng nhau, nhưng ở đây sự bình đẳng là một phần của tính đúng đắn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là so sánh ba giá trị theo cách thủ công. Chúng ta có thể kiểm tra cái nào trong ba cái nhỏ nhất bằng một chuỗi so sánh. Ví dụ: chúng ta có thể bắt đầu bằng cách giả sử giá trị đầu tiên là câu trả lời, sau đó so sánh nó với giá trị thứ hai, cập nhật nếu cần và sau đó so sánh lại với giá trị thứ ba. Điều này đã đủ vì kích thước đầu vào đã được cố định. 

Điều này có hiệu quả vì vấn đề về cơ bản là tính toán mức tối thiểu trên một tập hợp ba phần tử. Không có sự phụ thuộc giữa các giá trị và không cần chuyển đổi, chỉ có lựa chọn. 

Người ta có thể xem xét một cách tiếp cận tổng quát hơn như sắp xếp ba giá trị và lấy phần tử đầu tiên. Điều đó cũng hiệu quả, nhưng tạo ra chi phí không cần thiết trong cả lý luận và triển khai đối với đầu vào có kích thước cố định nhỏ như vậy. Vòng lặp trên ba phần tử cũng hợp lệ, nhưng lại phức tạp không cần thiết đối với cấu trúc có kích thước không đổi. 

Quan sát quan trọng là cấu trúc cố định và nhỏ bé. Sức mạnh tàn bạo và các giải pháp tối ưu thực sự giống nhau về độ phức tạp; sự khác biệt duy nhất là sự rõ ràng và trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| So sánh theo cặp | O(1) | O(1) | Đã chấp nhận | 
| Sắp xếp ba giá trị | O(1) | O(1) | Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

1. Đọc ba số nguyên từ đầu vào. Đây là những chi phí của ba kế hoạch ứng cử viên. 
2. Khởi tạo một biến`ans`với giá trị đầu tiên. Điều này cung cấp cho chúng tôi một ứng cử viên cơ bản hợp lệ theo mặc định. 
3. So sánh`ans`với giá trị thứ hai. Nếu giá trị thứ hai nhỏ hơn, hãy cập nhật`ans`đến giá trị thứ hai. Điều này đảm bảo chúng tôi luôn giữ được lựa chọn tốt nhất cho đến nay. 
4. So sánh`ans`với giá trị thứ ba. Nếu giá trị thứ ba nhỏ hơn, hãy cập nhật`ans`lại. Điều này đảm bảo rằng sau khi xử lý cả ba giá trị,`ans`giữ mức tối thiểu của tất cả các ứng cử viên. 
5. Đầu ra`ans`. 

### Tại sao nó hoạt động 

Tại mỗi bước, biến`ans`lưu trữ giá trị tối thiểu trong số tất cả các phần tử được xử lý cho đến nay. Sau bước đầu tiên, nó là mức tối thiểu của phần tử đầu tiên. Sau lần so sánh thứ hai, nó trở thành điểm tối thiểu của hai phần tử đầu tiên. Sau lần so sánh thứ ba, nó trở thành điểm tối thiểu của cả ba phần tử. Bất biến này đảm bảo rằng không thể bỏ qua giá trị nhỏ hơn, bởi vì mọi ứng cử viên đều được so sánh rõ ràng với giá trị tốt nhất hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b, c = map(int, input().split())

ans = a
if b < ans:
    ans = b
if c < ans:
    ans = c

print(ans)
```Giải pháp đọc ba số nguyên trong một dòng và lưu trữ chúng trong các biến. Biến`ans`được khởi tạo với giá trị đầu tiên để thiết lập điểm bắt đầu hợp lệ. Mỗi bản cập nhật so sánh tiếp theo`ans`chỉ khi tìm thấy một giá trị nhỏ hơn, duy trì tính chính xác ngay cả khi các giá trị bằng nhau. 

Không cần xử lý đặc biệt cho các giá trị bằng nhau vì điều kiện sử dụng`<`còn hơn là`<=`, điều này tự nhiên giữ mức tối thiểu gặp phải đầu tiên mà không ảnh hưởng đến tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`14 12 18`| Bước | một | b | c | trả lời | 
| --- | --- | --- | --- | --- | 
| ban đầu | 14 | 12 | 18 | 14 | 
| so sánh b | 14 | 12 | 18 | 12 | 
| so sánh c | 14 | 12 | 18 | 12 | 

Giá trị thứ hai trở thành câu trả lời vì nó nhỏ hơn giá trị ban đầu và giá trị thứ ba không thay đổi vì nó lớn hơn. 

### Ví dụ 2 

đầu vào:`1 1 45`| Bước | một | b | c | trả lời | 
| --- | --- | --- | --- | --- | 
| ban đầu | 1 | 1 | 45 | 1 | 
| so sánh b | 1 | 1 | 45 | 1 | 
| so sánh c | 1 | 1 | 45 | 1 | 

Điều này xác nhận rằng các giá trị bằng nhau không phá vỡ logic. Thuật toán giữ lại chính xác 1 ngay cả khi tồn tại trùng lặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có ba phép so sánh theo thời gian không đổi được thực hiện bất kể giá trị đầu vào | 
| Không gian | O(1) | Chỉ sử dụng một số biến cố định | 

Các ràng buộc hạn chế đầu vào ở ba số nguyên có độ lớn nhỏ, do đó việc xử lý thời gian không đổi nằm trong giới hạn một cách tầm thường. Giải pháp chạy ngay lập tức trong mọi môi trường hợp lý. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a, b, c = map(int, input().split())
    ans = a
    if b < ans:
        ans = b
    if c < ans:
        ans = c
    return str(ans)

# provided samples
assert run("14 12 18") == "12", "sample 1"
assert run("1 1 45") == "1", "sample 2"

# custom cases
assert run("1 2 3") == "1", "increasing order"
assert run("3 2 1") == "1", "decreasing order"
assert run("5 5 5") == "5", "all equal"
assert run("60 1 60") == "1", "minimum in middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 | 1 | tăng độ chính xác của lệnh | 
| 3 2 1 | 1 | đúng thứ tự ngược lại | 
| 5 5 5 | 5 | xử lý bình đẳng | 
| 60 1 60 | 1 | vị trí trung bình tối thiểu | 

## Vỏ cạnh 

### Tất cả các giá trị đều bằng nhau 

đầu vào:`7 7 7`Thuật toán bắt đầu với`ans = 7`, sau đó so sánh với giá trị thứ hai và thứ ba. Cả hai đều không kích hoạt cập nhật vì cả hai đều không nhỏ hơn. Đầu ra cuối cùng vẫn còn`7`, điều này đúng vì tất cả các tùy chọn đều tương đương nhau. 

### Giá trị tối thiểu ở các vị trí khác nhau 

đầu vào:`9 2 8`Thuật toán đặt đầu tiên`ans = 9`. Sau đó nó cập nhật lên`2`sau khi so sánh với giá trị thứ hai. Sự so sánh thứ ba không thay đổi nó. Đầu ra`2`phản ánh chính xác mức tối thiểu toàn cầu bất kể vị trí. 

### Thứ tự giảm dần 

đầu vào:`10 6 1`Thuật toán cập nhật dần dần`ans`ở mỗi bước, chứng minh rằng các cải tiến lặp đi lặp lại được xử lý chính xác. Kết quả cuối cùng`1`xác nhận rằng không có quyết định sớm nào ngăn cản việc sửa chữa sau này.
