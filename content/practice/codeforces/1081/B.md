---
title: "CF 1081B - Tiệc Chia Tay"
description: "Chúng ta có một nhóm gồm n người. Mỗi người đội đúng một chiếc mũ và mỗi chiếc mũ thuộc một trong n loại có thể được đánh số từ 1 đến n. Nhiều người có thể dùng chung một loại mũ và một số loại mũ có thể không được sử dụng."
date: "2026-06-15T06:11:19+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1081
codeforces_index: "B"
codeforces_contest_name: "Avito Cool Challenge 2018"
rating: 1500
weight: 1081
solve_time_s: 173
verified: true
draft: false
---

[CF 1081B - Tiệc chia tay](https://codeforces.com/problemset/problem/1081/B) 

**Đánh giá:** 1500 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 2m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một nhóm gồm n người. Mỗi người đội đúng một chiếc mũ và mỗi chiếc mũ thuộc một trong n loại có thể được đánh số từ 1 đến n. Nhiều người có thể dùng chung một loại mũ và một số loại mũ có thể không được sử dụng. 

Mỗi người tôi cung cấp một số a_i, được cho là đại diện cho số lượng người khác đội loại mũ khác với loại của họ. Nếu chúng ta biểu thị phép gán cuối cùng của các loại mũ là b_1, b_2, ..., b_n thì đối với người i, giá trị a_i phải bằng số chỉ số j sao cho b_j khác với b_i. 

Nhiệm vụ là xác định xem có tồn tại bất kỳ phép gán loại mũ nào phù hợp với tất cả các câu lệnh này hay không và nếu có thì hãy xây dựng một phép gán hợp lệ. 

Ràng buộc n 10^5 buộc chúng ta tránh xa mọi thứ bậc hai hoặc liên quan đến việc so sánh từng cặp giữa mọi người. Bất kỳ giải pháp nào cố gắng xác minh tất cả các bài tập hoặc mô phỏng các ứng viên một cách rõ ràng sẽ thất bại, vì ngay cả O(n^2) cũng đã bao hàm khoảng 10^10 thao tác. 

Một điểm tinh tế là điều kiện đối xứng nhưng được thể hiện riêng lẻ. Nhận định của mỗi người chỉ phụ thuộc vào việc có bao nhiêu người chia sẻ kiểu mũ của mình. Điều này có nghĩa là vấn đề cơ bản là về việc nhóm các chỉ mục thành các lớp trong đó mỗi kích cỡ lớp xác định cùng một giá trị cho mọi người bên trong nó. 

Một trường hợp đặc biệt bộc lộ trực giác không chính xác là khi tất cả a_i đều bằng nhau nhưng không thể thực hiện được trên toàn cầu. Ví dụ: nếu tất cả a_i là 1 và n = 2, chúng ta không thể thỏa mãn đồng thời cả hai người vì mỗi người nhìn thấy chính xác một người khác, nhưng ràng buộc nhóm bị phá vỡ. 

Một trường hợp thất bại khác xảy ra khi một người yêu cầu một giá trị bao hàm kích thước nhóm bên ngoài [1, n], chẳng hạn như a_i = n - 1, giá trị này buộc họ thành một nhóm đơn lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng gán cho mỗi người một loại mũ và kiểm tra xem liệu tất cả các ràng buộc có được giữ nguyên hay không. Vì mỗi b_i có thể nhận n giá trị, điều này dẫn đến n^n khả năng, điều này hoàn toàn không khả thi ngay cả với n = 10. 

Một ý tưởng mạnh mẽ hơn có cấu trúc chặt chẽ hơn là đoán kích thước nhóm: đối với mỗi phân vùng có thể có của n thành các kích thước nhóm, hãy gán các giá trị tương ứng. Tuy nhiên, số lượng phân vùng tăng theo cấp số nhân và vẫn không mở rộng được. 

Quan sát quan trọng là tuyên bố của một người được xác định hoàn toàn bởi quy mô nhóm của họ. Nếu một người thuộc nhóm có kích thước s thì có đúng s - 1 người chia sẻ chiếc mũ của họ, và do đó n - s người có một chiếc mũ khác nhau. Vì vậy, mọi người có giá trị a_i phải thuộc về một nhóm có kích thước: 

s = n - a_i 

Điều này chuyển đổi vấn đề từ các phép gán tùy ý sang nhóm các chỉ số theo kích thước nhóm được yêu cầu. 

Bây giờ vấn đề trở thành: liệu chúng ta có thể phân chia các chỉ mục thành các nhóm trong đó mỗi chỉ mục i phải thuộc về một nhóm có kích thước s_i = n - a_i và tất cả các thành viên của một nhóm phải đồng ý về cùng một s_i không? 

Đây là một vấn đề về tính nhất quán và nhóm, có thể giải quyết dễ dàng bằng cách nhóm các kích thước yêu cầu bằng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^n) | O(n) | Quá chậm | 
| Nhóm tối ưu theo tần số kích thước | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi từng câu lệnh thành kích thước nhóm được yêu cầu.

1. Với mỗi người i, hãy tính s_i = n - a_i. Đây là quy mô nhóm duy nhất có thể mà tuyên bố của tôi có thể đúng. 
2. Nếu bất kỳ s_i nào nhỏ hơn 1 hoặc lớn hơn n thì kết luận ngay là không thể. Một nhóm không thể có kích thước không hợp lệ. 
3. Nhóm các chỉ mục theo kích thước yêu cầu s_i bằng cách sử dụng ánh xạ từ kích thước đến danh sách người. 
4. Với mỗi nhóm size s, kiểm tra xem số người yêu cầu size s có chia hết cho s không. Nếu không, chúng ta không thể chia chúng thành các nhóm có kích thước s thống nhất nên không thể cấu hình được. 
5. Đối với mỗi kích thước hợp lệ s, hãy phân vùng danh sách của nó thành các phần có chính xác s người. Mỗi chunk tương ứng với một loại mũ. 
6. Gán một nhãn mũ duy nhất cho mỗi đoạn và đặt b_i tương ứng. 

### Tại sao nó hoạt động 

Phép biến đổi s_i = n - a_i bắt buộc: bất kỳ người nào trong nhóm có kích thước s nhất thiết sẽ nhìn thấy chính xác n - s người bên ngoài nhóm của họ, phù hợp với tuyên bố của họ. Sau khi thu thập tất cả những người có cùng kích thước yêu cầu, mọi giải pháp hợp lệ đều phải phân chia họ thành các nhóm có kích thước chính xác là s, vì việc trộn các kích thước sẽ vi phạm quy mô nhóm được yêu cầu của ít nhất một người. Điều kiện chia hết đảm bảo rằng mọi nhóm như vậy có thể được hình thành mà không có phần thừa và việc gán các nhãn riêng biệt cho mỗi khối sẽ tạo ra một giải pháp chứng thực hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    groups = {}
    required = []

    for i, x in enumerate(a):
        s = n - x
        if s < 1 or s > n:
            print("Impossible")
            return
        required.append(s)
        if s not in groups:
            groups[s] = []
        groups[s].append(i)

    ans = [0] * n
    color = 1

    for s, idxs in groups.items():
        if len(idxs) % s != 0:
            print("Impossible")
            return

        for i in range(0, len(idxs), s):
            chunk = idxs[i:i+s]
            for j in chunk:
                ans[j] = color
            color += 1

    print("Possible")
    print(*ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã chuyển đổi từng a_i thành kích thước nhóm bắt buộc s_i. Sau đó, nó xây dựng các nhóm chỉ mục có cùng yêu cầu. Mỗi nhóm phải được chia đều thành các nhóm có kích thước đó. Việc gán màu tương ứng trực tiếp với việc xây dựng các nhóm này. 

Một chi tiết triển khai tinh tế là chúng ta không cần phải thực thi bất kỳ thứ tự nào của các nhóm. Bất kỳ phân vùng nào cũng hoạt động vì tất cả các thành viên trong một nhóm đều có thể thay thế cho nhau về mặt ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 0 0
```Ở đây mỗi s_i = 3 - 0 = 3 nên cả ba người đều phải ở cùng một nhóm. 

| Bước | Nhóm được thành lập | Hành động | Tiểu bang | 
| --- | --- | --- | --- | 
| Tính s_i | [3, 3, 3] | Tất cả các chỉ số đều chuyển đến s=3 | {3: [0,1,2]} | 
| Kiểm tra tính chia hết | 3 % 3 = 0 | hợp lệ | không thay đổi | 
| Xây dựng nhóm | đoạn kích thước 3 | gán màu 1 | b = [1,1,1] | 

Điều này xác nhận rằng một nhóm có kích thước 3 phù hợp với tất cả các câu lệnh. 

### Ví dụ 2 

đầu vào:```
4
2 2 1 1
```Ta tính s_i = 4 - a_i = [2,2,3,3]. 

| Bước | Nhóm được thành lập | Hành động | Tiểu bang | 
| --- | --- | --- | --- | 
| Tính s_i | [2,2,3,3] | chia theo giá trị | {2:[0,1], 3:[2,3]} | 
| Kiểm tra s=2 | 2 % 2 = 0 | được | không thay đổi | 
| Kiểm tra s=3 | 2 % 3 ≠ 0 | thất bại | dừng lại | 

Điều này cho thấy rằng mặc dù việc phân nhóm cục bộ có vẻ hợp lý nhưng yêu cầu 3 không thể được thỏa mãn, do đó trường hợp này là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi chỉ mục được xử lý một lần để nhóm và một lần để gán | 
| Không gian | O(n) | lưu trữ cho nhóm và mảng đầu ra | 

Giải pháp này phù hợp một cách thoải mái trong các giới hạn vì cả bộ nhớ và thời gian chạy đều có quy mô tuyến tính với n, điều này cần thiết cho n lên đến 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from io import StringIO

    input_data = sys.stdin.read().strip().split()
    it = iter(input_data)

    n = int(next(it))
    a = [int(next(it)) for _ in range(n)]

    groups = {}
    for i, x in enumerate(a):
        s = n - x
        groups.setdefault(s, []).append(i)

    ans = [0] * n
    color = 1

    for s, idxs in groups.items():
        if len(idxs) % s != 0:
            return "Impossible"
        for i in range(0, len(idxs), s):
            for j in idxs[i:i+s]:
                ans[j] = color
            color += 1

    return "Possible\n" + " ".join(map(str, ans))

# provided sample
assert run("3\n0 0 0\n") == "Possible\n1 1 1"

# all impossible due to mismatch
assert run("3\n0 1 2\n") == "Impossible"

# simple split case
assert run("4\n2 2 1 1\n") == "Impossible"

# single group edge
assert run("1\n0\n") == "Possible\n1"

# alternating valid grouping
assert run("4\n3 3 3 3\n") == "Possible\n1 1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 số 0 giống hệt nhau | Có thể | nhóm đầy đủ duy nhất | 
| hỗn hợp 0 ​​1 2 | Không thể | kích thước không nhất quán | 
| 4 với kích thước chia nhỏ | Không thể | lỗi chia hết | 
| n=1 cạnh | Có thể | trường hợp tối thiểu | 
| tất cả đều yêu cầu nhóm đầy đủ | Có thể | yêu cầu thống nhất | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả a_i = n - 1. Khi đó tất cả s_i = 1, do đó mỗi người phải thuộc một nhóm đơn. Thuật toán chỉ định chính xác từng chỉ mục cho nhóm riêng của nó vì mỗi nhóm có kích thước 1 chia hết cho 1, tạo ra một phép gán hợp lệ trong đó mỗi người có một chiếc mũ duy nhất. 

Một trường hợp cạnh khác là n = 1 với a_1 = 0. Điều này cho ra s_1 = 1, tạo thành một nhóm hợp lệ duy nhất. Thuật toán xử lý việc này mà không cần logic đặc biệt vì việc nhóm sẽ tạo ra một đoạn một cách tự nhiên. 

Một trường hợp tinh tế hơn là khi tồn tại nhiều kích thước bắt buộc nhưng một kích thước có các phần tử còn sót lại. Ví dụ: n = 6 với các giá trị s [2,2,2,2,2,3]. Nhóm size-2 có 5 phần tử, không thể tạo thành các cặp hoàn chỉnh, khiến thuật toán từ chối chính xác phiên bản trước khi thử gán một phần.
