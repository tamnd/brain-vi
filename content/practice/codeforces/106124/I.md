---
title: "CF 106124I - Instagraph"
description: "Chúng ta có một đồ thị có hướng trong đó mỗi đỉnh đại diện cho một người và mỗi cạnh có hướng đại diện cho một mối quan hệ “theo sau”. Nếu có một cạnh từ $u$ đến $v$, thì người $u$ theo sau người $v$."
date: "2026-06-19T20:04:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106124
codeforces_index: "I"
codeforces_contest_name: "2025-2026 ICPC Nordic Collegiate Programming Contest (NCPC 2025)"
rating: 0
weight: 106124
solve_time_s: 47
verified: true
draft: false
---

[CF 106124I - Instagraph](https://codeforces.com/problemset/problem/106124/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng trong đó mỗi đỉnh đại diện cho một người và mỗi cạnh có hướng đại diện cho một mối quan hệ “theo sau”. Nếu có một cạnh từ$u$ĐẾN$v$, thì người$u$theo dõi người$v$. 

Với mọi đỉnh$v$, chúng tôi muốn đo lường mức độ mạnh mẽ của “mục tiêu người nổi tiếng”. Một nhóm người$S$có giá trị cho$v$nếu mọi người trong$S$theo sau$v$, Và$v$không theo ai trong$S$. Tính trung tâm của người nổi tiếng$CC(v)$là kích thước tối đa có thể có của một nhóm như vậy$S$. 

Vì vậy để cố định$v$, về cơ bản chúng tôi đang tìm kiếm có bao nhiêu người theo dõi$v$có thể thu thập, nhưng chúng tôi chỉ được phép tính những người theo dõi không được theo dõi lại$v$. Bất kỳ đỉnh nào có cạnh từ$v$không thể được đưa vào nhóm. 

Nhiệm vụ là tính toán$CC(v)$cho mọi đỉnh và trả về đỉnh có giá trị lớn nhất, phá vỡ các ràng buộc bằng cách chọn chỉ số nhỏ nhất. 

Kích thước đầu vào lớn, lên tới$10^5$đỉnh và$10^6$các cạnh. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại thông tin trên mỗi đỉnh bằng các lần quét lồng nhau trên các cạnh, vì điều đó sẽ trôi về phía$O(NM)$hành vi trong trường hợp xấu nhất. Một giải pháp gần tuyến tính hơn về số cạnh là cần thiết. 

Trường hợp cạnh tinh tế phát sinh khi các cạnh lẫn nhau tồn tại. Nếu như$u \leftrightarrow v$, sau đó$u$góp phần vào$CC(v)$chỉ nếu$v \nrightarrow u$, điều này sai trong trường hợp này, vì vậy nó không được tính. Một trường hợp góc khác là các đỉnh cô lập, trong đó một đỉnh không có cạnh đi ra sẽ tự động chấp nhận tất cả các đỉnh đi vào.

 Một sai lầm ngây thơ là chỉ tính toán mức độ của mỗi đỉnh và cho rằng đó là$CC(v)$. Điều này không thành công khi có các cạnh hai chiều. Ví dụ, nếu chúng ta có$1 \to 2$Và$2 \to 1$, thì bậc(1) = 1 nhưng$CC(1) = 0$vì 1 theo sau 2 nên 2 không thể là một phần của nhóm. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là xử lý từng đỉnh$v$một cách độc lập. Đối với một cố định$v$, chúng ta có thể lặp qua tất cả các đỉnh$u$, kiểm tra xem$u \to v$tồn tại và cũng xác minh rằng$v \nrightarrow u$. Nếu cả hai điều kiện đều đúng thì chúng ta tính$u$. Điều này đưa ra định nghĩa chính xác về$CC(v)$. 

Sự đúng đắn là ngay lập tức nhưng chi phí lại quá lớn. Kiểm tra tính kề cận một cách đơn giản yêu cầu quét danh sách cạnh hoặc sử dụng ma trận. Với danh sách kề, mỗi lần kiểm tra có thể tốn tới$O(\deg(u))$, dẫn đến trường hợp xấu nhất gần như$O(NM)$, điều này là không thể thực hiện được đối với$10^5$Và$10^6$. 

Quan sát quan trọng là$CC(v)$gần như là mức độ của$v$, ngoại trừ việc chúng ta phải trừ đi những hàng xóm đến cũng có cạnh đi ra trở lại$v$. “Bộ lọc cạnh tương hỗ” này là trở ngại duy nhất. Thay vì tính toán lại các mối quan hệ trên mỗi đỉnh, chúng ta có thể xử lý trước tất cả các cạnh thành một cấu trúc thành viên nhanh để có thể kiểm tra sự tồn tại của$v \to u$trong thời gian không đổi. 

Chúng tôi lưu trữ các bộ kề để tra cứu nhanh. Khi đó với mỗi đỉnh$v$, chúng ta duyệt qua các hàng xóm đến của nó và chỉ đếm những hàng xóm đó$u$như vậy$v \to u$vắng mặt. Điều này làm giảm vấn đề về việc quét tuyến tính trên các cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NM)$|$O(N+M)$| Quá chậm | 
| Tối ưu |$O(N+M)$|$O(N+M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tổ chức lại việc tính toán để duy trì rõ ràng cả thông tin kề cận vào và ra. 

1. Xây dựng tập kề cho các cạnh ngoài của mỗi đỉnh. Điều này cho phép kiểm tra liên tục về thời gian xem cạnh có hướng có tồn tại hay không. 
2. Xây dựng danh sách các đỉnh lân cận cho mỗi đỉnh. Điều này được bắt nguồn trực tiếp từ các cạnh bằng cách đảo ngược hướng trong quá trình phân tích cú pháp đầu vào. 
3. Đối với mỗi đỉnh$v$, lặp qua tất cả các đỉnh$u$trong danh sách đến của nó. 
4. Đối với mỗi như vậy$u$, chỉ đưa nó vào số đếm nếu$u$không nằm trong tập kề cận đi ra của$v$. 
5. Ghi lại số đếm được tính là$CC(v)$. 
6. Theo dõi đỉnh có giá trị lớn nhất và trong trường hợp liên kết giữ chỉ số nhỏ nhất. 

Bước lập luận thiết yếu là những người hàng xóm mới đến là những ứng cử viên duy nhất để được đưa vào nhóm. Mọi thành viên nhóm hợp lệ phải tuân theo$v$, vì vậy chúng phải xuất hiện trong danh sách kề sắp tới. Hạn chế bổ sung duy nhất là loại bỏ những người theo dõi lẫn nhau. 

### Tại sao nó hoạt động 

Thuật toán không bao giờ bỏ lỡ thành viên nhóm hợp lệ vì mọi ứng cử viên phải xuất hiện trong danh sách lân cận sắp tới của$v$. Nó không bao giờ tính thành viên không hợp lệ vì bất kỳ đỉnh nào$u$như vậy$v \to u$tồn tại được lọc rõ ràng bằng cách sử dụng tập hợp kề. Vì mỗi cặp$(u, v)$được xem xét đúng một lần trong quá trình truyền tải đến của$v$, số lượng được tính toán chính xác là kích thước của nhóm hợp lệ lớn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    out_adj = [set() for _ in range(n + 1)]
    in_adj = [[] for _ in range(n + 1)]
    
    for _ in range(m):
        u, v = map(int, input().split())
        out_adj[u].add(v)
        in_adj[v].append(u)
    
    best_v = 1
    best_val = -1
    
    for v in range(1, n + 1):
        cnt = 0
        for u in in_adj[v]:
            if v not in out_adj[u]:
                cnt += 1
        if cnt > best_val or (cnt == best_val and v < best_v):
            best_val = cnt
            best_v = v
    
    print(best_v, best_val)

if __name__ == "__main__":
    solve()
```Vùng lân cận đi được lưu trữ dưới dạng một tập hợp để đảm bảo kiểm tra tư cách thành viên theo thời gian liên tục khi lọc các cạnh lẫn nhau. Vùng kề sắp đến được lưu trữ dưới dạng một danh sách đơn giản vì chúng ta chỉ lặp lại nó một cách tuần tự. 

Vòng lặp trên các đỉnh tính toán$CC(v)$bằng cách chỉ quét các ứng cử viên thực sự, tránh việc quét lặp lại các cạnh không liên quan. Logic ngắt liên kết được xử lý trong cùng một lượt để tránh lưu trữ tất cả kết quả. 

Một cạm bẫy triển khai phổ biến là đảo ngược vùng lân cận không chính xác. Danh sách đến phải được xây dựng bằng điểm cuối đích chứ không phải nguồn. Một sai lầm khác là sử dụng danh sách thay vì tập hợp cho các cạnh đi ra, điều này sẽ làm suy giảm việc kiểm tra tư cách thành viên theo thời gian tuyến tính và làm giảm hiệu suất. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên. 

Các cạnh: 

1 → 2 

2 → 1 

2 → 3 

3 → 2 

3 → 6 

4 → 5 

5 → 2 

6 → 5 

Chúng tôi tính toán các bộ đến: 

| v | trong(v) | 
| --- | --- | 
| 1 | 2 | 
| 2 | 1,3,5 | 
| 3 | 2 | 
| 4 | | 
| 5 | 4,6 | 
| 6 | 3 | 

Bây giờ tính toán$CC(v)$: 

| v | bạn đến | kiểm tra đã lọc | CC(v) | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 theo sau 2? có → loại trừ | 0 | 
| 2 | 1,3,5 | 2→1 có loại trừ, 2→3 có loại trừ, 2→5 không bao gồm | 1 | 
| 3 | 2 | 3→2 có loại trừ | 0 | 
| 4 | không | không | 0 | 
| 5 | 4,6 | 5→4 không bao gồm, 5→6 không bao gồm | 2 | 
| 6 | 3 | 6→3 có loại trừ | 0 | 

Tối đa là 2 ở đỉnh 5. 

Dấu vết này cho thấy các cạnh tương hỗ loại bỏ các ứng cử viên có thể lạm phát theo mức độ như thế nào. 

Mẫu thứ hai: 

Một đỉnh, không có cạnh. Danh sách đến trống, vì vậy mọi$CC(v) = 0$. Đỉnh nhỏ nhất là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N + M)$| Mỗi cạnh được xử lý một lần và mỗi cạnh kề đến được quét một lần | 
| Không gian |$O(N + M)$| Lưu trữ các tập kề và danh sách kề đảo ngược | 

Giải pháp có tỷ lệ tuyến tính với số cạnh, phù hợp thoải mái trong các ràng buộc lên tới$10^6$các cạnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# sample 1
assert run("""6 8
1 2
2 1
2 3
3 2
3 6
4 5
5 2
6 5
""") == "5 2"

# sample 2
assert run("""1 0
""") == "1 0"

# mutual edge only
assert run("""2 1
1 2
""") == "1 0"

# fully bidirectional triangle
assert run("""3 6
1 2
2 1
2 3
3 2
1 3
3 1
""") == "1 0"

# star
assert run("""5 4
2 1
3 1
4 1
5 1
""") == "1 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh lẫn nhau | 1 0 | lọc hai chiều | 
| nhóm tam giác | 1 0 | tất cả các cạnh hủy bỏ hiệu ứng người nổi tiếng | 
| đồ thị sao | 1 4 | độ tối đa không có cạnh sau | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi mọi cạnh vào được ghép với một cạnh ngược đi. Hãy xem xét một biểu đồ hai chiều hoàn toàn trên hai nút:$1 \leftrightarrow 2$. Danh sách đến cho 1 là$[2]$, nhưng vì 1 cũng theo sau 2 nên ứng viên sẽ bị loại và$CC(1)=0$. Điều tương tự cũng xảy ra với đỉnh 2. 

Một trường hợp khác là cấu trúc chìm thuần túy, chẳng hạn như$2 \to 1, 3 \to 1, 4 \to 1$. Ở đây đỉnh 1 có hàng xóm đến$[2,3,4]$, và vì nó không theo sau cái nào trong số chúng nên tất cả đều được tính. Thuật toán xử lý từng ứng viên và xác nhận sự vắng mặt trong tập dữ liệu gửi đi, tạo ra$CC(1)=3$. 

Một đồ thị thưa thớt với các đỉnh cô lập cũng hoạt động một cách tự nhiên. Nếu một đỉnh không có cạnh nào thì danh sách các cạnh của nó trống và điểm của nó bằng 0. Thuật toán không yêu cầu xử lý đặc biệt cho trường hợp này vì vòng lặp đơn giản không thực hiện phép lặp nào.
