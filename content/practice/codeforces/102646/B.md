---
title: "CF 102646B - Kết hợp mảng"
description: "Nhiệm vụ là hợp nhất hai hàng số thành một mảng. Thao tác duy nhất được phép là lấy phần tử đầu tiên hiện tại từ một trong hai hàng đợi và thêm nó vào câu trả lời. Thứ tự bên trong của mỗi mảng ban đầu không thể thay đổi."
date: "2026-07-30T23:11:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102646
codeforces_index: "B"
codeforces_contest_name: "Testing Round #XVII"
rating: 0
weight: 102646
solve_time_s: 114
verified: true
draft: false
---

[CF 102646B - Kết hợp mảng](https://codeforces.com/problemset/problem/102646/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là hợp nhất hai hàng số thành một mảng. Thao tác duy nhất được phép là lấy phần tử đầu tiên hiện tại từ một trong hai hàng đợi và thêm nó vào câu trả lời. Thứ tự bên trong của mỗi mảng ban đầu không thể thay đổi. Trong số tất cả các mảng có thể được hợp nhất, chúng ta cần một mảng nhỏ nhất về mặt từ điển. Hai mảng đầu vào cùng chứa mọi số từ`1`ĐẾN`2n`chính xác một lần, có nghĩa là mỗi giá trị là duy nhất. 

Khó khăn chính là phần tử đầu tiên nhỏ hơn cục bộ không phải lúc nào cũng đủ. Nếu cả hai mảng hiện có giá trị lớn thì lựa chọn đúng sẽ phụ thuộc vào việc so sánh các hậu tố còn lại của cả hai mảng. Ví dụ như việc chọn`5`qua`6`không nhất thiết phải tối ưu nếu mảng bắt đầu bằng`6`tiếp tục với những giá trị nhỏ hơn nhiều. 

Độ dài của mỗi mảng có thể đạt tới`100000`, vậy tổng số phần tử là`200000`. Một cách tiếp cận thử tất cả các phép hợp nhất có thể là không thể vì số lượng các phép xen kẽ hợp lệ là theo cấp số nhân. Ngay cả việc so sánh các hậu tố với từng ký tự mỗi lần cũng có thể chuyển thành hành vi bậc hai, quá chậm đối với kích thước đầu vào này. 

Các trường hợp cạnh quan trọng đến từ việc so sánh hậu tố. Nếu hết một mảng thì mảng còn lại phải được sao chép vì không còn lựa chọn nào khác. Ví dụ:```
1
A = [3]
B = [1]
```Câu trả lời là:```
1 3
```Một giải pháp tham lam chỉ kiểm tra các phần tử đầu tiên hoạt động ở đây nhưng không thành công trong các trường hợp phức tạp hơn. 

Một trường hợp khác là khi các phần tử đầu tiên bằng nhau trong các bài toán chuỗi thông thường. Ở đây, thuộc tính hoán vị ngăn các giá trị bằng nhau giữa hai mảng, nhưng giải pháp giả định rằng nó luôn có thể so sánh chỉ một phần tử sẽ không thành công:```
n = 3
A = [6, 10, 4]
B = [9, 8, 11]
```Câu trả lời đúng bắt đầu bằng`6`, vì hậu tố`[6,10,4]`nhỏ hơn`[9,8,11]`. Quyết định không thể được đưa ra bằng cách xem xét các yếu tố sau này sau khi chọn sai. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là tạo ra mọi chuỗi lựa chọn có thể có. Tại mọi thời điểm chúng ta có thể lấy từ`A`hoặc từ`B`, vậy số lần kết hợp có thể là:$$\binom{2n}{n}$$Vì`n = 100000`, điều này vượt xa mọi giới hạn thực tế. Ngay cả việc lưu trữ một phần nhỏ những khả năng đó cũng là không thể. 

Một ý tưởng tham lam hợp lý hơn là luôn chọn phần tử phía trước hiện tại nhỏ hơn. Điều này là không đủ. Hãy xem xét điểm quyết định trong đó các mảng còn lại là:```
A = [4, 100]
B = [5, 1]
```Lấy`4`đầu tiên có vẻ đúng, nhưng nếu chúng ta có:```
A = [4, 100]
B = [4?]
```chúng ta sẽ cần phải so sánh các phần dài hơn. Trong bài toán này, các giá trị bằng nhau không thể xảy ra, nhưng nguyên tắc vẫn giữ nguyên: lựa chọn tiếp theo phụ thuộc vào toàn bộ hậu tố còn lại. 

Quan sát quan trọng là tại bất kỳ thời điểm nào, bước đi tối ưu là lấy hậu tố còn lại nhỏ hơn về mặt từ điển. Nếu như`A[i:] < B[j:]`, lấy từ`A`luôn an toàn vì mọi yếu tố trong tương lai của`A`đứng trước các phần tử còn lại của`B`cho đến khi chúng ta quyết định khác. Nếu chúng ta lấy từ hậu tố lớn hơn trước, thì vị trí đầu tiên mà hai hậu tố khác nhau sẽ đưa ra câu trả lời tệ hơn ngay lập tức. 

Thử thách còn lại là so sánh các hậu tố một cách hiệu quả. Chúng tôi ghép hai mảng với giá trị phân tách nhỏ hơn mọi phần tử và xây dựng thứ hạng cho tất cả các hậu tố. Việc tăng gấp đôi tiền tố mang lại những thứ hạng này trong`O(n log n)`thời gian. Sau đó, mỗi so sánh hậu tố sẽ trở thành so sánh xếp hạng theo thời gian không đổi. Việc nhân đôi tiền tố sẽ xây dựng thứ hạng từ điển của các tiền tố ngày càng dài hơn cho đến khi mỗi hậu tố có một thứ hạng duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(2n,n)) | O(2n) | Quá chậm | 
| So sánh hậu tố trực tiếp | O(n²) | O(1) | Quá chậm | 
| Cấp bậc hậu tố + hợp nhất tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một mảng kết hợp`S = A + [0] + B`. giá trị`0`hoạt động như một dấu phân cách vì tất cả các giá trị thực đều dương. Chúng ta chỉ sử dụng mảng này để so sánh các hậu tố chứ không dùng để tạo ra câu trả lời. 
2. Tính thứ hạng từ điển của mỗi hậu tố của`S`sử dụng nhân đôi tiền tố. Mỗi cấp bậc cho chúng ta biết hậu tố nào nhỏ hơn. 
3. Giữ hai con trỏ,`i`cho vị trí hiện tại trong`A`Và`j`cho vị trí hiện tại trong`B`. 
4. Trong khi cả hai mảng vẫn còn các phần tử, hãy so sánh thứ hạng của các hậu tố bắt đầu từ`i`TRONG`A`Và`j`TRONG`B`. Nối phần tử đầu tiên của hậu tố nhỏ hơn và di chuyển con trỏ của nó về phía trước. 
5. Khi một mảng trống, hãy nối trực tiếp các phần tử còn lại của mảng kia. Không còn quyết định nào để đưa ra nữa. 

Lý do lựa chọn tham lam có tác dụng là vì vị trí khác nhau đầu tiên giữa hai hậu tố ứng cử viên sẽ xác định thứ tự từ điển của mỗi lần hợp nhất bắt đầu bằng các hậu tố đó. Việc chọn hậu tố nhỏ hơn sẽ bảo toàn vị trí chưa quyết định đầu tiên tốt nhất có thể và việc lặp lại đối số này sau khi loại bỏ một phần tử chứng tỏ rằng mọi phần tử được chọn đều là một phần của sự hợp nhất tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_ranks(arr):
    n = len(arr)
    vals = {v: i for i, v in enumerate(sorted(set(arr)))}
    rank = [vals[x] for x in arr]
    k = 1

    while k < n:
        order = list(range(n))
        order.sort(key=lambda x: (rank[x], rank[x + k] if x + k < n else -1))

        new_rank = [0] * n
        for idx in range(1, n):
            a = order[idx - 1]
            b = order[idx]
            if (rank[a], rank[a + k] if a + k < n else -1) != (
                rank[b],
                rank[b + k] if b + k < n else -1,
            ):
                new_rank[b] = new_rank[a] + 1
            else:
                new_rank[b] = new_rank[a]

        rank = new_rank
        k <<= 1

    return rank

def solve():
    n = int(input())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))

    combined = A + [0] + B
    rank = build_ranks(combined)

    ans = []
    i = 0
    j = n + 1

    while i < n and j < 2 * n + 1:
        if rank[i] < rank[j]:
            ans.append(A[i])
            i += 1
        else:
            ans.append(B[j - n - 1])
            j += 1

    while i < n:
        ans.append(A[i])
        i += 1

    while j < 2 * n + 1:
        ans.append(B[j - n - 1])
        j += 1

    print(*ans)

if __name__ == "__main__":
    solve()
```các`build_ranks`chức năng là công cụ so sánh hậu tố. Ban đầu, mọi hậu tố chỉ được xếp hạng theo giá trị đầu tiên của nó. Mỗi bước nhân đôi sắp xếp các hậu tố bằng cách sử dụng thứ hạng của khối hiện tại và thứ hạng của khối tiếp theo. Sau đủ lần lặp lại, mảng xếp hạng hoàn toàn thể hiện thứ tự hậu tố. 

Giá trị phân cách`0`nhỏ hơn mọi số thực nên nó phân tách hai mảng một cách an toàn. Vì tất cả các số trong mảng ban đầu đều khác biệt nên hậu tố từ`A`không thể là tiền tố của hậu tố từ`B`, do đó dấu phân cách không tạo ra sự so sánh không chính xác. 

Vòng lặp hợp nhất duy trì hai con trỏ. Con trỏ cho`B`bắt đầu lúc`n + 1`vì mảng thứ hai bắt đầu sau dấu phân cách trong mảng kết hợp. Việc so sánh sử dụng thứ hạng được tính toán trước thay vì quét các giá trị, tránh làm việc lặp lại. 

Số nguyên Python không bị tràn và mã không bao giờ tạo ra số lần hợp nhất có thể theo cấp số nhân. Phần tế nhị duy nhất là giữ chỉ mục phân tách và chuyển đổi giữa các chỉ mục mảng kết hợp và`B`chỉ số chính xác. 

## Ví dụ đã hoạt động 

Dành cho:```
n = 3
A = [1,2,3]
B = [4,5,6]
```| tôi | j | So sánh | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | [1,2,3] < [4,5,6] | Lấy A | 1 | 
| 1 | 4 | [2,3] < [4,5,6] | Lấy A | 1 2 | 
| 2 | 4 | [3] < [4,5,6] | Lấy A | 1 2 3 | 
| 3 | 4 | Trống | Lấy B | 1 2 3 4 5 6 | 

Điều này cho thấy trường hợp một mảng hoàn toàn nhỏ hơn và thuật toán sử dụng mảng đó trước tiên. 

Vì:```
n = 7
A = [6,10,4,2,13,12,7]
B = [9,8,11,3,5,1,14]
```| tôi | j | So sánh | Lựa chọn | 
| --- | --- | --- | --- | 
| 0 | 8 | [6,10,4,...] < [9,8,11,...] | A | 
| 1 | 8 | [10,4,2,...] > [9,8,11,...] | B | 
| 1 | 9 | [10,4,2,...] > [8,11,...] | B | 
| 1 | 10 | [10,4,2,...] < [11,3,...] | A | 
| 2 | 10 | [4,2,...] < [11,3,...] | A | 
| 3 | 10 | [2,...] < [11,3,...] | A | 

Dấu vết cho thấy tại sao chỉ so sánh phần tử đầu tiên là không đủ. Sau khi uống`6`, thuật toán chuyển đúng sang`B`vì hậu tố còn lại của nó nhỏ hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc nhân đôi tiền tố thực hiện nhiều vòng sắp xếp theo logarit, sau đó là hợp nhất tuyến tính. | 
| Không gian | O(n) | Mảng kết hợp và mảng xếp hạng lưu trữ một số lượng giá trị không đổi cho mỗi phần tử. | 

Kích thước đầu vào cho phép các giải pháp gần như tuyến tính hoặc gần tuyến tính. Hệ số logarit từ xếp hạng hậu tố có thể chấp nhận được đối với`n = 100000`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    n = int(sys.stdin.readline())
    A = list(map(int, sys.stdin.readline().split()))
    B = list(map(int, sys.stdin.readline().split()))

    combined = A + [0] + B

    def build_ranks(arr):
        n = len(arr)
        rank = [x for x in arr]
        k = 1
        while k < n:
            order = sorted(range(n), key=lambda x: (rank[x], rank[x+k] if x+k < n else -1))
            nr = [0] * n
            for a, b in zip(order, order[1:]):
                nr[b] = nr[a] + ((rank[a], rank[a+k] if a+k < n else -1) != (rank[b], rank[b+k] if b+k < n else -1))
            rank = nr
            k *= 2
        return rank

    r = build_ranks(combined)
    ans = []
    i, j = 0, n + 1

    while i < n and j < 2*n + 1:
        if r[i] < r[j]:
            ans.append(A[i])
            i += 1
        else:
            ans.append(B[j-n-1])
            j += 1

    ans += A[i:]
    ans += B[j-n-1:]

    sys.stdin = old
    return " ".join(map(str, ans)) + "\n"

assert run("3\n1 2 3\n4 5 6\n") == "1 2 3 4 5 6\n"
assert run("5\n1 3 5 7 9\n2 4 6 8 10\n") == "1 2 3 4 5 6 7 8 9 10\n"
assert run("1\n2\n1\n") == "1 2\n"
assert run("3\n6 10 4\n9 8 11\n") == "6 9 8 10 4 11\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / [2] / [1]`|`1 2`| Kích thước tối thiểu và chọn hậu tố nhỏ hơn | 
| Mảng được sắp xếp | Hợp nhất được sắp xếp đầy đủ | Hành vi tham lam đơn giản | 
| Thứ tự hậu tố hỗn hợp | Chuyển đổi không tầm thường | So sánh hậu tố đầy đủ | 
| Mảng nhỏ | Lập chỉ mục chính xác | Ranh giới dấu phân cách và con trỏ | 

## Vỏ cạnh 

Khi một mảng trở nên trống, thuật toán sẽ không thử so sánh khác. Vì:```
n = 1
A = [3]
B = [1]
```Hậu tố so sánh xếp hạng chọn`B`, sản xuất`1 3`. Sau đó,`B`trống và phần còn lại`3`được thêm vào. 

Một lỗi phổ biến là chỉ so sánh các yếu tố hiện tại. Vụ án:```
A = [6,10,4]
B = [9,8,11]
```bắt đầu bằng`6 < 9`, Vì thế`A`được chọn. Sau đó sự so sánh thay đổi vì các hậu tố còn lại đã thay đổi. Thuật toán xử lý việc này bằng cách tính toán lại lựa chọn sau mỗi lần loại bỏ bằng cách sử dụng thứ hạng hậu tố được lưu trữ. 

Vị trí phân cách là một nơi khác xảy ra lỗi. Mảng kết hợp được lập chỉ mục là:```
A + [0] + B
```vậy phần tử đầu tiên của`B`đang ở chỉ mục`n + 1`. Việc triển khai giữ chỉ mục kết hợp để so sánh và chuyển đổi trở lại thành`B`chỉ lập chỉ mục khi thêm phần tử câu trả lời.
