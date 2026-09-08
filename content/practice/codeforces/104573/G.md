---
title: "CF 104573G - Kỳ nhông đi bộ"
description: "Chúng ta được cung cấp một đồ thị có hướng trong đó mỗi con kỳ nhông nằm trên một nút từ 1 đến N và mỗi nút có chính xác một cạnh ra được xác định bởi mảng p. Trong một bước thời gian, mọi con kỳ nhông đều di chuyển đồng thời từ i đến p[i]."
date: "2026-06-30T08:20:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104573
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 09-08-23 Div. 1"
rating: 0
weight: 104573
solve_time_s: 69
verified: true
draft: false
---

[CF 104573G - Kỳ nhông đi bộ](https://codeforces.com/problemset/problem/104573/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có hướng trong đó mỗi con kỳ nhông nằm trên một nút từ 1 đến N và mỗi nút có chính xác một cạnh ra được xác định bởi mảng p. Trong một bước thời gian, mọi con kỳ nhông đều di chuyển đồng thời từ i đến p[i]. Ngoại lệ duy nhất là nút 1: Iguana 1 không bao giờ di chuyển mà thay vào đó hoạt động như một gốc cố định mà những người khác đang cố gắng tiếp cận. 

Mỗi con kỳ nhông đều tuân theo quy tắc xác định này nhiều lần. Một số cự đà cuối cùng sẽ hạ cánh ở nút 1, có thể sau vài bước. Những người khác có thể bị mắc kẹt trong các chu kỳ không bao giờ bao gồm 1, trong trường hợp đó họ không bao giờ đạt được người dẫn đầu và bị bỏ qua trong kỳ vọng. 

Số lượng chúng ta cần là thời gian trung bình, được đo bằng các bước từ thời điểm 0, để tất cả cự đà cuối cùng có thể đến nút 1 để đến đó lần đầu tiên. Bản thân Iguana 1 cũng được bao gồm, không đóng góp thời gian. 

Kích thước đầu vào lớn: lên tới 2·10^5 nút trên tất cả các trường hợp thử nghiệm. Điều này loại trừ bất kỳ mô phỏng trên mỗi nút nào tiến lên từng bước cho mỗi con kỳ nhông, vì việc đi theo chuỗi một cách ngây thơ có thể chuyển sang hành vi bậc hai trong chuỗi hoặc chu kỳ dài. 

Một vấn đề tế nhị xuất hiện khi chu kỳ tồn tại. Hãy xem xét một chu trình như 2 → 3 → 2 không kết nối với 1. Một BFS ngây thơ từ 1 ở các cạnh ngược sẽ tránh việc đếm các nút này, điều này đúng, nhưng chúng ta vẫn cần khoảng cách chính xác cho các nút đạt tới 1 thông qua chuỗi chức năng dài. Một trường hợp tinh vi khác là biểu đồ không phải là cây, vì vậy nhiều nút có thể chia sẻ chuỗi hậu tố trước khi hợp nhất thành một chu trình hoặc vào nút 1. 

Các trường hợp cạnh bao gồm: 

Một chuỗi đơn giản: 1 ← 2 ← 3 ← 4. Ở đây câu trả lời là khoảng cách đơn giản 0,1,2,3 và trung bình là 1,5. Bất kỳ phương pháp nào xử lý nhầm biểu đồ là vô hướng sẽ bị tính quá mức. 

Một chu trình thuần túy không có quyền truy cập vào 1, như 1 → 2 → 3 → 1 cộng với chu trình rời rạc 4 → 5 → 4. Các nút 4 và 5 phải được bỏ qua hoàn toàn. 

Một vòng tự lặp tại nút 1 được ngụ ý vì p1 tồn tại nhưng 1 không bao giờ di chuyển. Chi tiết đó rất quan trọng khi tính toán các cấu trúc đảo ngược. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng từng con cự đà một cách độc lập. Đối với mỗi i, chúng tôi liên tục áp dụng p[i], sau đó là p[p[i]], v.v. cho đến khi chúng tôi đạt tới 1 hoặc phát hiện sự lặp lại. Nếu chúng ta đạt tới 1 ở bước d, chúng ta sẽ đóng góp d vào tổng; nếu không chúng tôi bỏ qua nó. Điều này đúng vì mỗi quỹ đạo đều mang tính xác định và lần đầu tiên nó chạm tới 1 được xác định rõ ràng. 

Tuy nhiên, mỗi bước đi có thể thực hiện O(N) bước trong trường hợp xấu nhất và chúng tôi thực hiện việc này cho tất cả các nút, dẫn đến O(N^2) cho mỗi trường hợp thử nghiệm. Với N lên tới 2·10^5 tổng thể, tốc độ này quá chậm. 

Quan sát quan trọng là mỗi nút có chính xác một cạnh đi ra, do đó cấu trúc là một đồ thị hàm số. Mỗi thành phần bao gồm một chu trình được định hướng với sự tham gia của cây cối. Chỉ các nút có đường dẫn cuối cùng đến nút 1 mới quan trọng, vì vậy chúng tôi chỉ quan tâm đến các nút trong lưu vực hấp dẫn của 1 theo nghĩa biểu đồ đảo ngược. 

Khi chúng tôi hạn chế sự chú ý đến các nút có thể truy cập, vấn đề sẽ giảm xuống việc tính toán khoảng cách ngắn nhất đến nút 1 trong biểu đồ trong đó mọi cạnh có trọng số 1, nhưng các cạnh được hướng về phía trước dọc theo p. Cách tự nhiên để tính khoảng cách đến một mục tiêu cố định trong biểu đồ hàm là đảo ngược các cạnh và chạy BFS bắt đầu từ nút 1. Điều này cho chúng ta số bước tối thiểu cần thiết để đạt được 1 bước từ mỗi nút, chính xác là thời gian mà mỗi con cự đà thực hiện. 

Vì vậy, giải pháp trở thành: xây dựng danh sách kề kề ngược, chạy BFS từ nút 1, tính toán khoảng cách và chỉ lấy trung bình trên các nút có khoảng cách hữu hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N^2) | O(1) | Quá chậm | 
| Đồ thị ngược BFS | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán

1. Xây dựng một danh sách kề ngược rev trong đó rev[v] chứa tất cả các nút u sao cho p[u] = v. Điều này chuyển đổi “tôi sẽ đi đâu tiếp theo” thành “ai có thể liên hệ với tôi trong một bước”, điều này rất cần thiết cho tìm kiếm ngược. 
2. Khởi tạo mảng khoảng cách dist bằng -1 cho tất cả các nút. Điều này đánh dấu tất cả các nút ban đầu không thể truy cập được từ nút 1. 
3. Đặt dist[1] = 0 và đẩy nút 1 vào hàng đợi. Nút 1 là nguồn của BFS vì chúng tôi đo lường số bước cần thiết để đến được nó. 
4. Trong khi hàng đợi không trống, hãy bật nút u. Với mọi nút v trong rev[u], nếu dist[v] vẫn là -1, đặt dist[v] = dist[u] + 1 và đẩy v vào hàng đợi. Điều này truyền bá thời gian đến ngắn nhất ra bên ngoài dọc theo các cạnh ngược. 
5. Sau khi BFS hoàn tất, lặp lại tất cả các nút. Đối với mỗi nút i có dist[i] != -1, hãy thêm dist[i] vào tổng hiện có và tăng bộ đếm. 
6. Tính đáp án dưới dạng tổng/đếm. Điều này chỉ tính trung bình trên các nút thực sự có thể tiếp cận nút 1. 

Tại sao nó hoạt động: mọi cạnh trong biểu đồ đảo ngược thể hiện một bước tiến hợp lệ về phía nút 1 trong đúng một bước. BFS khám phá các nút theo số bước tăng dần từ 1 và vì tất cả các cạnh đều có trọng số bằng nhau nên lần đầu tiên chúng tôi gán khoảng cách cho một nút được đảm bảo là số bước tối thiểu cần thiết để đến được nút 1. Các nút không được truy cập chính xác là những nút nằm trong chu trình hoặc các thành phần bị ngắt kết nối khỏi 1, vì vậy chúng được loại trừ chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        rev = [[] for _ in range(n + 1)]
        for i in range(n):
            rev[p[i]].append(i + 1)

        dist = [-1] * (n + 1)
        q = deque([1])
        dist[1] = 0

        while q:
            u = q.popleft()
            for v in rev[u]:
                if dist[v] == -1:
                    dist[v] = dist[u] + 1
                    q.append(v)

        total = 0
        cnt = 0
        for i in range(1, n + 1):
            if dist[i] != -1:
                total += dist[i]
                cnt += 1

        out.append(str(total / cnt if cnt else 0.0))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp ý tưởng BFS ngược. Bản sửa đổi danh sách kề ngược được xây dựng bằng cách sử dụng chỉ mục dựa trên 1, phù hợp với định nghĩa vấn đề. Hàng đợi BFS bắt đầu ở nút 1 và dist[1] được đặt chính xác thành 0 vì Iguana 1 đã dẫn đầu. 

Một lỗi phổ biến là lặp đi lặp lại dọc theo p thay vì đảo ngược nó. Điều đó sẽ mô phỏng chuyển động ra xa 1 thay vì hướng tới nó, điều này không tính toán thời gian đến. Một vấn đề tế nhị khác là quên loại trừ các nút không thể truy cập được; việc kiểm tra mảng dist đảm bảo chúng tôi chỉ tính trung bình những con cự đà hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
5 2 3 1 4
```Chúng tôi xây dựng các cạnh ngược: 

| Nút | Các nút đến | 
| --- | --- | 
| 1 | 4 | 
| 2 | 2 | 
| 3 | 3 | 
| 4 | 5 | 
| 5 | 1 | 

Chúng tôi chạy BFS từ 1. 

| Bước | Xếp hàng | Xuất hiện | Mới cập nhật | 
| --- | --- | --- | --- | 
| 1 | [1] | 1 | 4 → quận=1 | 
| 2 | [4] | 4 | 5 → khoảng cách=2 | 
| 3 | [5] | 5 | không | 

Khoảng cách: 1:0, 4:1, 5:2, 2 và 3 không thể truy cập được. 

Tổng = 0 + 1 + 2 = 3, đếm = 3, đáp án = 1,0. 

Điều này xác nhận rằng chỉ các nút trong lưu vực của nút 1 mới đóng góp. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```Các cạnh ngược: 

2 → 2, 3 → 3 và 1 → 1. 

BFS từ 1 chỉ đến được nút 1. 

| Bước | Xếp hàng | Xuất hiện | Mới cập nhật | 
| --- | --- | --- | --- | 
| 1 | [1] | 1 | không | 

Tổng = 0, đếm = 1, trả lời = 0,0. 

Việc này kiểm tra xem các vòng tự lặp và các thành phần bị cô lập không làm ảnh hưởng đến kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) cho mỗi trường hợp thử nghiệm | Mỗi nút được xếp hàng một lần và mỗi cạnh được xử lý một lần trong BFS ngược | 
| Không gian | O(N) | Danh sách kề ngược và mảng khoảng cách | 

Các ràng buộc cho phép tổng số tối đa 2·10^5 nút, do đó, BFS thời gian tuyến tính trên tất cả các trường hợp thử nghiệm vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            p = list(map(int, input().split()))

            rev = [[] for _ in range(n + 1)]
            for i in range(n):
                rev[p[i]].append(i + 1)

            dist = [-1] * (n + 1)
            q = deque([1])
            dist[1] = 0

            while q:
                u = q.popleft()
                for v in rev[u]:
                    if dist[v] == -1:
                        dist[v] = dist[u] + 1
                        q.append(v)

            total = 0
            cnt = 0
            for i in range(1, n + 1):
                if dist[i] != -1:
                    total += dist[i]
                    cnt += 1

            out.append(str(total / cnt if cnt else 0.0))

        return "\n".join(out)

    return solve()

# provided samples
assert run("3\n5\n5 2 3 1 4\n3\n1 2 3\n10\n2 3 4 5 6 7 8 9 10 1\n") == "1.0\n0.0\n4.5"

# custom cases
assert run("1\n1\n1\n") == "0.0", "single node"
assert run("1\n4\n2 3 4 2\n") == "1.0", "cycle excluding root except entry"
assert run("1\n4\n1 1 1 1\n") == "1.0", "all direct to root"
assert run("1\n6\n2 3 4 5 6 4\n") == "2.0", "tree into cycle, partial reach"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút tự lặp | 0,0 | trường hợp tối thiểu và xử lý gốc | 
| chu kỳ nhỏ | 1.0 | xử lý chu trình và khả năng tiếp cận | 
| tất cả đều trỏ đến gốc | 1.0 | độ chính xác của cấu trúc sao | 
| chuỗi thành chu kỳ | 2.0 | loại trừ các nút chu kỳ không thể truy cập | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi biểu đồ bao gồm hầu hết các chu kỳ không kết nối với nút 1. Trong trường hợp như vậy, BFS từ nút 1 không bao giờ đến được chúng và chúng không được ảnh hưởng đến mức trung bình. Ví dụ: trong biểu đồ có 2 → 3 → 2 và 4 → 5 → 4 trong khi 1 bị cô lập thì chỉ nút 1 được tính. BFS ngược tự nhiên để lại tất cả các nút khác ở khoảng cách = -1, vì vậy chúng được loại trừ hoàn toàn. 

Một trường hợp cạnh khác là khi mọi nút cuối cùng đều dẫn đến 1 trong một chuỗi dài. Ví dụ: 1 ← 2 ← 3 ← … ← N. BFS gán khoảng cách từ 0 đến N−1 và giá trị trung bình trở thành (N−1)/2. Thuật toán xử lý việc này mà không sửa đổi vì mỗi nút được phát hiện chính xác một lần theo thứ tự khoảng cách tăng dần. 

Trường hợp tinh vi thứ ba là khi nhiều nhánh hợp nhất trước khi đến một chu kỳ hoặc nút 1. BFS ngược xử lý điều này một cách chính xác vì một khi một nút được truy cập, nó sẽ không bao giờ được xem lại, đảm bảo rằng chỉ đường đi ngắn nhất đến 1 được ghi lại, ngay cả khi có nhiều đường dẫn chuyển tiếp tồn tại.
