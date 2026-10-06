---
title: "CF 104930C - Vịnh sô cô la của con bạc"
description: "Mỗi máy đánh bạc hoạt động giống như một máy tạo phần thưởng ngẫu nhiên. Khi bạn kéo một máy một lần, nó sẽ trả về một giá trị từ một tập hữu hạn cố định, mỗi giá trị có xác suất đã biết. Những xác suất đó không thay đổi theo thời gian và mỗi lần kéo đều độc lập với các lần kéo trước đó."
date: "2026-06-28T07:43:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104930
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 01-26-24 Div. 2 (Beginner)"
rating: 0
weight: 104930
solve_time_s: 253
verified: false
draft: false
---

[CF 104930C - Vịnh sô cô la của người cờ bạc](https://codeforces.com/problemset/problem/104930/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi máy đánh bạc hoạt động giống như một máy tạo phần thưởng ngẫu nhiên. Khi bạn kéo một máy một lần, nó sẽ trả về một giá trị từ một tập hữu hạn cố định, mỗi giá trị có xác suất đã biết. Những xác suất đó không thay đổi theo thời gian và mỗi lần kéo đều độc lập với các lần kéo trước đó. 

Bạn được phép thực hiện chính xác$n$tổng số lực kéo và mỗi lực kéo có thể được gán cho bất kỳ lực kéo nào$k$máy móc. Mục tiêu là phân phối chúng$n$kéo qua các máy theo cách tối đa hóa tổng số phần thưởng nhận được dự kiến. 

Đầu ra là một số thực duy nhất: tổng phần thưởng dự kiến ​​​​tối đa có thể sau khi thực hiện tất cả$n$các quyết định. 

Quan sát quan trọng ẩn trong công thức này là không có gì trong bài toán đưa ra sự tương tác giữa các lần kéo. Một chiếc máy không trở nên tệ hơn hay tốt hơn sau khi được sử dụng và không có ràng buộc nào về việc liên kết máy nào phải được sử dụng với máy khác. 

Từ những hạn chế,$n, k \le 100$và mỗi phân phối có tối đa 1000 kết quả. Ngay cả một giải pháp tính toán lại kỳ vọng trên mỗi máy cũng đủ nhanh. Khó khăn thực sự duy nhất là nhận ra rằng cấu trúc này loại bỏ mọi nhu cầu về lập trình động hoặc phân bổ tổ hợp. 

Một cách giải thích ngây thơ thường xuất hiện trong những lần thử đầu tiên là cho rằng việc phân phối kéo có thể yêu cầu tìm kiếm trên tất cả các phân bổ, điều đó có nghĩa là chỉ định$n$kéo giữa$k$máy móc. Điều đó sẽ dẫn đến$\binom{n+k-1}{k-1}$khả năng, tăng trưởng nhanh chóng ngay cả đối với các giá trị nhỏ. 

Một ví dụ nhỏ minh họa cái bẫy: 

Nếu$n = 3$,$k = 2$và máy A có giá trị kỳ vọng là 5 trong khi máy B có giá trị kỳ vọng là 4, mọi phân bổ như (2,1), (1,2) hoặc (3,0) đều phải được xem xét ở chế độ xem bạo lực. Một cách tiếp cận ngây thơ có thể cho rằng sự phân chia khác nhau sẽ tạo ra những hành vi khác nhau. Trong thực tế, chỉ có kỳ vọng cho mỗi lần kéo mới quan trọng, vì vậy (3,0) chiếm ưu thế. 

Trường hợp cạnh tinh tế thứ hai là khi phân phối không nguyên hoặc bị lệch nhiều. Ví dụ: một máy tạo ra 100 với xác suất 0,01 và 0 nếu không vẫn có kỳ vọng 1. Một trực giác ngây thơ có thể thiên về những phần thưởng nhỏ thường xuyên, nhưng kỳ vọng sẽ biến mọi thứ thành một vô hướng duy nhất. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê số lượng lực kéo được chỉ định cho mỗi máy, tôn trọng rằng tổng số lượng là$n$. Đối với mỗi lần phân bổ, chúng tôi tính toán giá trị mong đợi bằng cách nhân số lượng với kỳ vọng được tính toán trước của từng máy. 

Tính đúng đắn của cách tiếp cận đó rất đơn giản vì kỳ vọng là tuyến tính và độc lập với mỗi lần kéo. Tuy nhiên, số lượng phân bổ là theo cấp số nhân trong$k$, vì nó tương đương với việc đếm các thành phần của$n$vào trong$k$các bộ phận. Ngay cả đối với$n = 100$, không gian này trở nên rộng lớn về mặt thiên văn, khiến cho việc liệt kê là không thể. 

Sự đơn giản hóa quan trọng đến từ việc nhận ra rằng mọi lần kéo đều giống hệt nhau ngoại trừ việc chọn máy nào. Vì kỳ vọng có tính cộng gộp và độc lập nên mỗi lực kéo có thể được xử lý riêng biệt. Điều này thu gọn vấn đề thành việc chọn máy có giá trị kỳ vọng lớn nhất và sử dụng nó cho tất cả$n$kéo. 

Cấu trúc phân bổ biến mất hoàn toàn vì không có lợi nhuận giảm dần hoặc ràng buộc chia sẻ giữa các lần kéo vượt quá tổng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong$n$| O(n) | Quá chậm | 
| Tối ưu | O(k · m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi máy, hãy tính giá trị kỳ vọng của nó bằng cách tính tổng tất cả các kết quả$r_j \cdot p_j$. Điều này nén mỗi phân phối thành một đại lượng vô hướng duy nhất vì kỳ vọng mô tả đầy đủ sự đóng góp của nó cho mỗi lần kéo. 
2. Theo dõi giá trị mong đợi tối đa trong số tất cả các máy. Điều này thể hiện phần thưởng tốt nhất có thể đạt được cho mỗi lần kéo. 
3. Nhân kỳ vọng tối đa này với$n$, vì mỗi một trong số$n$lực kéo phải được chỉ định cho máy tốt nhất độc lập với các lựa chọn trước đó. 

### Tại sao nó hoạt động 

Mỗi lần kéo đóng góp độc lập vào tổng kỳ vọng và giá trị kỳ vọng của tổng là tổng của các giá trị kỳ vọng. Vì không có sự phụ thuộc giữa các lần kéo và không có chi phí cho việc tái sử dụng cùng một máy nên chiến lược tối ưu không bao giờ cần đa dạng hóa. Bất kỳ sự sai lệch nào so với việc luôn chọn chiếc máy được mong đợi tốt nhất sẽ làm giảm nghiêm trọng sự đóng góp của lực kéo đó mà không ảnh hưởng đến những chiếc máy khác. 

Điều bất biến là sau khi quyết định máy tối ưu cho một lần kéo, việc mở rộng quyết định thành nhiều lần kéo không làm thay đổi bất kỳ cấu trúc xác suất nào. Mỗi lần kéo bổ sung là một bản sao giống hệt của cùng một lựa chọn tối ưu hóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, k = map(int, input().split())
    
    best = 0.0

    for _ in range(k):
        m = int(input())
        r = list(map(int, input().split()))
        p = list(map(float, input().split()))
        
        exp = 0.0
        for rv, prob in zip(r, p):
            exp += rv * prob
        
        if exp > best:
            best = exp

    print(n * best)

if __name__ == "__main__":
    main()
```Cốt lõi của việc triển khai là giảm từng máy xuống một giá trị mong đợi duy nhất. Vòng lặp về phần thưởng và xác suất là nơi duy nhất sử dụng cấu trúc phân phối. 

Một lỗi phổ biến là cố gắng mô phỏng các lần kéo hoặc xây dựng DP trên các hoạt động còn lại. Điều đó là không cần thiết vì nhà nước không phát triển. Một lỗi khác có thể xảy ra là tính tổng xác suất dấu phẩy động không chính xác; ở đây chúng được đảm bảo có tổng bằng 1, do đó không cần chuẩn hóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử chúng ta có$n = 3$,$k = 2$. 

Máy A: kết quả (10 với xác suất 0,5, 0 với xác suất 0,5) 

Máy B: kết quả (4 với xác suất 1,0) 

Các giá trị dự kiến là: 

A = 5, B = 4 

| Bước | Máy A điểm kinh nghiệm | Máy B điểm kinh nghiệm | Tốt nhất cho đến nay | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 4 | 5 | chọn A | 
| 2 | 5 | 4 | 5 | chọn A | 
| 3 | 5 | 4 | 5 | chọn A | 

Câu trả lời cuối cùng là$3 \times 5 = 15$. 

Dấu vết này cho thấy rằng việc tính toán lại các lựa chọn cho mỗi lần kéo không làm thay đổi quyết định, củng cố rằng cùng một cỗ máy thống trị trên toàn cầu. 

### Ví dụ 2 

hãy để$n = 4$,$k = 3$. 

Máy A: luôn 2 

Máy B: 3 với xác suất 0,5, 0 nếu không 

Máy C: 1 với xác suất 1 

Kỳ vọng: 

A = 2, B = 1,5, C = 1 

| Bước | A | B | C | Tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 1,5 | 1 | A | 
| 2 | 2 | 1,5 | 1 | A | 
| 3 | 2 | 1,5 | 1 | A | 
| 4 | 2 | 1,5 | 1 | A | 

Câu trả lời là$4 \times 2 = 8$. 

Điều này xác nhận rằng ngay cả khi các phân phối đa dạng và không trực quan, việc thu gọn chúng theo kỳ vọng vẫn duy trì tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k \cdot m)$| kỳ vọng của mỗi máy được tính toán một lần | 
| Không gian |$O(1)$| chỉ lưu trữ giá trị tốt nhất hiện tại | 

Giới hạn$k \le 100$Và$m \le 1000$làm cho tổng số công việc nhiều nhất$10^5$phép nhân, đó là tầm thường trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n, k = map(int, sys.stdin.readline().split())
    best = 0.0

    for _ in range(k):
        m = int(sys.stdin.readline())
        r = list(map(int, sys.stdin.readline().split()))
        p = list(map(float, sys.stdin.readline().split()))
        exp = sum(x * y for x, y in zip(r, p))
        best = max(best, exp)

    return str(n * best)

# sample
assert run("3 2\n2\n10 0\n0.5 0.5\n1\n4\n1.0\n") == "15.0"

# all equal machines
assert run("2 2\n2\n1 2\n0.5 0.5\n2\n1 2\n0.5 0.5\n") == "3.0"

# single machine
assert run("4 1\n2\n3 0\n0.5 0.5\n") == "6.0"

# deterministic best
assert run("5 2\n1\n10\n1.0\n1\n1\n1.0\n") == "50.0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| máy đơn | 6.0 | tính đúng đắn của trường hợp cơ sở | 
| máy giống hệt nhau | 3.0 | xử lý đối xứng | 
| quyết định tốt nhất | 50,0 | tham lam lựa chọn kỳ vọng tốt nhất | 

## Vỏ cạnh 

Một cỗ máy có phân phối sai lệch cao có thể gây hiểu nhầm nếu người ta tập trung vào các kết quả hiếm gặp. Ví dụ: một máy xuất ra 1000 với xác suất 0,001 và 0 nếu không thì vẫn chỉ đóng góp 1 trong kỳ vọng. Thuật toán xử lý việc này một cách chính xác vì nó giảm mọi thứ xuống mức mong đợi trước khi so sánh. 

Một trường hợp khác là khi nhiều máy chia sẻ cùng một giá trị mong đợi. Trong trường hợp đó, bất kỳ giá trị nào trong số chúng đều tối ưu và thuật toán vẫn trả về kết quả chính xác vì chỉ giá trị tối đa quan trọng chứ không phải danh tính của nó. 

Cuối cùng, khi$n = 1$, giải pháp trả về chính xác kỳ vọng duy nhất tốt nhất trong số tất cả các máy, phù hợp với cách diễn giải trực quan của một vấn đề lựa chọn duy nhất.
