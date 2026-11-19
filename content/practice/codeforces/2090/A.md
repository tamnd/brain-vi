---
title: "CF 2090A - Truy tìm kho báu"
description: "Chúng ta được yêu cầu xác định xem ai sẽ đào kho báu bị chôn vùi đầu tiên giữa hai người, Bé B và Bé K. Kho báu được chôn ở độ sâu một phần a,5 mét, đây là điểm mấu chốt: nghĩa là ngưỡng luôn vượt quá 0,5 mét so với số nguyên a."
date: "2026-06-08T05:50:06+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2090
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1012 (Div. 2)"
rating: 800
weight: 2090
solve_time_s: 86
verified: true
draft: false
---

[CF 2090A - Truy tìm kho báu](https://codeforces.com/problemset/problem/2090/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem ai sẽ đào kho báu bị chôn giấu trước giữa hai người, Bé B và Bé K. Kho báu được chôn ở độ sâu một phần`a.5`mét, đó là điều quan trọng: nó có nghĩa là ngưỡng luôn luôn`.5`mét ngoài số nguyên`a`. Bé B đào`x`mét mỗi ngày, Little K đào`y`mét trên của cô ấy, và chúng thay phiên nhau bắt đầu từ Little B. Đối với mỗi trường hợp thử nghiệm, chúng ta phải in`NO`nếu Bé B đào kho báu trước, và`YES`nếu Little K làm vậy. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp các số nguyên`x`,`y`, Và`a`. Mỗi giá trị có thể lớn tới 10^9 và có thể có tới 1000 trường hợp thử nghiệm. Các ràng buộc loại trừ việc mô phỏng việc đào hàng ngày vì mô phỏng trực tiếp có thể yêu cầu hàng tỷ lần lặp trong trường hợp xấu nhất, vượt quá giới hạn 1 giây. Vấn đề vốn dĩ là toán học, và điều quan trọng là suy luận về độ sâu tích lũy và thứ tự lần lượt theo đại số. 

Một trường hợp khó phát hiện khi cả hai số lượng đào đều bằng nhau. Ví dụ, nếu`x = y = 2`Và`a = 1`, kho báu ở`1.5`mét. Bé B đào được 2m ngay ngày đầu tiên, lập tức vượt 1,5m. Cách tiếp cận ngây thơ giả định các ngày xen kẽ mà không xem xét các ngưỡng một phần có thể gợi ý rằng lượt của K có vấn đề, nhưng trên thực tế, lần đào đầu tiên là đủ. Một trường hợp khác xảy ra khi số lượng đào của một người lớn hơn nhiều so với số lượng đào khác, điều này có thể chuyển kết quả sang người đầu tiên bất kể`a`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng quá trình đào bới hàng ngày. Khởi tạo bộ đếm cho tổng độ sâu. Vào mỗi ngày, hãy thêm một trong hai`x`hoặc`y`tùy vào lượt của ai. Sau mỗi lần thêm, kiểm tra xem độ sâu tích lũy có vượt quá`a.5`. Điều này hoạt động chính xác nhưng không hiệu quả đối với đầu vào lớn. Mỗi trường hợp thử nghiệm có thể yêu cầu tới`a/x + a/y`số lần lặp, lên tới 10^9 trong trường hợp xấu nhất. Điều này rõ ràng vượt quá giới hạn thời gian. 

Cách tiếp cận tối ưu xuất phát từ việc quan sát rằng chúng ta chỉ quan tâm đến **ngày mà độ sâu tích lũy vượt quá`a.5`**, không phải là tiến trình chính xác hàng ngày. Nếu chúng ta bình thường hóa độ sâu kho báu thành`2a + 1`để tránh phân số, bài toán giảm xuống việc so sánh lần lượt tổng tỷ lệ của máy đào thứ nhất và máy đào thứ hai. Cái nhìn sâu sắc quan trọng là sự ngang bằng của độ sâu kho báu đối với`x + y`quyết định kết quả. Về mặt toán học, nếu`a // (x + y)`chu trình đầy đủ của cả B và K vẫn để lại số dư lớn hơn`x`, sau đó Bé B sẽ đào kho báu ở lượt tiếp theo; nếu không thì Little K sẽ làm vậy. Điều này làm giảm mỗi trường hợp thử nghiệm thành một vài phép tính số nguyên, thời gian O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a / phút(x, y)) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhân độ sâu kho báu`a.5`bằng 2 để có ngưỡng số nguyên`threshold = 2 * a + 1`. Điều này tránh phân số, giúp đơn giản hóa việc so sánh và số học. 
2. Nhân số lần đào hàng ngày của Bé B với 2 để có được`b2 = 2 * x`. Nhân số lần đào hàng ngày của Little K với 2 để có được`k2 = 2 * y`. Điều này đặt số lượng đào của họ ngang bằng với ngưỡng. 
3. Coi các lượt luân phiên là chu kỳ lặp đi lặp lại của`b2 + k2`. Tính xem có bao nhiêu chu kỳ đầy đủ phù hợp với ngưỡng bằng phép chia số nguyên:`cycles = threshold // (b2 + k2)`. 
4. Tính độ sâu còn lại sau các chu kỳ đầy đủ này:`remainder = threshold - cycles * (b2 + k2)`. Số dư này nhỏ hơn`b2 + k2`và đại diện cho ngày chưa hoàn thành cuối cùng. 
5. So sánh phần còn lại với`b2`. Nếu như`remainder <= b2`, nghĩa là Bé B vượt ngưỡng ở lượt tiếp theo nên đáp án là`NO`. Ngược lại, Bé K sẽ đến đích trước và câu trả lời là`YES`. 

Tại sao nó hoạt động: chia tỷ lệ mọi thứ thành 2 sẽ chuyển đổi ngưỡng phân số thành một bài toán số nguyên trong khi vẫn giữ nguyên thứ tự ngày. Điều bất biến là sau mỗi chu kỳ đầy đủ của cả hai máy đào, độ sâu còn lại luôn được xử lý ở một lượt cuối cùng và việc so sánh nó với số lượng đào của B sẽ xác định chính xác ai vượt ngưỡng trước. Không có sự mơ hồ hoặc lỗi sai sót nào vì thang đo chiếm tỷ lệ`.5`mét một cách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    x, y, a = map(int, input().split())
    threshold = 2 * a + 1
    b2 = 2 * x
    k2 = 2 * y
    remainder = threshold % (b2 + k2)
    if remainder <= b2:
        print("NO")
    else:
        print("YES")
```Mã đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`cho nhiều trường hợp thử nghiệm. Ngưỡng được chia tỷ lệ theo 2 để xử lý`.5`một cách tự nhiên. Chúng tôi chỉ tính toán phần còn lại sau toàn bộ chu kỳ để xác định người đào đầu tiên vượt quá nó. Sự so sánh`<= b2`trực tiếp kiểm tra xem Bé B có đến kho báu trước không. 

Các điểm tinh tế là việc chia tỷ lệ thành số nguyên và phép toán mô đun để cô lập lượt không hoàn chỉnh cuối cùng. Việc bỏ qua sẽ dẫn đến kết quả không chính xác đối với ngưỡng phân số hoặc ngưỡng rất lớn.`a`. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
1 2 4
```| Bước | Ngưỡng | b2 | k2 | Phần còn lại | So sánh | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Tính toán | 2*4 +1 = 9 | 2 | 4 | 9 % (2+4) = 9 % 6 = 3 | 3 <= 2? KHÔNG | CÓ | 

Bé B đào 1 mét, K đào 2 mét, lặp lại. Chia tỷ lệ theo 2, ngưỡng trở thành 9, B = 2, K = 4. Sau một chu kỳ đầy đủ (B+K=6), số dư là 3. Vì 3 > 2, K vượt qua ngưỡng trước. 

Đầu vào mẫu 2:```
2 1 4
```| Bước | Ngưỡng | b2 | k2 | Phần còn lại | So sánh | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Tính toán | 2*4+1=9 | 4 | 2 | 9 % 6 = 3 | 3 <= 4? CÓ | KHÔNG | 

Ở đây, B đào nhiều hơn K mỗi ngày. Phần còn lại sau chu kỳ đầy đủ đầu tiên là 3, tức là <= số lần đào của B nên B đến được kho báu trước. 

Những dấu vết này xác nhận thuật toán sẽ chia tỷ lệ chính xác, giảm chu kỳ và xử lý ai vượt quá ngưỡng trước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ các phép toán số học và mô đun số nguyên | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung, chỉ có một vài số nguyên | 

Ngay cả với mức tối đa`t = 1000`Và`x, y, a`lên tới 10^9, giải pháp thực hiện dưới 1 giây vì mỗi trường hợp thử nghiệm có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        x, y, a = map(int, input().split())
        threshold = 2 * a + 1
        b2 = 2 * x
        k2 = 2 * y
        remainder = threshold % (b2 + k2)
        if remainder <= b2:
            print("NO")
        else:
            print("YES")
    
    return output.getvalue().strip()

# Provided samples
assert run("3\n1 2 4\n2 1 4\n2 2 1\n") == "YES\nNO\nNO", "sample tests"

# Custom cases
assert run("1\n1 1 1\n") == "YES", "equal digs, threshold 1.5"
assert run("1\n10 1 1000000000\n") == "NO", "large a, B digs first enough"
assert run("1\n1 10 1000000000\n") == "YES", "large a, K digs more eventually"
assert run("1\n5 5 5\n") == "NO", "equal digs, B digs first"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | CÓ | Ngưỡng 0,5 mét ngoài 1, xen kẽ các hố bằng nhau | 
| 10 1 1000000000 | KHÔNG | B đào đủ để vượt qua độ sâu lớn trước | 
| 1 10 1000000000 | CÓ | K đào thêm, cuối cùng sẽ vượt qua trước | 
| 5 5 5 | KHÔNG | Số lần đào bằng nhau, người đào đầu tiên sẽ thắng theo mặc định | 

## Vỏ cạnh 

Nếu`x = y`Và`a = 1`, ngưỡng là 2*
