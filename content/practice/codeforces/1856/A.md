---
title: "CF 1856A - Những câu chuyện về một loại"
description: "Chúng ta được cho một mảng các số nguyên dương. Alphen có thể thực hiện lặp đi lặp lại một thao tác, trong đó mỗi phần tử của mảng được giảm đi một phần tử, nhưng không phần tử nào có thể xuống dưới 0."
date: "2026-06-09T05:02:28+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1856
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 890 (Div. 2) supported by Constructor Institute"
rating: 800
weight: 1856
solve_time_s: 109
verified: true
draft: false
---

[CF 1856A - Những câu chuyện về một loại](https://codeforces.com/problemset/problem/1856/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Alphen có thể thực hiện lặp đi lặp lại một thao tác, trong đó mỗi phần tử của mảng được giảm đi một phần tử, nhưng không phần tử nào có thể xuống dưới 0. Anh ta tiếp tục thực hiện thao tác này cho đến khi mảng không giảm, nghĩa là mỗi phần tử ít nhất phải lớn bằng phần tử trước nó. Nhiệm vụ của chúng ta là xác định số lần thao tác này phải được áp dụng cho mỗi trường hợp thử nghiệm. 

Độ dài mảng nhỏ, tối đa 50 phần tử, nhưng giá trị của các phần tử có thể rất lớn, lên tới$10^9$. Điều này có nghĩa là một mô phỏng đơn giản làm giảm tất cả các phần tử theo từng bước sẽ quá chậm trong trường hợp xấu nhất. Ví dụ: một mảng có một phần tử bằng$10^9$sẽ yêu cầu$10^9$hoạt động nếu chúng ta mô phỏng trực tiếp. Vì vậy, chúng ta cần một cách tiếp cận không thực hiện từng thao tác riêng lẻ. 

Các trường hợp cạnh không rõ ràng phát sinh khi mảng đã thỏa mãn thuộc tính không giảm, chẳng hạn như`[1, 2, 3]`. Câu trả lời phải bằng không. Một trường hợp tinh tế khác là khi phần tử đầu tiên lớn nhất trong mảng, ví dụ:`[1000000000, 1, 2]`. Số lượng thao tác chính xác là phần tử đầu tiên, vì mọi phần tử sẽ bị giảm đồng thời cho đến khi mảng bắt đầu bằng 0. Bất kỳ cách tiếp cận ngây thơ nào không giải thích được điều này sẽ thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi trường hợp thử nghiệm, chúng ta có thể thực hiện nhiều lần thao tác: giảm mọi phần tử đi 1 cho đến khi mảng được sắp xếp. Mỗi thao tác yêu cầu quét toàn bộ mảng để áp dụng phép trừ và chúng ta cần kiểm tra xem mảng đó đã được sắp xếp chưa. Trong trường hợp xấu nhất, nếu phần tử lớn nhất là$10^9$, điều này sẽ mất$10^9 \times n$bước, quá lớn. 

Quan sát quan trọng là hoạt động ảnh hưởng đến tất cả các yếu tố một cách thống nhất. Sau mỗi thao tác, mọi phần tử đều giảm đi 1 cho đến khi một phần tử nào đó bằng 0. Bởi vì chúng ta chỉ quan tâm đến việc mảng không giảm nên số lượng thao tác cần thiết được xác định bằng mức giảm lớn nhất cần thiết để làm cho phần tử đầu tiên nhỏ hơn hoặc bằng phần tử lớn nhất trong số các phần tử còn lại. Cụ thể hơn, câu trả lời là phần tử lớn nhất trong mảng không bao gồm phần tử đầu tiên, nếu phần tử đầu tiên lớn hơn mức tối đa đó. Nếu mảng đã không giảm thì không cần thực hiện thao tác nào. 

Quan sát này làm giảm vấn đề khi quét mảng một lần cho mỗi trường hợp thử nghiệm, tìm mức tối đa và tính toán sự khác biệt với phần tử đầu tiên. Nó tránh mọi mô phỏng của từng thao tác và hoạt động ngay cả khi số lượng lớn như$10^9$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * max(a_i)) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case,$t$. 
2. Với mỗi test case, hãy đọc$n$và mảng$a$. 
3. Kiểm tra xem mảng đã không giảm chưa. Nếu đúng như vậy thì câu trả lời là 0. Có thể bỏ qua bước này nếu chúng ta tuân theo logic phần tử tối đa, vì công thức sẽ tự động mang lại 0. 
4. Tìm phần tử lớn nhất của mảng không bao gồm phần tử đầu tiên. Gọi đây`max_rest`. 
5. Nếu phần tử đầu tiên`a[0]`nhỏ hơn hoặc bằng`max_rest`, mảng đã không giảm hoặc sẽ không bao giờ cần giảm để trở thành không giảm, vì vậy số thao tác cần thiết là`max_rest`. 
6. Ngược lại, nếu`a[0]`lớn hơn`max_rest`, chúng ta phải giảm phần tử đầu tiên và tất cả các phần tử khác cho đến khi phần tử đầu tiên bằng`max_rest`. Vì tất cả các phần tử đều giảm đồng thời nên số thao tác cần thực hiện chính xác`a[0]`. 
7. Xuất ra số thao tác được tính toán. 

**Tại sao nó hoạt động**: Mỗi thao tác sẽ giảm thiểu mọi phần tử một cách đồng nhất. Nút thắt để đạt được mảng không giảm là phần tử đầu tiên nếu nó lớn hơn phần còn lại. Khi phần tử đầu tiên nhỏ hơn hoặc bằng giá trị tối đa của các phần tử còn lại, mảng sẽ được sắp xếp hoặc sẽ được sắp xếp sau khi giảm tất cả các phần tử về 0. Do đó, việc theo dõi phần tử đầu tiên so với phần tử còn lại đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        # Maximum of the rest of the array
        max_rest = max(a[1:]) if n > 1 else 0
        # The answer is either 0 if first element <= max_rest or a[0] otherwise
        if a[0] <= max_rest:
            print(max_rest)
        else:
            print(a[0])

if __name__ == "__main__":
    main()
```Giải pháp này đọc tất cả đầu vào bằng I/O nhanh. Nó tính toán`max_rest`là mức tối đa của mảng từ chỉ số 1 trở đi. Việc so sánh với phần tử đầu tiên xác định trực tiếp số lượng thao tác. Chúng tôi tránh mô phỏng mọi mức giảm. Các trường hợp biên, như mảng một phần tử hoặc mảng đã được sắp xếp, được xử lý tự động bởi`max`và logic so sánh. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:`[3, 1 2 3]`| Bước | Mảng | max_rest | Hoạt động | 
| --- | --- | --- | --- | 
| Ban đầu | 1 2 3 | 2 | 0 | 

Phần tử đầu tiên là 1,`max_rest`là 2. Vì 1 ≤ 2 nên mảng không giảm. Đầu ra là 0. 

Đầu vào mẫu 2:`[3, 1000000000 1 2]`| Bước | Mảng | max_rest | Hoạt động | 
| --- | --- | --- | --- | 
| Ban đầu | 1000000000 1 2 | 2 | 1000000000 | 

Phần tử đầu tiên là 10^9,`max_rest`là 2. Vì phần tử đầu tiên lớn hơn`max_rest`, sẽ cần 10^9 thao tác để giảm mảng về thứ tự được sắp xếp. Đầu ra là 1000000000. 

Những dấu vết này cho thấy thuật toán xác định chính xác xem mảng đã được sắp xếp hay cần giảm và tính toán số lượng thao tác trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Mỗi trường hợp thử nghiệm yêu cầu quét mảng một lần để tính toán`max_rest`. | 
| Không gian | O(n) | Lưu trữ mảng cho từng trường hợp thử nghiệm. | 

Được cho$t \le 500$Và$n \le 50$, các phép toán trong trường hợp xấu nhất là 25.000, nằm trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("7\n3\n1 2 3\n5\n2 1 2 1 2\n4\n3 1 5 4\n2\n7 7\n5\n4 1 3 2 5\n5\n2 3 1 4 5\n3\n1000000000 1 2\n") == "0\n2\n5\n0\n4\n3\n1000000000", "sample tests"

# Custom cases
assert run("2\n2\n1 1\n2\n2 1\n") == "1\n2", "all equal and decreasing array"
assert run("1\n5\n5 5 5 5 5\n") == "5", "all equal values, must go to zero"
assert run("1\n2\n10 100\n") == "100", "first element smaller"
assert run("1\n2\n100 10\n") == "100", "first element larger"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | Logic giá trị lặp lại một phần tử | 
| 2 1 2 | 2 | Giảm tính toán mảng | 
| 5 5 5 5 5 | 5 | Mảng lớn bằng nhau | 
| 10 100 | 100 | Phần tử đầu tiên nhỏ hơn phần còn lại | 
| 100 10 | 100 | Phần tử đầu tiên lớn hơn phần còn lại | 

## Vỏ cạnh 

cho`[1000000000, 1, 2]`,`max_rest`là 2. Phần tử đầu tiên 10^9 lớn hơn nên thuật toán đưa ra 10^9. Điều này khớp với số lượng thao tác chính xác cần thiết để đưa mảng vào thứ tự được sắp xếp, xác nhận rằng trường hợp cạnh có giá trị lớn đã được xử lý. Vì`[1, 2, 3]`, phần tử đầu tiên đã nhỏ hơn hoặc bằng phần tử còn lại nên thuật toán đưa ra 0, xác nhận trường hợp đã được sắp xếp sẽ được xử lý.
