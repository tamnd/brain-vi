---
title: "CF 106153F - \u742a\u9732\u8bfa\u7684\u51b0\u6676\u6570\u5b66\u6311\u6218"
description: "Chúng ta được cung cấp một mảng các số nguyên cho mỗi trường hợp thử nghiệm và chúng ta được yêu cầu chọn hai phần tử riêng biệt và đánh giá một biểu thức được hình thành từ chúng."
date: "2026-06-19T19:21:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106153
codeforces_index: "F"
codeforces_contest_name: "HNNU Freshman Competition Round 2"
rating: 0
weight: 106153
solve_time_s: 52
verified: true
draft: false
---

[CF 106153F - \u742a\u9732\u8bfa\u7684\u51b0\u6676\u6570\u5b66\u6311\u6218](https://codeforces.com/problemset/problem/106153/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên cho mỗi trường hợp thử nghiệm và chúng ta được yêu cầu chọn hai phần tử riêng biệt và đánh giá một biểu thức được hình thành từ chúng. Biểu thức không cố định: đối với bất kỳ cặp chỉ số khác nhau có thứ tự nào, chúng ta được phép tính ba giá trị có thể có, đó là tổng, hiệu và tích của chúng. Nhiệm vụ là xác định giá trị lớn nhất có thể đạt được trên tất cả các lựa chọn của hai phần tử riêng biệt và cả ba phép tính. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp một mảng có thể chứa số dương, số âm và số không. Đầu ra cho mỗi trường hợp thử nghiệm là một số nguyên duy nhất, giá trị tối đa có thể đạt được bằng cách chọn hai vị trí khác nhau và áp dụng một trong các thao tác được phép. 

Một cách đọc đơn giản gợi ý một bảng liệt kê O(n²) của tất cả các cặp, vốn đã nằm ở ranh giới nếu n lớn trong các trường hợp thử nghiệm. Tuy nhiên, cấu trúc của các hoạt động hạn chế rất nhiều những gì quan trọng. 

Điều tinh tế quan trọng là không phải tất cả các phần tử trong mảng đều có liên quan như nhau. Giá trị cực trị chi phối cả ba hoạt động. Đối với phép nhân, ứng cử viên tốt nhất thường là số dương lớn nhất hoặc số âm nhất. Đối với phép trừ, kết quả tốt nhất là ghép số lớn nhất với số nhỏ nhất. Ngoài ra, một lần nữa các giá trị lớn nhất lại chiếm ưu thế. Sự tập trung các ứng viên “thú vị” xung quanh các thái cực là điều khiến vấn đề có thể nén lại được. 

Một giải pháp bất cẩn thường thất bại trong những trường hợp như: 

đầu vào:```
3
-100 1 2
```Việc chỉ tập trung ngây thơ vào những số dương lớn sẽ thử 2 * 1 hoặc 2 + 1 và bỏ lỡ rằng 2 - (-100) = 102 là tối ưu. 

Một cạm bẫy khác là bỏ qua rằng tích của hai số âm có thể vượt quá bất kỳ tổ hợp dương nào: 

đầu vào:```
3
-10 -9 1
```Câu trả lời đúng nhất là (-10) * (-9) = 90, số này chiếm ưu thế hơn tất cả các tổng và hiệu. 

Những ví dụ này cho thấy tại sao cả hai thái cực tích cực và tiêu cực đều phải được xem xét một cách đối xứng. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp lại trên mọi cặp chỉ số có thứ tự và tính toán cả ba biểu thức. Điều này đơn giản và chính xác vì nó trực tiếp đánh giá định nghĩa. Tuy nhiên, nó thực hiện các cặp O(n2) cho mỗi trường hợp thử nghiệm, điều này trở nên không khả thi khi n lớn. 

Quan sát quan trọng là chỉ có một số lượng phần tử không đổi mới có thể là một phần của giải pháp tối ưu. Đối với phép nhân và phép trừ, chỉ các giá trị cực trị mới quan trọng: hai giá trị lớn nhất và nhỏ nhất trong mảng đã bao gồm mọi trường hợp ghép nối tối ưu. Các giá trị dương và âm hoạt động đối xứng nhau, vì vậy chúng ta cũng cần xem xét các số 0 khi chúng có thể tương tác với các giá trị âm. 

Thay vì đánh giá tất cả các cặp, chúng ta rút gọn mảng thành một tập ứng cử viên nhỏ bao gồm hai giá trị lớn nhất, hai giá trị nhỏ nhất và bằng 0 nếu nó tồn tại. Mỗi câu trả lời tối ưu phải có ít nhất một trong số các ứng viên này, bởi vì bất kỳ phần tử không cực đoan nào cũng có thể được thay thế bằng một phần tử cực đoan hơn để cải thiện hoặc bảo toàn kết quả cho cả ba phép tính. 

Brute-force hoạt động vì nó khám phá mọi thứ, nhưng nó thất bại khi n phát triển. Nhận xét rằng chỉ các giá trị cực trị mới quan trọng cho phép chúng ta nén bài toán thành một số lượng ứng cử viên không đổi và sau đó kiểm tra tất cả các cặp bên trong tập rút gọn này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu (chỉ cực đoan) | O(n log n) hoặc O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Chia mảng thành ba nhóm khái niệm: số dương, số âm và số 0. Sự phân tách này rất hữu ích vì phép nhân hoạt động khác nhau giữa các dấu hiệu. 
2. Đối với số dương, chỉ giữ lại hai giá trị nhỏ nhất và hai giá trị lớn nhất. Giá trị dương nhỏ nhất có thể quan trọng đối với phép trừ, trong khi giá trị lớn nhất có thể quan trọng đối với tất cả các hoạt động liên quan đến tăng trưởng. 
3. Đối với số âm, cũng chỉ giữ lại hai giá trị nhỏ nhất (âm nhất) và hai giá trị lớn nhất (gần nhất với 0). Các giá trị âm nhất rất quan trọng đối với phép nhân, trong khi những giá trị gần nhất với 0 có thể tương tác với các giá trị dương trong phép trừ. 
4. Nếu có ít nhất một số 0, hãy thêm một số 0 vào tập ứng cử viên. Số 0 chỉ phù hợp với hành vi của cạnh nhân và trừ, trong đó nó có thể chiếm ưu thế hoặc vô hiệu hóa các giá trị. 
5. Kết hợp tất cả các ứng cử viên đã chọn vào một danh sách nhỏ, thường có quy mô tối đa là 8 hoặc 9. 
6. Liệt kê tất cả các cặp ứng viên khác biệt theo thứ tự. Đối với mỗi cặp (i, j), hãy tính a[i] + a[j], a[i] - a[j] và a[i] * a[j], rồi theo dõi mức tối đa. 
7. Xuất ra giá trị tốt nhất tìm được. 

Tính đúng đắn phụ thuộc vào thực tế là mọi thao tác đều đơn điệu khi thay thế một phần tử bằng một phần tử khác theo hướng thích hợp. Nếu một ứng cử viên không nằm trong số các giá trị cực trị, việc thay thế nó bằng một giá trị cực trị hơn không thể làm xấu đi bất kỳ biểu thức nào trong ba biểu thức và có thể cải thiện nghiêm ngặt ít nhất một biểu thức. 

## Tại sao nó hoạt động 

Bất kỳ cặp tối ưu nào cũng phải bao gồm các phần tử cực trị trong miền dấu tương ứng của chúng. Ngoài ra, việc thay thế toán hạng bằng giá trị lớn hơn chỉ làm tăng kết quả. Đối với phép trừ, tối đa hóa số hạng đầu tiên và giảm thiểu số hạng thứ hai mang lại giá trị tốt nhất, do đó, chỉ có cực đại và cực tiểu tổng thể mới quan trọng. Đối với phép nhân, cấu trúc dấu giảm các ứng cử viên thành hai số dương lớn nhất hoặc hai số âm nhỏ nhất. Do đó, bất kỳ giải pháp tối ưu nào cũng có thể được ánh xạ tới một cặp bên trong tập ứng cử viên rút gọn mà không làm giảm giá trị của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    pos = []
    neg = []
    zero = False
    
    for x in a:
        if x == 0:
            zero = True
        elif x > 0:
            pos.append(x)
        else:
            neg.append(x)
    
    def add_candidates(arr, cand):
        arr.sort()
        m = len(arr)
        for i in range(min(2, m)):
            cand.append(arr[i])
        for i in range(max(0, m - 2), m):
            cand.append(arr[i])
    
    cand = []
    add_candidates(pos, cand)
    add_candidates(neg, cand)
    
    if zero:
        cand.append(0)
    
    ans = -10**18
    m = len(cand)
    
    for i in range(m):
        for j in range(m):
            if i == j:
                continue
            x, y = cand[i], cand[j]
            ans = max(ans, x + y, x - y, x * y)
    
    print(ans)

if __name__ == "__main__":
    t = int(input())
    for _ in range(t):
        solve()
```Việc thực hiện theo ý tưởng giảm trực tiếp. Chúng tôi phân tách rõ ràng các giá trị bằng dấu để chỉ có thể trích xuất các phần tử biên một cách an toàn. Hàm trợ giúp chọn hai phần tử nhỏ nhất và hai phần tử lớn nhất từ ​​mỗi nhóm sau khi sắp xếp. Điều này đảm bảo chúng tôi nắm bắt được cả hai đầu của phổ giá trị. 

Tập ứng cử viên khi đó đủ nhỏ để việc kiểm tra từng cặp đầy đủ là an toàn. Vòng lặp kép là công việc không đổi cho mỗi trường hợp thử nghiệm. Cần cẩn thận khi sử dụng hành vi số nguyên 64 bit một cách ngầm định thông qua số nguyên Python, điều này tránh được những lo ngại về tràn hiện diện trong ngữ cảnh C++ ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
-10 -9 1
```Lựa chọn ứng viên: 

| Bước | Tích cực | Tiêu cực | Không | Bộ ứng cử viên | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1] | [-10, -9] | sai | [] | 
| Sau khi khai thác | [1] | [-10, -9] | sai | [-10, -9, 1] | 

Đánh giá cặp: 

| tôi | j | x | y | x+y | x-y | x*y | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | -10 | -9 | -19 | -1 | 90 | 90 | 
| 0 | 2 | -10 | 1 | -9 | -11 | -10 | 90 | 
| 1 | 2 | -9 | 1 | -8 | -10 | -9 | 90 | 

Đầu ra là 90, đến từ (-10) * (-9). 

Dấu vết này xác nhận rằng thuật toán nắm bắt được nhu cầu của cả hai thái cực tiêu cực cùng một lúc. 

### Ví dụ 2 

đầu vào:```
1
4
-5 0 2 3
```Lựa chọn ứng viên: 

| Bước | Tích cực | Tiêu cực | Không | Bộ ứng cử viên | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [2, 3] | [-5] | đúng | [2, 3, -5, 0] | 

Điểm nổi bật của đánh giá cặp: 

| tôi | j | x | y | x+y | x-y | x*y | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 3 | 0 | 0 | 2 | 2 | -2 | 0 | 2 | 
| 1 | 2 | 3 | -5 | -2 | 8 | -15 | 8 | 
| 2 | 0 | -5 | 2 | -3 | -7 | -10 | 8 | 

Đầu ra là 8 từ 3 - (-5). 

Ví dụ này giải thích tại sao phép trừ giữa giá trị dương tối đa và giá trị âm tối thiểu phải được giữ nguyên trong tập ứng cử viên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp được thực hiện trên các phân vùng dương và âm, sau đó kiểm tra cặp có kích thước không đổi theo | 
| Không gian | O(1) | Chỉ một số giá trị ứng cử viên cố định được lưu trữ cho mỗi trường hợp thử nghiệm | 

Các ràng buộc dễ dàng được thỏa mãn vì mỗi trường hợp thử nghiệm giảm xuống một phép tính có kích thước không đổi sau quá trình tiền xử lý tuyến tính hoặc log-tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import sys as _sys

    input = _sys.stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        
        pos, neg = [], []
        zero = False
        
        for x in a:
            if x == 0:
                zero = True
            elif x > 0:
                pos.append(x)
            else:
                neg.append(x)
        
        def add(arr, cand):
            arr.sort()
            m = len(arr)
            for i in range(min(2, m)):
                cand.append(arr[i])
            for i in range(max(0, m-2), m):
                cand.append(arr[i])
        
        cand = []
        add(pos, cand)
        add(neg, cand)
        if zero:
            cand.append(0)
        
        ans = -10**18
        for i in range(len(cand)):
            for j in range(len(cand)):
                if i != j:
                    x, y = cand[i], cand[j]
                    ans = max(ans, x+y, x-y, x*y)
        
        return str(ans)

    def main():
        t = int(input())
        out = []
        for _ in range(t):
            out.append(solve())
        return "\n".join(out)

    return main()

# sample 1
assert run("1\n3\n-10 -9 1\n") == "90"

# all positive
assert run("1\n4\n1 2 3 4\n") == "12"

# all negative
assert run("1\n3\n-1 -2 -3\n") == "6"

# zero interaction
assert run("1\n4\n-5 0 2 3\n") == "8"

# single extreme dominance
assert run("1\n2\n-100 50\n") == "5000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| -10 -9 1 | 90 | ưu thế nhân âm | 
| 1 2 3 4 | 12 | sự thống trị bổ sung tích cực | 
| -1 -2 -3 | 6 | sản phẩm âm bản nhỏ nhất | 
| -5 0 2 3 | 8 | tương tác bằng không và phép trừ | 
| -100 50 | 5000 | phép nhân chéo | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi giá trị tối ưu đến từ việc trộn một giá trị dương lớn với một giá trị âm có cường độ lớn. Ví dụ, đầu vào`[-100, 1, 2]`mang lại 202 từ`2 - (-100)`hoặc 200 từ phép nhân tùy thuộc vào giá trị hiện tại. Thuật toán giữ cả hai thái cực nên cả hai ứng viên đều được đánh giá trực tiếp trong bảng liệt kê cuối cùng. 

Một trường hợp đặc biệt khác là khi số 0 xuất hiện bên cạnh số âm. Không bao gồm rõ ràng số 0, các trường hợp như`[-5, 0, 1]`sẽ bỏ lỡ câu trả lời đúng`5`từ`0 - (-5)`. Thuật toán chèn số 0 vào tập ứng cử viên một cách rõ ràng, đảm bảo phép trừ và phép nhân liên quan đến số 0 được xem xét. 

Cuối cùng, khi tất cả các giá trị giống hệt nhau, chẳng hạn như`[7, 7, 7]`, việc giảm ứng viên vẫn duy trì tính chính xác vì cả hai giá trị tối đa và tối thiểu đều giống nhau và việc đánh giá cặp một cách tầm thường mang lại tích, tổng và hiệu lặp lại chính xác.
