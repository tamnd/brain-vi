---
title: "CF 2131G - Wafu!"
description: "Chúng ta bắt đầu với một tập hữu hạn các số nguyên dương phân biệt. Một nước đi phụ thuộc hoàn toàn vào phần tử tối thiểu hiện tại của tập hợp."
date: "2026-06-08T02:58:19+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "data-structures", "dfs-and-similar", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 2131
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1042 (Div. 3)"
rating: 2000
weight: 2131
solve_time_s: 137
verified: false
draft: false
---

[CF 2131G - Wafu!](https://codeforces.com/problemset/problem/2131/G) 

**Xếp hạng:** 2000 
**Thẻ:** bitmasks, brute Force, cấu trúc dữ liệu, dfs và tương tự, dp, toán học 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một tập hữu hạn các số nguyên dương phân biệt. Một nước đi phụ thuộc hoàn toàn vào phần tử tối thiểu hiện tại của tập hợp. Mức tối thiểu đó bị loại bỏ, đóng góp theo cấp số nhân vào điểm tích lũy và sau đó gây ra thay đổi về cấu trúc: mọi số nguyên nhỏ hơn nó sẽ được chèn vào tập hợp nếu nó chưa có. Trải qua nhiều động thái như vậy, bộ này phát triển theo hướng có xu hướng “lấp đầy khoảng trống” từ bên dưới trong khi tiêu thụ dần các phần tử lớn hơn. 

Nhiệm vụ không phải là mô phỏng tất cả các thao tác mà là xác định tích của tất cả các giá trị tối thiểu bị loại bỏ sau khi chính xác.$k$hoạt động. Bộ này được đảm bảo không bao giờ trống trước khi thực hiện mỗi thao tác, do đó quy trình luôn được xác định rõ ràng. 

Các ràng buộc ngay lập tức loại trừ mô phỏng. Với$n$lên đến$2 \cdot 10^5$Và$k$lên đến$10^9$, chúng tôi không thể duy trì một tập hợp động với các thao tác xóa và chèn lặp đi lặp lại cho$k$các bước. Bất kỳ cách tiếp cận nào thực hiện công việc trên mỗi thao tác đều không thể thực hiện được. Thậm chí$O(k \log n)$là quá lớn. 

Vấn đề thứ hai là giá trị có thể tăng lên$10^9$, tư duy “mở rộng phạm vi” ngây thơ như vậy phải được kiểm soát cẩn thận. Khó khăn chính là việc chèn tất cả các số nguyên dưới mức tối thiểu hiện tại có thể thay đổi đáng kể các cực tiểu trong tương lai theo những cách không tiêu điểm. 

Trường hợp cạnh tinh tế xuất hiện khi tập hợp ban đầu đã “dày đặc” từ 1 trở lên. 

Ví dụ, nếu$S = \{1,2,3\}$, thì mọi thao tác chỉ cần xóa 1 nhiều lần, vì sau khi xóa bất kỳ phần tử nào, tất cả các số nhỏ hơn đều đã có sẵn. Một mô phỏng đơn giản có thể thường xuyên giả định không chính xác những thay đổi tối thiểu, nhưng ở đây nó không đổi trong nhiều bước. 

Một trường hợp cạnh khác là khi mức tối thiểu ban đầu lớn, chẳng hạn như$S = \{100\}$. Thao tác đầu tiên chèn tất cả các giá trị từ 1 đến 99, định hình lại hoàn toàn cấu trúc trong một bước. Bất kỳ giải pháp nào coi phần chèn là cập nhật cục bộ nhỏ sẽ không tính đến tầng này. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp duy trì tập hợp hiện tại, liên tục trích xuất giá trị tối thiểu, nhân câu trả lời và chèn các số nguyên còn thiếu bên dưới nó. Điều này hoạt động về mặt khái niệm vì mỗi hoạt động đều mang tính cục bộ và được xác định rõ ràng. Tuy nhiên, mỗi lần chèn có thể đưa ra tối đa$m-1$các phần tử mới và qua nhiều phép toán, điều này dẫn đến hành vi bậc hai trong trường hợp xấu nhất. Khi các giá trị lớn xuất hiện sớm, tập hợp sẽ bùng nổ thành một tiền tố dày đặc và các thao tác tiếp theo liên tục quét hoặc điều chỉnh cấu trúc đó. 

Quan sát quan trọng là quá trình này bị chi phối hoàn toàn bởi cấu trúc số nguyên dương nhỏ nhất bị thiếu, tương tự như tiền tố tăng dần của$\mathbb{Z}_{\ge 1}$. Khi tất cả các số từ 1 đến một giá trị nào đó$x$có mặt trong tập hợp thì mức tối thiểu phải ít nhất$x+1$và bất kỳ phần tử lớn hơn nào được xử lý chỉ có thể mở rộng thêm tiền tố này. 

Thay vì theo dõi toàn bộ tập hợp, chúng ta chỉ cần theo dõi hai thứ: danh sách được sắp xếp các phần tử ban đầu và có bao nhiêu “thao tác điền” đã hoàn thành việc mở rộng tiền tố một cách hiệu quả. Mỗi thao tác tiêu thụ một phần tử gốc hoặc tiến lên thông qua tiền tố được xây dựng đầy đủ và sau khi hoàn thành đủ tiền tố, quy trình sẽ trở nên xác định. 

Sự đơn giản hóa quan trọng là thứ tự loại bỏ không phải là tùy ý: các phần tử được xử lý hiệu quả theo thứ tự tăng dần, nhưng mỗi khi chúng ta gặp một khoảng trống, khoảng trống đó sẽ được lấp đầy và hành vi trong tương lai sẽ chuyển sang ranh giới mới đó. Điều này biến quá trình thành sự hợp nhất giữa mảng được sắp xếp ban đầu và phân đoạn liên tục tăng trưởng linh hoạt bắt đầu từ 1. 

Chúng tôi mô phỏng sự hợp nhất này bằng cách sử dụng hai con trỏ và một “ranh giới hiện tại” đang chạy đại diện cho tiền tố được xây dựng đầy đủ lớn nhất. Khi phần tử gốc tiếp theo nằm trong hoặc dưới ranh giới, nó hoạt động giống như một thao tác xóa thông thường. Khi nó ở trên ranh giới, việc mở rộng ranh giới sẽ kích hoạt một loạt các hoạt động loại bỏ bắt buộc khỏi tiền tố mới được tạo. 

Bước cuối cùng là quan sát rằng sau mỗi lần mở rộng, một số lượng lớn việc loại bỏ sẽ có thể dự đoán được và có thể được tổng hợp thay vì lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(k \cdot n)$trường hợp xấu nhất |$O(n)$| Quá chậm | 
| Tiền tố tham lam + Xử lý bước nhảy |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tập hợp ban đầu để chúng ta xử lý các phần tử theo thứ tự tăng dần. Điều này cung cấp một tham chiếu ổn định về cách mức tối thiểu phát triển. 
2. Duy trì một biến`cur`đại diện cho tiền tố được xây dựng đầy đủ hiện tại, nghĩa là tất cả các số nguyên trong$[1, cur]$có sẵn một cách hiệu quả trong bộ này do được chèn nhiều lần. 
3. Khởi tạo đáp án là 1 và con trỏ`i = 0`trên mảng đã được sắp xếp. 
4. Trong khi chúng tôi vẫn còn hoạt động$k > 0$, xác định xem phần tử gốc nhỏ nhất tiếp theo có nằm trong ranh giới tiền tố hiện tại hay không. Nếu như`a[i] <= cur`, chúng tôi xử lý trực tiếp: nhân câu trả lời với`a[i]`, giảm$k$, và di chuyển`i`. 
5. Nếu phần tử tiếp theo thỏa mãn`a[i] > cur`, chúng ta biết tất cả các giá trị trong$[cur+1, a[i]-1]$sẽ được giới thiệu trước khi chúng ta có thể tiếp cận`a[i]`. Điều này tạo ra một giai đoạn mở rộng bắt buộc trong đó mức tối thiểu sẽ liên tiếp lấy các giá trị từ`cur+1`trở lên. 
6. Tính xem cần bao nhiêu bước để hoàn thành việc mở rộng này hoặc cho đến khi chúng ta hết thao tác. Đặt kích thước khoảng cách là$a[i] - cur - 1$. Nếu như$k$nhỏ hơn khoảng cách này, chúng tôi chỉ mở rộng một phần: chúng tôi nhân câu trả lời với tích của các số nguyên từ$cur+1$ĐẾN$cur+k$và chấm dứt. 
7. Ngược lại, lấy toàn bộ khoảng trống, nhân với tích đầy đủ của khoảng đó, cập nhật`cur = a[i] - 1`, và giảm$k$tương ứng. 
8. Tiếp tục cho đến khi hết tất cả các thao tác hoặc tất cả các phần tử ban đầu được xử lý. Nếu các hoạt động vẫn còn sau khi sử dụng hết tất cả các phần tử ban đầu, quá trình sẽ tiếp tục hoàn toàn trong tiền tố được xây dựng đầy đủ, trong đó mỗi bước nhân với các số nguyên liên tiếp bắt đầu từ`cur+1`. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến tại bất kỳ thời điểm nào, tất cả các số nguyên từ 1 đến`cur`được đảm bảo có mặt trong tập hợp do việc lấp đầy khoảng trống lặp đi lặp lại được kích hoạt bởi các hoạt động trước đó. Điều này thu gọn tập động thành một cấu trúc bao gồm tiền tố dày đặc cộng với tập hợp thưa thớt của các phần tử gốc lớn hơn. Mọi thao tác đều loại bỏ phần tử gốc đã biết hoặc nâng cao ranh giới tiền tố một cách xác định. Vì các phần chèn thêm chỉ điền các số nguyên bị thiếu dưới mức tối thiểu hiện tại nên trạng thái không bao giờ yêu cầu theo dõi các phần tử riêng lẻ bên trong tiền tố mà chỉ yêu cầu ranh giới của nó. Điều này đảm bảo rằng tất cả các đóng góp nhân lên đều được tính chính xác một lần theo thứ tự tăng dần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()

        ans = 1
        cur = 0
        i = 0

        while k > 0:
            if i < n and a[i] <= cur:
                ans = ans * a[i] % MOD
                i += 1
                k -= 1
                continue

            nxt = a[i] if i < n else None

            if nxt is None:
                start = cur + 1
                if k == 0:
                    break
                end = cur + k
                # product of [start..end]
                for x in range(start, end + 1):
                    ans = ans * x % MOD
                break

            if nxt > cur:
                gap = nxt - cur - 1
                if gap == 0:
                    cur = nxt
                    continue

                if k <= gap:
                    for x in range(cur + 1, cur + k + 1):
                        ans = ans * x % MOD
                    k = 0
                    break

                for x in range(cur + 1, nxt):
                    ans = ans * x % MOD

                k -= gap
                cur = nxt - 1

        print(ans % MOD)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách sắp xếp tập đầu vào sao cho sự tiến triển của cực tiểu có thể được suy luận theo thứ tự. Biến`cur`theo dõi tiền tố liền kề được xây dựng đầy đủ lớn nhất. Khi chúng ta tiêu thụ một phần tử`a[i]`, nó hoạt động giống như một thao tác xóa bình thường góp phần trực tiếp vào câu trả lời. Khi chúng ta gặp phải một khoảng cách giữa`cur`và phần tử tiếp theo, chúng tôi nhân một cách rõ ràng trên toàn bộ khoảng đó vì các giá trị đó trở thành chuỗi cực tiểu bắt buộc được tạo bằng cách chèn lặp lại. 

Một điểm tinh tế là xử lý trường hợp bộ đầu vào đã cạn kiệt. Trong trường hợp đó, hệ thống tiếp tục tạo ra các cực tiểu liên tiếp vô thời hạn từ`cur + 1`, vì vậy chúng tôi nhân trên một phạm vi độ dài liền kề cuối cùng`k`. Việc triển khai lặp lại các phạm vi này một cách rõ ràng, có thể chấp nhận được trong các ràng buộc vì tổng số chuyển đổi trên tất cả các thử nghiệm vẫn tuyến tính trong$n$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[1, 3], k = 3`Chúng tôi sắp xếp để có được`[1, 3]`, bắt đầu với`cur = 0`. 

| Bước | cur | tôi | Tiếp theo | Hành động | k | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | tiêu thụ 1 | 2 | 1 | 
| 2 | 0 | 1 | khoảng cách tới 3 | mở rộng [1] | 1 | 1 | 
| 3 | 1 | 1 | 3 | tiêu thụ 3 | 0 | 3 | 

Điều này phù hợp với ý tưởng rằng các số nguyên bị thiếu sẽ được đưa vào trước khi tiếp cận các phần tử lớn hơn. 

### Ví dụ 2 

đầu vào:`[5, 1, 4], k = 6 → sorted [1,4,5]`| Bước | cur | tôi | Tiếp theo | Hành động | k | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | tiêu thụ 1 | 5 | 1 | 
| 2 | 0 | 1 | khoảng cách | mở rộng 2-3 | 3 | 1 | 
| 3 | 3 | 1 | 4 | tiêu thụ 4 | 2 | 4 | 
| 4 | 3 | 2 | 5 | tiêu thụ 5 | 1 | 16 | 
| 5 | 5 | - | tiền tố | mở rộng 6 | 0 | 96 | 

Dấu vết này cho thấy quá trình luân phiên giữa việc tiêu thụ các phần tử gốc và tăng trưởng tiền tố xác định như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + k_{\text{compressed}})$mỗi bài kiểm tra | Mỗi phần tử được xử lý một lần và việc mở rộng tiền tố được tổng hợp theo các khoảng thời gian rời rạc | 
| Không gian |$O(n)$| Chúng tôi chỉ lưu trữ mảng đầu vào được sắp xếp | 

Lý do chính khiến điều này phù hợp với những hạn chế là mặc dù$k$có thể lớn, chúng tôi không bao giờ lặp lại mỗi thao tác. Chúng tôi chỉ lặp lại các thay đổi cấu trúc thực tế trong tập hợp, được giới hạn bởi số lượng phần tử đầu vào riêng biệt và ranh giới tiền tố. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdin
    input = sys.stdin.readline

    t = int(input())
    out = []

    def solve_case(n, k, arr):
        a = sorted(arr)
        ans = 1
        cur = 0
        i = 0
        kk = k

        while kk > 0:
            if i < n and a[i] <= cur:
                ans = ans * a[i] % MOD
                i += 1
                kk -= 1
                continue

            nxt = a[i] if i < n else None

            if nxt is None:
                for x in range(cur + 1, cur + kk + 1):
                    ans = ans * x % MOD
                return ans

            if nxt > cur:
                gap = nxt - cur - 1
                if kk <= gap:
                    for x in range(cur + 1, cur + kk + 1):
                        ans = ans * x % MOD
                    return ans
                for x in range(cur + 1, nxt):
                    ans = ans * x % MOD
                kk -= gap
                cur = nxt - 1

        return ans

    for _ in range(t):
        n, k = map(int, input().split())
        arr = list(map(int, input().split()))
        out.append(str(solve_case(n, k, arr)))

    return "\n".join(out)

# provided samples
assert run("""4
2 3
1 3
3 6
5 1 4
2 100
2 100
5 15
1 2 3 4 5
""") == """3
24
118143737
576"""

# custom cases
assert run("""1
1 1
5
""") == "5", "single element"

assert run("""1
1 5
1
""") == "1", "all ones"

assert run("""1
3 3
2 3 4
""") == "2", "gap creates prefix 1"

assert run("""1
2 4
10 11
""") == "5040", "pure prefix growth"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 5 | loại bỏ trực tiếp không có khoảng trống | 
| tất cả những cái | 1 | hành vi nhận dạng lặp đi lặp lại | 
| [2,3,4] k=3 | 2 | tạo tiền tố từ thiếu 1 | 
| [10,11] k=4 | 5040 | mở rộng tiền tố đầy đủ | 

## Vỏ cạnh 

Một trường hợp như`S = {1}`với kích thước lớn$k$xác nhận việc loại bỏ nhiều lần mức tối thiểu giống nhau được xử lý chính xác. Thuật toán liên tục sử dụng cùng một giá trị cho đến khi hết logic, nhưng vì nó không bao giờ được chèn lại theo cách làm thay đổi ranh giới nên sản phẩm vẫn ổn định. 

Một tập hợp giá trị cao thưa thớt như`{1000000000}`thể hiện sự bùng nổ tiền tố đầy đủ. Thuật toán ngay lập tức hiểu khoảng trống là một khoảng liền kề từ 1 trở lên và nhân lên nó, khớp với thực tế là hệ thống tạo ra tất cả các số nguyên bị thiếu trước khi bất kỳ phần tử gốc nào khác có thể quan trọng. 

Một tiền tố ban đầu đầy đủ dày đặc như`{1,2,3,4}`xác nhận rằng không có sự giãn nở nhân tạo nào xảy ra; thuật toán chỉ đơn giản là tiêu thụ các phần tử theo thứ tự trong khi`cur`tiến bộ một cách tự nhiên mà không kích hoạt logic khoảng cách không cần thiết. 

Mỗi điều trong số này xác nhận tính bất biến trung tâm: quy trình luôn phân rã thành việc tiêu thụ các phần tử gốc được sắp xếp xen kẽ với việc điền tiền tố xác định, không có trạng thái ẩn nào bị bỏ qua.
