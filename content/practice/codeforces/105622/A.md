---
title: "CF 105622A - Cây tuần hoàn"
description: "Chúng ta được cho một cái cây, nghĩa là một đồ thị liên thông không có chu trình. Nhiệm vụ là thêm chính xác một cạnh mới vào giữa hai nút chưa được kết nối trước đó. Sau khi thêm cạnh này, một chu trình phải xuất hiện và chu trình phải chứa ít nhất ba nút riêng biệt."
date: "2026-06-26T18:15:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105622
codeforces_index: "A"
codeforces_contest_name: "TheForces Round #38 (Tree-Forces)"
rating: 0
weight: 105622
solve_time_s: 39
verified: true
draft: false
---

[CF 105622A - Cây tuần hoàn](https://codeforces.com/problemset/problem/105622/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cái cây, nghĩa là một đồ thị liên thông không có chu trình. Nhiệm vụ là thêm chính xác một cạnh mới vào giữa hai nút chưa được kết nối trước đó. Sau khi thêm cạnh này, một chu trình phải xuất hiện và chu trình phải chứa ít nhất ba nút riêng biệt. 

Trong một cây, đã có chính xác một đường dẫn đơn giản giữa bất kỳ cặp nút nào. Khi chúng ta thêm một cạnh bổ sung giữa hai nút u và v, cạnh đó sẽ đóng đường đi duy nhất giữa chúng và tạo ra một chu trình có độ dài chính xác bằng số cạnh trên đường đi giữa u và v, cộng với một cho cạnh mới. Vì vậy, độ dài chu trình ít nhất là 3 khi và chỉ nếu đường đi ban đầu giữa u và v chứa ít nhất 2 cạnh, nghĩa là khoảng cách giữa u và v trong cây ít nhất là 2. 

Đầu vào mô tả cấu trúc cây với n nút và n − 1 cạnh. Đầu ra là bất kỳ cặp nút hợp lệ nào không được kết nối trực tiếp bởi một cạnh trong cây, bởi vì cặp nút như vậy đảm bảo rằng khoảng cách của chúng ít nhất là 2 (nếu không chúng sẽ liền kề với khoảng cách 1). Việc thêm một cạnh vào giữa bất kỳ cặp nào như vậy sẽ tạo ra một chu trình có độ dài ít nhất là 3. 

Các ràng buộc rất nhỏ, n nhiều nhất là 100. Điều này ngay lập tức loại trừ mọi thứ nằm ngoài phép tính tuyến tính hoặc gần bậc hai, nhưng cấu trúc của bài toán đơn giản hơn thế: chúng ta chỉ cần tìm một cặp không có cạnh. 

Trường hợp cạnh tinh tế xuất hiện khi cây là một ngôi sao. Trong một ngôi sao, nhiều cặp nút là những chiếc lá và không được kết nối trực tiếp. Ví dụ: nếu nút 1 được kết nối với tất cả các nút khác thì bất kỳ cặp lá nào cũng hoạt động. Một cách tiếp cận đơn giản chỉ cố gắng chọn các nút có bậc > 1 hoặc giả sử tồn tại một đường dẫn dài có thể thất bại nếu được triển khai không chính xác, vì đường kính có thể vẫn là 2. 

Một trường hợp góc khác là cây hợp lệ nhỏ nhất, n = 3. Cây luôn là một đường đi có độ dài 2. Cạnh được thêm hợp lệ duy nhất là giữa hai điểm cuối. Nếu người ta tránh nhầm các lá hoặc chỉ kiểm tra tính kề cận cục bộ thì vẫn dễ bỏ sót câu trả lời đúng là các điểm cuối. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ là thử tất cả các cặp nút (u, v), kiểm tra xem chúng có được kết nối bằng một cạnh hay không và nếu không, hãy xuất chúng ra. Vì biểu đồ có tối đa 100 nút nên việc kiểm tra tất cả các cặp có nhiều nhất là 10.000 kết hợp và đối với mỗi cặp, chúng ta có thể kiểm tra tính kề cận bằng cách sử dụng ma trận kề hoặc một tập hợp trên mỗi nút. Điều này đã phù hợp thoải mái trong thời gian. 

Quan sát quan trọng là chúng tôi không thực sự quan tâm đến khoảng cách hoặc việc xây dựng chu trình ngoài thực tế là các nút không liền kề luôn hoạt động. Vì vậy, bài toán quy về việc tìm bất kỳ cặp đỉnh nào không có cạnh giữa chúng. Điều này chuyển vấn đề từ lý luận đồ thị sang kiểm tra kề đơn giản. 

Một cách nhìn có cấu trúc hơn một chút là trong bất kỳ cây nào có n ≥ 3, đồ thị phần bù không bao giờ trống, nghĩa là luôn có ít nhất một cạnh bị thiếu. Do đó, một cặp hợp lệ luôn tồn tại. Điều này đảm bảo rằng một lần quét đơn giản sẽ thành công. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bắt buộc tất cả các cặp bằng kiểm tra kề | O(n²) | O(n2) hoặc O(n) | Đã chấp nhận | 
| Quét tối ưu với bộ kề | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc cây và lưu trữ thông tin lân cận trong cấu trúc cho phép kiểm tra sự tồn tại của cạnh nhanh chóng. Một tập hợp cho mỗi nút hoặc một ma trận boolean đều hoạt động. Điều này là cần thiết vì quyết định chỉ phụ thuộc vào việc một cạnh đã tồn tại hay chưa. 
2. Lặp lại tất cả các cặp nút (u, v) với u < v. Thứ tự tránh trùng lặp và đảm bảo đầu ra xác định. 
3. Với mỗi cặp, hãy kiểm tra xem u và v có được nối trực tiếp bằng một cạnh hay không. Nếu chúng không được kết nối, hãy xuất ngay cặp này. 
4. Dừng lại sau khi tìm thấy cặp hợp lệ đầu tiên, vì bất kỳ cặp nào như vậy đều đảm bảo chu kỳ có độ dài ít nhất là 3 khi được kết nối. 
5. Nếu được triển khai với các tập kề cận, việc kiểm tra sự tồn tại là trung bình O(1), do đó quá trình quét vẫn nhanh. 

Lý do đằng sau việc chọn cặp hợp lệ đầu tiên là vấn đề không yêu cầu tối ưu hóa, chỉ có sự tồn tại. 

### Tại sao nó hoạt động 

Trong một cây, bất kỳ cặp nút nào cũng được kết nối bằng chính xác một đường dẫn đơn giản. Việc thêm một cạnh giữa hai nút sẽ tạo ra một chu trình bao gồm đường dẫn đó cộng với cạnh mới. Nếu các nút liền kề nhau, đường dẫn đó có độ dài 1 và chu trình sẽ có độ dài 2, điều này không hợp lệ. Nếu chúng không liền kề thì đường đi có độ dài ít nhất là 2, do đó độ dài chu trình ít nhất là 3. Vì mỗi cây có ít nhất 3 nút luôn chứa ít nhất một cặp không có cạnh nên việc tìm kiếm được đảm bảo thành công. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
adj = [set() for _ in range(n + 1)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    adj[u].add(v)
    adj[v].add(u)

found = False

for u in range(1, n + 1):
    for v in range(u + 1, n + 1):
        if v not in adj[u]:
            print(u, v)
            found = True
            break
    if found:
        break
```Danh sách kề được lưu trữ dưới dạng bộ để kiểm tra sự tồn tại của cạnh theo thời gian không đổi. Vòng lặp kép đảm bảo cuối cùng chúng ta sẽ gặp phải một cặp không có cạnh. Việc nghỉ sớm rất quan trọng vì có nhiều câu trả lời hợp lệ và chúng ta chỉ cần một câu trả lời. 

Một lỗi triển khai phổ biến là sử dụng danh sách kề rồi kiểm tra tư cách thành viên với`in`, sẽ giảm xuống O(n³) trong trường hợp xấu nhất. Với n ≤ 100 nó vẫn có thể đạt, nhưng đó là rủi ro không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2
2 3
```Chúng tôi xây dựng sự liền kề: 

1 → {2} 

2 → {1, 3} 

3 → {2} 

Chúng tôi quét các cặp: 

| bạn | v | Cạnh tồn tại | Hành động | 
| --- | --- | --- | --- | 
| 1 | 2 | vâng | bỏ qua | 
| 1 | 3 | không | đầu ra | 

Đầu ra là`1 3`. 

Điều này xác nhận rằng các điểm cuối của đường dẫn luôn tạo thành một chu trình hợp lệ khi được kết nối. 

### Ví dụ 2 

đầu vào:```
4
1 2
1 3
3 4
```liền kề: 

1 → {2,3} 

2 → {1} 

3 → {1,4} 

4 → {3} 

Quét: 

| bạn | v | Cạnh tồn tại | Hành động | 
| --- | --- | --- | --- | 
| 1 | 2 | vâng | bỏ qua | 
| 1 | 3 | vâng | bỏ qua | 
| 1 | 4 | không | đầu ra | 

Đầu ra là`1 4`. 

Điều này chứng tỏ rằng ngay cả trong một cây phân nhánh, chúng ta luôn có thể sớm tìm thấy một cặp không liền kề trong quá trình truyền tải. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Chúng tôi kiểm tra tất cả các cặp nút một lần và mỗi lần kiểm tra cạnh là O(1) bằng cách sử dụng bộ | 
| Không gian | O(n) | Bộ nhớ kề cho n−1 cạnh | 

Với n 100, số thao tác tối đa là khoảng 10.000 lần kiểm tra, con số này không đáng kể trong giới hạn 1 giây. Việc sử dụng bộ nhớ cũng không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    n = int(input())
    adj = [set() for _ in range(n + 1)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        adj[u].add(v)
        adj[v].add(u)

    for u in range(1, n + 1):
        for v in range(u + 1, n + 1):
            if v not in adj[u]:
                return f"{u} {v}"
    return ""

# sample 1
assert run("3\n1 2\n2 3\n") == "1 3"

# sample 2
assert run("4\n1 2\n1 3\n3 4\n") == "1 4"

# minimum case
assert run("3\n1 2\n2 3\n") in {"1 3", "3 1"} or run("3\n1 2\n2 3\n") == "1 3"

# star tree
assert run("5\n1 2\n1 3\n1 4\n1 5\n") != ""

# path tree
assert run("5\n1 2\n2 3\n3 4\n4 5\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đường dẫn 3 nút | 1 3 | trường hợp hợp lệ nhỏ nhất, điểm cuối | 
| Cây mẫu 4 nút | 1 4 | phân nhánh cây đúng cách | 
| cây sao | bất kỳ cặp không cạnh nào | trường hợp kề cận dày đặc | 
| chuỗi 5 | bất kỳ cặp không liền kề nào | đảm bảo quá trình quét tìm thấy cặp sâu | 

## Vỏ cạnh 

Trong cây hình ngôi sao, nút 1 kết nối với tất cả các nút khác. Thuật toán sẽ kiểm tra sớm các cặp như (2, 3), không thấy cạnh nào và xuất ra ngay lập tức. Điều này xác nhận rằng các trung tâm cấp cao không ảnh hưởng đến tính chính xác. 

Trong cây có hình đường dẫn, sự kề cận là cục bộ, vì vậy cặp không liền kề đầu tiên thường là các điểm cuối hoặc các nút được phân tách bằng ít nhất một nút trung gian. Quá trình quét tự nhiên tìm thấy (1, n) hoặc một cặp hợp lệ khác. 

Trong trường hợp nhỏ nhất n = 3, cặp không cạnh duy nhất là điểm cuối. Vòng lặp sẽ bỏ qua phần kề giữa một cách chính xác và trả về câu trả lời đúng.
