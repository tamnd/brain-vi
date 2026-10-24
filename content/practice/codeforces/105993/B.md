---
title: "CF 105993B - Cùng bơi nào!"
description: "Đề thi đưa ra hai thời điểm bơi do hai giám khảo đề xuất. Mỗi giám khảo gợi ý một thời điểm nguyên khi họ muốn đi bơi và quyết định cuối cùng là chọn thời điểm sớm nhất có thể để họ vẫn có thể tham dự cuộc thi. Nhiệm vụ là xuất ra thời gian sớm nhất đó."
date: "2026-06-25T13:27:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105993
codeforces_index: "B"
codeforces_contest_name: "Latakia and Tartus Collegiate Programming Contest 2025"
rating: 0
weight: 105993
solve_time_s: 34
verified: true
draft: false
---

[CF 105993B - Cùng bơi nào!](https://codeforces.com/problemset/problem/105993/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đề thi đưa ra hai thời điểm bơi do hai giám khảo đề xuất. Mỗi giám khảo gợi ý một thời điểm nguyên khi họ muốn đi bơi và quyết định cuối cùng là chọn thời điểm sớm nhất có thể để họ vẫn có thể tham dự cuộc thi. Nhiệm vụ là xuất ra thời gian sớm nhất đó. 

Đầu vào chứa hai số nguyên dương biểu thị hai thời điểm được đề xuất. Đầu ra là giá trị nhỏ hơn trong hai giá trị vì bất kỳ lựa chọn nào lớn hơn sẽ chỉ làm trì hoãn chuyến đi bơi mà không giúp ích gì cho họ. 

Các ràng buộc giữ cả hai giá trị nhiều nhất là 1000. Giá trị này đủ nhỏ để ngay cả một mô phỏng trực tiếp cũng có thể hoạt động, nhưng mô phỏng là không cần thiết vì câu trả lời chỉ phụ thuộc vào thứ tự của hai số. Các hoạt động khả dụng là không đổi một cách hiệu quả, vì vậy giải pháp sẽ chạy ngay lập tức bất kể kích thước đầu vào chính xác là bao nhiêu. 

Các trường hợp cạnh chính đến từ các giá trị bằng nhau và từ giá trị nhỏ hơn xuất hiện ở một trong hai vị trí. Giải pháp bất cẩn luôn in số đầu tiên sẽ thất bại khi đề xuất thứ hai đến sớm hơn. 

Ví dụ: nếu đầu vào là:```
4 2
```đầu ra đúng là:```
2
```In số đầu tiên sẽ chọn sai thời gian bơi sau. 

Một trường hợp khác là:```
7 7
```Đầu ra đúng là:```
7
```Hai lựa chọn giống nhau nên một trong hai lựa chọn đều hợp lệ. Một giải pháp chỉ xử lý trường hợp nhỏ hơn nghiêm ngặt và quên sự bình đẳng có thể tạo ra hành vi không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là so sánh mọi thời điểm có thể giữa hai đề xuất và giữ lại đề xuất nhỏ nhất. Điều này có tác dụng vì câu trả lời phải là một trong hai thời điểm được đề xuất. Tuy nhiên, ngay cả mô tả này cũng đã tốn nhiều công sức hơn mức cần thiết vì không có phạm vi để tìm kiếm. Hoạt động có ý nghĩa duy nhất là quyết định giá trị nào trong hai giá trị nhỏ hơn. 

Quan sát quan trọng là bài toán yêu cầu tối thiểu đúng hai giá trị. Mức tối thiểu có thể được tìm thấy trực tiếp bằng một so sánh duy nhất, giảm toàn bộ nhiệm vụ về thời gian không đổi. 

Lực lượng vũ phu và các phương pháp tối ưu ở đây thực sự giống nhau về độ phức tạp vì đầu vào chỉ có hai số, nhưng so sánh trực tiếp thể hiện cấu trúc thực tế của vấn đề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) | O(1) | Không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai thời gian bơi được đề xuất từ đầu vào. 
2. So sánh hai giá trị và chọn giá trị nhỏ hơn. Thời gian càng nhỏ luôn là thời điểm sớm nhất mà cả hai giám khảo có thể đồng ý đi bơi. 
3. In giá trị đã chọn. 

Tại sao nó hoạt động: câu trả lời duy nhất có thể là hai thời điểm được đề xuất. Nếu lần đầu tiên nhỏ hơn, việc chọn lần thứ hai sẽ làm chuyến đi bị trì hoãn. Nếu lần thứ hai nhỏ hơn thì việc chọn lần thứ nhất cũng sẽ làm trì hoãn. Khi chúng bằng nhau, cả hai đều đại diện cho cùng một thời điểm. Việc so sánh luôn chọn mức tối thiểu hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b = map(int, input().split())
print(min(a, b))
```Chương trình đọc cả hai số nguyên trên một dòng và sử dụng thao tác tối thiểu được tích hợp sẵn của Python. Điều này trực tiếp phù hợp với định nghĩa toán học của câu trả lời. 

Không có vòng lặp hoặc cấu trúc dữ liệu bổ sung nên không có điều kiện biên liên quan đến chỉ số. Bình đẳng được xử lý tự động vì`min`trả về giá trị được chia sẻ khi cả hai đầu vào giống nhau. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2
```Trạng thái thực thi là: 

| Bước | một | b | Giá trị được chọn | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 4 | 2 | không | 
| So sánh giá trị | 4 | 2 | 2 | 
| In câu trả lời | 4 | 2 | 2 | 

Đề xuất thứ hai đến sớm hơn nên thuật toán sẽ chọn nó. 

### Mẫu 2 

đầu vào:```
5 9
```Trạng thái thực thi là: 

| Bước | một | b | Giá trị được chọn | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 5 | 9 | không | 
| So sánh giá trị | 5 | 9 | 5 | 
| In câu trả lời | 5 | 9 | 5 | 

Đề xuất đầu tiên đã là thời gian sớm nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chương trình thực hiện một phép so sánh giữa hai số. | 
| Không gian | O(1) | Chỉ có hai giá trị đầu vào được lưu trữ. | 

Các ràng buộc dễ dàng được thỏa mãn vì thuật toán không phụ thuộc vào độ lớn của các số hoặc yêu cầu bất kỳ lần lặp nào. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    a, b = map(int, input().split())
    print(min(a, b))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("4 2\n") == "2\n", "sample 1"
assert run("5 9\n") == "5\n", "sample 2"

assert run("1 1\n") == "1\n", "equal values"
assert run("1000 999\n") == "999\n", "maximum boundary values"
assert run("999 1000\n") == "999\n", "reverse ordering"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Đề xuất bình đẳng được xử lý chính xác. | 
|`1000 999`|`999`| Giá trị biên lớn hơn và giá trị thứ hai nhỏ hơn. | 
|`999 1000`|`999`| Giá trị đầu tiên nhỏ hơn. | 

## Vỏ cạnh 

Đối với trường hợp giá trị bằng nhau:```
1 1
```Thuật toán so sánh các giá trị và thấy rằng không có giá trị nào nhỏ hơn. Hoạt động tối thiểu trả về`1`, điều này đúng vì cả hai giám khảo đều chọn cùng một thời điểm. 

Đối với giá trị thứ hai sớm hơn:```
10 3
```Thuật toán không cho rằng số đầu tiên có bất kỳ mức độ ưu tiên nào. Nó so sánh cả hai giá trị và chọn`3`, tránh lỗi phổ biến khi trả về đầu vào đầu tiên. 

Đối với các giá trị được phép lớn nhất:```
1000 1000
```Cả hai số đều hợp lệ và bằng nhau. Thuật toán vẫn thực hiện một phép so sánh đơn và đưa ra`1000`mà không có bất kỳ xử lý đặc biệt nào.
