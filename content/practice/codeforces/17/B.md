---
title: "CF 17B - Hệ thống cấp bậc"
description: "Chúng tôi có nhân viên và người giám sát trực tiếp đưa ra lời đề nghị giữa họ. Lời đề nghị (a, b, c) có nghĩa là nhân viên a sẵn sàng giám sát nhân viên b về chi phí c. Tiêu chuẩn đang giảm dần theo mỗi lời đề nghị, vì vậy q[a] > q[b]."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "greedy", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 17
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 17"
rating: 1500
weight: 17
solve_time_s: 94
verified: true
draft: false
---
[CF 17B - Hệ thống phân cấp](https://codeforces.com/problemset/problem/17/B) 

**Đánh giá:** 1500 
**Tags:** dfs và các con đường tương tự, dsu, tham lam, ngắn nhất 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có nhân viên và người giám sát trực tiếp đưa ra lời đề nghị giữa họ. Một lời đề nghị`(a, b, c)`có nghĩa là nhân viên`a`sẵn sàng giám sát nhân viên`b`về chi phí`c`. Trình độ chuyên môn đang giảm dần theo mỗi lời đề nghị, vì vậy`q[a] > q[b]`. 

Hệ thống phân cấp cuối cùng phải là một cây có gốc. Một nhân viên trở thành ông chủ tổng thể và không có người giám sát. Mỗi nhân viên khác phải có chính xác một biên đến, nghĩa là có chính xác một người giám sát trực tiếp. Tổng chi phí của tất cả các ưu đãi được chọn phải càng nhỏ càng tốt. 

Hạn chế về trình độ chuyên môn là thuộc tính cấu trúc quan trọng. Vì mọi cạnh đều đi từ trình độ cao hơn đến trình độ thấp hơn nên chu kỳ không bao giờ có thể tồn tại. Biểu đồ đã là một DAG được sắp xếp theo tiêu chuẩn. 

Các giới hạn đủ nhỏ để cho phép các thuật toán đồ thị có hành vi bậc hai. Chúng tôi có nhiều nhất`1000`nhân viên và`10000`cung cấp. MỘT`O(m log m)`hoặc`O(m α(n))`giải pháp dễ dàng đủ nhanh. Thậm chí`O(n^2)`vượt qua được chấp nhận được. Việc tìm kiếm toàn diện trên tất cả các tập con của các cạnh là không thể vì có thể có tới`2^10000`sự lựa chọn. 

Phần nguy hiểm là cạnh đầu vào có chi phí tối thiểu cho mỗi nút không tự động đảm bảo hệ thống phân cấp hợp lệ. Các cạnh được chọn phải kết nối tất cả nhân viên vào một cấu trúc gốc. 

Hãy xem xét ví dụ này:```
3
10 9 8
2
1 2 1
1 3 1
```Câu trả lời là`2`. Người lao động`1`là gốc và cả hai cái còn lại đều trỏ đến nó một cách gián tiếp thông qua các cạnh được chọn. 

Bây giờ hãy xem xét:```
3
10 9 8
1
1 2 1
```Người lao động`3`không có người giám sát nào cả. Câu trả lời đúng là`-1`. 

Việc triển khai bất cẩn cũng có thể yêu cầu biểu đồ được kết nối không chính xác theo nghĩa vô hướng. Điều đó là không cần thiết vì thứ tự trình độ chuyên môn đã cấm các chu kỳ. Chúng tôi chỉ cần mọi nút không phải gốc đều có một nút cha. 

Một trường hợp tinh tế khác là khi nhiều nhân viên không nhận được lời đề nghị nào.```
4
10 8 6 4
1
1 2 5
```Người lao động`3`Và`4`không thể được giám sát bởi bất cứ ai. Một cây có gốc chỉ có thể có một gốc, vì vậy câu trả lời là`-1`. 

Thứ tự trình độ cũng hàm ý một cái gì đó mạnh mẽ hơn. Nhân viên có trình độ tối đa không bao giờ có thể có người giám sát, bởi vì không có nhân viên có trình độ cao hơn tồn tại. Nếu có một số nhân viên có trình độ chuyên môn tối đa như nhau thì không có ranh giới nào có thể tồn tại giữa họ vì các lời đề nghị đòi hỏi sự bất bình đẳng nghiêm ngặt. Khi đó nhiều gốc trở thành điều tất yếu và câu trả lời là ngay lập tức`-1`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử tất cả các tập hợp con các ưu đãi với chính xác`n - 1`các cạnh và kiểm tra xem chúng có tạo thành một cây có gốc hợp lệ hay không. Bản thân việc xác thực rất dễ dàng vì biểu đồ có tính chu kỳ, nhưng số lượng tập hợp con là rất lớn. Với`10000`các cạnh, thậm chí việc lặp qua tất cả các tập hợp con là vô vọng. 

Một ý tưởng mạnh mẽ hơn là suy nghĩ về các cạnh sắp tới. Mỗi nhân viên ngoại trừ người gốc phải chọn chính xác một người giám sát. Vì biểu đồ đã có tính tuần hoàn nên nếu mọi nút ngoại trừ một nút đều có chính xác một bậc thì cấu trúc sẽ tự động trở thành một cây có gốc. 

Quan sát này thay đổi hoàn toàn vấn đề. 

Bởi vì trình độ giảm dần dọc theo các cạnh nên chu kỳ là không thể. Điều đó có nghĩa là chúng tôi hoàn toàn không cần DSU, MST logic hoặc phát hiện chu kỳ. Điều quan trọng duy nhất là liệu mọi nhân viên không có quyền root đều có ít nhất một lời đề nghị sắp tới hay không. Nếu vậy, việc chọn lợi thế đầu vào rẻ nhất cho mỗi nhân viên như vậy luôn là tối ưu. 

Tại sao? Bởi vì sự lựa chọn của một nhân viên không bao giờ ảnh hưởng đến giá trị lựa chọn của một nhân viên khác. Không có chu kỳ nào được tạo ra một cách vô tình. Cấu trúc biểu đồ đảm bảo tính nhất quán một cách tự động. 

Vậy thuật toán tối ưu trở thành: 

Đối với mỗi nhân viên, hãy tìm cạnh đầu vào có chi phí tối thiểu. Chính xác một nhân viên được phép không có cạnh đến, nhân viên đó sẽ trở thành gốc. Nếu có nhiều hơn một nhân viên thiếu các cạnh đầu vào thì việc xây dựng hệ thống phân cấp là không thể. 

Điều này làm giảm vấn đề xuống còn một lần quét đơn giản trên tất cả các cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m) | O(m) | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`best`Ở đâu`best[i]`lưu trữ chi phí tối thiểu của bất kỳ cạnh nào đi vào nhân viên`i`. 
2. Khởi tạo mọi giá trị trong`best`với vô cùng, có nghĩa là chưa có lời đề nghị giám sát nào được nhìn thấy. 
3. Xử lý mọi hồ sơ`(a, b, c)`. 

Người lao động`a`có thể giám sát nhân viên`b`về chi phí`c`. Vì chỉ có cạnh đến rẻ nhất mới quan trọng, hãy cập nhật:```
best[b] = min(best[b], c)
```4. Sau khi xử lý tất cả các đề nghị, hãy quét tất cả nhân viên. 

Nếu như`best[i]`vẫn là vô cực nhé nhân viên`i`không có người giám sát có thể. 
5. Đếm xem có bao nhiêu nhân viên không có lợi thế. 

Một hệ thống phân cấp hợp lệ phải có chính xác một nhân viên như vậy, đó là nhân viên gốc. 
6. Nếu số đếm không chính xác bằng một, hãy in`-1`. 

Căn bậc 0 là không thể vì đồ thị có tính tuần hoàn. Nhiều hơn một gốc có nghĩa là công ty chia thành các bộ phận không kết nối với nhau. 
7. Ngược lại, tính tổng tất cả các giá trị hữu hạn trong`best`. 

Đây chính xác là chi phí của các cạnh giám sát đã chọn. 

### Tại sao nó hoạt động 

Bất biến quan trọng là biểu đồ ưu đãi không có tính chu kỳ vì trình độ chuyên môn giảm dần theo mọi cạnh. 

Giả sử mọi nhân viên ngoại trừ một người đều có chính xác một cạnh tới. Trong bất kỳ DAG hữu hạn nào, cấu trúc đó phải tạo thành một cây có gốc. Các chu kỳ không thể tồn tại và tất cả các nút cuối cùng đều theo dõi hướng lên tới nút duy nhất có độ bằng 0. 

Vì lựa chọn gốc cho một nhân viên không bao giờ ảnh hưởng đến tính khả thi của nhân viên khác, nên việc giảm thiểu từng cạnh đến sẽ giảm thiểu tổng chi phí toàn cầu một cách độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

n = int(input())
q = list(map(int, input().split()))

m = int(input())

best = [INF] * n

for _ in range(m):
    a, b, c = map(int, input().split())
    b -= 1
    best[b] = min(best[b], c)

roots = 0
answer = 0

for x in best:
    if x == INF:
        roots += 1
    else:
        answer += x

if roots != 1:
    print(-1)
else:
    print(answer)
```Mảng`best`lưu trữ chi phí biên đến tối thiểu cho mỗi nhân viên. Chúng ta không bao giờ cần danh sách kề vì câu trả lời cuối cùng chỉ phụ thuộc vào cạnh đến rẻ nhất trên mỗi nút. 

Các giá trị chất lượng được đọc nhưng không bao giờ được sử dụng trực tiếp. Mục đích của họ đã được mã hóa trong các đảm bảo về các cạnh. Vì mọi lời đề nghị đều đáp ứng`q[a] > q[b]`, đồ thị tự động có tính chu kỳ. 

Một lỗi phổ biến là cố chạy Kruskal hoặc Prim. Các thuật toán cây bao trùm tối thiểu giải quyết các vấn đề kết nối vô hướng, nhưng vấn đề này về cơ bản là khác nhau. Chúng ta đã biết hướng đồ thị sẽ loại bỏ chu trình, vì vậy tối thiểu hóa cục bộ độc lập là đủ. 

Một sai lầm dễ mắc phải khác là quên rằng việc đánh số nhân viên trong đầu vào là`1`-dựa trên. Mã chuyển đổi`b`ĐẾN`0`lập chỉ mục dựa trên trước khi cập nhật`best`. 

Biến`roots`đếm nhân viên không có cạnh đến. Chính xác phải có một nhân viên như vậy tồn tại. Nếu có nhiều nhân viên không có người giám sát thì họ không thể kết nối thành một hệ thống phân cấp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
7 2 3 1
4
1 2 5
2 4 1
3 4 1
1 3 5
```Các cạnh xử lý: 

| Cạnh | Đã cập nhật`best`| 
| --- | --- | 
| 1 → 2 giá 5 | [INF, 5, INF, INF] | 
| 2 → 4 giá 1 | [INF, 5, INF, 1] | 
| 3 → 4 chi phí 1 | [INF, 5, INF, 1] | 
| 1 → 3 giá 5 | [INF, 5, 5, 1] ​​| 

Quét lần cuối: 

| Nhân viên | Chi phí đầu vào tối thiểu | 
| --- | --- | 
| 1 | INF | 
| 2 | 5 | 
| 3 | 5 | 
| 4 | 1 | 

Có đúng một gốc đó nhân viên`1`. Tổng chi phí là`5 + 5 + 1 = 11`. 

Dấu vết này thể hiện ý tưởng trung tâm của giải pháp. Người lao động`4`có thể có hai người giám sát, nhưng chỉ có cạnh đầu vào rẻ hơn mới quan trọng. 

### Ví dụ không thể thực hiện được 

đầu vào:```
3
10 9 8
1
1 2 1
```Các cạnh xử lý: 

| Cạnh | Đã cập nhật`best`| 
| --- | --- | 
| 1 → 2 giá 1 | [INF, 1, INF] | 

Quét lần cuối: 

| Nhân viên | Chi phí đầu vào tối thiểu | 
| --- | --- | 
| 1 | INF | 
| 2 | 1 | 
| 3 | INF | 

Hai nhân viên không có lợi thế nào. Điều đó có nghĩa là cần có hai gốc riêng biệt, vi phạm yêu cầu phân cấp. Thuật toán in`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một lần vượt qua nhân viên và cung cấp | 
| Không gian | O(n) | Lưu trữ chi phí đầu vào tối thiểu cho mỗi nhân viên | 

Với nhiều nhất`10000`cung cấp, quét tuyến tính cực kỳ nhanh. Việc sử dụng bộ nhớ rất nhỏ vì chỉ có một mảng kích thước`n`được duy trì. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    INF = 10**18

    n = int(input())
    q = list(map(int, input().split()))

    m = int(input())

    best = [INF] * n

    for _ in range(m):
        a, b, c = map(int, input().split())
        b -= 1
        best[b] = min(best[b], c)

    roots = 0
    ans = 0

    for x in best:
        if x == INF:
            roots += 1
        else:
            ans += x

    print(ans if roots == 1 else -1)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run(
"""4
7 2 3 1
4
1 2 5
2 4 1
3 4 1
1 3 5
"""
) == "11", "sample 1"

# impossible case
assert run(
"""3
10 9 8
1
1 2 1
"""
) == "-1", "multiple roots"

# minimum size
assert run(
"""1
5
0
"""
) == "0", "single employee is root"

# choose cheapest incoming edge
assert run(
"""3
10 5 1
3
1 2 10
1 2 3
2 3 7
"""
) == "10", "minimum incoming edge"

# disconnected components
assert run(
"""4
10 9 8 7
2
1 2 1
3 4 1
"""
) == "-1", "two roots"

# chain structure
assert run(
"""5
9 8 7 6 5
4
1 2 1
2 3 2
3 4 3
4 5 4
"""
) == "10", "simple chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nhân viên độc thân, không có lợi thế | 0 | Hệ thống phân cấp một nút | 
| Nhiều cạnh vào cùng một nút | 10 | Lựa chọn cạnh đến rẻ nhất | 
| Hai chuỗi bị ngắt kết nối | -1 | Phát hiện nhiều rễ | 
| Dây chuyền đơn giản | 10 | Hệ thống phân cấp hợp lệ tiêu chuẩn | 
| Thiếu người giám sát cho một nút | -1 | Xây dựng bất khả thi | 

## Vỏ cạnh 

Hãy xem xét trường hợp có nhiều lời đề nghị nhắm đến cùng một nhân viên.```
3
10 5 1
3
1 2 10
1 2 3
2 3 7
```Người lao động`2`có chi phí đầu vào`10`Và`3`. Thuật toán chỉ lưu trữ giá trị tối thiểu,`3`. Người lao động`3`nhận được chi phí`7`. Tổng số trở thành`10`. Vì đồ thị đã có tính chất không tuần hoàn nên không bao giờ có lý do để chọn cạnh tới đắt hơn. 

Bây giờ hãy xem xét nhiều gốc rễ không thể tránh khỏi.```
4
10 9 8 7
2
1 2 1
3 4 1
```Người lao động`1`Và`3`cả hai đều không có cạnh đến. Thuật toán đếm hai nghiệm và trả về ngay`-1`. Mặc dù mọi nhân viên khác đều có người giám sát nhưng công ty vẫn chia thành hai hệ thống phân cấp độc lập. 

Đầu vào nhỏ nhất có thể cũng rất quan trọng.```
1
42
0
```Có chính xác một nhân viên và không có cạnh nào. Người nhân viên đơn độc đương nhiên trở thành người gốc rễ. Thuật toán tìm một nút không có cạnh đầu vào và tính tổng chi phí bằng 0, tạo ra`0`. 

Cuối cùng, hãy xem xét một nút không có người giám sát.```
3
10 8 6
1
1 2 5
```Người lao động`3`không bao giờ nhận được cạnh đến, vì vậy cả hai nhân viên`1`Và`3`sẽ cần phải có rễ. Thuật toán phát hiện hai nút có chi phí đầu vào và bản in vô hạn`-1`.
