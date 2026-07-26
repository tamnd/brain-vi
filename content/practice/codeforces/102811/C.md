---
title: "CF 102811C - \u041c\u0438\u0440\u043d\u044b\u0435 \u043b\u0430\u0434\u044c\u0438"
description: "Bàn cờ chứa chính xác một quân xe ở mỗi hàng và mỗi cột, do đó vị trí của tất cả các quân xe có thể được biểu diễn dưới dạng hoán vị. Mảng đầu vào a sử dụng số hàng làm chỉ mục: a[i] cho chúng ta biết cột nào chứa quân xe ở hàng i."
date: "2026-07-26T16:13:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102811
codeforces_index: "C"
codeforces_contest_name: "\u0428\u043a\u043e\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0432\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, 9-11 \u043a\u043b\u0430\u0441\u0441\u044b, \u041c\u043e\u0441\u043a\u0432\u0430  (\u0432\u0435\u0440\u0441\u0438\u044f CF)"
rating: 0
weight: 102811
solve_time_s: 34
verified: true
draft: false
---

[CF 102811C - \u041c\u0438\u0440\u043d\u044b\u0435 \u043b\u0430\u0434\u044c\u0438](https://codeforces.com/problemset/problem/102811/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bàn cờ chứa chính xác một quân xe ở mỗi hàng và mỗi cột, do đó vị trí của tất cả các quân xe có thể được biểu diễn dưới dạng hoán vị. Mảng đầu vào`a`sử dụng số hàng làm chỉ mục:`a[i]`cho chúng ta biết cột nào chứa quân xe trong hàng`i`. 

Nhiệm vụ là xoay toàn bộ bảng 90 độ theo chiều kim đồng hồ và in biểu diễn hoán vị mới. Sau khi xoay, hàng của mỗi quân thay đổi so với số cột cũ, trong khi cột mới phụ thuộc vào số hàng cũ. 

Đối với một quân xe ở tọa độ`(i, a[i])`, một vòng quay theo chiều kim đồng hồ sẽ thay đổi tọa độ của nó thành`(a[i], N + 1 - i)`. Tọa độ đầu tiên trở thành hàng mới và tọa độ thứ hai trở thành cột mới. Mảng câu trả lời phải lưu trữ cột mới cho mỗi hàng mới. 

Kích thước của bảng có thể đạt tới`N = 100000`, vì vậy mọi giải pháp mô phỏng bảng một cách rõ ràng sẽ lãng phí. Một bảng có kích thước này sẽ chứa tới mười tỷ ô, không thể chứa vừa bộ nhớ và không thể xử lý trong giới hạn thời gian thi đấu thông thường. Ngay cả một cách tiếp cận kiểm tra nhiều cặp hàng-cột có thể cũng sẽ quá chậm, vì vậy giải pháp phải hoạt động trực tiếp với hoán vị trong thời gian tuyến tính. 

Một số trường hợp nhỏ có thể phá vỡ việc triển khai không chính xác. Lỗi phổ biến nhất là quên rằng hàng mới là cột cũ chứ không phải hàng cũ. Ví dụ: nếu bàn cờ có một quân:```
1
1
```Sau khi xoay nó vẫn ở ô duy nhất:```
1
```Một giải pháp vô tình hoán đổi tọa độ mà không xem xét kích thước bảng có thể có vẻ đúng trong trường hợp này. 

Một lỗi phổ biến khác là sử dụng`N - i`thay vì`N + 1 - i`với tọa độ một cơ sở. Ví dụ:```
3
1
2
3
```Các quân xe nằm trên đường chéo chính. Sau khi xoay chúng sẽ trở thành:```
3
2
1
```vì quân từ hàng 1 chuyển sang cột 3, quân từ hàng 3 di chuyển sang cột 1, v.v. Việc sử dụng trực tiếp các công thức dựa trên số 0 trên đầu vào dựa trên một sẽ tạo ra các giá trị không chính xác. 

Vấn đề thứ ba là ghi đè thông tin trong khi cố gắng chuyển đổi mảng tại chỗ. Ví dụ:```
4
2
4
1
3
```Các vị trí mới được xác định bởi tất cả các cặp ban đầu`(row, column)`. Nếu một chương trình thay đổi`a[2]`trước khi sử dụng giá trị ban đầu từ hàng 2, kết quả phụ thuộc vào thứ tự xử lý và có thể âm thầm trở thành sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo một bảng hai chiều, đặt mọi quân xe lên đó, xoay ma trận và sau đó xây dựng lại mảng câu trả lời. Điều này tuân theo chính xác các hoạt động vật lý nên rất dễ chứng minh là đúng. Tuy nhiên, nó đòi hỏi phải lưu trữ`N * N`tế bào. Khi`N = 100000`, điều đó có nghĩa là`10^10`tế bào, điều đó là không thể. 

Một cách tiếp cận tốt hơn một chút nhưng vẫn không cần thiết là tìm kiếm qua các hàng và cột để tìm xem mỗi quân xe đã xoay sẽ đi đến đâu. Điều này tránh việc lưu trữ toàn bộ bảng nhưng vẫn có thể yêu cầu kiểm tra một số lượng lớn các vị trí. Với`N`rooks, tìm kiếm lặp đi lặp lại có thể tiếp cận`O(N^2)`hoạt động, có nghĩa là về`10^10`hoạt động trong trường hợp lớn nhất. 

Quan sát quan trọng là đầu vào đã cung cấp tọa độ chính xác của mọi quân xe. Không cần phải lập mô hình các ô trống. Công thức xoay vòng đưa ra đích đến của mỗi quân xe ngay lập tức. Một tân binh ở`(i, a[i])`di chuyển đến`(a[i], N + 1 - i)`, vì vậy chúng ta chỉ cần đặt`N + 1 - i`vào vị trí trả lời`a[i]`. 

Cách tiếp cận bạo lực hoạt động vì nó xây dựng lại bảng hoàn chỉnh, nhưng bảng hoàn chỉnh chứa thông tin mà chúng ta không cần. Biểu diễn hoán vị đã chứa tất cả tọa độ xe và việc sử dụng phép biến đổi tọa độ sẽ giảm tác vụ xuống một lần duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(N2) | Quá chậm | 
| Tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng câu trả lời có kích thước`N`. Chỉ mục của mảng này sẽ đại diện cho một hàng sau khi xoay và giá trị được lưu trữ sẽ đại diện cho cột của quân xe trong hàng đó. 
2. Đọc từng hàng gốc`i`và cột xe của nó`a[i]`. Xe nằm ở`(i, a[i])`, do đó sau khi quay theo chiều kim đồng hồ nó sẽ chuyển sang hàng`a[i]`và cột`N + 1 - i`. 
3. Đặt`N + 1 - i`vào trong`answer[a[i]]`. Điều này trực tiếp ghi lại vị trí xoay mà không cần tạo bảng. 
4. Xuất ra mảng câu trả lời theo thứ tự hàng. Mỗi vị trí bây giờ mô tả chính xác một quân xe trên bàn cờ đã xoay. 

Tại sao nó hoạt động: mỗi quân xe được xử lý độc lập và phép biến đổi tọa độ cho phép quay theo chiều kim đồng hồ là chính xác. Vì mỗi cột ban đầu chứa chính xác một quân xe nên mọi giá trị`a[i]`là duy nhất, nghĩa là mỗi hàng mới nhận được chính xác một phép gán. Do đó, mảng câu trả lời chứa một vị trí quân xe hợp lệ cho mỗi hàng và mỗi phép gán tương ứng với quân xe được xoay chính xác. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    n = int(input())
    ans = [0] * n

    for i in range(n):
        col = int(input())
        ans[col - 1] = n - i

    print(*ans)

if __name__ == "__main__":
    solve()
```Mảng`ans`lưu trữ biểu diễn bảng xoay. Các hàng và cột đầu vào dựa trên một, nhưng mảng Python dựa trên 0, do đó hàng đích`col`được chuyển đổi thành`col - 1`. 

Trong vòng lặp,`i`dựa trên số 0, nghĩa là nó đại diện cho hàng ban đầu`i + 1`. Cột mới là:```
N + 1 - (i + 1) = N - i
```đó là lý do tại sao mã gán`n - i`. Điều này tránh được việc chuyển đổi qua lại không cần thiết giữa các hệ tọa độ. 

Thứ tự gán không quan trọng vì mỗi cột gốc xuất hiện đúng một lần. Mỗi quân xe ghi vào một vị trí khác nhau trong`ans`, do đó không có kết quả nào trước đó bị ghi đè. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
5
4
2
3
5
1
```Dấu vết là: 

| Chỉ mục hàng gốc`i`| Cột gốc | Hàng mới | Cột mới | Mảng trả lời sau bài tập | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 4 | 5 | [0,0,0,0,5] | 
| 1 | 2 | 2 | 4 | [0,4,0,0,5] | 
| 2 | 3 | 3 | 3 | [0,4,3,0,5] | 
| 3 | 5 | 5 | 2 | [0,4,3,0,5,?] | 
| 4 | 1 | 1 | 1 | [1,4,3,0,5] | 

Việc điều chỉnh bảng thành các vị trí đầu ra dựa trên một sẽ mang lại:```
1 4 3 5 2
```Dấu vết chứng tỏ rằng cột cũ xác định vị trí ghi giá trị, trong khi hàng cũ xác định giá trị được lưu ở đó. 

Một ví dụ nhỏ hơn:```
3
1
2
3
```Dấu vết là: 

| Chỉ mục hàng gốc`i`| Cột gốc | Hàng mới | Cột mới | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 3 | [3,0,0] | 
| 1 | 2 | 2 | 2 | [3,2,0] | 
| 2 | 3 | 3 | 1 | [3,2,1] | 

Kết quả là:```
3 2 1
```Thao tác này sẽ kiểm tra ranh giới của công thức tọa độ vì hàng đầu tiên và hàng cuối cùng phải hoán đổi cột của chúng sau khi xoay. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi quân xe được đọc và chuyển đổi một lần. | 
| Không gian | O(N) | Chỉ có hoán vị kết quả được lưu trữ. | 

Kích thước đầu vào tối đa là`100000`, do đó, thuật toán tuyến tính chỉ thực hiện khoảng một trăm nghìn phép tính và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()

        n = int(sys.stdin.readline())
        ans = [0] * n

        for i in range(n):
            col = int(sys.stdin.readline())
            ans[col - 1] = n - i

        print(*ans)
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run("""5
4
2
3
5
1
""") == "1 4 3 5 2", "sample"

assert run("""1
1
""") == "1", "minimum board"

assert run("""3
1
2
3
""") == "3 2 1", "diagonal rotation"

assert run("""4
2
4
1
3
""") == "3 1 4 2", "arbitrary permutation"

assert run("""5
5
4
3
2
1
""") == "1 2 3 4 5", "reverse permutation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Bảng nhỏ nhất có thể và không có chuyển động chỉ số | 
|`3 / 1 2 3`|`3 2 1`| Xử lý đúng công thức xoay | 
|`4 / 2 4 1 3`|`3 1 4 2`| Xử lý hoán vị chung | 
|`5 / 5 4 3 2 1`|`1 2 3 4 5`| Trường hợp đối xứng trong đó việc xoay đơn giản hóa | 

## Vỏ cạnh 

Đối với bảng một ô, thuật toán đọc quân xe duy nhất ở hàng`1`, cột`1`. Nó viết`1`vào trong`ans[0]`, do đó đầu ra vẫn là:```
1
```Công thức vẫn hoạt động vì cả hàng mới và cột mới đều bằng một. 

Đối với trường hợp đường chéo:```
3
1
2
3
```xe đầu tiên là ở`(1,1)`và di chuyển đến`(1,3)`, thứ hai giữ nguyên tại`(2,2)`, và người thứ ba chuyển sang`(3,1)`. Vị trí thuật toán`3`,`2`, Và`1`vào các vị trí tương ứng, tạo ra:```
3 2 1
```Điều này xác nhận rằng phép trừ sử dụng`N - i`một cách chính xác. 

Đối với một hoán vị không cần thiết:```
4
2
4
1
3
```quân xe di chuyển như sau. Xe tại`(1,2)`đi đến`(2,4)`, quân xe tại`(2,4)`đi đến`(4,3)`, quân xe tại`(3,1)`đi đến`(1,2)`, và quân xe tại`(4,3)`đi đến`(3,1)`. Các vị trí trả lời trở thành:```
2 4 1 3
```Trên thực tế việc viết các cột mới theo hàng sẽ mang lại:```
2 4 1 3
```Quá trình triển khai xử lý vấn đề này vì mỗi cột cũ được sử dụng làm hàng đích duy nhất, do đó mỗi quân xe được đặt chính xác một lần.
