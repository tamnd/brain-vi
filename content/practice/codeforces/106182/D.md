---
title: "CF 106182D - Bộ phận chữ số"
description: "Nhiệm vụ là chia một chuỗi các chữ số thành các phần liên tiếp sao cho các phần đó khi được hiểu là số nguyên sẽ tạo thành một chuỗi tăng dần. Chúng ta cần tạo ít nhất hai phần và thứ tự các chữ số không thể thay đổi. Nếu tồn tại sự phân chia hợp lệ, chúng tôi sẽ xuất bất kỳ một trong số chúng."
date: "2026-06-25T10:51:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106182
codeforces_index: "D"
codeforces_contest_name: "Petrozavodsk Summer Camp 2025. Day 6. Xeppelin Contest The 4rd Universal Cup. Stage 2: Grand Prix of Paris)"
rating: 0
weight: 106182
solve_time_s: 32
verified: true
draft: false
---

[CF 106182D - Phân chia chữ số](https://codeforces.com/problemset/problem/106182/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là chia một chuỗi các chữ số thành các phần liên tiếp sao cho các phần đó khi được hiểu là số nguyên sẽ tạo thành một chuỗi tăng dần. Chúng ta cần tạo ít nhất hai phần và thứ tự các chữ số không thể thay đổi. Nếu tồn tại sự phân chia hợp lệ, chúng tôi sẽ xuất bất kỳ một trong số chúng. 

Mỗi truy vấn đưa ra một độ dài và một chuỗi chữ số. Độ dài có thể lớn tới 300 và có thể có tới 300 truy vấn. Các giới hạn này đủ nhỏ để có thể dễ dàng chấp nhận việc quét tuyến tính trên mỗi truy vấn. Một thuật toán với các phép toán chuỗi bậc hai hoặc bậc ba vẫn không cần thiết, vì cấu trúc của bài toán cho phép quyết định thời gian không đổi sau khi đọc chuỗi. 

Phần khó khăn không phải là độ dài của chuỗi mà là hiểu được khi nào việc phân tách có thể không thành công. Một giải pháp bất cẩn có thể thử phân chia các vị trí tùy ý hoặc so sánh toàn bộ chuỗi bằng số, điều này là không cần thiết và cũng có thể tạo ra lỗi với các chuỗi dài. 

Ví dụ, hãy xem xét:```
2
99
```Sự phân chia duy nhất có thể là`9 | 9`. Hai số bằng nhau nên đáp án là:```
NO
```Giải pháp chỉ kiểm tra xem chuỗi có chứa nhiều chữ số có thể chấp nhận chuỗi đó không chính xác hay không. 

Một trường hợp ranh giới khác là:```
2
12
```Sự phân chia duy nhất là`1 | 2`, và bởi vì`1 < 2`, đầu ra đúng là bất kỳ biểu diễn hợp lệ nào, chẳng hạn như:```
YES
2
1 2
```Trường hợp độ dài lớn hơn hai hành xử khác nhau. Ví dụ:```
3
654
```Chia thành`6 | 54`có tác dụng vì mọi số có hai chữ số được tạo từ các chữ số này ít nhất là 11, số này luôn lớn hơn số có một chữ số. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi nơi có thể để chèn dấu phân cách. Đối với mỗi lần phân chia, chúng tôi sẽ xây dựng các số kết quả và kiểm tra xem mọi số có nhỏ hơn số tiếp theo hay không. Điều này đúng vì nó xem xét mọi phép chia có thể, nhưng có tới`n - 1`vị trí phân cách và nhiều phần kết quả để so sánh. Với các ràng buộc lớn hơn, việc chuyển đổi liên tục các chuỗi con thành số có thể khiến việc triển khai trở nên tốn kém một cách không cần thiết. 

Quan sát quan trọng là sự phân chia đầu tiên là đủ. Nếu chuỗi có nhiều hơn hai chữ số, hãy chia nó thành chữ số đầu tiên và hậu tố còn lại. Phần đầu tiên là một chữ số nên giá trị của nó nhiều nhất là 9. Phần thứ hai có ít nhất hai chữ số và mỗi chữ số đều từ 1 đến 9 nên giá trị của nó ít nhất là 11. Phần thứ hai đảm bảo lớn hơn. 

Tình huống duy nhất mà cấu trúc này không tự động hoạt động là khi độ dài chuỗi chính xác là hai. Trong trường hợp đó, phép chia duy nhất có thể là hai chữ số riêng lẻ, vì vậy chúng ta chỉ cần so sánh chúng. 

Lực lượng vũ phu hoạt động vì nó tìm kiếm tất cả các vết cắt hợp lệ. Quan sát ở trên loại bỏ hoàn toàn việc tìm kiếm bằng cách chứng minh rằng hầu hết mọi chuỗi đều có một lần cắt hợp lệ rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Không cần thiết | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi chữ số cho truy vấn hiện tại. Độ dài của chuỗi xác định xem có thể sử dụng tính năng phân tách tự động hay không. 
2. Nếu độ dài chính xác là hai, hãy so sánh chữ số thứ nhất và chữ số thứ hai. Nếu chữ số đầu tiên nhỏ hơn thì việc chia thành hai phần có một chữ số là hợp lệ. Ngược lại, không có sự phân chia hợp lệ nào tồn tại. 
3. Nếu độ dài lớn hơn hai thì chia sau chữ số đầu tiên. Hai phần kết quả là ký tự đầu tiên và hậu tố còn lại. Điều này luôn có tác dụng vì hậu tố có ít nhất hai chữ số và do đó biểu thị một số lớn hơn bất kỳ chữ số đơn lẻ nào. 
4. Xuất ra phép chia đã xây dựng. 

Tại sao nó hoạt động: thuật toán duy trì đặc tính là mọi cặp bộ phận liền kề được tạo ra đều tăng lên. Đối với trường hợp có độ dài hai, không có cách chia thay thế nào nên việc so sánh trực tiếp sẽ quyết định câu trả lời. Đối với các chuỗi dài hơn, phần đầu tiên có giá trị tối đa là 9 trong khi phần thứ hai có giá trị tối thiểu là 11, do đó phép phân tách đã chọn luôn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        s = input().strip()

        if n == 2:
            if s[0] < s[1]:
                ans.append("YES")
                ans.append("2")
                ans.append(s[0] + " " + s[1])
            else:
                ans.append("NO")
        else:
            ans.append("YES")
            ans.append("2")
            ans.append(s[0] + " " + s[1:])

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mã xử lý từng truy vấn một cách độc lập. Trường hợp đặc biệt`n == 2`được xử lý đầu tiên vì cấu trúc chung yêu cầu phần thứ hai phải chứa ít nhất hai chữ số. 

Đối với các chuỗi dài hơn, biểu thức`s[1:]`tạo hậu tố sau chữ số đầu tiên. Không cần chuyển đổi số, điều này tránh được các vấn đề với giá trị lớn và giữ cho giải pháp tuyến tính. 

Việc so sánh trong trường hợp hai chữ số sử dụng ký tự thay vì số nguyên. Vì cả hai ký tự đều biểu thị các chữ số đơn nên so sánh từ điển cho kết quả giống hệt như so sánh số. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
6
654321
4
1337
2
33
4
2122
```Đối với truy vấn đầu tiên: 

| Bước | Chuỗi | Tình trạng | Hành động | 
| --- | --- | --- | --- | 
| 1 | 654321 | Độ dài lớn hơn 2 | Chia sau chữ số đầu tiên | 
| 2 | 6, 54321 | 54321 > 6 | Chấp nhận chia nhỏ | 

Đối với truy vấn thứ hai: 

| Bước | Chuỗi | Tình trạng | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1337 | Độ dài lớn hơn 2 | Chia sau chữ số đầu tiên | 
| 2 | 1, 337 | 337 > 1 | Chấp nhận chia nhỏ | 

Đối với truy vấn thứ ba: 

| Bước | Chuỗi | Tình trạng | Hành động | 
| --- | --- | --- | --- | 
| 1 | 33 | Độ dài là 2 | So sánh các chữ số | 
| 2 | 3, 3 | 3 không nhỏ hơn 3 | Từ chối | 

Đối với truy vấn thứ tư: 

| Bước | Chuỗi | Tình trạng | Hành động | 
| --- | --- | --- | --- | 
| 1 | 2122 | Độ dài lớn hơn 2 | Chia sau chữ số đầu tiên | 
| 2 | 2, 122 | 122 > 2 | Chấp nhận chia nhỏ | 

Dấu vết cho thấy tại sao chỉ có trường hợp hai chữ số cần kiểm tra thực sự. Các chuỗi dài hơn được giải quyết ngay lập tức bằng chênh lệch kích thước giữa một chữ số và nhiều chữ số. 

### Ví dụ tùy chỉnh 

đầu vào:```
3
2
12
5
98765
2
98
```| Bước | Chuỗi | Tình trạng | Hành động | 
| --- | --- | --- | --- | 
| 1 | 12 | Độ dài là 2 | So sánh 1 và 2 | 
| 2 | 1, 2 | Tăng | Chấp nhận | 
| 3 | 98765 | Độ dài lớn hơn 2 | Chia thành 9 và 8765 | 
| 4 | 98 | Độ dài là 2 | So sánh 9 và 8 | 
| 5 | 9, 8 | Không tăng | Từ chối | 

Ví dụ này thực hiện phép chia hợp lệ nhỏ nhất, phép tách chuỗi dài tự động và trường hợp hai chữ số bị lỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chuỗi chữ số được đọc một lần và chỉ có một lượng công việc không đổi được thực hiện sau khi đọc nó. | 
| Không gian | O(1) | Ngoài việc lưu trữ đầu ra, thuật toán chỉ sử dụng một số biến. | 

Độ dài chuỗi lớn nhất là nhỏ nhưng giải pháp cũng có quy mô tự nhiên vì nó không bao giờ thử phân tách nhiều vị trí hoặc thực hiện các phép toán số lớn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(data: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(data)
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        s = input().strip()

        if n == 2:
            if s[0] < s[1]:
                ans.append("YES")
                ans.append("2")
                ans.append(s[0] + " " + s[1])
            else:
                ans.append("NO")
        else:
            ans.append("YES")
            ans.append("2")
            ans.append(s[0] + " " + s[1:])

    sys.stdin = old_stdin
    return "\n".join(ans)

assert solve_data("""4
6
654321
4
1337
2
33
4
2122
""") == """YES
2
6 54321
YES
2
1 337
NO
YES
2
2 122""", "sample"

assert solve_data("""1
2
12
""") == """YES
2
1 2""", "smallest valid case"

assert solve_data("""1
2
99
""") == "NO", "equal two digits"

assert solve_data("""1
300
""" + "9" * 300 + "\n") == """YES
2
9 """ + "9" * 299, "large length case"

assert solve_data("""1
3
111
""") == """YES
2
1 11""", "all equal digits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 12`|`YES`| Độ dài tối thiểu có mức phân chia hợp lệ | 
|`2 / 99`|`NO`| Các chữ số bằng nhau không thể tăng | 
| Chiều dài`300`chuỗi |`YES`| Xử lý đầu vào lớn | 
|`111`|`YES`| Tất cả các chữ số bằng nhau có độ dài lớn hơn hai | 

## Vỏ cạnh 

Đối với chuỗi có hai chữ số có các chữ số bằng nhau, chẳng hạn như:```
2
55
```thuật toán nhập trường hợp đặc biệt và so sánh`5`với`5`. Vì chữ số đầu tiên không nhỏ hơn nên nó in ra`NO`. Điều này tránh được lỗi phổ biến khi cho rằng mọi chuỗi có ít nhất hai chữ số đều có thể chia được. 

Đối với chuỗi có hai chữ số trong đó chữ số đầu tiên nhỏ hơn, chẳng hạn như:```
2
34
```thuật toán so sánh`3`Và`4`, tìm thứ tự tăng dần theo yêu cầu và xuất ra hai chữ số riêng lẻ. 

Đối với chuỗi dài hơn chứa các chữ số lớn lặp lại, chẳng hạn như:```
5
99999
```thuật toán không so sánh toàn bộ hậu tố bằng số. Nó chỉ đơn giản là sử dụng`9 | 9999`. Hậu tố có bốn chữ số, vì vậy giá trị của nó lớn hơn 9. Lý do tương tự cũng áp dụng cho mọi độ dài lớn hơn hai.
