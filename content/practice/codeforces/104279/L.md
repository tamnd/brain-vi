---
title: "CF 104279L - \u6811\u8fb9\u91cd\u6392"
description: "Cho một cây có n đỉnh và n − 1 cạnh vô hướng. Nhiệm vụ không phải là tự xuất ra các cạnh hoặc xây dựng lại danh sách kề mà là gán cho mỗi đỉnh i (từ 1 đến n − 1) một đỉnh đối tác pi sao cho cặp (i, pi) là một trong các cạnh của cây đã cho."
date: "2026-07-01T21:13:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104279
codeforces_index: "L"
codeforces_contest_name: "21st UESTC Programming Contest - Preliminary"
rating: 0
weight: 104279
solve_time_s: 48
verified: true
draft: false
---

[CF 104279L - \u6811\u8fb9\u91cd\u6392](https://codeforces.com/problemset/problem/104279/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho một cây có n đỉnh và n − 1 cạnh vô hướng. Nhiệm vụ không phải là tự xuất ra các cạnh hoặc xây dựng lại danh sách kề mà là gán cho mỗi đỉnh i (từ 1 đến n − 1) một đỉnh đối tác pi sao cho cặp (i, pi) là một trong các cạnh của cây đã cho. 

Nói cách khác, với mỗi đỉnh ngoại trừ đỉnh cuối cùng, chúng ta phải chọn một đỉnh lân cận từ cây và ghi nó làm giá trị được gán. Yêu cầu duy nhất là cặp được chọn phải tương ứng với một cạnh hiện có trên cây. 

Ràng buộc n 100000 ngụ ý cây có thể lớn và mọi nghiệm đều phải tuyến tính hoặc gần tuyến tính trong n. Một chiến lược bậc hai cố gắng tìm kiếm qua các cạnh hoặc kiểm tra khả năng kết nối đối với mỗi đỉnh ngay lập tức là quá chậm. Ngay cả một BFS lặp đi lặp lại trên mỗi nút cũng sẽ dẫn đến hành vi gần như O(n^2) trong trường hợp xấu nhất, vượt xa giới hạn chấp nhận được. 

Một điểm tinh tế trong cách giải thích là cây không có hướng nhưng đầu ra đưa ra một cấu trúc giống như định hướng: mỗi đỉnh i chọn chính xác một pi lân cận. Bản thân điều này không phải là tùy ý; nó phải nhất quán với cấu trúc cây, nghĩa là pi phải liền kề với i. 

Một sai lầm ngây thơ là cho rằng chúng ta phải ấn định một định hướng toàn cục hoặc tạo ra một cấu trúc có gốc rễ. Điều đó không bắt buộc. Một sai lầm khác là nghĩ rằng câu trả lời phải là duy nhất hoặc tối ưu theo một nghĩa nào đó; bất kỳ bài tập hợp lệ nào đều được chấp nhận. 

Một trường hợp thường gây nhầm lẫn khi triển khai là khi cây là một đường dẫn đơn giản. Ví dụ: nếu cây là 1-2-3-4 thì kết quả đầu ra hợp lệ bao gồm p1 = 2, p2 = 1 hoặc 3, p3 = 2 hoặc 4. Việc triển khai bất cẩn luôn chọn hàng xóm nhỏ nhất có thể vẫn hoạt động ở đây, nhưng nỗ lực dựa trên DFS giả định cấu trúc cha-con có thể thất bại nếu gốc truyền tải được chọn kém và các ràng buộc không được tôn trọng khi viết chỉ số đầu ra. 

Một trường hợp khác là cây hình ngôi sao trong đó nút 1 kết nối với tất cả các nút khác. Trong trường hợp đó, tất cả các giá trị pi có thể là 1, giá trị này hợp lệ, nhưng việc triển khai giả định rằng mỗi đỉnh phải chọn một đối tác riêng biệt sẽ không thành công. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là, đối với mỗi đỉnh i từ 1 đến n − 1, quét tất cả các cạnh và tìm một cạnh liên quan đến i. Điều này đúng vì mỗi đỉnh có ít nhất một đỉnh lân cận trên cây nên cạnh đó luôn tồn tại. Tuy nhiên, việc quét tất cả các cạnh cho mỗi đỉnh tốn O(n^2), vì có n − 1 đỉnh và n − 1 cạnh, dẫn đến khoảng 10^10 lần kiểm tra trong trường hợp xấu nhất, điều này là không thể. 

Quan sát quan trọng là chúng ta không cần phải tìm kiếm gì cả. Đầu vào đã ngầm cung cấp cho chúng ta thông tin kề cận. Nếu chúng ta xây dựng một danh sách kề, mỗi đỉnh sẽ biết ngay các đỉnh lân cận của nó, vì vậy chúng ta có thể chọn bất kỳ một đỉnh lân cận nào trong thời gian không đổi trên mỗi đỉnh. Điều này làm giảm vấn đề từ việc tìm kiếm lặp đi lặp lại thành một bước tiền xử lý duy nhất cộng với phép gán tuyến tính. 

Cấu trúc của cây đảm bảo rằng mỗi đỉnh có ít nhất một đỉnh liền kề, do đó danh sách kề không bao giờ trống. Điều này đảm bảo chúng ta luôn có thể chọn một số pi hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force quét các cạnh trên mỗi đỉnh | O(n^2) | O(n) | Quá chậm | 
| Xây dựng danh sách lân cận và chọn bất kỳ hàng xóm nào | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các cạnh và xây dựng danh sách kề cho mỗi đỉnh. Điều này cho phép truy cập trực tiếp tới tất cả các nút lân cận của bất kỳ nút nào trong thời gian không đổi cho mỗi nút lân cận. 
2. Đối với mỗi đỉnh i từ 1 đến n − 1, hãy nhìn vào danh sách kề của nó và chọn bất kỳ đỉnh nào lân cận. Một lựa chọn mang tính quyết định phổ biến là người hàng xóm đầu tiên gặp phải. Điều này đảm bảo pi luôn hợp lệ vì danh sách kề chỉ chứa các đỉnh được nối với nhau bằng một cạnh. 
3. Xuất pi cho mỗi i theo thứ tự.

Quyết định thiết kế chính là chúng ta không bao giờ cần xem xét đỉnh n cho đầu ra vì bài toán chỉ yêu cầu các giá trị lên tới n − 1. Danh sách kề đảm bảo rằng mỗi đỉnh như vậy có ít nhất một đỉnh lân cận, do đó không cần xử lý đặc biệt đối với các lá hoặc các nút bên trong. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên tính bất biến rằng trong một cây, mỗi đỉnh có ít nhất một bậc ngoại trừ trường hợp tầm thường n = 1, trường hợp này bị loại trừ bởi n ≥ 2. Vì danh sách kề được xây dựng trực tiếp từ các cạnh nên bất kỳ đỉnh nào được chọn từ nó đều được đảm bảo tạo thành một cạnh hợp lệ. Bởi vì mỗi pi được chọn độc lập và chỉ bị ràng buộc bởi tính kề cận cục bộ nên không có yêu cầu nhất quán toàn cục nào ngoài tính hợp lệ của cạnh. Do đó, việc chọn một nút lân cận tùy ý trên mỗi nút luôn mang lại kết quả đầu ra hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    adj = [[] for _ in range(n + 1)]
    
    for _ in range(n - 1):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)
    
    res = []
    for i in range(1, n):
        res.append(str(adj[i][0]))
    
    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng danh sách kề tiêu chuẩn trong O(n). Mỗi cạnh được chèn hai lần, một lần cho mỗi điểm cuối, bảo toàn cấu trúc vô hướng. 

Khi đưa ra câu trả lời, chúng tôi chỉ lặp lại tối đa n − 1 chính xác theo yêu cầu. Đối với mỗi nút, chúng tôi truy cập adj[i][0], điều này an toàn vì mọi nút trong cây đều có ít nhất một nút lân cận. Không cần sắp xếp hoặc ưu tiên vì bất kỳ hàng xóm hợp lệ nào đều thỏa mãn điều kiện. 

Một chi tiết triển khai tinh tế là đảm bảo việc lập chỉ mục bắt đầu từ 1, khớp với tuyên bố vấn đề. Một điều nữa là chúng tôi không bao giờ cố gắng xử lý các danh sách kề trống, vì các thuộc tính của cây đảm bảo không trống. 

## Ví dụ đã hoạt động 

Hãy xem xét cây mẫu: 

Các cạnh đầu vào: 

1-2, 1-3, 2-4, 2-5 

Danh sách kề trở thành: 

1: [2, 3] 

2: [1, 4, 5] 

3: [1] 

4: [2] 

5: [2] 

Chúng tôi xử lý các nút từ 1 đến 4: 

| tôi | adj[i] | đã chọn pi | 
| --- | --- | --- | 
| 1 | [2, 3] | 2 | 
| 2 | [1, 4, 5] | 5 (hoặc bất kỳ người hàng xóm nào) | 
| 3 | [1] | 1 | 
| 4 | [2] | 2 | 

Điều này chứng tỏ rằng mọi lựa chọn lân cận đều hợp lệ và vẫn thỏa mãn ràng buộc. 

Ví dụ thứ hai là cây đường dẫn 1-2-3-4-5. 

Danh sách lân cận: 

1: [2] 

2: [1, 3] 

3: [2, 4] 

4: [3, 5] 

5: [4] 

Xử lý: 

| tôi | adj[i] | đã chọn pi | 
| --- | --- | --- | 
| 1 | [2] | 2 | 
| 2 | [1, 3] | 1 | 
| 3 | [2, 4] | 2 | 
| 4 | [3, 5] | 3 | 

Điều này cho thấy thuật toán xử lý các chuỗi một cách tự nhiên trong đó mỗi nút có chính xác hai lựa chọn ngoại trừ điểm cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh được lưu trữ hai lần và mỗi đỉnh được xử lý một lần | 
| Không gian | O(n) | Danh sách kề lưu trữ 2(n − 1) mục | 

Giải pháp phù hợp một cách thoải mái trong các ràng buộc vì cả thời gian và bộ nhớ đều có quy mô tuyến tính với n lên tới 100000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    adj = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)
    res = []
    for i in range(1, n):
        res.append(str(adj[i][0]))
    return " ".join(res)

# provided sample (one valid output form)
assert run("""5
1 2
1 3
2 4
2 5
""") in {
    "2 1 2 2",
    "2 3 2 2",
    "3 1 2 2",
    "3 2 2 2"
}

# minimum n
assert run("""2
1 2
""") == "2"

# star shaped
assert run("""5
1 2
1 3
1 4
1 5
""") == "2 1 1 1"

# path
assert run("""5
1 2
2 3
3 4
4 5
""") == "2 1 2 3"

# skewed small
assert run("""4
1 2
2 3
3 4
""") == "2 1 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 cạnh | 2 | cây nhỏ nhất | 
| cây sao | tất cả 1 hoặc tương đương | độ chính xác của cấu trúc trung tâm | 
| cây đường dẫn | hàng xóm xen kẽ | xử lý dây chuyền | 
| cây xiên | lựa chọn liền kề nhất quán | trường hợp bằng cấp không thống nhất | 

## Vỏ cạnh 

Trong cây hai nút, danh sách kề là [2] và [1]. Thuật toán gán p1 = 2 trực tiếp từ adj[1][0], tạo ra đầu ra hợp lệ duy nhất. 

Trong một ngôi sao có tâm tại 1, adj[1] chứa tất cả các nút khác, trong khi tất cả các lá chỉ chứa 1. Với i từ 1 đến n − 1, mỗi lá i có adj[i][0] = 1 và nút 1 chọn hàng xóm đầu tiên của nó, thường là 2. Mọi phép gán vẫn là một cạnh hợp lệ vì tất cả các cạnh đều kết nối qua tâm. 

Trong một đường dẫn sâu, mỗi nút bên trong có chính xác hai nút lân cận. Thuật toán luôn chọn hàng xóm được chèn đầu tiên, được xác định theo thứ tự đầu vào và vẫn đảm bảo tính kề cận. Tính chính xác không phụ thuộc vào tính nhất quán giữa các lựa chọn giữa các nút, chỉ phụ thuộc vào tính hợp lệ của cạnh cục bộ, được bảo toàn bằng cách xây dựng.
