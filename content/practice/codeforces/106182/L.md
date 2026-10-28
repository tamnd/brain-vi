---
title: "CF 106182L - Chấy nhảy"
description: "Căn phòng được mô hình hóa như một cái cây, trong đó mỗi người là một đỉnh và hai người được kết nối với nhau nếu chấy có thể di chuyển trực tiếp giữa họ trong một ngày huấn luyện. Sau khi huấn luyện trong d ngày, chấy có thể nhảy giữa hai đỉnh bất kỳ có khoảng cách giữa cây tối đa là d."
date: "2026-06-25T10:52:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106182
codeforces_index: "L"
codeforces_contest_name: "Petrozavodsk Summer Camp 2025. Day 6. Xeppelin Contest The 4rd Universal Cup. Stage 2: Grand Prix of Paris)"
rating: 0
weight: 106182
solve_time_s: 35
verified: true
draft: false
---

[CF 106182L - Chấy nhảy](https://codeforces.com/problemset/problem/106182/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Căn phòng được mô hình hóa như một cái cây, trong đó mỗi người là một đỉnh và hai người được kết nối với nhau nếu chấy có thể di chuyển trực tiếp giữa họ trong một ngày huấn luyện. Sau khi đào tạo cho`d`ngày, chấy có thể nhảy giữa hai đỉnh bất kỳ có khoảng cách giữa cây tối đa`d`. Mục tiêu là chọn người xuất phát và thứ tự đến thăm mỗi người đúng một lần, đồng thời giảm thiểu khoảng cách nhảy cần thiết. 

Đầu vào cho một số cây độc lập. Với mỗi cây, chúng ta cần giá trị nhỏ nhất của`d`cho phép một đơn đặt hàng thăm viếng như vậy. Đầu ra là thời gian đào tạo tối thiểu cho mỗi trường hợp thử nghiệm. 

Ràng buộc rằng tổng số đỉnh trong tất cả các trường hợp thử nghiệm nhiều nhất là`10^6`có nghĩa là giải pháp phải gần tuyến tính. Bất kỳ cách tiếp cận nào thử nhiều đường dẫn, hoán vị hoặc tìm kiếm biểu đồ lặp đi lặp lại sẽ quá chậm. Chúng ta cần trích xuất một đặc tính cấu trúc của cây và quyết định câu trả lời chỉ bằng một vài lần duyệt qua các cạnh. 

Các trường hợp chính xuất phát từ việc nhầm lẫn sự tồn tại của một đường truyền ngắn với thứ tự tìm kiếm theo chiều sâu thông thường. Lệnh DFS có thể buộc phải nhảy từ điểm cuối của nhánh dài sang nhánh ở xa khác, mặc dù lệnh khác vẫn hoạt động. 

Ví dụ: cây sao:```
3
1 2
1 3
```Đầu ra đúng là:```
2
```Một giải pháp bất cẩn có thể cho rằng mọi cây đều có thể được đi qua chỉ bằng các cạnh vì mọi đỉnh đều được kết nối. Tuy nhiên, sau khi thăm một lá và quay lại trung tâm thì không thể thăm lại trung tâm nữa. Lá thứ hai phải đạt được bằng cách nhảy khoảng cách 2. 

Cây đường dẫn thì khác:```
4
1 2
2 3
3 4
```Đầu ra đúng là:```
1
```Các đỉnh có thể được truy cập theo thứ tự`1 -> 2 -> 3 -> 4`, chỉ sử dụng các cạnh hiện có. Một giải pháp luôn in 2 sẽ bỏ lỡ các đường dẫn đặc biệt. 

Một lỗi phổ biến khác là chỉ kiểm tra đường kính. Cây có đường kính lớn vẫn có thể có đáp án 2, vì chấy không cần đi theo đường kính. Họ chỉ cần một đường đi Hamilton trong đồ thị trong đó mọi cặp đỉnh có khoảng cách tối đa là 2 đều được nối với nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng xây dựng thứ tự truy cập cho mọi khoảng cách nhảy có thể. Đối với một khoảng cách cố định, chúng ta có thể tìm kiếm biểu đồ được hình thành bằng cách cho phép các bước nhảy có độ dài đó và kiểm tra xem nó có chứa đường đi Hamilton hay không. Điều này đúng vì bài toán yêu cầu chính xác đường đi như vậy, nhưng tìm kiếm đường đi Hamilton nói chung là theo cấp số nhân. Ngay cả khi ở trên cây, việc cố gắng suy luận về tất cả các mệnh lệnh có thể nhanh chóng trở nên bất khả thi. 

Quan sát quan trọng là cây có một tính chất rất mạnh: bình phương của mỗi cây là Hamilton. Bình phương của một cây là đồ thị trong đó mọi đỉnh được kết nối với tất cả các đỉnh có khoảng cách tối đa là 2. Điều này có nghĩa là với hai ngày huấn luyện, mọi cây đều có thể được thăm quan. 

Bây giờ chỉ còn lại những trường hợp một ngày là đủ. Với một ngày huấn luyện, mỗi lần nhảy đều phải tuân theo mép cây gốc. Một cây chỉ có thể có một đường đi đi qua tất cả các đỉnh bằng các cạnh của chính nó nếu bản thân cây đó là một đường đi đơn. Trong cây, điều này xảy ra chính xác khi mỗi đỉnh có bậc nhiều nhất là 2. 

Lực lượng vũ phu không thành công vì nó coi lệnh thăm viếng là đối tượng chính. Việc quan sát cấu trúc làm giảm toàn bộ vấn đề xuống việc kiểm tra mức độ tối đa của cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây và tính bậc của mỗi đỉnh. Thông tin duy nhất cần thiết là liệu một số đỉnh có phân nhánh thành ba hướng trở lên hay không. 
2. Tìm bậc lớn nhất của tất cả các đỉnh. Nếu mỗi bậc nhiều nhất là 2 thì cây là một đường đi, do đó có thể đi dọc theo các cạnh và câu trả lời là 1. 
3. Nếu một số đỉnh có bậc ít nhất là 3 thì cây không phải là đường đi. Các bước nhảy trong một ngày không thể đi qua mọi đỉnh, trong khi các bước nhảy trong hai ngày luôn là đủ vì hình vuông của cây luôn có đường đi Hamilton. Câu trả lời là 2. 

Tại sao nó hoạt động: 

Cây có bậc lớn nhất là 2 thì không thể phân nhánh nên nó chính xác là một chuỗi các đỉnh. Theo chuỗi thăm mọi đỉnh có khoảng cách 1 lần nhảy. 

Nếu bậc tối đa lớn hơn 2 thì cây có điểm phân nhánh. Một đường dẫn chỉ sử dụng các cạnh của cây không thể đi vào nhiều nhánh mà không xem lại điểm phân nhánh, vì vậy khoảng cách 1 là không thể. Cho phép khoảng cách 2 kết nối mọi đỉnh với cha mẹ, con và anh chị em của nó, đủ để sắp xếp tất cả các đỉnh thành một chuỗi truy cập hợp lệ. 

Điều bất biến đằng sau thuật toán là các câu trả lời duy nhất có thể là 1 và 2. Việc kiểm tra mức độ phân biệt chính xác những cây đạt được giá trị nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = list(map(int, sys.stdin.buffer.read().split()))
    if not data:
        return

    t = data[0]
    idx = 1
    ans = []

    for _ in range(t):
        n = data[idx]
        idx += 1

        deg = [0] * n

        for _ in range(n - 1):
            u = data[idx] - 1
            v = data[idx + 1] - 1
            idx += 2
            deg[u] += 1
            deg[v] += 1

        if max(deg) <= 2:
            ans.append("1")
        else:
            ans.append("2")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Trình phân tích cú pháp đầu vào sử dụng`sys.stdin.buffer.read()`bởi vì tổng số đỉnh có thể lên tới một triệu và việc đọc toàn bộ đầu vào cùng một lúc sẽ tránh được chi phí từ nhiều chi phí nhỏ`input()`cuộc gọi. 

Đối với mỗi trường hợp thử nghiệm, mã chỉ lưu trữ mảng độ. Mỗi cạnh đều tăng cấp chính xác của hai điểm cuối, do đó không cần xây dựng danh sách kề. 

điều kiện`max(deg) <= 2`là toàn bộ bước quyết định. Một cây không thể chứa một chu trình, do đó, mỗi bậc đỉnh có nhiều nhất là 2 buộc nó phải là một đường đi duy nhất. Đối với tất cả các cây khác, câu trả lời chính xác là 2. 

Không có tính toán khoảng cách hoặc truyền tải, giúp tránh được những công việc không cần thiết. Các giá trị đủ nhỏ cho các số nguyên Python bình thường và mức độ tối đa có thể chỉ là`n - 1`. 

## Ví dụ đã hoạt động 

Đầu vào kiểu mẫu:```
4
3
1 2
1 3
5
1 2
1 3
1 4
1 5
7
1 2
1 3
2 4
2 5
3 6
3 7
15
1 2
1 3
2 4
2 5
3 6
3 7
4 8
4 9
5 10
5 11
6 12
6 13
7 14
7 15
```Các trạng thái độ là: 

| Cây | Mức độ tối đa | Quyết định | Trả lời | 
| --- | --- | --- | --- | 
| 3 đỉnh trên một đường đi | 2 | Cây là lối đi | 1 | 
| Ngôi sao 4 lá | 4 | Phân nhánh tồn tại | 2 | 
| Cây nhị phân hoàn hảo 7 đỉnh | 3 | Phân nhánh tồn tại | 2 | 
| Cây nhị phân hoàn hảo 15 đỉnh | 3 | Phân nhánh tồn tại | 2 | 

Trường hợp đầu tiên cho thấy tình huống duy nhất là một ngày là đủ. Các trường hợp khác cho thấy ngay cả những cây có cấu trúc cao và nhiều nhánh cũng chỉ cần hai ngày. 

Một dấu vết nhỏ thứ hai: 

đầu vào:```
2
1 2
```Cây có hai đỉnh. 

| Kích thước cây | Bằng cấp | Mức độ tối đa | Trả lời | 
| --- | --- | --- | --- | 
| 2 | [1, 1] | 1 | 1 | 

Điều này xác nhận trường hợp cây có kích thước tối thiểu. Chuyển động duy nhất có thể xảy ra là dọc theo một cạnh, vì vậy một ngày là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh được xử lý một lần và mảng độ được quét một lần. | 
| Không gian | O(n) | Mảng độ lưu trữ một giá trị cho mỗi đỉnh. | 

Tổng số đỉnh trong tất cả các trường hợp thử nghiệm được giới hạn bởi`10^6`, do đó nghiệm tuyến tính vừa vặn với các giới hạn. Việc triển khai tránh danh sách kề và truyền tải đồ thị, giữ mức sử dụng bộ nhớ ở mức thấp. 

## Trường hợp thử nghiệm```python
import sys, io

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

# minimum size
assert run("""1
2
1 2
""") == "1", "two nodes"

# path
assert run("""1
5
1 2
2 3
3 4
4 5
""") == "1", "path tree"

# star
assert run("""1
6
1 2
1 3
1 4
1 5
1 6
""") == "2", "star tree"

# balanced branching tree
assert run("""1
7
1 2
1 3
2 4
2 5
3 6
3 7
""") == "2", "binary tree"

# multiple test cases
assert run("""2
3
1 2
2 3
4
1 2
1 3
1 4
""") == "1\n2", "mixed cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh | 1 | Kích thước cây tối thiểu và việc duyệt cạnh cơ bản | 
| Chuỗi dài | 1 | Phát hiện cây có hình đường đi | 
| Cây sao | 2 | Phát hiện các đỉnh phân nhánh | 
| Cây nhị phân | 2 | Khẳng định cây không đường cần hai ngày | 
| Các trường hợp hỗn hợp | 1 và 2 | Kiểm tra việc xử lý nhiều trường hợp thử nghiệm | 

## Vỏ cạnh 

Đối với cây có hình đường dẫn:```
5
1 2
2 3
3 4
4 5
```Bằng cấp là`[1, 2, 2, 2, 1]`. Bậc tối đa là 2 nên thuật toán trả về 1. Thứ tự truy cập có thể chỉ cần theo chuỗi từ đầu này đến đầu kia. 

Đối với cây phân nhánh:```
5
1 2
1 3
1 4
1 5
```Bằng cấp là`[4, 1, 1, 1, 1]`. Trung tâm có cấp 4 nên đi bộ một ngày là không thể. Thuật toán trả về 2, phù hợp với thực tế là các lá có thể đến được từ các lá khác thông qua bước nhảy ở khoảng cách 2. 

Đối với cây cân bằng lớn, nhiều đỉnh có bậc 3 nhưng thuật toán không cần mô phỏng bước nhảy. Nó chỉ kiểm tra sự tồn tại của một đỉnh phân nhánh và ngay lập tức trả về 2. Thuộc tính Hamilton của hình vuông cây xử lý tất cả các trường hợp như vậy.
