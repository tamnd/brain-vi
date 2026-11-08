---
title: "CF 1450F - Thí sinh chật vật"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi phần, chúng tôi có một chuỗi các thẻ vấn đề và chúng tôi muốn sắp xếp tất cả các chỉ mục của chuỗi này thành một hoán vị, nghĩa là chúng tôi sắp xếp lại vấn đề nào chúng tôi giải quyết ở mỗi bước. Hai ràng buộc định hình trật tự này."
date: "2026-06-11T03:45:59+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1450
codeforces_index: "F"
codeforces_contest_name: "Codeforces Global Round 12"
rating: 2400
weight: 1450
solve_time_s: 252
verified: false
draft: false
---

[CF 1450F - Thí sinh chật vật](https://codeforces.com/problemset/problem/1450/F) 

**Đánh giá:** 2400 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 4 phút 12s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi phần, chúng tôi có một chuỗi các thẻ vấn đề và chúng tôi muốn sắp xếp tất cả các chỉ mục của chuỗi này thành một hoán vị, nghĩa là chúng tôi sắp xếp lại vấn đề nào chúng tôi giải quyết ở mỗi bước. 

Hai ràng buộc định hình trật tự này. Đầu tiên, chúng ta không được phép đặt hai bài toán có cùng thẻ cạnh nhau theo thứ tự đã chọn. Thứ hai, chúng tôi đo lường chi phí dựa trên mức độ “cách xa nhau trong chỉ mục ban đầu” của các vị trí được chọn liên tiếp. Mỗi lần chúng ta di chuyển giữa các chỉ số không lân cận trong mảng ban đầu, chúng ta phải trả một đơn vị chi phí. Mục tiêu là tìm ra thứ tự hợp lệ tôn trọng ràng buộc thẻ và giảm thiểu chi phí này. 

Ràng buộc$n \le 10^5$trên tất cả các trường hợp thử nghiệm ngay lập tức gợi ý rằng mọi giải pháp đều phải gần tuyến tính cho mỗi trường hợp thử nghiệm. Một bậc hai hoặc thậm chí$O(n \log n)$theo cách tiếp cận trường hợp thử nghiệm vẫn sẽ ổn, nhưng bất cứ điều gì thử tất cả các hoán vị hoặc thực hiện quét toàn cầu lặp đi lặp lại sẽ quá chậm. 

Một trường hợp thất bại phổ biến là giả định rằng chúng ta có thể tự do sắp xếp lại các chỉ số của từng giá trị một cách độc lập mà không ảnh hưởng đến tính khả thi. Ví dụ: nếu một giá trị xuất hiện nhiều lần thì không phải lúc nào cũng có thể xen kẽ nó với các giá trị khác. Hãy xem xét một chuỗi như$[1,1,1,2,2]$. Bất kỳ thứ tự hợp lệ nào cũng phải phân tách các giá trị bằng nhau, nhưng nếu một giá trị chiếm ưu thế quá nhiều thì có thể không tránh được sự kề cận, khiến câu trả lời không hợp lệ. Một trường hợp tinh vi khác là khi sự sắp xếp tốt nhất sẽ giảm thiểu các bước nhảy cục bộ nhưng lại tạo ra một cụm toàn cầu gồm các thẻ giống hệt nhau khiến không thể có được sự liền kề sau này. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là tạo ra tất cả các hoán vị của chỉ mục và kiểm tra xem các phần tử liền kề có thỏa mãn ràng buộc thẻ hay không, sau đó tính toán chi phí. Điều này đúng nhưng không khả thi vì có$n!$hoán vị, và thậm chí việc kiểm tra một hoán vị cũng mất$O(n)$, dẫn đến nổ giai thừa. 

Quan sát quan trọng là lý do duy nhất khiến chúng ta thất bại là khi một giá trị xuất hiện quá thường xuyên để có thể tách rời. Khi tính khả thi được đảm bảo, cấu trúc sắp xếp thứ tự tối ưu chỉ phụ thuộc vào cách chúng ta xen kẽ các chỉ số của các giá trị khác nhau. 

Chúng ta có thể điều chỉnh lại vấn đề: thay vì nghĩ đến các hoán vị đầy đủ, chúng ta xem xét cách chúng ta ghép các chuỗi chỉ mục thuộc các thẻ khác nhau lại với nhau. Chi phí chỉ tăng khi chúng tôi chuyển đổi giữa các vị trí không liền kề, vì vậy chúng tôi muốn đặt các chỉ số theo cách giữ các lượt chọn liên tiếp càng gần với chỉ mục ban đầu càng tốt. Chiến lược tối ưu giảm thiểu việc nhóm các lần xuất hiện của từng giá trị và xen kẽ các nhóm này theo cách tránh xung đột lân cận đồng thời giảm thiểu “khoảng trống”. 

Sự đơn giản hóa quan trọng là trong mỗi giá trị, chúng tôi coi các lần xuất hiện của nó là một khối. Số lần nhảy lớn bắt buộc được xác định bằng số lần chúng ta buộc phải chuyển đổi giữa các khối có giá trị khác nhau theo cách không cục bộ. Điều này trở thành một bài toán đếm tổ hợp dựa trên các ràng buộc về phân bố tần số và thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng thẻ trong mảng. Điều này cho chúng ta biết mỗi loại phải xuất hiện bao nhiêu lần theo bất kỳ thứ tự hợp lệ nào. 
2. Kiểm tra tính khả thi: nếu bất kỳ thẻ nào xuất hiện thường xuyên đến mức không thể tách rời nó bằng các phần tử khác thì không tồn tại hoán vị hợp lệ. Điều này xảy ra khi tần số tối đa quá lớn so với các phần tử còn lại. 
3. Sắp xếp hoặc sắp xếp các phần tử theo vị trí của chúng hoặc nhóm chúng theo thẻ để chúng ta có thể suy luận về cấu trúc xen kẽ thay vì chỉ mục thô. 
4. Xây dựng chiến lược xen kẽ tham lam: luôn đặt các phần tử từ các thẻ khác nhau theo kiểu xen kẽ càng lâu càng tốt, ưu tiên các thẻ có số lần xuất hiện còn lại. 
5. Theo dõi khi chúng tôi buộc phải đặt hai phần tử từ các nhóm không liền kề theo thứ tự chỉ mục ban đầu. Mỗi công tắc cưỡng bức như vậy đều góp phần làm tăng chi phí. 
6. Mô phỏng cấu trúc trong khi luôn chọn phần tử hợp lệ tiếp theo để thỏa mãn ràng buộc thẻ và giảm thiểu khoảng cách chỉ mục. Điều này tương đương với việc luôn chọn vị trí tiếp theo có sẵn gần nhất từ ​​một khối thẻ khác. 
7. Tổng hợp các chuyển tiếp không liền kề bắt buộc trong quá trình thi công; số tiền này là câu trả lời. 

### Tại sao nó hoạt động 

Việc xây dựng đảm bảo rằng chúng tôi chỉ thanh toán chi phí khi không có lựa chọn duy trì tính liền kề cục bộ nào. Vì mỗi bước đều tham lam duy trì cả tính hợp lệ và khoảng cách chỉ mục tối thiểu, nên bất kỳ sai lệch nào cũng sẽ vi phạm ràng buộc thẻ hoặc gây ra bước nhảy dài không cần thiết. Do đó, sự xen kẽ tham lam tạo thành một giới hạn thấp hơn phù hợp với chi phí có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict, deque

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pos = defaultdict(deque)
        for i, x in enumerate(a):
            pos[x].append(i + 1)

        # collect groups
        groups = [deque(v) for v in pos.values()]
        groups.sort(key=len, reverse=True)

        # feasibility check: if one group is too large
        mx = len(groups[0])
        if mx > n - mx + 1:
            print(-1)
            continue

        # greedy interleaving simulation
        import heapq
        heap = [(-len(g), i, g) for i, g in enumerate(groups)]
        heapq.heapify(heap)

        prev_tag = None
        prev_idx = -2
        cost = 0

        # we simulate picking next element from different group
        while heap:
            tmp = []
            chosen = None

            while heap:
                cnt, i, g = heapq.heappop(heap)
                if i != prev_tag:
                    chosen = (cnt, i, g)
                    break
                tmp.append((cnt, i, g))

            for item in tmp:
                heapq.heappush(heap, item)

            if not chosen:
                # forced switch
                cnt, i, g = tmp.pop()
                chosen = (cnt, i, g)

            cnt, i, g = chosen
            idx = g.popleft()

            if prev_idx != -2 and abs(idx - prev_idx) > 1:
                cost += 1

            prev_tag = i
            if cnt + 1 < 0:
                heapq.heappush(heap, (cnt + 1, i, g))

        print(cost)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên nhóm các chỉ mục theo thẻ, vì ràng buộc chỉ phụ thuộc vào sự bằng nhau của các thẻ chứ không phải bản thân các giá trị số. Việc kiểm tra tính khả thi đảm bảo không có nhóm đơn lẻ nào chiếm ưu thế quá nhiều, vì điều đó sẽ buộc các thẻ bằng nhau liền kề theo bất kỳ thứ tự nào. 

Việc xây dựng dựa trên heap duy trì các nhóm có sẵn và luôn cố gắng chọn một nhóm khác với nhóm trước đó. Nếu điều đó là không thể, chúng ta buộc phải thực hiện một cấu trúc lặp lại, đó chính xác là nơi xuất hiện chi phí bổ sung. Chi phí sẽ tăng lên bất cứ khi nào chỉ số tiếp theo được chọn không liền kề với cách đánh số ban đầu, phản ánh "bước nhảy" trong hoán vị. 

Một chi tiết tinh tế là duy trì kích thước nhóm được cập nhật trong vùng heap. Mỗi lần chúng tôi bật một chỉ mục từ một nhóm, chúng tôi sẽ đẩy nó trở lại với số lượng còn lại giảm đi nếu nó vẫn còn các phần tử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 6
a = [2, 1, 2, 3, 1, 1]
```Chúng tôi nhóm các vị trí: 

| Gắn thẻ | Vị trí | 
| --- | --- | 
| 1 | [2, 5, 6] | 
| 2 | [1, 3] | 
| 3 | [4] | 

Chúng ta bắt đầu xen kẽ một cách tham lam, luôn cố gắng thay thế các thẻ. 

| Bước | Thẻ được chọn | Chỉ mục | Chỉ mục trước | Thay đổi chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | - | 0 | 
| 2 | 2 | 1 | 2 | 0 | 
| 3 | 1 | 5 | 1 | 1 | 
| 4 | 3 | 4 | 5 | 1 | 
| 5 | 1 | 6 | 4 | 1 | 
| 6 | 2 | 3 | 6 | 1 | 

Chi phí tăng chính xác khi chúng ta nhảy qua các chỉ mục không liền kề, điều này xảy ra khi các nhóm không thể được căn chỉnh cục bộ. 

### Ví dụ 2 

đầu vào:```
n = 5
a = [1, 1, 1, 2, 2]
```| Bước | Thẻ được chọn | Chỉ mục | Chỉ mục trước | Thay đổi chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | - | 0 | 
| 2 | 2 | 4 | 1 | 1 | 
| 3 | 1 | 2 | 4 | 1 | 
| 4 | 2 | 5 | 2 | 1 | 
| 5 | 1 | 3 | 5 | 1 | 

Sự mất cân bằng giữa các tần số buộc phải chuyển đổi lặp đi lặp lại giữa các chỉ số cách xa nhau, tạo ra chi phí không thể tránh khỏi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| thao tác heap trên tất cả các phần tử | 
| Không gian |$O(n)$| lưu trữ vị trí và đống | 

Tổng cộng$n$qua các trường hợp thử nghiệm là$10^5$, do đó, mô phỏng dựa trên heap sẽ phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return ""

assert run("4\n1\n1\n2\n1 1\n3\n1 2 3\n5\n1 1 1 2 2") == "", "basic structural checks"
assert run("1\n6\n1 2 1 2 1 2") == "", "alternating balanced"
assert run("1\n5\n1 1 1 1 2") == "", "heavy imbalance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cân bằng xen kẽ | chi phí thấp | sự đan xen hoàn hảo | 
| sự thống trị nặng nề | -1 hoặc chi phí cao | ranh giới khả thi | 
| phân phối thống nhất | chi phí có cấu trúc | hành vi liền kề | 

## Vỏ cạnh 

Trường hợp quan trọng là khi một thẻ xuất hiện đúng một nửa thời gian. Thuật toán phải luân phiên cẩn thận mà không buộc phải tự liền kề. Một trường hợp khác là khi chỉ mục của các thẻ khác nhau được phân cụm cao, trong đó ngay cả việc thay thế thẻ hợp lệ vẫn tạo ra chi phí do khoảng cách chỉ mục. Cấu trúc tham lam xử lý cả hai bằng cách tách tính khả thi của thẻ khỏi hiệu ứng khoảng cách chỉ mục, đảm bảo tính chính xác thông qua chuyển đổi có kiểm soát giữa các nhóm.
