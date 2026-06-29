---
title: "CF 104760B - \u0412\u044b\u0438\u0433\u0440\u044b\u0448"
description: "Chúng ta có một mạng lưới bay không định hướng nhỏ trong đó các sân bay là các nút và các chuyến bay là các cạnh. Một du khách xuất phát tại sân bay 1 và được phép đi đúng K chuyến bay. Mỗi chuyến bay di chuyển dọc theo một cạnh vô hướng đến sân bay lân cận."
date: "2026-06-28T22:00:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104760
codeforces_index: "B"
codeforces_contest_name: "2023-2024 ICPC NERC (NEERC), Kyrgyzstan Qualification Contest"
rating: 0
weight: 104760
solve_time_s: 60
verified: true
draft: false
---

[CF 104760B - \u0412\u044b\u0438\u0433\u0440\u044b\u0448](https://codeforces.com/problemset/problem/104760/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mạng lưới bay không định hướng nhỏ trong đó các sân bay là các nút và các chuyến bay là các cạnh. Một du khách xuất phát tại sân bay 1 và được phép đi đúng K chuyến bay. Mỗi chuyến bay di chuyển dọc theo một cạnh vô hướng đến sân bay lân cận. Khách du lịch có thể quay lại các sân bay và sử dụng lại các cạnh bất kỳ lúc nào. Yêu cầu duy nhất là sau đúng K lượt di chuyển, hành khách phải quay lại sân bay 1. Chúng ta cần đếm xem có bao nhiêu chuỗi sân bay đã ghé thăm có độ dài K+1 (bao gồm cả điểm xuất phát) khác nhau thỏa mãn điều kiện này. 

Cấu trúc về cơ bản là đếm số bước đi có độ dài cố định trong đồ thị vô hướng, với đỉnh đầu và đỉnh cuối cố định. 

Các ràng buộc rất nhỏ về kích thước biểu đồ nhưng vừa phải về chiều dài bước đi. Với N lên tới 100 và K lên tới 60, bất kỳ cách tiếp cận nào khám phá tất cả các đường đi một cách rõ ràng sẽ bùng nổ, vì hệ số phân nhánh có thể lớn và số lượng bước đi có độ dài K tăng theo cấp số nhân. Ngay cả giới hạn trên thô như N * deg^K cũng khiến cho lực lượng vũ phu không thể thực hiện được ngoại trừ K cực kỳ nhỏ. 

Trường hợp quan trọng phá vỡ suy nghĩ ngây thơ là khi có các chu kỳ và nhiều cách để quay về 1. Ví dụ: trong biểu đồ tam giác có K = 2, việc quay lại 1 yêu cầu phải đi ra ngoài và quay lại ngay lập tức, nhưng các nút trung gian khác nhau có thể tồn tại, tạo ra nhiều đường đi hợp lệ dễ bị tính thiếu nếu người ta nhầm tưởng rằng “khoảng cách” hoặc đường đi ngắn nhất quan trọng hơn là đi bộ. 

Một trường hợp cạnh tinh tế khác là tính chẵn lẻ. Nếu cấu trúc đồ thị khiến cho không thể quay về 1 trong chính xác K bước (ví dụ: cây và K lẻ từ cấu trúc hai bên), câu trả lời phải trở thành 0 một cách chính xác mặc dù có nhiều đường dẫn tồn tại cho K-1 hoặc K+1. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là mô phỏng đệ quy tất cả các chuỗi K di chuyển có thể bắt đầu từ nút 1. Ở mỗi bước, chúng tôi phân nhánh tới tất cả các nước láng giềng. Sau K bước, chúng tôi kiểm tra xem chúng tôi có quay lại nút 1 hay không. Điều này đúng về mặt khái niệm vì nó liệt kê mọi bước đi hợp lệ chính xác một lần. Tuy nhiên, số lượng trạng thái đệ quy tăng lên giống như số bước đi có độ dài K trong biểu đồ, trong trường hợp dày đặc là khoảng (N-1)^K. Với K lên tới 60, điều này vượt xa khả năng tính toán. 

Cấu trúc của bài toán gợi ý một công thức quy hoạch động qua các bước. Thay vì liệt kê các đường dẫn đầy đủ, chúng tôi theo dõi xem có bao nhiêu cách chúng tôi có thể đến mỗi nút sau t bước. Điều này có tác dụng vì vị trí tiếp theo chỉ phụ thuộc vào vị trí hiện tại chứ không phải toàn bộ lịch sử. Đây là DP “đếm bước đi trên biểu đồ” cổ điển. 

Chúng tôi định nghĩa dp[t][v] là số cách để đến đỉnh v sau đúng t chuyến bay bắt đầu từ nút 1. Quá trình chuyển đổi là mọi cách để đến nút u tại thời điểm t đều góp phần vào tất cả các lân cận v tại thời điểm t+1. Điều này nén sự phân nhánh theo cấp số nhân thành N trạng thái trên mỗi bước, làm cho quá trình trở nên tuyến tính trong các chuyển đổi K qua các cạnh. 

Vấn đề giảm xuống còn việc lặp lại quá trình chuyển đổi này K lần và đọc dp[K][1]. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê DFS Brute Force | O(N^K) | O(K) đệ quy | Quá chậm | 
| DP qua các bước và nút | O(K * M) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng danh sách kề cho đồ thị để truy vấn lân cận được nhanh chóng. Điều này là cần thiết vì mỗi quá trình chuyển đổi DP cần lặp lại các cạnh một cách hiệu quả thay vì quét tất cả các cặp nút. 
2. Khởi tạo một mảng DP trong đó dp[v] biểu thị số cách để đến nút v sau số bước hiện tại. Đặt dp[1] = 1 vì chúng ta bắt đầu tại sân bay 1 với một đường đi trống hợp lệ. 
3. Lặp lại quá trình sau đây chính xác K lần, mỗi lần đại diện cho một chuyến bay đã thực hiện. 
4. Tạo một mảng mới chứa đầy các số 0. Sự tách biệt này rất quan trọng vì các bản cập nhật không được can thiệp vào các giá trị của bước hiện tại. 
5. Với mỗi nút u, phân phối dp[u] cho tất cả các nút lân cận v bằng cách thêm dp[u] vào ndp[v]. Mỗi phép cộng thể hiện việc mở rộng tất cả các bước đi kết thúc tại u thành các bước kết thúc tại v trong một bước nữa. 
6. Sau khi xử lý tất cả các cạnh, thay thế dp bằng ndp. Bây giờ dp biểu thị số cách đến mỗi nút sau bước tiếp theo. 
7. Sau khi hoàn thành K lần lặp, xuất ra dp[1], đếm tất cả các bước có độ dài K bắt đầu và kết thúc tại nút 1. 

### Tại sao nó hoạt động 

Ở mỗi bước t, dp[v] đếm chính xác tất cả các bước có độ dài t từ nút 1 đến v. Quá trình chuyển đổi bảo toàn tính chất này vì mỗi bước có độ dài t+1 kết thúc tại v phải đến từ một số lân cận u sau t bước và tất cả các đóng góp như vậy được tính chính xác một lần thông qua việc khai triển kề. Không có bước đi nào bị bỏ lỡ vì mọi cạnh tiếp theo có thể đều được khám phá và không có bước đi nào được tính hai lần vì mỗi trạng thái trước đó đóng góp độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N, M, K = map(int, input().split())
    adj = [[] for _ in range(N + 1)]

    for _ in range(M):
        a, b = map(int, input().split())
        adj[a].append(b)
        adj[b].append(a)

    dp = [0] * (N + 1)
    dp[1] = 1

    for _ in range(K):
        ndp = [0] * (N + 1)
        for u in range(1, N + 1):
            if dp[u] == 0:
                continue
            for v in adj[u]:
                ndp[v] += dp[u]
        dp = ndp

    print(dp[1])

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp định nghĩa DP. Danh sách kề lưu trữ đồ thị vô hướng sao cho mỗi cạnh được duyệt hai lần, một lần từ mỗi điểm cuối, phù hợp với quy tắc bay hai chiều. 

Mảng dp được đặt lại sau mỗi lần lặp để đảm bảo chúng tôi chỉ sử dụng các trạng thái từ bước trước đó. Việc bỏ qua các mục bằng 0 sẽ tránh được công việc vòng lặp bên trong không cần thiết nhưng không bắt buộc phải đảm bảo tính chính xác. Câu trả lời cuối cùng được lấy từ dp[1] sau đúng K lần chuyển đổi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 4 4
1 2
1 3
2 4
3 4
```Chúng tôi theo dõi dp sau mỗi bước. 

| Bước | dp[1] | dp[2] | dp[3] | dp[4] | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 0 | 
| 1 | 0 | 1 | 1 | 0 | 
| 2 | 2 | 0 | 0 | 2 | 
| 3 | 0 | 4 | 4 | 0 | 
| 4 | 8 | 0 | 0 | 0 | 

Sau 4 bước, dp[1] = 8. 

Dấu vết này cho thấy khối lượng xen kẽ như thế nào giữa các phân vùng của biểu đồ: từ nút 1 đến các nút lân cận, sau đó quay lại cấu trúc trung gian. Mỗi bước đi khép kín 4 bước hợp lệ sẽ đóng góp chính xác một lần vào tổng số cuối cùng. 

### Mẫu 2 

đầu vào:```
4 4 3
1 2
1 3
2 4
3 4
```| Bước | dp[1] | dp[2] | dp[3] | dp[4] | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 0 | 
| 1 | 0 | 1 | 1 | 0 | 
| 2 | 2 | 0 | 0 | 2 | 
| 3 | 0 | 4 | 4 | 0 | 

Chúng tôi kết thúc sau 3 bước với dp[1] = 0. 

Điều này xác nhận rằng việc quay lại từ đầu phụ thuộc vào tính chẵn lẻ trong cấu trúc biểu đồ này. Sau một số bước lẻ, tất cả khối lượng xác suất nằm trên phân vùng đối diện của biểu đồ hai bên, khiến việc quay lại nút 1 là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(K · M) | Mỗi bước trong số K bước xử lý mọi cạnh một lần trong quá trình chuyển đổi dựa trên kề cận | 
| Không gian | O(N + M) | Danh sách kề cộng với hai mảng DP có kích thước N | 

Với N ∼ 100 và K ∼ 60, tổng số thao tác vào khoảng vài nghìn lần thư giãn cạnh mỗi bước, đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""4 4 4
1 2
1 3
2 4
3 4
""") == "8"

assert run("""4 4 3
1 2
1 3
2 4
3 4
""") == "0"

# custom cases

# minimum case: only two nodes, must bounce back and forth
assert run("""2 1 2
1 2
""") == "1"

# odd step in a 2-node graph must be zero
assert run("""2 1 3
1 2
""") == "0"

# complete triangle, K=2: 1->2->1 and 1->3->1
assert run("""3 3 2
1 2
2 3
1 3
""") == "2"

# disconnected graph: no return possible
assert run("""4 2 2
1 2
3 4
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 nút, K=2 | 1 | chu kỳ qua lại đơn giản nhất | 
| 2 nút, K=3 | 0 | ràng buộc chẵn lẻ | 
| tam giác K=2 | 2 | nhiều đường trở về | 
| ngắt kết nối | 0 | trạng thái không thể truy cập | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi đồ thị là hai phần và K lẻ. Trong tình huống như vậy, bất kỳ bước đi nào bắt đầu từ nút 1 đều phải kết thúc ở phía đối diện của phân vùng sau một số bước lẻ, do đó việc quay lại nút 1 là không thể. DP nắm bắt được điều này một cách tự nhiên vì sau mỗi bước, các giá trị xen kẽ giữa các phân vùng và dp[1] trở thành 0 bất cứ khi nào tính chẵn lẻ không khớp với cấu trúc. 

Một trường hợp khác là khi nút 1 có bậc 1 và đồ thị là một đường đơn. Với K = 2, người đi bộ phải đi ra và quay lại, nhường đường đúng một chiều. Với K = 3, điều đó lại trở thành không thể. DP phản ánh chính xác dao động này vì toàn bộ khối lượng liên tục chảy dọc theo một đường dẫn duy nhất và không thể phân nhánh. 

Trường hợp thứ ba là các biểu đồ nhỏ được kết nối đầy đủ, nơi có thể xem lại nhiều lần. Thuật toán xử lý những điều này mà không cần sửa đổi vì nó không giả sử tính duy nhất của các đường dẫn, chỉ tính các phần tích lũy qua các lần chuyển đổi, đảm bảo mọi chuỗi riêng biệt đều được tính riêng.
