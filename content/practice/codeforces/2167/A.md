---
title: "CF 2167A - Hình vuông?"
description: "Chúng ta được cấp bốn chiều dài thanh cho mỗi trường hợp thử nghiệm và chúng ta muốn biết liệu bốn thanh này có thể được sắp xếp để tạo thành ranh giới của một hình vuông hay không."
date: "2026-06-07T23:24:13+07:00"
tags: ["codeforces", "competitive-programming", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2167
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1062 (Div. 4)"
rating: 800
weight: 2167
solve_time_s: 68
verified: true
draft: false
---

[CF 2167A - Hình vuông?](https://codeforces.com/problemset/problem/2167/A) 

**Đánh giá:** 800 
**Tags:** toán, sắp xếp 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp bốn chiều dài thanh cho mỗi trường hợp thử nghiệm và chúng ta muốn biết liệu bốn thanh này có thể được sắp xếp để tạo thành ranh giới của một hình vuông hay không. Mỗi que phải được sử dụng như một đoạn nguyên và chúng ta không được phép cắt hoặc làm biến dạng chúng, vì vậy quyền tự do duy nhất mà chúng ta có là gán mỗi que vào một cạnh của hình vuông. 

Một hình vuông có bốn cạnh có chiều dài bằng nhau. Vì chúng ta không kết hợp các que nên mỗi cạnh phải tương ứng với chính xác một que, nghĩa là bốn độ dài đã cho phải đại diện chung cho bốn cạnh bằng nhau của hình vuông. 

Điều này ngay lập tức làm giảm nhiệm vụ kiểm tra xem liệu tất cả bốn số có thể bằng nhau hay không sau khi không cần sắp xếp lại vì thứ tự không quan trọng. 

Các ràng buộc nhỏ trên mỗi trường hợp thử nghiệm, với mỗi độ dài từ 1 đến 10, nhưng số lượng trường hợp thử nghiệm có thể lớn tới 10^4. Điều này thúc đẩy chúng tôi đạt được O(1) hoặc O(log n) cho mỗi giải pháp thử nghiệm, vì ngay cả việc quét tuyến tính cho mỗi thử nghiệm cũng không đáng kể nhưng bất kỳ điều gì liên quan đến việc kiểm tra tổ hợp đều không cần thiết. 

Một trường hợp thất bại tinh tế xuất hiện khi ai đó cố gắng trở nên linh hoạt quá mức, chẳng hạn như bằng cách cố gắng “ghép” các cây gậy hoặc kết hợp chúng trong đầu. Ví dụ, với đầu vào`1 2 1 2`, người ta có thể nghĩ rằng việc ghép các giá trị bằng nhau sẽ giúp hình thành các cạnh đối diện của hình vuông. Nhưng điều này không hợp lệ vì mỗi bên phải chính xác là một que, do đó các giá trị không bằng nhau không thể tạo thành hình vuông ngay cả khi chúng xuất hiện theo cặp cân đối. 

Một trường hợp gây nhầm lẫn khác là`1 1 5 5`. Nó có thể giống với trực giác của hình chữ nhật, nhưng hình vuông đòi hỏi cả bốn cạnh đều bằng nhau, vì vậy điều này vẫn không thể thực hiện được. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là thử gán mỗi que vào một trong bốn cạnh và kiểm tra xem liệu có thể hình thành một hình vuông hợp lệ hay không. Tuy nhiên, vì tất cả bốn cạnh của một hình vuông phải bằng nhau nên bất kỳ phép gán nào sử dụng cả bốn que đều ngay lập tức buộc phải tuân theo một điều kiện duy nhất: tất cả độ dài các cạnh đã chọn phải khớp chính xác. 

Một ý tưởng tổng quát hơn là hoán đổi phép gán các que sang các cạnh, nhưng chỉ có bốn que, vì vậy nhiều nhất là 4! = 24 cách sắp xếp. Đối với mỗi sự sắp xếp, chúng tôi kiểm tra xem tất cả các bên có bằng nhau hay không. Đây đã là công việc liên tục cho mỗi trường hợp thử nghiệm, do đó, ngay cả cách tiếp cận đơn giản cũng đủ hiệu quả. 

Sự đơn giản hóa chính là nhận ra rằng các hoán vị là không liên quan. Vì tất cả bốn cạnh phải giống nhau nên thuộc tính duy nhất quan trọng là liệu tất cả bốn giá trị có bằng nhau hay không. Việc sắp xếp là tùy chọn nhưng ngay cả khi không sắp xếp, chúng tôi chỉ kiểm tra sự bằng nhau giữa tất cả các phần tử. 

Điều này làm giảm vấn đề thành một so sánh duy nhất: mức tối đa bằng mức tối thiểu hoặc tương đương tất cả các giá trị đều giống hệt nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O(1) mỗi bài kiểm tra (24 lần kiểm tra) | O(1) | Đã chấp nhận | 
| Kiểm tra bình đẳng trực tiếp | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chiều dài bốn thanh cho một ca kiểm thử. Chúng tôi coi chúng như một tập hợp nhỏ trong đó thứ tự không liên quan. 
2. Kiểm tra xem tất cả bốn giá trị có bằng nhau không. Điều này có thể được thực hiện bằng cách so sánh từng giá trị với giá trị đầu tiên. 
3. Nếu mọi so sánh thành công, ghi "CÓ" vì tất cả các que đều có thể đóng vai trò là các cạnh bằng nhau của một hình vuông. 
4. Nếu không thì xuất ra "NO" vì có ít nhất một chiều dài cạnh khác nhau, khiến cho không thể tạo thành hình vuông nếu không cắt hoặc sửa đổi que. 

### Tại sao nó hoạt động 

Một ranh giới hình vuông bao gồm chính xác bốn cạnh có chiều dài bằng nhau. Vì mỗi thanh đầu vào phải được sử dụng như một cạnh duy nhất nên cấu hình hợp lệ duy nhất là tất cả bốn giá trị đầu vào đều giống hệt nhau. Không có sự sắp xếp lại hoặc nhóm nào có thể thay đổi ràng buộc này, vì việc hợp nhất các que là không được phép. Vì vậy, sự bình đẳng giữa cả 4 giá trị này vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c, d = map(int, input().split())
    if a == b == c == d:
        print("YES")
    else:
        print("NO")
```Giải pháp đọc từng trường hợp thử nghiệm một cách độc lập và thực hiện chuỗi so sánh theo thời gian không đổi. Kiểm tra sự bình đẳng theo chuỗi`a == b == c == d`trực tiếp thực thi điều kiện là cả bốn bên đều khớp nhau, tránh mọi cấu trúc dữ liệu sắp xếp hoặc bổ sung. 

Không cần có thứ tự đặc biệt hay tiền xử lý nào vì bài toán không phụ thuộc vào sự sắp xếp mà chỉ phụ thuộc vào sự bình đẳng. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào: một trường hợp tích cực và một trường hợp tiêu cực. 

### Ví dụ 1:`1 1 1 1`| Bước | một | b | c | d | Kiểm tra kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | tất cả đều bình đẳng | 

Tất cả các so sánh đều thành công, do đó thuật toán xuất ra CÓ. Điều này xác nhận điều kiện trong đó một hình vuông tồn tại tầm thường với tất cả các cạnh bằng nhau. 

### Ví dụ 2:`1 2 1 2`| Bước | một | b | c | d | Kiểm tra kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 2 | không khớp ở lần so sánh đầu tiên | 

Bất đẳng thức đầu tiên đã thất bại kể từ đó`a != b`, do đó điều kiện là sai và đầu ra là KHÔNG. Điều này chứng tỏ rằng việc phân bổ tần số cân bằng cũng không thành vấn đề trừ khi tất cả các giá trị khớp chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu số lượng so sánh không đổi giữa bốn số nguyên | 
| Không gian | O(1) | Chỉ có bốn số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Giải pháp này có tỷ lệ tuyến tính theo số lượng trường hợp thử nghiệm, tối ưu với tối đa 10^4 đầu vào. Mỗi thao tác đều có thời gian không đổi nên toàn bộ chương trình dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    out = []
    for _ in range(t):
        a, b, c, d = map(int, input().split())
        out.append("YES" if a == b == c == d else "NO")
    return "\n".join(out)

# provided samples
assert run("""7
1 2 3 4
1 1 1 1
2 2 2 2
1 2 1 2
1 1 5 5
5 5 5 5
4 10 5 9
""") == """NO
YES
YES
NO
NO
YES
NO"""

# custom cases
assert run("1\n1 1 1 1\n") == "YES"
assert run("1\n1 1 1 2\n") == "NO"
assert run("1\n10 10 10 10\n") == "YES"
assert run("1\n2 3 2 3\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`| CÓ | hình vuông hợp lệ tối thiểu | 
|`1 1 1 2`| KHÔNG | sự không phù hợp duy nhất phá vỡ hiệu lực | 
|`10 10 10 10`| CÓ | trường hợp thống nhất giá trị tối đa | 
|`2 3 2 3`| KHÔNG | tập cân bằng nhưng không bằng nhau thất bại | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi ba que bằng nhau và một que khác nhau, chẳng hạn như`5 5 5 4`. Thuật toán so sánh tuần tự:`a == b == c == d`thất bại ở`c == d`, do đó nó xuất ra NO một cách chính xác. Điều này xác nhận rằng tính đồng nhất một phần là không đủ. 

Một trường hợp khác là các giá trị xen kẽ như`2 3 2 3`. Mặc dù nhiều tập hợp được cân bằng, chuỗi đẳng thức sẽ thất bại ngay lập tức ở`a == b`, tạo ra NO Điều này cho thấy cân bằng tần số không thành vấn đề; sự bình đẳng về cơ cấu là cần thiết. 

Một trường hợp thống nhất tối đa như`10 10 10 10`vượt qua tất cả các so sánh một cách rõ ràng, xác nhận rằng các giá trị biên hoạt động giống hệt với các giá trị nhỏ.
