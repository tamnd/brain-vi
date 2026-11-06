---
title: "CF 1249A - Lại một lần chia đội"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi tình huống có một danh sách các số nguyên riêng biệt biểu thị trình độ kỹ năng của học sinh."
date: "2026-06-13T20:51:51+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1249
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 595 (Div. 3)"
rating: 800
weight: 1249
solve_time_s: 140
verified: true
draft: false
---

[CF 1249A - Lại một lần nữa chia thành các nhóm](https://codeforces.com/problemset/problem/1249/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi tình huống có một danh sách các số nguyên riêng biệt biểu thị trình độ kỹ năng của học sinh. Mục tiêu là chia những học sinh này thành số nhóm nhỏ nhất có thể sao cho không có nhóm nào có hai học sinh có kỹ năng khác nhau đúng một. 

Hạn chế rất cục bộ: chỉ các cặp có chênh lệch chính xác 1 bị cấm trong cùng một nhóm. Bất kỳ cặp nào khác đều được phép, dù chênh lệch của chúng lớn hay nhỏ nhưng không bằng 1. 

Từ đó, bài toán trở thành bài toán nhóm trên các số nguyên với quy tắc kề rất cụ thể. Nếu chúng ta tưởng tượng mỗi giá trị là một nút trên trục số thì mỗi giá trị chỉ “xung đột” với giá trị lân cận của nó trừ một và giá trị cộng một. 

Mỗi truy vấn có kích thước nhỏ, với tối đa 100 sinh viên và giá trị kỹ năng cũng bị giới hạn bởi 100. Điều này ngay lập tức loại trừ mọi nhu cầu về tối ưu hóa mạnh mẽ hoặc cấu trúc dữ liệu nâng cao. Ngay cả giải pháp O(n^2) cho mỗi truy vấn cũng đủ nhanh. 

Trường hợp chính thường đánh lừa cách tiếp cận ngây thơ là giả định rằng chỉ các chuỗi liên tiếp mới quan trọng mà không xem xét mức độ chồng chéo giữa nhiều chuỗi sẽ buộc nhiều nhóm như thế nào. 

Ví dụ, hãy xem xét các giá trị`[2, 3, 4]`. Một ý tưởng ngây thơ có thể là nghĩ rằng chúng ta chỉ cần hai đội vì chúng ta có thể luân phiên phân công nhiệm vụ. Nhưng vì 2 xung đột với 3 và 3 xung đột với 4, nhưng 2 và 4 vẫn ổn, việc phân nhóm chính xác phụ thuộc vào cách chúng ta gán 3, hoạt động như một cầu nối. 

Một trường hợp tinh tế khác là các bộ bị ngắt kết nối như`[1, 3, 5]`, nơi không có xung đột nào tồn tại và một đội duy nhất là đủ. 

Khó khăn chính là xác định chính xác có bao nhiêu “lớp xung đột” bị ràng buộc bởi các ràng buộc liền kề. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để nghĩ về điều này là phân từng học sinh vào các nhóm, thử mọi cách sắp xếp có thể trong khi vẫn tôn trọng các ràng buộc. Đối với mỗi học sinh, chúng tôi cố gắng xếp họ vào một nhóm hiện có nếu không có thành viên nào khác nhau đúng 1, nếu không chúng tôi sẽ tạo một nhóm mới. Điều này giống như một nỗ lực tô màu tham lam, nhưng nếu được thực hiện một cách ngây thơ bằng cách quay lại, nó sẽ khám phá các khả năng theo cấp số nhân vì mỗi phép gán đều ảnh hưởng đến tính khả thi trong tương lai. 

Ngay cả một kẻ tham lam đơn giản hơn, không có cấu trúc cũng có thể thất bại nếu trật tự không may mắn. Ví dụ: chèn theo thứ tự đầu vào mà không xem xét sắp xếp có thể dẫn đến việc tạo ra các nhóm mới không cần thiết. 

Quan sát quan trọng là ràng buộc chỉ phụ thuộc vào các giá trị nguyên liền kề. Nếu sắp xếp mảng, chúng ta có thể suy luận về cách các con số tương tác qua các phân đoạn liên tiếp. Bất kỳ giá trị nào`x`chỉ xung đột với`x-1`Và`x+1`. Điều này có nghĩa là bài toán tương đương với việc tô màu các số trên một đường thẳng trong đó các cạnh chỉ tồn tại giữa các số nguyên liên tiếp. 

Bây giờ hãy xem xét cần bao nhiêu đội. Nếu chúng ta nhóm các số theo giá trị thì lần duy nhất chúng ta buộc phải tách thành nhiều nhóm là khi có chuỗi các số nguyên liên tiếp. Bên trong một lần chạy liên tiếp như`[k, k+1, k+2, ..., k+m]`, mỗi số xung đột với các số lân cận của nó, tạo thành một biểu đồ đường dẫn. Biểu đồ đường dẫn có màu sắc số 2, nhưng nhiều đường dẫn rời rạc có thể chồng lên nhau theo cách vẫn chỉ yêu cầu 2 màu trên toàn cầu. 

Điều này dẫn đến một cách nhìn đơn giản hơn: chúng ta chỉ cần biết liệu có tồn tại cặp số nguyên liên tiếp nào trong tập hợp hay không. Nếu không có cặp nào như vậy tồn tại thì tất cả các giá trị đều độc lập và chúng ta cần chính xác 1 đội. Nếu tồn tại ít nhất một cặp như vậy thì chúng ta cần chính xác 2 đội, bởi vì bất kỳ chuỗi xung đột nào cũng luôn có thể có 2 màu bằng cách phân công xen kẽ theo tính chẵn lẻ của giá trị. 

Vì vậy, toàn bộ vấn đề giảm xuống còn việc kiểm tra xem có tồn tại`x`sao cho cả hai`x`Và`x+1`đang có mặt. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân công/quay lại Brute Force | Ồ (n!) | O(n) | Quá chậm | 
| Kiểm tra các cặp liên tiếp | O(n) mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các giá trị kỹ năng của học sinh cho một truy vấn và lưu trữ chúng trong một bộ để kiểm tra tư cách thành viên nhanh chóng. Điều này là cần thiết vì chúng ta cần tra cứu liên tục để kiểm tra xem các số nguyên liên tiếp có tồn tại hay không. 
2. Lặp qua từng giá trị`x`trong bộ và kiểm tra xem`x + 1`cũng tồn tại trong tập hợp. Điều này trực tiếp phát hiện xem có ít nhất một cạnh xung đột trong biểu đồ ẩn hay không. 
3. Nếu chúng tôi tìm thấy bất kỳ cặp nào như vậy`(x, x+1)`, chúng tôi ngay lập tức kết luận rằng cần có ít nhất hai đội. Điều này là do hai giá trị này không thể chia sẻ một nhóm, buộc phải có sự phân chia lan truyền qua bất kỳ chuỗi kết nối nào. 
4. Nếu không có cặp nào như vậy tồn tại sau khi kiểm tra tất cả các giá trị thì không có ràng buộc nào được kích hoạt và tất cả học sinh có thể được xếp vào một nhóm duy nhất. 

### Tại sao nó hoạt động 

Các giá trị tạo thành một biểu đồ trong đó các cạnh chỉ tồn tại giữa các số nguyên liên tiếp. Biểu đồ này có cấu trúc lưỡng cực nên luôn yêu cầu tối đa 2 màu. Trường hợp duy nhất mà 1 màu là đủ là khi không có cạnh nào cả, nghĩa là không tồn tại hai số nguyên liên tiếp trong đầu vào. Do đó, việc phát hiện sự tồn tại của ít nhất một cạnh sẽ xác định đầy đủ câu trả lời là 1 hay 2. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

q = int(input())
for _ in range(q):
    n = int(input())
    a = list(map(int, input().split()))
    
    s = set(a)
    
    ans = 1
    for x in s:
        if x + 1 in s:
            ans = 2
            break
    
    print(ans)
```Giải pháp dựa trên một tập hợp để cho phép kiểm tra liên tục theo thời gian xem liệu một giá trị có tồn tại hay không. Vòng lặp trên tập hợp đảm bảo chúng tôi chỉ kiểm tra các giá trị duy nhất, tránh các công việc lặp lại không cần thiết ngay cả khi không có các giá trị trùng lặp trong câu lệnh đầu vào theo vấn đề. 

Chi tiết triển khai chính là thoát sớm: ngay khi tìm thấy một cặp liền kề, chúng tôi dừng lại vì câu trả lời không thể vượt quá 2. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[2, 10, 1, 20]`| x | x+1 trong bộ? | Quyết định | 
| --- | --- | --- | 
| 2 | 3 không | tiếp tục | 
| 10 | 11 không | tiếp tục | 
| 1 | 2 có | đáp án = 2, dừng lại | 

Điều này khẳng định rằng một cặp xung đột duy nhất là đủ để buộc hai đội phải tham gia. 

### Ví dụ 2 

đầu vào:`[3, 6]`| x | x+1 trong bộ? | Quyết định | 
| --- | --- | --- | 
| 3 | 4 không | tiếp tục | 
| 6 | 7 không | tiếp tục | 

Không có xung đột tồn tại, vì vậy một đội là đủ. 

Những dấu vết này cho thấy thuật toán đang phát hiện một cách hiệu quả xem đồ thị cảm ứng có bất kỳ cạnh nào hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi truy vấn | Mỗi giá trị được kiểm tra một lần với tra cứu tập hợp O(1) | 
| Không gian | O(n) | Đặt lưu trữ tất cả các giá trị của truy vấn | 

Giới hạn hạn chế`n`ở mức 100, vì vậy ngay cả trên 100 truy vấn thì tổng công việc cũng không đáng kể. Giải pháp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    q = int(input())
    out = []
    for _ in range(q):
        n = int(input())
        a = list(map(int, input().split()))
        s = set(a)
        ans = 1
        for x in s:
            if x + 1 in s:
                ans = 2
                break
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""4
4
2 10 1 20
2
3 6
5
2 3 4 99 100
1
42
""") == """2
1
2
1"""

# custom cases
assert run("""1
3
1 3 5
""") == "1"

assert run("""1
2
1 2
""") == "2"

assert run("""1
4
10 11 12 13
""") == "2"

assert run("""1
5
100 1 50 2 99
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 / 1 3 5`| 1 | không có cặp liên tiếp | 
|`1 2 / 1 2`| 2 | xung đột đơn lẻ chia rẽ lực lượng | 
|`1 4 / 10 11 12 13`| 2 | chuỗi dài liên tiếp | 
|`1 5 / 100 1 50 2 99`| 2 | nhiều xung đột rải rác | 

## Vỏ cạnh 

Đối với đầu vào không có số nguyên liên tiếp, chẳng hạn như`[1, 4, 7]`, thuật toán xây dựng một tập hợp`{1, 4, 7}`và tìm thấy không`x`Ở đâu`x+1`tồn tại. Vòng lặp kết thúc mà không kích hoạt điều kiện, vì vậy câu trả lời vẫn là 1, phù hợp với thực tế là không tồn tại hạn chế nào. 

Đối với một đầu vào hoàn toàn liên tiếp như`[5, 6, 7, 8]`, tập hợp này chứa nhiều cặp liền kề. Thuật toán phát hiện`5-6`ngay lập tức (hoặc bất kỳ cặp nào khác tùy thuộc vào thứ tự lặp), đặt câu trả lời thành 2 và dừng lại. Điều này phù hợp với nhu cầu luân phiên phân công dọc theo chuỗi, xác nhận rằng chỉ cần kiểm tra một lần là đủ để nắm bắt toàn bộ cấu trúc.
