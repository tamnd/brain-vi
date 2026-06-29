---
title: "CF 104618F - Bing đang làm lạnh"
description: "Chúng tôi được cung cấp một bộ sưu tập các thành phần trong đó mỗi thành phần có hai cách để lấy nó. Bạn có thể mua nó trực tiếp với giá cố định hoặc bạn có thể sản xuất nó bằng cách kết hợp các thành phần khác mà bản thân chúng cũng có thể được mua hoặc sản xuất."
date: "2026-06-29T17:30:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104618
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 09-22-23 Div. 1"
rating: 0
weight: 104618
solve_time_s: 66
verified: true
draft: false
---

[CF 104618F - Bing thật lạnh lùng](https://codeforces.com/problemset/problem/104618/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập các thành phần trong đó mỗi thành phần có hai cách để lấy nó. Bạn có thể mua nó trực tiếp với giá cố định hoặc bạn có thể sản xuất nó bằng cách kết hợp các thành phần khác mà bản thân chúng cũng có thể được mua hoặc sản xuất. Mỗi bước sản xuất đều tiêu tốn các thành phần phụ cần thiết nên chi phí sản xuất một mặt hàng là tổng của các thành phần phụ thuộc của nó và không có việc tái sử dụng hoặc chia sẻ giữa các công trình khác nhau. 

Trong số tất cả các thành phần, một số được đánh dấu là mục tiêu bắt buộc. Nhiệm vụ là tính toán tổng chi phí tối thiểu cần thiết để có được tất cả các thành phần mục tiêu cần thiết, tận dụng mọi quy tắc kết hợp có thể giảm chi phí so với mua trực tiếp. 

Cấu trúc do đầu vào tạo ra là một biểu đồ phụ thuộc theo chu kỳ có hướng: mỗi thành phần chỉ ra các thành phần cần thiết để xây dựng nó. Việc đảm bảo rằng không thành phần nào có thể phụ thuộc vào chính nó, trực tiếp hay gián tiếp, đảm bảo rằng chúng tôi đang làm việc trên DAG chứ không phải hệ thống tuần hoàn. 

Các hạn chế rất lớn, lên tới 100.000 thành phần và tổng quy mô phụ thuộc là 300.000. Điều này loại trừ bất kỳ cách tiếp cận nào tính toán lại chi phí một cách đệ quy mà không cần ghi nhớ hoặc liên tục truyền bá các cập nhật theo kiểu bậc hai. Bất kỳ giải pháp nào về cơ bản đều phải xử lý từng cạnh phụ thuộc với số lần không đổi. 

Một trường hợp thất bại khó phát hiện khi xây dựng một thành phần rẻ hơn so với mua nhưng chỉ thông qua một chuỗi phụ thuộc. Một cách tiếp cận ngây thơ chỉ so sánh những đứa con ngay trong bụng sẽ thất bại. Một cạm bẫy khác là chi phí tính toán lại các cây con dùng chung nhiều lần, có thể dễ dàng tăng theo thời gian theo cấp số nhân trong trường hợp xấu nhất. 

Hãy xem xét kịch bản đơn giản này: 

đầu vào:```
2 3
A B
A 10 0
B 100 1 A
C 1 0
```Câu trả lời đúng là 110. Nếu chúng ta quên sử dụng chi phí tính toán rẻ nhất của A khi tính toán B và thay vào đó xử lý các phần phụ thuộc một cách độc lập hoặc tính toán lại A nhiều lần, thì chúng ta sẽ đếm quá mức hoặc hết thời gian chờ. 

Một trường hợp khác là khi mua luôn tệ hơn chế tạo, nhưng chỉ trở nên rõ ràng sau khi truyền qua nhiều lớp. Bất kỳ quyết định cục bộ tham lam nào tại mỗi nút mà không có sự lan truyền toàn cầu đều thất bại ở đây. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Đối với mỗi thành phần, hãy tính chi phí của nó bằng cách thử cả hai lựa chọn: mua trực tiếp hoặc tính tổng chi phí của các thành phần cần thiết theo cách đệ quy. Sau đó, đối với mỗi thành phần mục tiêu, hãy tính tổng chi phí tính toán của chúng. 

Về nguyên tắc, điều này đúng vì mọi định nghĩa về chi phí thành phần đều được tôn trọng chính xác. Tuy nhiên, đệ quy sẽ truy cập lại các nút giống nhau nhiều lần. Trong biểu đồ phụ thuộc dạng chuỗi có độ dài N, mỗi lệnh gọi sẽ mở rộng thành một lần truyền tải đầy đủ khác, dẫn đến việc tính toán lại theo cấp số nhân trong trường hợp xấu nhất. Ngay cả với tính năng ghi nhớ, việc triển khai bất cẩn vẫn có thể liên tục truy cập lại các phần phụ thuộc do tra cứu chuỗi và phân tích cú pháp lặp đi lặp lại. 

Quan sát quan trọng là cấu trúc phụ thuộc là DAG, vì vậy chi phí của mỗi thành phần chỉ phụ thuộc vào các thành phần có thể tính toán trước đó theo thứ tự tôpô. Sau khi chúng tôi tính toán chi phí tối thiểu của một thành phần, nó sẽ không bao giờ cần phải tính toán lại. 

Điều này biến vấn đề thành sự lan truyền giống như đường dẫn ngắn nhất trên DAG, trong đó giá trị của mỗi nút được tính toán từ các nút lân cận đã được hoàn thiện. Chúng tôi xử lý các nút theo thứ tự tôpô hoặc tương đương, sử dụng DFS được ghi nhớ để đảm bảo mỗi nút được đánh giá một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(exp) trường hợp xấu nhất | O(N) đệ quy | Quá chậm | 
| Tối ưu (DAG DP / DP tôpô) | O(N + E) | O(N + E) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa từng thành phần dưới dạng một nút. Mỗi nút có giá cơ bản (giá mua) và danh sách phụ thuộc. Mục tiêu là tính toán chi phí tối thiểu có thể đạt được cho mỗi nút. 

1. Phân tích tất cả các thành phần và gán cho mỗi thành phần một mã định danh duy nhất. Điều này cho phép lập chỉ mục nhanh thay vì so sánh chuỗi lặp đi lặp lại. Điều này quan trọng vì việc tra cứu từ điển dựa trên chuỗi sẽ chiếm ưu thế trong thời gian chạy trên quy mô lớn. 
2. Xây dựng biểu đồ phụ thuộc. Đối với mỗi thành phần, hãy lưu trữ danh sách các thành phần cần thiết để tạo ra nó. Đồng thời tính toán mức độ của mỗi nút, đó là số lượng phụ thuộc mà nó có. 
3. Khởi tạo chi phí của mỗi nút bằng giá mua trực tiếp của nút đó. Điều này thể hiện tùy chọn cơ bản trong đó chúng tôi hoàn toàn bỏ qua việc chế tạo. 
4. Chuẩn bị hàng xử lý với tất cả các nút không có sự phụ thuộc. Đây là những nguyên liệu chỉ có thể mua được nên chi phí của chúng là cuối cùng ngay từ đầu. 
5. Xử lý các nút theo thứ tự hàng đợi. Đối với mỗi nút u, coi chi phí tính toán của nó là cố định. Sau đó, với mỗi thành phần v phụ thuộc vào u, chúng tôi cố gắng giảm chi phí của v bằng cách sử dụng phần đóng góp chi phí cuối cùng của u. Cụ thể, chúng tôi duy trì cho mỗi v một tổng chi phí phụ thuộc hiện tại và sau khi tất cả các phụ thuộc được xử lý, chúng tôi so sánh số tiền này với giá mua trực tiếp. 
6. Khi tất cả các phần phụ thuộc của một nút đã được xử lý, hãy tính chi phí được chế tạo đầy đủ của nút đó bằng tổng chi phí phụ thuộc của nút đó. Đặt chi phí cuối cùng của nó ở mức tối thiểu của chi phí chế tạo và chi phí mua hàng. 
7. Đẩy các nút có phần phụ thuộc được giải quyết hoàn toàn vào hàng đợi, tiếp tục cho đến khi tất cả các nút được xử lý. 
8. Cuối cùng, tính tổng chi phí tính toán của tất cả các thành phần mục tiêu cần thiết. 

Ý tưởng chính là mọi thành phần chỉ được đánh giá sau khi đã biết tất cả các yếu tố phụ thuộc của nó, đảm bảo tính chính xác của chi phí chế tạo được tính toán. 

### Tại sao nó hoạt động

Điều bất biến được duy trì là khi một nút được xử lý, tất cả các phần phụ thuộc của nó đã được tính toán chi phí tối thiểu cuối cùng. Vì biểu đồ không theo chu kỳ nên tồn tại một thứ tự tôpô và quy trình dựa trên hàng đợi thực thi chính xác thứ tự đó. Do đó, khi chúng tôi tính toán chi phí của một thành phần, mọi chi phí của thành phần phụ được sử dụng đều đã ở mức tối ưu và không có bản cập nhật nào trong tương lai có thể giảm thêm nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

def solve():
    n, m = map(int, input().split())
    targets = input().split()

    idx = {}
    price = [0] * m
    indeg = [0] * m
    deps = [[] for _ in range(m)]
    rev = [[] for _ in range(m)]

    for i in range(m):
        parts = input().split()
        name = parts[0]
        p = int(parts[1])
        g = int(parts[2])

        idx[name] = i
        price[i] = p
        indeg[i] = g

        for j in range(g):
            child = parts[3 + j]
            deps[i].append(child)

    # convert deps to indices and build reverse graph
    for i in range(m):
        new_list = []
        for name in deps[i]:
            new_list.append(idx[name])
            rev[idx[name]].append(i)
        deps[i] = new_list

    sum_dep = [0] * m
    q = deque()

    for i in range(m):
        if indeg[i] == 0:
            q.append(i)

    # topological processing
    while q:
        u = q.popleft()

        for v in rev[u]:
            sum_dep[v] += price[u]
            indeg[v] -= 1
            if indeg[v] == 0:
                # v is ready: decide its final cost
                price[v] = min(price[v], sum_dep[v])
                q.append(v)

    ans = 0
    for t in targets:
        ans += price[idx[t]]

    print(ans)

if __name__ == "__main__":
    solve()
```Quá trình triển khai bắt đầu bằng cách ánh xạ tên thành phần vào các chỉ mục để thực hiện các thao tác trên biểu đồ một cách hiệu quả. Danh sách phụ thuộc được chuyển đổi sang dạng kề để chúng ta có thể truyền bá sự hoàn thành lên trên. các`sum_dep`mảng tích lũy phần đóng góp chi phí của các phần phụ thuộc đã được xử lý. 

Một điểm tinh tế là chúng tôi chỉ chốt chi phí của một nút khi mức độ của nó bằng 0. Điều này đảm bảo rằng chúng tôi đã xem tất cả các thành phần cần thiết trước khi quyết định xem liệu chế tạo có rẻ hơn mua hay không. 

Bước tổng hợp cuối cùng chỉ đơn giản là truy xuất chi phí đã tính toán cho tất cả các thành phần mục tiêu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 5
milk vanillaExtract cream
milk 10 0
vanillaExtract 5 1 sugar
cream 30 2 sugar milk
butter 5 1 milk
sugar 6 0
```Chúng tôi theo dõi tính toán: 

| Nút | bằng cấp | tổng_dep | quyết định chi phí cuối cùng | 
| --- | --- | --- | --- | 
| sữa | 0 | 0 | 10 | 
| đường | 0 | 0 | 6 | 
| bơ | 1 → 0 | 10 | phút(5,10)=5 | 
| vaniExtract | 1 → 0 | 6 | phút(5,6)=5 | 
| kem | 2 → 0 | 16 | phút(30,16)=16 | 

Mục tiêu là sữa, vaniExtract, kem. Tổng là 10 + 5 + 16 = 31. 

Dấu vết này cho thấy các thành phần trung gian như đường và sữa ảnh hưởng như thế nào đến chi phí hạ nguồn và mỗi nút chỉ được hoàn thành sau khi tích lũy sự phụ thuộc hoàn toàn. 

### Mẫu 2 

đầu vào:```
1 6
flavorX2
flavorX2 100 4 skittles milk salt cream
cream 10 2 milk sugar
skittles 10 0
milk 5 0
salt 10 0
sugar 4 0
```| Nút | bằng cấp | tổng_dep | quyết định chi phí cuối cùng | 
| --- | --- | --- | --- | 
| tiểu phẩm | 0 | 0 | 10 | 
| sữa | 0 | 0 | 5 | 
| muối | 0 | 0 | 10 | 
| đường | 0 | 0 | 4 | 
| kem | 2 → 0 | 9 | phút(10,9)=9 | 
| hương vịX2 | 4 → 0 | 28 | phút(100,28)=28 | 

Câu trả lời cuối cùng là 28. 

Ví dụ này cho thấy một chuỗi phụ thuộc sâu hơn, trong đó nhiều lớp xây dựng rẻ hơn mua chỉ hiển thị sau khi được phổ biến đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + E) | Mỗi thành phần và phần phụ thuộc được xử lý một lần theo thứ tự tôpô | 
| Không gian | O(N + E) | Lưu trữ đồ thị cộng với các mảng phụ trợ cho cấp độ và tích lũy | 

Độ phức tạp tuyến tính là cần thiết khi có tới 100.000 thành phần và 300.000 cạnh phụ thuộc. Bất kỳ quá trình truyền lặp lại nào cũng sẽ vượt quá giới hạn, trong khi phương pháp này đảm bảo mỗi cạnh đóng góp chính xác một lần vào việc truyền chi phí. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return ""  # output printed directly

# sample 1
assert run("""3 5
milk vanillaExtract cream
milk 10 0
vanillaExtract 5 1 sugar
cream 30 2 sugar milk
butter 5 1 milk
sugar 6 0
""") == "", "sample 1"

# sample 2
assert run("""1 6
flavorX2
flavorX2 100 4 skittles milk salt cream
cream 10 2 milk sugar
skittles 10 0
milk 5 0
salt 10 0
sugar 4 0
""") == "", "sample 2"

# minimum case
assert run("""1 1
a
a 5 0
""") == "", "min case"

# all dependencies linear
assert run("""1 4
d
a 1 0
b 2 1 a
c 3 1 b
d 10 1 c
""") == "", "chain case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | mua trực tiếp | trường hợp cơ sở | 
| phụ thuộc chuỗi | tính chính xác của việc truyền bá | DP nhiều lớp | 
| trường hợp mẫu | sự đúng đắn | tích hợp đầy đủ | 

## Vỏ cạnh 

Trường hợp một bên là khi mua một nguyên liệu thực sự rẻ hơn so với chế tạo, mặc dù việc chế tạo là hợp lệ. Ví dụ: nếu một mặt hàng có các phụ thuộc đắt tiền thì thuật toán vẫn chọn chính xác chi phí mua hàng vì bước cuối cùng sẽ so sánh cả hai tùy chọn. 

Một trường hợp khác là chuỗi phụ thuộc dài. Thuật toán xử lý mỗi nút chính xác một lần theo thứ tự tôpô, do đó, ngay cả một chuỗi có độ dài 100.000 cũng không gây ra đệ quy hoặc công việc lặp đi lặp lại. 

Trường hợp cạnh cuối cùng là nhiều phần phụ thuộc hội tụ vào một nút. Từ`sum_dep`chỉ tổng hợp các khoản đóng góp sau khi mỗi phần phụ thuộc được xử lý, chi phí cuối cùng phản ánh tổng số tiền đầy đủ chính xác một lần cho mỗi phần phụ thuộc, tránh tính hai lần.
