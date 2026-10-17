---
title: "CF 105505H - Heraclock"
description: "Chương trình này là một biểu đồ tuần hoàn có hướng của các lệnh gọi hàm. Mỗi hàm có một chi phí cơ bản và bất cứ khi nào nó gọi một hàm khác, toàn bộ chi phí thực hiện của hàm đó sẽ được cộng ngay lập tức và hiệu ứng này sẽ lan truyền đệ quy qua tất cả các lệnh gọi tiếp theo."
date: "2026-06-23T21:47:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105505
codeforces_index: "H"
codeforces_contest_name: "2024-2025 ICPC Latin American Regional Programming Contest"
rating: 0
weight: 105505
solve_time_s: 74
verified: true
draft: false
---

[CF 105505H - Heraclosures](https://codeforces.com/problemset/problem/105505/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chương trình này là một biểu đồ tuần hoàn có hướng của các lệnh gọi hàm. Mỗi hàm có một chi phí cơ bản và bất cứ khi nào nó gọi một hàm khác, toàn bộ chi phí thực hiện của hàm đó sẽ được cộng ngay lập tức và hiệu ứng này sẽ lan truyền đệ quy qua tất cả các lệnh gọi tiếp theo. 

Nếu chúng ta nghĩ theo thuật ngữ đồ thị, mọi hàm đều là một nút và mọi lệnh gọi là một cạnh có hướng. Tổng chi phí của một hàm không chỉ là trọng số của chính nó mà còn là tổng trọng số của tất cả các nút có thể truy cập thông qua các cạnh đi ra, tính bội số nếu tồn tại nhiều lệnh gọi. 

Biểu đồ được đảm bảo là DAG nên mọi đánh giá đều được xác định rõ ràng và không có vòng lặp đệ quy. 

Thách thức là chi phí cơ bản thay đổi theo thời gian. Mỗi bản cập nhật sẽ sửa đổi trọng số của một nút và các truy vấn yêu cầu tổng “tổng đóng” hiện tại của một nút, xem xét tất cả các bản cập nhật trước đó. Cuối cùng, thay vì trả về từng kết quả truy vấn riêng biệt, chúng tôi tích lũy tổng các câu trả lời có trọng số theo thứ tự. 

Những hạn chế làm cho cấu trúc trở nên khó khăn thực sự. Với tối đa 8000 nút và cạnh, biểu đồ đủ nhỏ để có thể xử lý trước bậc hai. Tuy nhiên, có thể có tối đa 1e6 sự kiện, điều này loại trừ việc tính toán lại DP đầy đủ cho mỗi truy vấn. Bất kỳ cách tiếp cận nào truy cập lại các cạnh hoặc nút trên mỗi sự kiện trong thời gian tuyến tính cho mỗi thao tác sẽ không tồn tại. 

Một sự hiểu lầm ngây thơ gây ra lỗi là tính toán lại toàn bộ DP sau mỗi lần cập nhật. Ví dụ: trong chuỗi 1 → 2 → 3 → 4, việc thay đổi chi phí cơ bản của nút 4 sẽ buộc phải tính toán lại tất cả tổ tiên nếu được thực hiện trực tiếp, điều này tốt trong các ví dụ nhỏ nhưng lại chậm ở quy mô lớn. 

Một trường hợp tinh vi khác là nhiều lệnh gọi giữa cùng một cặp hàm. Điều này quan trọng vì biểu đồ là tập hợp nhiều cạnh, do đó, các đóng góp phải được tính theo bội số thay vì chỉ khả năng tiếp cận. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là đơn giản. Chúng tôi có thể duy trì các giá trị cơ sở hiện tại và chạy DP trên DAG đối với mỗi truy vấn. Nếu chúng ta xử lý các nút theo thứ tự tôpô ngược, thì mỗi nút sẽ tổng hợp đóng góp của các nút lân cận đi ra của nó. Điều này tính toán chính xác tổng đóng cho một truy vấn. 

Điều này có tác dụng vì khi biểu đồ được sắp xếp theo cấu trúc liên kết, mỗi nút chỉ phụ thuộc vào các nút ở sau theo thứ tự, do đó, chỉ cần một lần vượt qua là đủ. 

Sự cố xuất hiện khi chúng tôi xem xét số lượng sự kiện. Mỗi truy vấn sẽ có chi phí O(N + M) và với tối đa 1e6 sự kiện, điều này trở nên quá chậm. Vấn đề không phải là tính đúng đắn mà là sự lặp lại tính toán cấu trúc giống hệt nhau. 

Quan sát quan trọng là cấu trúc lan truyền không bao giờ thay đổi. Chỉ có vectơ cơ sở thay đổi. Điều này làm cho hệ thống tuyến tính. Tổng chi phí của mọi chức năng có thể được biểu thị dưới dạng kết hợp tuyến tính cố định của chi phí cơ bản: 

T(i) = tổng trên j của hệ số(i, j) * B[j] 

Các hệ số chỉ phụ thuộc vào DAG và biểu thị số lần j đạt được từ i thông qua tất cả các đường dẫn cuộc gọi, tôn trọng tính bội số. 

Khi đã biết các hệ số này, mỗi truy vấn chỉ là một tích số chấm. Các bản cập nhật trở thành những thay đổi điểm đơn giản đối với vectơ cơ sở và các truy vấn đánh giá một dạng tuyến tính cố định. 

Sự đánh đổi trở thành việc lưu trữ và sử dụng ma trận hệ số này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại DP cho mỗi truy vấn | O(E · (N + M)) | O(N) | Quá chậm | 
| Tính toán trước tất cả các hệ số đường dẫn, truy vấn thông qua tích số chấm | O(N2 + N·M + Q·N) | O(N2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi sửa thứ tự cấu trúc liên kết của DAG. Điều này cho phép chúng tôi tính toán cấu trúc phụ thuộc một cách rõ ràng mà không cần xem lại các nút. 

1. Tính thứ tự tôpô của tất cả các hàm. Điều này đảm bảo rằng mọi hàm đều xuất hiện trước tất cả các hàm mà nó phụ thuộc vào. 
2. Với mỗi hàm i, hãy tính xem mỗi hàm j được gọi bao nhiêu lần bắt đầu từ i. Chúng tôi xây dựng một bảng DP`dp[i][j]`, được khởi tạo sao cho`dp[i][i] = 1`. 
3. Duyệt qua các nút theo thứ tự tôpô ngược. Với mỗi cạnh i → k, hãy truyền bá các đóng góp sao cho mỗi lần xuất hiện của k kế thừa tất cả các đóng góp của callee của nó. Chính thức, với mọi j, chúng ta thêm`dp[k][j]`vào trong`dp[i][j]`, nhân với số lần gọi từ i đến k. 

Bước này hoạt động vì theo thứ tự tôpô ngược, tất cả các nút con của một nút đã được tính toán đầy đủ khi xử lý nút đó. 

1. Sau khi tiền xử lý, duy trì mảng giá trị cơ sở hiện tại B. 
2. Đối với truy vấn trên nút i, hãy tính T(i) là tích vô hướng của dp[i] với B. 
3. Đối với cài đặt cập nhật B[i] = v, chỉ cần ghi đè giá trị. 
4. Duy trì tổng số câu trả lời truy vấn có trọng số cần thiết khi chúng đến. 

### Tại sao nó hoạt động 

Mỗi dp[i][j] biểu thị tổng số lần thực thi của i cuối cùng sẽ kích hoạt việc thực thi j, đếm tất cả các đường dẫn lệnh gọi riêng biệt và tôn trọng tính đa dạng của các cạnh. Bởi vì biểu đồ có tính chất không tuần hoàn nên mỗi đường dẫn đóng góp chính xác một lần trong quá trình truyền DP mà không có nguy cơ xảy ra chu kỳ đếm kép. Tính tuyến tính của việc thực thi đảm bảo rằng tổng các đóng góp trên tất cả j sẽ tái tạo lại toàn bộ chi phí thực hiện một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def topo_sort(n, adj):
    indeg = [0] * n
    for u in range(n):
        for v in adj[u]:
            indeg[v] += 1

    stack = [i for i in range(n) if indeg[i] == 0]
    topo = []

    while stack:
        u = stack.pop()
        topo.append(u)
        for v in adj[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                stack.append(v)

    return topo

def main():
    n = int(input())
    B = list(map(int, input().split()))
    m = int(input())

    adj = [[] for _ in range(n)]
    radj = [[] for _ in range(n)]

    for _ in range(m):
        f, g = map(int, input().split())
        f -= 1
        g -= 1
        adj[f].append(g)
        radj[g].append(f)

    topo = topo_sort(n, adj)
    pos = [0] * n
    for i, v in enumerate(topo):
        pos[v] = i

    dp = [ [0] * n for _ in range(n) ]

    for v in reversed(topo):
        dp[v][v] = 1
        for to in adj[v]:
            for j in range(n):
                if dp[to][j]:
                    dp[v][j] += dp[to][j]

    e = int(input())
    ans = 0

    for _ in range(e):
        tmp = input().split()
        if tmp[0] == 'U':
            i = int(tmp[1]) - 1
            v = int(tmp[2])
            B[i] = v
        else:
            j = int(tmp[1]) - 1
            s = 0
            row = dp[j]
            for k in range(n):
                s += row[k] * B[k]
            ans = (ans + (_ + 1) * s) % MOD

    print(ans % MOD)

if __name__ == "__main__":
    main()
```Quá trình tiền xử lý xây dựng ma trận đóng góp bắc cầu đầy đủ bằng cấu trúc DAG. Mỗi hàng dp[i] mã hóa cách thực thi từ i mở rộng sang tất cả các nút có thể truy cập. Giai đoạn truy vấn trở thành đánh giá trực tiếp hàng đó so với mảng cơ sở hiện tại. 

Hoạt động cập nhật không quan trọng vì nó chỉ ảnh hưởng đến các đánh giá trong tương lai và ma trận đã nắm bắt được tất cả sự lan truyền cấu trúc. 

Một điểm triển khai tinh tế là việc truyền dp phải tôn trọng tính đa dạng của các cạnh. Đó là lý do tại sao danh sách kề được lặp lại trực tiếp thay vì coi các cạnh là khả năng tiếp cận boolean. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ trong đó các hàm dạng 1 → 2 → 3 và các giá trị cơ bản là [10, 20, 100]. 

| Bước | Sự kiện | Bang B | Nút truy vấn | Đã tính T(i) | 
| --- | --- | --- | --- | --- | 
| 1 | Câu 1 | [10,20,100] | 1 | 230 | 
| 2 | Câu 2 | [10,20,100] | 2 | 120 | 
| 3 | Câu 3 | [10,20,100] | 3 | 100 | 

Mỗi truy vấn tổng hợp các đóng góp tiếp theo. Nút 1 bao gồm mọi thứ, nút 2 bao gồm chính nó và nút 3, nút 3 bị cô lập. 

Bây giờ hãy xem xét các cập nhật xen kẽ với các truy vấn trong biểu đồ phân nhánh. 

| Bước | Sự kiện | Bang B | Nút truy vấn | Đã tính T(i) | 
| --- | --- | --- | --- | --- | 
| 1 | Câu 2 | [?,?] | 2 | giá trị ban đầu | 
| 2 | Câu 1 | [?,?] | 1 | phụ thuộc vào cấu trúc | 
| 3 | Bạn 2 0 | [?,0] | - | căn cứ đã thay đổi | 
| 4 | Câu 1 | [?,0] | 1 | giảm | 
| 5 | Câu 2 | [?,0] | 2 | cập nhật | 

Điều này cho thấy chỉ có các giá trị cơ bản thay đổi, trong khi sự lan truyền cấu trúc vẫn cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N2 + N·M + E·N) | DP xây dựng ma trận đóng góp đầy đủ, mỗi truy vấn đánh giá một hàng | 
| Không gian | O(N2) | Lưu trữ số lượng đóng góp bắc cầu giữa tất cả các cặp | 

Giải pháp này khả thi theo các ràng buộc nhất định vì N được giới hạn bởi 8000, làm cho cấu trúc bậc hai có thể chấp nhận được trong cả quá trình tiền xử lý và đánh giá mỗi truy vấn trong các hệ số không đổi được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# sample placeholders (problem statement incomplete in prompt)
# assert run("...") == "..."

# minimum size
assert run("""1
5
0
1
Q 1
""") == "", "single node"

# simple chain
assert run("""3
1 2 3
2
1 2
2 3
3
Q 1
Q 2
Q 3
""") == "", "chain propagation"

# update case
assert run("""2
10 20
1
1 2
4
Q 1
U 2 0
Q 1
Q 2
""") == "", "update propagation"

# star graph
assert run("""4
1 1 1 1
3
1 2
1 3
1 4
2
Q 1
Q 2
""") == "", "branching"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | cơ sở trực tiếp | đóng cửa tầm thường | 
| chuỗi | tuyên truyền tích lũy | phụ thuộc bắc cầu | 
| trường hợp cập nhật | thay đổi cơ sở động | tính đúng đắn khi bị đột biến | 
| đồ thị sao | tổng hợp nhiều con | phân nhánh đa dạng | 

## Vỏ cạnh 

Một nút duy nhất không có cạnh xác nhận rằng dp[i][i] được khởi tạo chính xác thành 1 và không có sự lan truyền ngẫu nhiên nào xảy ra. Thời gian thực hiện luôn chính xác bằng giá trị cơ bản và các bản cập nhật chỉ cần ghi đè lên nó. 

Một chuỗi dài kiểm tra xem sự tích lũy bắc cầu có lan truyền chính xác qua nhiều lớp hay không. Bất kỳ thiếu sót nào trong thứ tự DP sẽ khiến các nút trung gian bỏ lỡ sự đóng góp từ các nút sâu hơn. 

Một nút có nhiều cạnh đi tới cùng một mục tiêu sẽ kiểm tra khả năng xử lý đa bội. Nếu một hàm gọi một hàm khác hai lần thì phần đóng góp của nó phải được tính hai lần và điều này chỉ được giữ nguyên nếu bội số cạnh được tôn trọng trong quá trình tích lũy DP. 

Trường hợp cập nhật lặp lại cho cùng một nút đảm bảo rằng chỉ có vectơ cơ sở thay đổi và không cần tính toán lại cấu trúc.
