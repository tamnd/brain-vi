---
title: "CF 1041A - Trộm cắp"
description: "Chúng tôi được cung cấp một bộ chỉ số bàn phím còn sót lại sau một vụ trộm. Cấu trúc ẩn chính là trước khi bị đánh cắp, tất cả các bàn phím đều tạo thành một khối số nguyên liên tục, đại loại như x, x+1, x+2, v.v. cho đến độ dài không xác định."
date: "2026-06-16T17:58:31+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1041
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 509 (Div. 2)"
rating: 800
weight: 1041
solve_time_s: 161
verified: true
draft: false
---

[CF 1041A - Trộm cướp](https://codeforces.com/problemset/problem/1041/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ chỉ số bàn phím còn sót lại sau một vụ trộm. Cấu trúc ẩn chính là trước khi bị đánh cắp, tất cả các bàn phím đều tạo thành một khối số nguyên liên tục, đại loại như x, x+1, x+2, v.v. cho đến độ dài không xác định. Sau khi bị đánh cắp, một số số nguyên liên tiếp đó bị thiếu và chúng ta chỉ còn lại một tập hợp con không có thứ tự của đoạn liên tiếp ban đầu đó. 

Nhiệm vụ của chúng ta là xác định số lượng bàn phím nhỏ nhất có thể đã bị đánh cắp, giả sử chúng ta có thể tự do chọn cả giá trị bắt đầu x và độ dài ban đầu của toàn bộ đoạn liên tiếp. 

Những gì chúng tôi thực sự đang làm là nhúng các số đã cho vào một khoảng nguyên có độ dài tối thiểu gồm các giá trị liên tiếp. Khi khoảng đó được cố định, mọi số nguyên bên trong nó không nằm trong tập hợp đã cho chắc chắn đã bị đánh cắp. Vì vậy, việc giảm thiểu bàn phím bị đánh cắp tương đương với việc giảm thiểu số lượng số nguyên nằm trong khoảng đã chọn nhưng không có trong đầu vào. 

Ràng buộc n 1000 có nghĩa là chúng ta có thể đủ khả năng suy luận O(n^2) một cách thoải mái. Bất kỳ giải pháp nào sắp xếp và sau đó kiểm tra tất cả các khoảng trống giữa các phần tử đều nằm trong giới hạn. Bất cứ điều gì hình khối hoặc tệ hơn là không cần thiết. 

Trường hợp cạnh tinh tế xuất phát từ thực tế là điểm bắt đầu x không cố định. Ví dụ: nếu giá trị quan sát được nhỏ nhất là 8 thì trình tự thực có thể bắt đầu ở 1, 8 hoặc thậm chí 10 tùy thuộc vào cách chúng tôi diễn giải các giá trị bị thiếu. Một sai lầm ngây thơ là cho rằng chuỗi phải bắt đầu ở min(a), bỏ qua khả năng khối liên tiếp ban đầu kéo dài sang trái. 

Một dạng lỗi khác là bỏ qua những khoảng trống bên trong. Ví dụ: nếu các bàn phím còn lại là 1, 10, 11, 12 thì chỉ xét đến các cực trị là không đủ; khoảng cách giữa 1 và 10 ngụ ý thiếu các phần tử từ 2 đến 9, là một phần của cùng một khối liên tiếp ban đầu trong bất kỳ quá trình tái tạo nhất quán nào. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force là thử mọi điểm bắt đầu có thể x và mọi điểm kết thúc có thể có y sao cho tất cả các số đã cho đều nằm trong [x, y]. Đối với mỗi khoảng ứng cử viên, chúng tôi kiểm tra xem có bao nhiêu số nguyên bị thiếu trong khoảng đó. Điều này đúng vì nó mô phỏng trực tiếp cấu hình cửa hàng gốc không xác định. Tuy nhiên, ranh giới khoảng có thể lên tới 10^9 và việc thử tất cả các khả năng sẽ yêu cầu lặp lại trên một phạm vi rất lớn, khiến nó không khả thi. 

Quan sát quan trọng là khoảng tối ưu phải bao bọc chặt chẽ các số hiện có mà không có độ trễ không cần thiết ở các đầu. Nếu chúng ta sửa một thứ tự bằng cách sắp xếp các bàn phím còn lại thì bất kỳ chuỗi ban đầu hợp lệ nào cũng phải bao gồm từ giá trị quan sát được nhỏ nhất đến giá trị quan sát được lớn nhất, nhưng cũng có thể tối ưu là chỉ kéo dài khoảng vào trong các khoảng trống khi cần thiết để giảm sự không nhất quán. Tuy nhiên, vì việc kéo dài khoảng chỉ làm tăng số lượng phần tử có thể bị thiếu nên chiến lược tốt nhất là cố định khoảng là khối liên tiếp tối thiểu chứa tất cả các giá trị đã cho. 

Trong khoảng thời gian đó, số lượng bàn phím bị thiếu chỉ đơn giản là sự khác biệt giữa độ dài của khoảng thời gian và số lượng phần tử được quan sát. Độ dài khoảng là max(a) - min(a) + 1 và chúng tôi trừ n. 

Vì vậy, vấn đề giảm xuống còn việc sắp xếp (hoặc chỉ quét tìm giá trị tối thiểu và tối đa) và tính toán một biểu thức số học đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force theo từng khoảng thời gian | O(phạm vi · n) hoặc tệ hơn | O(1) | Quá chậm | 
| Sắp xếp + tính toán nhịp | O(n log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả các chỉ mục bàn phím còn lại và lưu chúng vào danh sách. Chúng đại diện cho các điểm bên trong một khoảng thời gian liên tiếp không xác định. 
2. Xác định giá trị nhỏ nhất và lớn nhất trong số đó. Hai giá trị này xác định khoảng thời gian tối thiểu có thể chứa tất cả các bàn phím còn lại, vì bất kỳ khối liên tiếp ban đầu hợp lệ nào ít nhất phải trải dài từ chỉ mục được quan sát tối thiểu đến tối đa. 
3. Tính tổng số số nguyên trong khoảng này bằng cách sử dụng max_value - min_value + 1. Giá trị này thể hiện toàn bộ kho hàng ban đầu của ứng viên nếu không xem xét ràng buộc nào từ các phần tử bị thiếu. 
4. Trừ số bàn phím được quan sát n từ độ dài khoảng này. Những gì còn lại là các số nguyên phải tồn tại trong khối ban đầu nhưng không có trong tập cuối cùng, tương ứng chính xác với bàn phím bị đánh cắp. 

### Tại sao nó hoạt động 

Bất kỳ cấu hình ban đầu hợp lệ nào cũng là một chuỗi số nguyên liền kề phải chứa tất cả các giá trị được quan sát. Do đó, điểm cuối của nó phải lớn nhất là min(a) và ít nhất là max(a), nếu không một số giá trị quan sát được sẽ nằm ngoài chuỗi. Việc mở rộng ra ngoài các giới hạn này chỉ làm tăng số lượng các yếu tố bị thiếu ngụ ý mà không cải thiện tính khả thi. Do đó, chuỗi ban đầu tối thiểu có thể chính xác là khoảng [min(a), max(a)] và mọi số nguyên vắng mặt trong khoảng đó tương ứng với một bàn phím bị đánh cắp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    mn = min(a)
    mx = max(a)
    
    print((mx - mn + 1) - n)

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào theo thời gian tuyến tính và chỉ tính toán hai số liệu thống kê tổng hợp: mức tối thiểu và tối đa. Phép trừ`(mx - mn + 1) - n`đếm trực tiếp các số nguyên còn thiếu trong khoảng liền kề nhỏ nhất có thể bao phủ tất cả các bàn phím còn lại. 

Một lỗi triển khai phổ biến là sắp xếp và sau đó cố gắng đếm các khoảng trống riêng lẻ, điều này không cần thiết ở đây và làm tăng nguy cơ xảy ra lỗi riêng lẻ. Một sai lầm nữa là quên`+1`khi tính toán độ dài khoảng thời gian, vì cả hai điểm cuối đều được bao gồm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
10 13 12 8
```Mảng được sắp xếp: [8, 10, 12, 13] 

| Bước | phút | tối đa | khoảng thời gian | n | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | - | - | - | - | - | 
| sau khi quét | 8 | 13 | 6 | 4 | 2 | 

Khoảng [8, 13] chứa 6 số nguyên. Vì có 4 nên thiếu 2. Điều này phù hợp với trực giác rằng số 9 và số 11 đã bị đánh cắp. 

Dấu vết này xác nhận rằng chỉ có giới hạn toàn cầu mới quan trọng chứ không phải sự sắp xếp nội bộ. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```| Bước | phút | tối đa | khoảng thời gian | n | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | - | - | - | - | - | 
| sau khi quét | 1 | 3 | 3 | 3 | 0 | 

Các bàn phím còn lại đã tạo thành một đoạn hoàn chỉnh liên tiếp nên không còn thiếu phần tử nào. Điều này thể hiện tính đúng đắn khi không có khoảng trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần để tính toán tối thiểu và tối đa | 
| Không gian | O(1) | Chỉ một số biến được lưu trữ ngoài đầu vào | 

Các ràng buộc cho phép tối đa 1000 giá trị, do đó, một lần quét tuyến tính đơn lẻ có hiệu suất không đáng kể. Ngay cả cách tiếp cận O(n log n) được sắp xếp cũng sẽ an toàn nhưng không cần thiết. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    return str(max(a) - min(a) + 1 - n)

# provided sample
assert run("4\n10 13 12 8\n") == "2"

# single element
assert run("1\n100\n") == "0"

# already consecutive
assert run("5\n1 2 3 4 5\n") == "0"

# large gap
assert run("3\n1 100 101\n") == "98"

# unordered input
assert run("4\n8 10 13 12\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | không bị trộm khi chỉ có một bàn phím | 
| khối liên tiếp | 0 | xử lý các trình tự hoàn hảo | 
| khoảng cách lớn | 98 | chính xác trong khoảng thời gian rộng | 
| đầu vào không có thứ tự | 2 | trật tự độc lập | 

## Vỏ cạnh 

Đối với một bàn phím duy nhất, chẳng hạn như đầu vào`1 / 100`, thuật toán tính min = max = 100, độ dài khoảng 1, kết quả 0. Điều này xác nhận rằng mô hình xử lý chính xác các khoảng suy biến. 

Đối với một bộ hoàn toàn liên tiếp như`1 2 3 4`, tối thiểu là 1 và tối đa là 4, cho độ dài khoảng 4 và kết quả 0. Điều này đảm bảo không có kết quả dương tính giả khi không có phần tử nào bị thiếu. 

Đối với những khoảng trống lớn như`1 100 101`, min = 1 và max = 101 cho độ dài khoảng 101 và trừ 3 mang lại 98 bàn phím bị thiếu. Điều này trực tiếp chứng tỏ cách phương thức tính đến tất cả các số nguyên không nhìn thấy được bên trong khoảng giới hạn, bất kể phân bố như thế nào.
