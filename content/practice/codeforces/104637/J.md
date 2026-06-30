---
title: "CF 104637J - Tanya và cầu thang"
description: "Chúng ta được cung cấp một dãy số nguyên thể hiện những gì Tanya nói khi leo cầu thang. Mỗi lần cô bước vào một cầu thang mới, cô bắt đầu đếm từ 1 và tiếp tục đi lên từng bước cho đến khi hết cầu thang đó."
date: "2026-06-29T17:02:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104637
codeforces_index: "J"
codeforces_contest_name: "\u041c\u0438\u0441\u0438\u0441 2023 \u043e\u0441\u0435\u043d\u044c - \u0431\u0430\u0437\u043e\u0432\u0430\u044f \u043c\u0430\u0442\u0435\u043c\u0430\u0442\u0438\u043a\u0430, \u0443\u0441\u043b\u043e\u0432\u0438\u044f, \u0446\u0438\u043a\u043b\u044b"
rating: 0
weight: 104637
solve_time_s: 78
verified: false
draft: false
---

[CF 104637J - Tanya và Cầu thang](https://codeforces.com/problemset/problem/104637/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số nguyên thể hiện những gì Tanya nói khi leo cầu thang. Mỗi lần cô bước vào một cầu thang mới, cô bắt đầu đếm từ 1 và tiếp tục đi lên từng bước cho đến khi hết cầu thang đó. Sau đó cô ấy ngay lập tức tiếp tục đi lên một cầu thang khác, lại bắt đầu từ số 1. 

Vì vậy, chuỗi được hình thành bằng cách nối nhiều lần chạy liên tiếp, trong đó mỗi lần chạy có dạng chính xác là 1, 2, 3, ..., k đối với một số nguyên dương k. Nhiệm vụ của chúng ta là chia chuỗi thành các lần chạy hợp lệ tối đa này và xuất ra số lần chạy như vậy tồn tại cũng như độ dài của chúng. 

Cấu trúc cực kỳ cứng nhắc: mọi cầu thang phải bắt đầu bằng 1, mọi phần tử tiếp theo tăng đúng 1 và thời điểm mô hình này bị phá vỡ, một cầu thang mới phải bắt đầu. 

Ràng buộc n 1000 đủ nhỏ để ngay cả phương pháp O(n²) cũng có thể chấp nhận được, nhưng cấu trúc cho thấy quét tuyến tính trực tiếp là đủ. 

Một số trường hợp đặc biệt quan trọng. Chuỗi có thể bao gồm một cầu thang đơn, ví dụ 1 2 3 4 5, trong trường hợp đó câu trả lời là một khối có chiều dài 5. Nó cũng có thể bao gồm nhiều cầu thang một bước, ví dụ 1 1 1 1, trong đó mỗi phần tử là cầu thang riêng vì mỗi phần tử 1 không thể mở rộng về phía trước. Một trường hợp tinh tế khác là khi một chuỗi khởi động lại sạch sẽ sau khi ngắt, ví dụ 1 2 3 1 2 3 4, trong đó chúng ta không được hợp nhất nhầm qua điểm đặt lại. 

Một sai lầm ngây thơ là cho rằng bất cứ khi nào trình tự giảm đi hoặc giữ nguyên thì một cầu thang mới sẽ bắt đầu. Mặc dù gần như vậy nhưng quy tắc đúng còn chặt chẽ hơn: một cầu thang mới bắt đầu chính xác khi giá trị dự kiến tiếp theo bị vi phạm và giá trị dự kiến đó được đặt lại về 1. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực là thử tất cả các phân vùng có thể có của chuỗi thành các phân đoạn và kiểm tra xem mỗi phân đoạn có tạo thành một tiến trình hợp lệ từ 1 đến k hay không. Đối với mỗi phân đoạn ứng cử viên, việc xác minh tính hợp lệ cần có thời gian tuyến tính theo độ dài phân đoạn và số lượng phân vùng tăng theo cấp số nhân với n, dẫn đến sự bùng nổ trong tính toán. Ngay cả một cách tiếp cận hạn chế hơn, cố gắng cắt ở mọi vị trí vẫn có nguy cơ xảy ra hành vi bậc hai hoặc tệ hơn tùy thuộc vào cách thực hiện kiểm tra. 

Quan sát quan trọng là điều kiện hợp lệ là cục bộ và xác định. Khi chúng ta ở bên trong một cầu thang, con số dự kiến ​​tiếp theo luôn là current_index_in_stairway + 1. Điều này có nghĩa là chúng ta không cần đoán xem cầu thang kết thúc ở đâu, chúng ta có thể phát hiện nó một cách tham lam ngay khi mô hình bị phá vỡ. 

Chúng tôi chỉ cần quét từ trái sang phải, duy trì giá trị mong đợi tiếp theo trong bậc thang hiện tại. Nếu con số hiện tại phù hợp với kỳ vọng, chúng tôi sẽ tiếp tục. Nếu không, cầu thang trước đó sẽ kết thúc và cầu thang mới sẽ bắt đầu ở vị trí này. 

Điều này làm giảm toàn bộ vấn đề xuống còn một lần truyền qua mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Phân vùng Brute Force | O(2^n · n) | O(n) | Quá chậm | 
| Quét tuyến tính tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Bắt đầu quét chuỗi từ trái sang phải, coi mỗi lần chạy tăng dần hợp lệ liền kề là một bậc thang. Chúng tôi sẽ lưu trữ độ dài của những cầu thang này khi chúng tôi khám phá ra chúng. 
2. Khởi tạo độ dài bậc thang đầu tiên là 1, vì phần tử đầu tiên phải luôn là 1 trong bất kỳ chuỗi hợp lệ nào. Chúng tôi không cần xác thực điều này trên toàn cầu vì vấn đề đảm bảo tính hợp lệ, nhưng chúng tôi vẫn sử dụng nó để neo cấu trúc. 
3. Đối với mỗi phần tử tiếp theo trong chuỗi, hãy so sánh nó với phần tử trước đó cộng thêm một. Đây là sự tiếp nối dự kiến ​​của cầu thang hiện tại. Nếu nó khớp, chúng tôi sẽ kéo dài chiều dài cầu thang hiện tại thêm 1. 
4. Nếu giá trị không khớp với mong đợi, điều đó có nghĩa là bậc thang trước đó đã kết thúc. Chúng tôi lưu trữ độ dài của nó, bắt đầu một cầu thang mới và đặt lại kỳ vọng cho đoạn mới. Phân đoạn mới phải bắt đầu bằng 1, phù hợp với cấu trúc chuỗi. 
5. Sau khi xử lý tất cả các phần tử, hãy đảm bảo ghi lại chiều dài cầu thang cuối cùng vì nó sẽ không bị đóng do không khớp. 

### Tại sao nó hoạt động 

Tại mọi chỉ mục, thuật toán thực thi quy tắc tiếp tục duy nhất có thể có cho một bậc thang hợp lệ: số tiếp theo phải chính xác trước đó + 1. Bởi vì mọi bậc thang đều độc lập và luôn bắt đầu từ 1, nên tại thời điểm điều kiện này không thành công, không có phân đoạn thay thế nào vẫn có thể tạo ra các bậc thang hợp lệ qua ranh giới đó. Điều này làm cho mỗi lần cắt bị ép buộc thay vì được chọn, do đó phân vùng tham lam được xác định duy nhất và khớp với cấu trúc ban đầu của chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    res = []
    current_len = 0
    prev = None
    
    for x in a:
        if current_len == 0:
            current_len = 1
        else:
            if x == prev + 1:
                current_len += 1
            else:
                res.append(current_len)
                current_len = 1
        prev = x
    
    res.append(current_len)
    
    print(len(res))
    print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì chiều dài chạy cho cầu thang hiện tại. Khi mẫu +1 liên tiếp bị ngắt, phân đoạn hiện tại sẽ được đẩy vào danh sách kết quả và phân đoạn mới bắt đầu. Biến`prev`được yêu cầu để kiểm tra điều kiện liên tục một cách hiệu quả trong thời gian O(1) cho mỗi phần tử. 

Điều tinh tế duy nhất là đảm bảo phân đoạn cuối cùng được thêm vào sau vòng lặp, vì việc kết thúc không tự nhiên gây ra tình trạng ngắt. 

## Ví dụ đã hoạt động 

### Mẫu 1:`1 2 3 1 2 3 4`| tôi | giá trị | trước | dự kiến ​​| hành động | phân đoạn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | - | bắt đầu | đoạn bắt đầu | [ ] | 
| 2 | 2 | 1 | 2 | mở rộng | [ ] | 
| 3 | 3 | 2 | 3 | mở rộng | [ ] | 
| 4 | 1 | 3 | 4 | nghỉ, đóng 3 | [3] | 
| 5 | 2 | 1 | 2 | bắt đầu mới | [3] | 
| 6 | 3 | 2 | 3 | mở rộng | [3] | 
| 7 | 4 | 3 | 4 | mở rộng | [3] → [3,4] | 

Đầu ra cuối cùng là 2 cầu thang có độ dài 3 và 4. 

Dấu vết này cho thấy một sự không khớp đơn lẻ sẽ tạo ra ranh giới phân đoạn chính xác như thế nào tại điểm khởi động lại. 

### Mẫu 2:`1 1 1 1`| tôi | giá trị | trước | dự kiến ​​| hành động | phân đoạn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | - | bắt đầu | đoạn bắt đầu | [ ] | 
| 2 | 1 | 1 | 2 | nghỉ | [1] | 
| 3 | 1 | 1 | 2 | nghỉ | [1,1] | 
| 4 | 1 | 1 | 2 | nghỉ | [1,1,1] | 

Mỗi phần tử tạo thành một bậc thang riêng vì không có hai số 1 liên tiếp nào có thể thuộc về một chuỗi tăng hợp lệ. 

Điều này thể hiện tính nghiêm ngặt của quy tắc +1: bình đẳng là không đủ trừ khi nó phù hợp với mức tăng dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi phần tử được xử lý một lần trong một lần quét | 
| Không gian | O(n) | lưu trữ độ dài đoạn trong trường hợp xấu nhất khi mỗi bậc thang có chiều dài 1 | 

Các ràng buộc n 1000 làm cho giải pháp này nhanh chóng, nhưng cấu trúc tuyến tính vẫn tối ưu và có khả năng mở rộng rõ ràng cho các đầu vào lớn hơn nhiều. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    output = io.StringIO()
    with contextlib.redirect_stdout(output):
        solve()
    return output.getvalue().strip()

# provided samples
assert run("7\n1 2 3 1 2 3 4\n") == "2\n3 4"
assert run("4\n1 1 1 1\n") == "4\n1 1 1 1"
assert run("5\n1 2 3 4 5\n") == "1\n5"

# custom cases
assert run("1\n1\n") == "1\n1"
assert run("2\n1 2\n") == "1\n2"
assert run("3\n1 2 1\n") == "2\n2 1"
assert run("6\n1 2 3 1 2 1\n") == "3\n3 2 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 1 | trường hợp cạnh phần tử đơn | 
| 1 2 | 1 2 | cầu thang đơn | 
| 1 2 1 | 2 2 1 | thiết lập lại giữa chuỗi | 
| 1 2 3 1 2 1 | 3 2 1 | khởi động lại nhiều lần | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chuỗi thay thế thường xuyên, chẳng hạn như`1 1 1 1`. Thuật toán xử lý mọi sự không khớp với mức tăng dự kiến ​​dưới dạng cắt bắt buộc. Trên phần tử đầu tiên, một phân đoạn mới bắt đầu. Ở phần tử thứ hai, giá trị mong đợi là 2 nhưng chúng tôi thấy 1, vì vậy chúng tôi đóng phân đoạn đầu tiên ngay lập tức và bắt đầu một phân đoạn mới. Điều này lặp lại cho mọi phần tử, tạo ra bốn đoạn có độ dài 1, phù hợp với cách diễn giải hợp lệ duy nhất. 

Một trường hợp khác là leo núi liên tục hoàn hảo như`1 2 3 4 5`. Ở đây, giá trị mong đợi luôn khớp với giá trị thực tế nên không có vết cắt nào được kích hoạt trong quá trình quét. Kết quả là một phân đoạn duy nhất và thuật toán chỉ thêm phân đoạn đó vào cuối, xác nhận rằng không có phần không khớp nào tương ứng với một cầu thang. 

Một cấu trúc hỗn hợp như`1 2 3 1 2`thể hiện tính đúng đắn tại các ranh giới. Sự không khớp đầu tiên xảy ra ở phần tử thứ tư, trong đó giá trị mong đợi là 4 nhưng chúng tôi thấy 1, buộc phải phân chia chính xác sau độ dài 3. Sau đó, phân đoạn thứ hai sẽ tiếp tục hoàn toàn từ điểm đặt lại đó, tạo ra phân đoạn cuối cùng khớp duy nhất với cấu trúc ban đầu.
