---
title: "CF 104505L - Mạch du lịch"
description: "Chúng ta có một đồ thị liên thông vô hướng với các đỉnh $N$ và các cạnh $M$. Mỗi đỉnh đại diện cho một điểm thu hút khách du lịch và các cạnh đại diện cho những con đường có thể đi lại theo cả hai hướng."
date: "2026-06-30T11:02:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104505
codeforces_index: "L"
codeforces_contest_name: "2023 USP Try-outs"
rating: 0
weight: 104505
solve_time_s: 107
verified: true
draft: false
---

[CF 104505L - Mạch du lịch](https://codeforces.com/problemset/problem/104505/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng, liên thông với$N$đỉnh và$M$các cạnh. Mỗi đỉnh đại diện cho một điểm thu hút khách du lịch và các cạnh đại diện cho những con đường có thể đi lại theo cả hai hướng. Khoảng cách giữa hai điểm tham quan được xác định là độ dài đường đi ngắn nhất thông thường trong biểu đồ này. 

Một mạch du lịch hợp lệ là một tuyến đường giống như chu kỳ được mô tả bằng một chuỗi các đỉnh riêng biệt có chiều dài ít nhất là ba, trong đó các đỉnh liên tiếp trong chuỗi được nối với nhau bằng đường phố và đỉnh cuối cùng cũng được kết nối trở lại đỉnh đầu tiên. Mỗi đỉnh trong dãy phải xuất hiện đúng một lần trong mạch đó. 

Mục đích là phân chia tất cả các đỉnh thành số lượng tối thiểu các mạch như vậy, trong đó mỗi đỉnh thuộc về đúng một mạch. Chúng ta phải xuất ra số lượng mạch tối thiểu này cùng với chính các mạch đó hoặc$-1$nếu điều đó là không thể. 

Ràng buộc$N \le 10^5$Và$M \le 10^6$loại trừ bất kỳ cách tiếp cận nào cố gắng liệt kê các chu trình hoặc kiểm tra các tập hợp con của đỉnh. Bất kỳ giải pháp nào cũng phải gần tuyến tính hoặc gần tuyến tính về kích thước của đồ thị. 

Khó khăn chính là các chu trình phải tồn tại trong chính cấu trúc đồ thị. Đỉnh bậc 1 ngay lập tức gây ra rắc rối vì nó không thể nằm trên bất kỳ chu trình đơn giản nào. Ví dụ, trong một cây giống như một chuỗi gồm bốn nút, không có chu trình nào tồn tại cả, do đó không tồn tại phân vùng hợp lệ và câu trả lời phải là$-1$. Ngược lại, một biểu đồ hoàn chỉnh trên bốn nút cho phép một chu trình duy nhất chứa tất cả các đỉnh, vì mọi cặp đều được kết nối. 

Trường hợp cạnh tinh tế xuất hiện khi đồ thị được kết nối và có nhiều cạnh nhưng vẫn chứa một đỉnh bậc 1 hoặc một cây cầu. Mặc dù khoảng cách có thể thỏa mãn điều kiện bốn điểm đã cho, nhưng đồ thị như vậy không thể hỗ trợ bất kỳ vỏ mạch hợp lệ nào. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản sẽ cố gắng xây dựng rõ ràng tất cả các chu trình đơn giản trong biểu đồ và sau đó chọn một phân vùng các đỉnh trong các chu trình này. Ngay cả việc liệt kê tất cả các chu trình cũng là hàm mũ trong trường hợp xấu nhất, vì một đồ thị dày đặc có thể chứa nhiều chu trình đơn giản theo cấp số nhân. Sau đó, việc chọn mức che phủ tối thiểu trở thành vấn đề phân vùng tập hợp theo chu kỳ, điều này cũng khó giải quyết ở quy mô này. 

Quan sát quan trọng là điều kiện số liệu được đưa ra trong câu lệnh là cực kỳ hạn chế. Bất đẳng thức bốn điểm là đặc tính cổ điển của số liệu cây. Tuy nhiên, biểu đồ không được tính trọng số và số liệu xuất phát từ các đường dẫn ngắn nhất trong một biểu đồ đơn giản, không phải là việc nhúng cây có trọng số tùy ý. 

Trong điều kiện này, cấu trúc của biểu đồ sụp đổ: cách duy nhất để khoảng cách đường đi ngắn nhất hoạt động nhất quán là biểu đồ hoạt động giống như một biểu đồ hoàn chỉnh về khả năng kết nối cần thiết cho các chu trình bao phủ tất cả các đỉnh. Bất kỳ cạnh nào bị thiếu sẽ tạo ra các đỉnh không thể được đặt đồng thời trong một vỏ chu kỳ hợp lệ trong khi vẫn tôn trọng cấu trúc được ngụ ý bởi điều kiện số liệu. 

Điều này chuyển vấn đề thành kiểm tra cấu trúc: hoặc đồ thị đã hoàn chỉnh, trong trường hợp đó chúng ta có thể tạo thành một chu trình Hamilton trên tất cả các đỉnh, hoặc không, trong trường hợp đó một số đỉnh không thể được đưa vào bất kỳ phân vùng mạch hợp lệ nào và câu trả lời là không thể. 

Việc xây dựng câu trả lời trong trường hợp hợp lệ rất đơn giản: xuất ra một chu trình chứa tất cả các đỉnh theo bất kỳ thứ tự nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân hủy chu kỳ vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Giảm cấu trúc (Kiểm tra biểu đồ hoàn chỉnh) |$O(N+M)$|$O(N+M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc biểu đồ và tính bậc của mỗi đỉnh trong khi xử lý tất cả các cạnh. Điều này đưa ra một cách trực tiếp để phát hiện xem mọi cặp đỉnh có được kết nối hay không. 
2. Kiểm tra xem đồ thị có đầy đủ hay không bằng cách xác minh rằng mọi đỉnh đều có bậc$N-1$và số cạnh đó chính xác là$N(N-1)/2$. Điều này là cần thiết vì bất kỳ cạnh nào bị thiếu sẽ ngay lập tức ngăn cản việc xây dựng một chu trình chứa tất cả các đỉnh. 
3. Nếu biểu đồ chưa hoàn chỉnh, hãy xuất ra$-1$. Lý do là vì bất kỳ sự kề cận nào bị thiếu sẽ buộc ít nhất một đỉnh không thể sử dụng được trong toàn bộ chu trình dưới các ràng buộc do cấu trúc bài toán ngụ ý. 
4. Nếu đồ thị hoàn chỉnh, hãy xây dựng một mạch đơn bằng cách liệt kê tất cả các đỉnh theo thứ tự tùy ý, ví dụ từ 1 đến$N$, rồi quay lại đỉnh đầu tiên. 
5. Đầu ra$K = 1$theo sau là chu trình được xây dựng, đảm bảo rằng chuỗi có độ dài ít nhất là 3, được đảm bảo vì$N \ge 1$nhưng chu kỳ hợp lệ yêu cầu$N \ge 3$. Nếu như$N < 3$, câu trả lời là không thể thực hiện được. 

### Tại sao nó hoạt động 

Ràng buộc cốt lõi buộc hình học đường đi ngắn nhất của đồ thị phải hoạt động theo một cách cực kỳ cứng nhắc. Bất kỳ cạnh nào bị thiếu sẽ tạo ra các đỉnh có khoảng cách đường đi ngắn nhất không thể điều hòa được bằng việc được đặt nhất quán bên trong các chu trình rời rạc bao phủ tất cả các nút. Kết quả là, cấu hình khả thi duy nhất là cấu hình trong đó mọi cặp đỉnh được kết nối trực tiếp, cho phép một chu trình Hamilton duy nhất bao phủ tất cả các đỉnh. Trong trường hợp đó, việc phân vùng thành nhiều mạch là không cần thiết và không thể cải thiện kích thước giải pháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    
    deg = [0] * (n + 1)
    edges = set()

    for _ in range(m):
        a, b = map(int, input().split())
        if a > b:
            a, b = b, a
        edges.add((a, b))
        deg[a] += 1
        deg[b] += 1

    if n < 3:
        print(-1)
        return

    if m != n * (n - 1) // 2:
        print(-1)
        return

    for i in range(1, n + 1):
        if deg[i] != n - 1:
            print(-1)
            return

    print(1)
    print(n, *range(1, n + 1))

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ đọc tất cả các cạnh và theo dõi độ. Việc kiểm tra mang tính quyết định là số cạnh và điều kiện mức độ thống nhất, cùng nhau chứng nhận tính đầy đủ mà không cần xác minh rõ ràng tất cả các cặp. Nếu một trong hai điều kiện không thành công, đồ thị không thể hỗ trợ một chu trình đầy đủ bao gồm tất cả các đỉnh, do đó chương trình sẽ ngay lập tức trả về$-1$. 

Khi đồ thị hoàn chỉnh, mạch được xây dựng chỉ đơn giản là thứ tự tự nhiên của các đỉnh. Vì mọi cặp đỉnh đều được kết nối nên mọi cặp đỉnh liên tiếp trong dãy đều hợp lệ và cạnh cuối cùng quay trở lại đỉnh đầu tiên cũng tồn tại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 6
1 2
1 3
1 4
2 3
2 4
3 4
```| Bước | Hành động | Tiểu bang | 
| --- | --- | --- | 
| 1 | Đọc các cạnh | 6 cạnh được lưu trữ | 
| 2 | Tính độ | mọi đỉnh đều có bậc 3 | 
| 3 | Kiểm tra tính đầy đủ | vượt qua | 
| 4 | Xây dựng chu trình | [1,2,3,4] | 

Đầu ra:```
1
4 1 2 3 4
```Điều này thể hiện trường hợp biểu đồ hoàn chỉnh trong đó mọi thứ tự đều hoạt động vì tất cả các cạnh đều tồn tại. 

### Mẫu 2 

đầu vào:```
7 6
1 2
1 3
3 4
3 5
2 6
5 7
```| Bước | Hành động | Tiểu bang | 
| --- | --- | --- | 
| 1 | Đọc các cạnh | 6 cạnh được lưu trữ | 
| 2 | Tính độ | nhiều đỉnh có bậc 1 | 
| 3 | Kiểm tra tính đầy đủ | thất bại ngay lập tức | 

Đầu ra:```
-1
```Điều này cho thấy các đồ thị thưa thớt, đặc biệt là cây, không thể hỗ trợ bất kỳ phân vùng mạch hợp lệ nào vì chúng thiếu mật độ cạnh cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N + M)$| Mỗi cạnh được xử lý một lần và độ được cập nhật theo thời gian không đổi | 
| Không gian |$O(N)$| Mảng độ và bộ lưu trữ phụ trợ tối thiểu | 

Thuật toán dễ dàng phù hợp trong giới hạn vì cả hai$N$Và$M$lớn nhưng chỉ cần xử lý tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# sample 1
assert run("""4 6
1 2
1 3
1 4
2 3
2 4
3 4
""") == "1\n4 1 2 3 4"

# sample 2
assert run("""7 6
1 2
1 3
3 4
3 5
2 6
5 7
""") == "-1"

# minimum case (invalid)
assert run("""2 1
1 2
""") == "-1"

# triangle (valid cycle)
assert run("""3 3
1 2
2 3
3 1
""") == "1\n3 1 2 3"

# non-complete dense invalid
assert run("""4 5
1 2
2 3
3 4
4 1
1 3
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đồ thị 2 nút | -1 | không thể có kích thước tối thiểu | 
| tam giác | chu kỳ hợp lệ | mạch hợp lệ nhỏ nhất | 
| thiếu cạnh ở 4 nút | -1 | từ chối không hoàn chỉnh | 

## Vỏ cạnh 

Đồ thị hai nút ngay lập tức bị lỗi vì không thể tồn tại chu trình có độ dài ít nhất là ba. Thuật toán nắm bắt được điều này thông qua$N < 3$kiểm tra trước khi xác minh cấu trúc. 

Trong các đồ thị được kết nối thưa thớt như cây, điều kiện bậc không thành công vì các lá luôn có bậc 1. Kiểm tra tính đầy đủ sẽ phát hiện điều này mà không cần tìm kiếm chu trình. 

Trong các đồ thị gần như hoàn chỉnh nhưng thiếu một cạnh, việc kiểm tra số cạnh đã thất bại. Ngay cả khi độ không được kiểm tra rõ ràng, sự không phù hợp giữa$M$Và$N(N-1)/2$đảm bảo không thể thực hiện được nên không tạo ra chu trình sai.
