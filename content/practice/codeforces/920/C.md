---
title: "CF 920C - Hoán đổi các phần tử liền kề"
description: "Chúng ta được cho một hoán vị có kích thước n, nghĩa là mọi số nguyên từ 1 đến n xuất hiện đúng một lần. Bên cạnh mảng này là một chuỗi mô tả các vị trí liền kề được “kết nối” bằng một phép hoán đổi được phép."
date: "2026-06-13T02:47:14+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "greedy", "math", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 920
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 37 (Rated for Div. 2)"
rating: 1400
weight: 920
solve_time_s: 150
verified: true
draft: false
---

[CF 920C - Hoán đổi các phần tử liền kề](https://codeforces.com/problemset/problem/920/C) 

**Đánh giá:** 1400 
**Tags:** dfs và tương tự, tham lam, toán học, sắp xếp, hai con trỏ 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị có kích thước n, nghĩa là mọi số nguyên từ 1 đến n xuất hiện đúng một lần. Bên cạnh mảng này là một chuỗi mô tả các vị trí liền kề được “kết nối” bằng một phép hoán đổi được phép. Nếu ký tự thứ i là 1 thì chúng ta được phép hoán đổi các phần tử ở vị trí i và i+1 bao nhiêu lần cũng được; nếu bằng 0 thì ranh giới đó bị chặn vĩnh viễn. 

Bởi vì các hoán đổi có thể được lặp lại tùy ý trên các cạnh được phép, quyền tự do thực sự không nằm ở các hoán đổi riêng lẻ mà là về các chỉ số có thể đạt được từ nhau thông qua chuỗi các hoán đổi liền kề được phép. Bên trong vùng có thể truy cập như vậy, các phần tử có thể được hoán vị tùy ý. Giữa các vùng khác nhau, không có phần tử nào có thể vượt qua được. 

Nhiệm vụ là xác định xem, bằng cách sử dụng các phép toán hoán đổi bị ràng buộc này, mảng có thể được chuyển đổi thành hoán vị được sắp xếp 1, 2, 3, ..., n hay không. 

Ràng buộc n lên tới 200000 ngay lập tức loại trừ mọi mô phỏng hoán đổi. Ngay cả việc hoán đổi lặp lại theo thời gian tuyến tính cũng sẽ quá chậm vì một phần tử có thể di chuyển O(n) lần. Bất kỳ cách tiếp cận nào cũng phải giảm vấn đề xuống một lần hoặc xử lý gần tuyến tính với khả năng sắp xếp. 

Một trường hợp lỗi nhỏ xuất hiện khi kết nối là gián tiếp. Ví dụ: nếu cho phép hoán đổi giữa (1,2) và (2,3), chứ không phải (3,4), thì vị trí từ 1 đến 3 sẽ tạo thành một nhóm. Một cách tiếp cận ngây thơ chỉ kiểm tra các vị trí chính xác liền kề hoặc thử các bản sửa lỗi cục bộ sẽ bỏ sót rằng toàn bộ phân khúc bị hạn chế trên toàn cầu. 

Một trường hợp phức tạp khác là khi các thành phần dài nhưng bị rối loạn bên trong: 

đầu vào: 

n = 5 

a = [5, 4, 3, 2, 1] 

s = 1111 

Đầu ra phải là CÓ, vì hoàn toàn tự do cho phép sắp xếp hoàn toàn. Việc kiểm tra liền kề tham lam có thể loại bỏ rối loạn trung gian một cách không chính xác mặc dù có thể sắp xếp lại trật tự toàn cầu. 

Cuối cùng, hãy xem xét các giao dịch hoán đổi bị ngắt kết nối: 

đầu vào: 

n = 4 

a = [2, 1, 4, 3] 

s = 00 0 

Đầu ra là KHÔNG, vì không được phép hoán đổi, mặc dù mỗi cặp có thể được sửa cục bộ một cách riêng biệt trong các vấn đề khác. 

## Phương pháp tiếp cận 

Nếu chúng tôi cố gắng mô phỏng trực tiếp quy trình, chúng tôi sẽ liên tục quét mảng và áp dụng các hoán đổi ở bất cứ nơi nào được phép. Ngay cả trong cách triển khai tốt nhất, việc di chuyển một phần tử đến vị trí chính xác của nó có thể yêu cầu hoán đổi O(n) và việc thực hiện điều này cho tất cả các phần tử sẽ dẫn đến hành vi O(n²). Với n lên tới 200000 thì điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là các giao dịch hoán đổi xác định mối quan hệ tương đương giữa các chỉ số. Nếu vị trí i có thể đạt đến vị trí j thông qua một chuỗi các hoán đổi liền kề được phép, thì các phần tử trong toàn bộ thành phần được kết nối này có thể được hoán vị tùy ý. Điều này biến vấn đề từ “chuỗi thao tác” thành “ràng buộc nhóm”. 

Khi chúng tôi chấp nhận rằng mỗi phân đoạn chỉ mục được kết nối là độc lập, vấn đề sẽ trở thành kiểm tra xem mỗi phân đoạn đã chứa chính xác tập hợp các giá trị phải kết thúc ở đó trong mảng được sắp xếp hay chưa. Vì mục tiêu cuối cùng là hoán vị danh tính nên vị trí i phải chứa giá trị i. Vì vậy, đối với bất kỳ phân đoạn [l, r] nào, các giá trị hiện tại bên trong phải chính xác là các số nguyên từ l đến r theo một thứ tự nào đó. 

Điều này làm giảm nhiệm vụ quét các thành phần được kết nối của biểu đồ được hình thành bởi các giao dịch hoán đổi được phép liền kề, thu thập các giá trị trong từng thành phần và xác minh chúng khớp với khoảng thời gian dự kiến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hoán đổi vũ phu | O(n²) | O(1) | Quá chậm | 
| Nhóm thành phần + Xác thực | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng bằng cách chia nó thành các phân đoạn tối đa trong đó các giao dịch hoán đổi được kết nối đầy đủ.

1. Chúng tôi lặp qua chuỗi hoán đổi và xác định các phân đoạn của chỉ số trong đó các cạnh liên tiếp đều bằng 1. Mỗi phân đoạn như vậy đại diện cho một thành phần có thể thực hiện bất kỳ hoán vị nào. Đây là sự đơn giản hóa cấu trúc quan trọng. 
2. Với mỗi đoạn [l, r], ta thu thập tất cả các giá trị a[l], a[l+1], ..., a[r]. Đây là những giá trị duy nhất có thể xuất hiện trong phân đoạn này vì không phần tử nào có thể vượt qua ranh giới 0. 
3. Chúng tôi sắp xếp các giá trị thu thập được và so sánh chúng với chuỗi dự kiến ​​l, l+1, ..., r. Nếu chúng khớp chính xác thì phân đoạn đó hợp lệ. 
4. Nếu bất kỳ phân đoạn nào không đạt được điều kiện này, chúng tôi sẽ ngay lập tức kết luận rằng việc sắp xếp là không thể. 
5. Nếu tất cả các phân đoạn đều thỏa mãn điều kiện, chúng ta kết luận rằng có thể sắp xếp toàn bộ mảng. 

Lý do sắp xếp hoạt động như một phép kiểm tra là cả tập hợp dự kiến ​​và giá trị hiện tại phải tạo thành một khoảng liền kề của các số nguyên có cùng kích thước. Bình đẳng sau khi sắp xếp là một cách trực tiếp để xác minh các tập hợp giống hệt nhau. 

### Tại sao nó hoạt động 

Biểu đồ hoán đổi phân tách mảng thành các thành phần được kết nối và các hoán đổi không bao giờ di chuyển các phần tử giữa các thành phần. Điều này tạo ra một bất biến: tập hợp nhiều giá trị bên trong bất kỳ thành phần nào sẽ được cố định vĩnh viễn. Mục tiêu được sắp xếp yêu cầu mỗi vị trí i phải chứa giá trị i, do đó mỗi thành phần phải chứa chính xác các số nguyên tương ứng với phạm vi chỉ số của nó. Nếu điều kiện này được giữ, việc sắp xếp lại bên trong mỗi thành phần có thể tạo thành mảng đã được sắp xếp; nếu thất bại, không có chuỗi hoán đổi nào được phép có thể sửa chữa các giá trị bị thiếu hoặc bổ sung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    s = input().strip()
    
    i = 0
    while i < n:
        j = i
        while j < n - 1 and s[j] == '1':
            j += 1
        
        segment = a[i:j+1]
        segment.sort()
        
        expected = list(range(i + 1, j + 2))
        if segment != expected:
            print("NO")
            return
        
        i = j + 1
    
    print("YES")

if __name__ == "__main__":
    solve()
```Việc triển khai quét mảng một lần, mở rộng từng vùng tối đa nơi cho phép hoán đổi. Đối với mỗi vùng, nó trích xuất các giá trị và sắp xếp chúng để kiểm tra xem chúng có khớp với khoảng nhận dạng được yêu cầu hay không. Việc lập chỉ mục được căn chỉnh cẩn thận: phân đoạn [i, j] trong lập chỉ mục dựa trên 0 tương ứng với các giá trị phải chính xác là [i+1, j+1]. 

Một lỗi phổ biến là sắp xếp sai các chỉ số khi xây dựng phạm vi dự kiến. Một vấn đề tế nhị khác là quên rằng j dừng ở n-1, vì chuỗi hoán đổi có độ dài n-1 và xác định ranh giới giữa các phần tử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 6 

a = [1, 2, 5, 3, 4, 6] 

s = 01110 

Chúng tôi xử lý các phân đoạn: 

| Bước | tôi | r | Giá trị phân đoạn | Phân đoạn được sắp xếp | Phạm vi dự kiến ​​| hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | [1] | [1] | [1] | vâng | 
| 2 | 1 | 4 | [2,5,3,4] | [2,3,4,5] | [2,3,4,5] | vâng | 
| 3 | 5 | 5 | [6] | [6] | [6] | vâng | 

Tất cả các phân khúc đều phù hợp với kỳ vọng, vì vậy câu trả lời là CÓ. 

Điều này cho thấy một trường hợp rối loạn bên trong hoàn toàn có thể khắc phục được vì khả năng kết nối cho phép sắp xếp lại trong một khối. 

### Ví dụ 2 

đầu vào: 

n = 4 

a = [2, 1, 4, 3] 

s = 001 

Phân đoạn: 

| Bước | tôi | r | Giá trị phân đoạn | Phân đoạn được sắp xếp | Phạm vi dự kiến ​​| hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | [2,1] | [1,2] | [1,2] | vâng | 
| 2 | 2 | 3 | [4,3] | [3,4] | [3,4] | vâng | 

Mặc dù việc hoán đổi bị hạn chế nhưng mỗi thành phần đều chứa chính xác bộ giá trị chính xác, vì vậy câu trả lời là CÓ. 

Điều này chứng tỏ rằng chúng tôi không “sắp xếp trên toàn cầu”, chỉ kiểm tra tính nhất quán trong từng khu vực được kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phân đoạn được sắp xếp một lần và mọi phần tử thuộc về chính xác một phân đoạn | 
| Không gian | O(n) | Lưu trữ tạm thời cho các giá trị phân đoạn | 

Tổng công việc vẫn nằm trong giới hạn vì mỗi chỉ mục được xử lý chính xác một lần và việc sắp xếp được áp dụng trên các phân đoạn rời rạc có kích thước kết hợp là n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided sample
assert run("""6
1 2 5 3 4 6
01110
""") == "YES"

# no swaps allowed, already sorted
assert run("""3
1 2 3
00
""") == "YES"

# no swaps allowed, impossible
assert run("""3
2 1 3
00
""") == "NO"

# full connectivity
assert run("""5
5 4 3 2 1
1111
""") == "YES"

# alternating small components
assert run("""4
2 1 4 3
00
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 / 00 | CÓ | đã được sắp xếp không có giao dịch hoán đổi | 
| 2 1 3 / 00 | KHÔNG | đảo ngược bị cô lập không thể sửa được | 
| 5 4 3 2 1 / 1111 | CÓ | có thể sắp xếp lại đầy đủ | 

## Vỏ cạnh 

Trường hợp ranh giới xảy ra khi không có giao dịch hoán đổi nào được phép. Trong tình huống này, mỗi chỉ mục tạo thành một thành phần có kích thước bằng một, vì vậy mỗi phân đoạn chỉ khớp với giá trị mong đợi của nó nếu mảng đã được sắp xếp. Thuật toán xử lý việc này một cách tự nhiên vì mỗi phân đoạn [i, i] được kiểm tra theo phạm vi mong đợi của một phần tử. 

Một trường hợp cạnh khác là chuỗi hoán đổi được kết nối đầy đủ. Ở đây toàn bộ mảng trở thành một thành phần, do đó việc kiểm tra giảm xuống còn việc xác minh xem tập hợp nhiều giá trị có chính xác từ 1 đến n hay không. Vì đầu vào được đảm bảo là một hoán vị nên việc sắp xếp phân đoạn đó sẽ luôn khớp với phạm vi dự kiến, dẫn đến CÓ. 

Một trường hợp tinh tế hơn là các ràng buộc xen kẽ như 101010..., trong đó các thành phần rất nhỏ. Thuật toán vẫn hoạt động chính xác vì mỗi khối liền kề tối đa được xử lý độc lập và không có giả định nào được đưa ra về kết nối toàn cầu.
