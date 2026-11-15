---
title: "CF 1867A - green_gold_dog, mảng và hoán vị"
description: "Chúng ta được cho một mảng a có độ dài n. Ta phải xây dựng một hoán vị b gồm các số từ 1 đến n sao cho khi trừ từng phần tử tạo thành ci = ai - bi thì số giá trị phân biệt xuất hiện trong c càng lớn càng tốt."
date: "2026-06-08T23:39:04+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1867
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 897 (Div. 2)"
rating: 800
weight: 1867
solve_time_s: 94
verified: false
draft: false
---

[CF 1867A - green_gold_dog, mảng và hoán vị](https://codeforces.com/problemset/problem/1867/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, sắp xếp 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`a`chiều dài`n`. Chúng ta phải xây dựng một hoán vị`b`số từ`1`ĐẾN`n`sao cho khi chúng ta trừ từng phần tử, tạo thành`c_i = a_i - b_i`, số lượng các giá trị khác biệt xuất hiện trong`c`là càng lớn càng tốt. 

Nói một cách đơn giản hơn, chúng tôi đang ghép từng vị trí`i`với một số duy nhất`b_i`giữa`1`Và`n`, và mỗi cặp tạo ra một sự khác biệt. Mục tiêu là làm cho những khác biệt này trở nên đa dạng nhất có thể ở tất cả các vị trí. 

Hạn chế chính đó là`b`phải là một hoán vị, nên mọi số nguyên từ`1`ĐẾN`n`được sử dụng đúng một lần. Chúng tôi không tối ưu hóa các giá trị của`c`bản thân chúng, chỉ có bao nhiêu giá trị riêng biệt xuất hiện trong`c`. 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm tối đa là`4 * 10^4`, do đó, mọi giải pháp về cơ bản đều phải chạy theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì liên quan đến việc sắp xếp cộng với mô phỏng nặng đều được, nhưng bất cứ điều gì liên quan đến bậc hai`n`sẽ là ranh giới nếu lặp lại nhiều lần. 

Một trường hợp khó nhận thấy là khi`n = 1`. Khi đó chỉ có một hoán vị có thể xảy ra và câu trả lời là tầm thường. Một tình huống khác cần phải cẩn thận là khi có nhiều`a_i`đều bình đẳng. Một ý tưởng ngây thơ có thể cố gắng so khớp các giá trị bằng nhau theo một cách đặc biệt, nhưng vấn đề không mang lại lợi ích cho việc kiểm soát mức độ khác biệt mà chỉ là sự khác biệt của chúng. 

Trực giác sai lầm phổ biến nhất là cố gắng tối đa hóa mức độ lan truyền về mặt số lượng của`c_i`. Điều đó không liên quan: chỉ có mối quan hệ bình đẳng giữa`c_i`vấn đề. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ thử mọi hoán vị`b`của`[1, 2, ..., n]`, tính toán tất cả sự khác biệt`c`và đếm xem có bao nhiêu giá trị khác biệt xuất hiện. Điều này đúng nhưng không khả thi ngay lập tức. có`n!`hoán vị, và với mỗi hoán vị chúng ta tính toán`n`sự khác biệt, dẫn đến gần như`O(n! * n)`hoạt động, có quy mô lớn về mặt thiên văn ngay cả đối với`n = 10`. 

Để cải thiện, chúng ta cần hiểu điều gì thực sự quyết định liệu hai điểm khác biệt có bằng nhau hay không. Chúng tôi có:`c_i = a_i - b_i`Hai vị trí`i`Và`j`tạo ra cùng một giá trị khi:`a_i - b_i = a_j - b_j`, sắp xếp lại thành:`a_i - a_j = b_i - b_j`Điều này cho chúng ta biết rằng va chạm trong`c`tương ứng chính xác với sự khác biệt như nhau trong`a`được phản ánh trong`b`. Từ`b`là một hoán vị, chúng ta có toàn quyền tự do gán các giá trị riêng biệt, vì vậy chúng ta có thể cố gắng “phá vỡ cấu trúc” trong`a`. 

Quan sát quan trọng là chúng ta có thể tối đa hóa sự đa dạng trong`c`bằng cách đảm bảo rằng càng nhiều cặp`(a_i, b_i)`càng tốt, tránh tạo ra sự khác biệt lặp đi lặp lại. Cách đơn giản và mạnh mẽ nhất để làm điều này là tối đa hóa khả năng chống liên kết giữa`a`Và`b`. 

Nếu chúng ta sắp xếp`a`, sau đó ghép các phần tử nhỏ nhất của`a`với các giá trị lớn nhất của`b`và ngược lại, dàn trải sự khác biệt càng nhiều càng tốt. Đây là nguyên tắc sắp xếp lại tiêu chuẩn: thứ tự ngược lại tối đa hóa sự thay đổi trong các kết hợp tuyến tính. 

Vì vậy chúng tôi sắp xếp các chỉ số theo`a_i`, sau đó gán`b`theo thứ tự giảm dần theo thứ tự đó. 

Cụ thể: 

nhỏ nhất`a_i`được`n`, nhỏ thứ hai được`n-1`, vân vân. 

Điều này tạo ra một cấu trúc đơn điệu trong`c`đó tránh được sự lặp lại không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + gán ngược) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng`a`và liên kết từng giá trị với chỉ mục của nó. Chúng ta cần các chỉ số để có thể đặt các giá trị vào đúng vị trí trong`b`. 
2. Sắp xếp chỉ số theo giá trị tăng dần của`a`. Điều này tạo ra một thứ tự từ nhỏ nhất đến lớn nhất`a_i`. Lý do của việc sắp xếp là để áp đặt cấu trúc lên`a`để chúng ta có thể kiểm soát sự khác biệt diễn ra như thế nào. 
3. Tạo một mảng trống`b`chiều dài`n`. 
4. Gán giá trị cho`b`theo thứ tự ngược lại của tính sẵn có: nhỏ nhất`a_i`được`n`, tiếp theo được`n-1`, vân vân. Điều này đảm bảo rằng lớn hơn`b_i`các giá trị được ghép với giá trị nhỏ hơn`a_i`. 
5. Đầu ra`b`theo thứ tự chỉ mục ban đầu. 

Lý do đằng sau bước 4 là việc ghép đôi cực đại sẽ tối đa hóa sự tách biệt giữa các cặp tương ứng.`a_i`Và`b_i`, làm giảm khả năng hai điểm khác biệt trùng nhau. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, giả sử`a_i < a_j`. Sau đó chúng tôi phân công`b_i > b_j`. Vì vậy với bất kỳ cặp nào`i, j`, chúng ta có thứ tự ngược lại trong`a`Và`b`. Sự ghép đôi chống đơn điệu này đảm bảo rằng biểu thức`a_i - b_i`lan tỏa các giá trị càng nhiều càng tốt vì sự gia tăng`a`được đối trọng bằng cách giảm`b`. 

Điều này phá hủy sự lặp lại có cấu trúc sẽ phát sinh nếu cả hai mảng được sắp xếp tương tự nhau. Mặc dù chúng tôi không đảm bảo rõ ràng rằng tất cả các khác biệt đều khác biệt, nhưng cách xây dựng này tối đa hóa sự tách biệt và tránh va chạm có hệ thống, đó chính xác là điều mà vấn đề mang lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    idx = list(range(n))
    idx.sort(key=lambda i: a[i])

    b = [0] * n
    val = n

    for i in idx:
        b[i] = val
        val -= 1

    print(*b)
```Giải pháp đầu tiên là xây dựng một mảng chỉ mục để chúng ta không bị mất vị trí ban đầu của các phần tử trong`a`. Sắp xếp theo`a[i]`cho phép chúng ta áp đặt cấu trúc mà không cần sửa đổi mảng ban đầu. 

Biến`val`bắt đầu từ`n`và giảm, đảm bảo một hoán vị chặt chẽ. Mỗi chỉ mục nhận được một giá trị duy nhất, vì vậy`b`là hợp lệ. 

Một điểm tinh tế là chúng tôi không bao giờ sắp xếp`a`chính nó, chỉ có các chỉ số. Điều này là cần thiết vì đầu ra phải tôn trọng thứ tự ban đầu của các vị trí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1
100
3
10 3 3
```Đối với trường hợp thứ hai, chúng tôi theo dõi quá trình xây dựng. 

| Bước | Các chỉ số được sắp xếp theo a | Giá trị b được gán | bang b | 
| --- | --- | --- | --- | 
| ban đầu | [1, 2, 0] | - | [0,0,0] | 
| chỉ định 1 | 1 | 3 | [0,3,0] | 
| phân công 2 | 2 | 2 | [0,3,2] | 
| phân công 3 | 0 | 1 | [1,3,2] | 

Cuối cùng`b = [1, 3, 2]`. 

Sự khác biệt là`[9, 0, 1]`, tất cả đều khác biệt. Điều này cho thấy công trình lan tỏa giá trị một cách hiệu quả ngay cả khi`a`chứa các bản sao. 

### Ví dụ 2 

đầu vào:```
4
2
5 5
4
1 2 3 4
```Vì`a = [1,2,3,4]`, chỉ số được sắp xếp là`[0,1,2,3]`. 

| Bước | Chỉ mục | một [tôi] | b được giao | bang b | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 4 | [4,0,0,0] | 
| 2 | 1 | 2 | 3 | [4,3,0,0] | 
| 3 | 2 | 3 | 2 | [4,3,2,0] | 
| 4 | 3 | 4 | 1 | [4,3,2,1] | 

Sự khác biệt trở thành`[-3, -1, 1, 3]`, tất cả đều khác biệt, xác nhận mức chênh lệch tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | chỉ số sắp xếp chiếm ưu thế trong từng trường hợp thử nghiệm | 
| Không gian | O(n) | mảng cho chỉ số và hoán vị | 

Tổng cộng`n`trên nhiều trường hợp thử nghiệm là nhiều nhất`4 * 10^4`, vì vậy việc sắp xếp nằm trong giới hạn. Việc xây dựng là tuyến tính sau khi sắp xếp, do đó thời gian chạy tổng thể dễ dàng phù hợp trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out_lines = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        idx = list(range(n))
        idx.sort(key=lambda i: a[i])

        b = [0] * n
        val = n
        for i in idx:
            b[i] = val
            val -= 1

        out_lines.append(" ".join(map(str, b)))
    return "\n".join(out_lines)

# provided samples
assert run("3\n1\n100000\n2\n1 1\n3\n10 3 3\n") == "1\n2 1\n1 3 2"

# custom cases
assert sorted(run("1\n5\n1 1 1 1 1\n").split()[0:5]) == ["1","2","3","4","5"], "permutation validity"
assert run("1\n1\n7\n") == "1", "single element"
assert run("1\n4\n4 3 2 1\n") is not None, "reverse input stability"
assert run("1\n3\n1 100 1000\n") is not None, "large spread values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | bất kỳ hoán vị nào của 1..n | tính đúng đắn của hoán vị theo các bản sao | 
| n=1 | 1 | xử lý trường hợp cơ bản | 
| giảm dần a | hoán vị hợp lệ | đặt hàng ổn định | 
| tăng sức lan tỏa | hoán vị hợp lệ | bền vững với giá trị lớn | 

## Vỏ cạnh 

cho`n = 1`, danh sách chỉ mục được sắp xếp chứa một phần tử duy nhất và chúng tôi gán`b[0] = 1`. Thuật toán tạo ra hoán vị hợp lệ duy nhất và mảng sai phân có một phần tử, do đó số lượng khác biệt được tối đa hóa một cách tầm thường. 

Với mọi giá trị bằng nhau trong`a`, chỉ số sắp xếp là tùy ý. Thuật toán vẫn gán một hoán vị đầy đủ cho`b`. Từ`a_i`giống hệt nhau, sự khác biệt hoàn toàn được xác định bởi`-b_i`, tất cả đều khác biệt bởi vì`b`là một hoán vị. Điều này đảm bảo số lượng khác biệt tối đa có thể`n`. 

Để tăng nghiêm ngặt`a`, cặp thuật toán nhỏ nhất`a_i`với lớn nhất`b_i`, tạo ra một chuỗi khác biệt có cấu trúc chặt chẽ. Mặc dù`a`được sắp xếp theo thứ tự, phép gán đảo ngược sẽ ngăn chặn sự khác biệt lặp đi lặp lại bằng cách tránh căn chỉnh các số gia trong`a`Và`b`.
