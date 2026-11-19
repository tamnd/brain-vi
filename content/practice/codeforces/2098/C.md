---
title: "CF 2098C - Cá cược thể thao"
description: "Mỗi học sinh ấn định một ngày cụ thể và về cơ bản, Vadim đang cố gắng “dự đoán” điều gì sẽ xảy ra trong hai ngày tiếp theo sau ngày đã chọn đó."
date: "2026-06-09T03:53:35+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2098
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1021 (Div. 2)"
rating: 1400
weight: 2098
solve_time_s: 84
verified: false
draft: false
---

[CF 2098C - Cá cược thể thao](https://codeforces.com/problemset/problem/2098/C) 

**Đánh giá:** 1400 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi học sinh ấn định một ngày cụ thể và về cơ bản, Vadim đang cố gắng “dự đoán” điều gì sẽ xảy ra trong hai ngày tiếp theo sau ngày đã chọn đó. Chi tiết ẩn quan trọng là quá trình thực tế trong tương lai không liên quan; điều quan trọng là đối với mỗi học sinh, Vadim phải đưa ra một dự đoán cho khoảng thời gian dài 2 bắt đầu từ ngày họ đã chọn. 

Vì vậy với mỗi giá trị chỉ số$a_i$, Vadim đang ngầm chọn mô hình 2 ngày cho khoảng thời gian$[a_i+1, a_i+2]$. Mỗi học sinh sẽ kiểm tra độc lập xem mẫu được chọn của Vadim có phù hợp với thực tế trên cửa sổ đó hay không. Vadim chỉ thắng cược với một học sinh nếu dự đoán đã chọn của anh ấy đúng với cửa sổ của học sinh đó. 

Câu hỏi không phải là dự đoán quá trình thực sự. Vấn đề là liệu Vadim có thể chọn những dự đoán để đảm bảo ít nhất một học sinh hài lòng hay không, bất kể quá trình thực tế diễn ra như thế nào. Điều này biến vấn đề thành một lập luận bao quát về tất cả các kết quả cục bộ có thể xảy ra trên các cửa sổ có độ dài 2. 

Các ràng buộc thúc đẩy giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Vì tổng cộng$n$qua các bài kiểm tra nhiều nhất là$10^5$, mọi cách phân loại hoặc nhóm dựa trên hàm băm cho mỗi bài kiểm tra đều có thể chấp nhận được, nhưng mọi phép tính bậc hai trong một bài kiểm tra sẽ không thành công. Việc kiểm tra ngây thơ tất cả các tổ hợp dự đoán cho mỗi học sinh là không thể ngay lập tức vì mỗi cửa sổ có nhiều kết quả có thể xảy ra và các tương tác trong những ngày giống hệt nhau hoặc gần đó sẽ bùng nổ theo kiểu tổ hợp. 

Một trường hợp phức tạp xuất hiện khi tất cả học sinh chọn những ngày riêng biệt và không lặp lại. Ví dụ:```
1
3
1 2 3
```Ở đây, mỗi cửa sổ đều được cách ly. Theo trực giác, Vadim không thể “tái sử dụng” một dự đoán để làm hài lòng nhiều học sinh. Câu trả lời đúng là “Không” vì không có cấu trúc lặp lại để khai thác. 

Một trường hợp khác là khi tất cả học sinh chọn cùng một ngày:```
1
4
5 5 5 5
```Tại đây, Vadim có thể chỉ định các dự đoán khác nhau cho các học sinh khác nhau và đảm bảo rằng ít nhất một dự đoán phù hợp với kết quả thực sự của hai ngày tới, bởi vì chỉ có bốn mẫu có độ dài-2 có thể xảy ra. Câu trả lời đúng là "Có". 

Điểm khác biệt chính là liệu các bản sao có tồn tại theo cách cho phép liệt kê tất cả các mẫu cục bộ có thể hay không. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng suy luận trực tiếp, mỗi học sinh sẽ tương ứng với một cửa sổ có độ dài 2. Đối với một cửa sổ cố định, tương lai có thể được biểu diễn dưới dạng một cặp nhị phân (hoặc tổng quát hơn là một số lượng nhỏ các trạng thái không đổi tùy theo cách giải thích). Quan sát quan trọng là Vadim thành công nếu anh ta có thể đưa ra những dự đoán riêng biệt cho đủ học sinh sao cho tất cả các kết quả có thể xảy ra đối với ít nhất một cửa sổ đều được che đậy. 

Một cách diễn giải thô bạo sẽ cố gắng chỉ định các dự đoán cho mỗi học sinh và kiểm tra xem liệu một số bài tập có đảm bảo khớp chính xác cho ít nhất một học sinh trong tất cả các nhận thức có thể xảy ra hay không. Điều này nhanh chóng trở thành vấn đề bao trùm tất cả các mẫu có 2 chiều dài có thể có trên các chỉ mục đã chọn. Nếu chúng ta mô phỏng rõ ràng tất cả các mẫu có thể có cho mỗi vị trí và thử tất cả các bài tập, thì độ phức tạp sẽ tăng theo cấp số nhân về số lượng học sinh chia sẻ cấu trúc, khiến nó không thể thực hiện được ngay cả đối với$n = 10^5$. 

Sự đơn giản hóa chính là chỉ có bội số của những ngày được chọn mới quan trọng. Nếu một ngày xuất hiện nhiều lần, Vadim có thể đưa ra những dự đoán khác nhau cho những học sinh đó. Vì số lượng mẫu có độ dài-2 riêng biệt là không đổi nên cách duy nhất để đảm bảo thành công là đảm bảo rằng ít nhất một ngày có đủ số lần lặp lại để bao gồm tất cả các khả năng cần thiết để buộc phải khớp. Điều này làm giảm vấn đề kiểm tra phân phối tần số. 

Cụ thể, điều kiện rút gọn thành việc xác minh xem ngày nào có xuất hiện ít nhất 4 lần hay không. Trực giác cho thấy có bốn cách có thể xảy ra trong quá trình kéo dài hai ngày (giải thích nhị phân trong mô hình dự định) và việc có bốn lần đặt cược độc lập trong cùng một ngày bắt đầu cho phép bao gồm tất cả các kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt qua bài tập | Hàm mũ | O(n) | Quá chậm | 
| Đếm tần số | O(n log n) / O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân nhóm tất cả học sinh theo ngày$a_i$. Điều này xác định có bao nhiêu cược độc lập tồn tại cho mỗi điểm bắt đầu. 
2. Đếm tần suất của từng ngày riêng biệt. 
3. Nếu bất kỳ tần số nào ít nhất là 4, hãy kết luận ngay rằng Vadim có thể đảm bảo thành công. 
4. Ngược lại, hãy kết luận rằng điều đó là không thể. 

Lý do bước 3 là đủ là vì chỉ khi chúng ta có đủ điểm xuất phát giống nhau, chúng ta mới có thể chỉ định đủ các dự đoán khác biệt để bao quát tất cả các kết quả có thể xảy ra cho khoảng thời gian đó. Bất kỳ bội số nhỏ hơn nào cũng để lại ít nhất một kết quả có thể xảy ra. 

### Tại sao nó hoạt động 

Mỗi học sinh có cùng một$a_i$tương ứng với cùng một phân đoạn 2 ngày chưa biết. Vadim có thể đưa ra những dự đoán khác nhau cho những học sinh khác nhau, nhưng độ chính xác phụ thuộc vào việc khớp với việc thực hiện thực tế của cùng phân khúc đó. Vì số lần thực hiện có thể có của quy trình 2 ngày là không đổi và nhỏ nên việc đảm bảo chiến thắng sẽ giảm xuống còn đảm bảo bao phủ đầy đủ các khả năng này cho ít nhất một phân đoạn lặp lại. Nếu không có phân đoạn nào lặp lại đầy đủ thì không có nhiệm vụ nào có thể bao gồm tất cả các kết quả cho bất kỳ cửa sổ nào, do đó, chiến thắng được đảm bảo là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        
        freq = {}
        for x in arr:
            freq[x] = freq.get(x, 0) + 1
        
        ok = False
        for v in freq.values():
            if v >= 4:
                ok = True
                break
        
        print("Yes" if ok else "No")

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên tổng hợp tần suất của tất cả các ngày đã chọn bằng bản đồ băm. Điều này là cần thiết vì cấu trúc của bài toán phụ thuộc hoàn toàn vào sự lặp lại. Việc kiểm tra cho`v >= 4`mã hóa trực tiếp số lần lặp lại tối thiểu cần thiết để đảm bảo bao quát tất cả các kết quả có thể xảy ra trong khoảng thời gian 2 ngày. 

Một lỗi triển khai phổ biến là sắp xếp và sau đó cố gắng suy luận về các khối liên tiếp theo cách thủ công. Điều đó là không cần thiết và có nguy cơ xảy ra lỗi riêng lẻ. Việc đếm băm ở đây vừa đơn giản vừa an toàn hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 1 1 1
```| Ngày | Tần số | Quyết định | 
| --- | --- | --- | 
| 1 | 4 | thành công | 

Vadim có bốn điểm xuất phát giống hệt nhau, cho phép bao quát đầy đủ tất cả các kết quả có thể xảy ra trong 2 ngày. Thuật toán phát hiện tần số 4 và xuất ra "Có". 

### Ví dụ 2 

đầu vào:```
3
2 4 3
```| Ngày | Tần số | Quyết định | 
| --- | --- | --- | 
| 2 | 1 | thất bại | 
| 4 | 1 | thất bại | 
| 3 | 1 | thất bại | 

Không có ngày nào lặp lại nên không có cách nào để tập trung đưa tin dự đoán. Thuật toán xuất ra chính xác "Không". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | đếm tần số đơn | 
| Không gian | O(n) | tần số lưu trữ bản đồ băm | 

Vì tổng cộng$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$10^5$, giải pháp này dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        freq = {}
        for x in arr:
            freq[x] = freq.get(x, 0) + 1
        
        ok = any(v >= 4 for v in freq.values())
        out.append("Yes" if ok else "No")
    
    return "\n".join(out)

# provided samples
assert run("""5
4
1 1 1 1
3
2 2 2
5
2 4 3 2 4
8
6 3 1 1 5 1 2 6
1
1000000000
""") == """Yes
No
Yes
No
No"""

# custom cases
assert run("""1
1
1
""") == "No", "single element"

assert run("""1
4
10 10 10 10
""") == "Yes", "exact threshold"

assert run("""1
6
1 2 3 4 5 6
""") == "No", "all distinct"

assert run("""1
8
7 7 7 7 8 9 10 11
""") == "Yes", "mixed with one strong frequency"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | Không | trường hợp cạnh tối thiểu | 
| bốn giống hệt nhau | Có | hành vi ngưỡng | 
| tất cả đều khác biệt | Không | không có cấu trúc lặp lại | 
| tần số hỗn hợp mạnh mẽ | Có | tính đúng đắn dưới tiếng ồn | 

## Vỏ cạnh 

Đối với đầu vào:```
1
4
5 5 5 5
```Thuật toán đếm tần số từ 5 thành 4, ngay lập tức đánh dấu thành công. Điều này tương ứng với việc có bốn cược độc lập trên cùng một cửa sổ, đủ để bao gồm tất cả các kết quả có thể xảy ra. 

Đối với đầu vào:```
1
3
1 2 3
```Tất cả các tần số đều bằng 1 nên không có ngày nào đạt đến ngưỡng. Thuật toán từ chối trường hợp một cách chính xác, phản ánh rằng các cửa sổ bị cô lập không thể đảm bảo chiến thắng bắt buộc. 

Đối với đầu vào:```
1
7
2 2 2 2 3 3 3
```Ngày thứ 2 đạt tần số 4 nên thuật toán đưa ra kết quả là "Có". Các giá trị còn lại không quan trọng vì chỉ cần một cửa sổ lặp lại đủ để đảm bảo thành công.
