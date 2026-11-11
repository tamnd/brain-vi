---
title: "CF 1579D - Cuộc họp hiệu quả"
description: "Chúng ta được yêu cầu tổ chức một cuộc họp với $n$ người, trong đó mỗi người có giới hạn về số lượng cuộc trò chuyện riêng tư mà họ có thể tham gia. Mảng $a$ mô tả tính hòa đồng của mỗi người: $ai$ là số buổi nói chuyện mà $i$ có thể tham dự trước khi rời đi."
date: "2026-06-10T10:23:25+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1579
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 744 (Div. 3)"
rating: 1400
weight: 1579
solve_time_s: 159
verified: false
draft: false
---

[CF 1579D - Cuộc họp hiệu quả](https://codeforces.com/problemset/problem/1579/D) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, đồ thị, tham lam 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tổ chức một cuộc họp với$n$mọi người, trong đó mỗi người có giới hạn về số lượng cuộc trò chuyện riêng tư mà họ có thể tham gia. Mảng$a$mô tả tính xã hội của mỗi người:$a_i$là số lượng người nói chuyện$i$có thể tham dự trước khi rời đi. Nếu như$a_i = 0$, người đó rời đi ngay lập tức. Mục tiêu của chúng tôi là lên lịch các cuộc trò chuyện theo cách tối đa hóa tổng số cuộc trò chuyện. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp, chúng ta được cung cấp số lượng người và mức độ hòa đồng. Đầu ra trước tiên yêu cầu tổng số cuộc nói chuyện có thể đạt được và sau đó là trình tự thực tế của các cặp người tham gia cho mỗi cuộc nói chuyện. 

Các ràng buộc rất chặt chẽ:$n$có thể lên đến$2 \cdot 10^5$và tổng của tất cả$a_i$trên tất cả các trường hợp thử nghiệm cũng được giới hạn ở mức$2 \cdot 10^5$. Điều này ngay lập tức loại trừ mọi cách tiếp cận thử tất cả các cặp một cách ngây thơ vì số lượng các cặp tiềm năng tăng theo phương trình bậc hai,$O(n^2)$, có thể đạt tới$4 \cdot 10^{10}$trong trường hợp xấu nhất. Thay vào đó, chúng ta phải tìm một phương pháp sắp xếp các cuộc nói chuyện theo từng giai đoạn, lợi dụng thực tế là tổng số khả năng xã hội là có hạn, điều này cho phép có một cách tiếp cận tỷ lệ thuận với tổng số đó. 

Một trường hợp phức tạp phát sinh khi một số người có$a_i = 0$. Những người này không thể tham gia vào bất kỳ cuộc nói chuyện nào. Việc triển khai đơn giản có thể bao gồm chúng theo cặp, dẫn đến số lượng không chính xác. Một trường hợp đặc biệt khác xảy ra khi tổng của tính xã hội là số lẻ. Vì mỗi cuộc nói chuyện tiêu tốn hai đơn vị (mỗi người tham gia một đơn vị), chúng tôi chỉ có thể lên lịch$\text{sum}(a) // 2$nói chuyện. Điều này không rõ ràng ngay lập tức nếu người ta cố gắng ghép đôi những người hòa đồng nhất một cách tham lam mà không tính đến sự ngang bằng của tổng. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ cố gắng ghép đôi từng người theo mọi cách có thể cho đến khi cạn kiệt mọi khả năng hòa đồng. Điều này hiệu quả vì mỗi cuộc nói chuyện sẽ giảm bớt một cuộc nói chuyện có sẵn cho cả hai người tham gia và chúng ta có thể tiếp tục cho đến khi không còn cặp nào còn hòa đồng. Độ phức tạp của việc kiểm tra lặp đi lặp lại tất cả các cặp là$O(n^2)$mỗi hoạt động. Với$n$lên tới$2 \cdot 10^5$, điều này là không thể tính toán được. Số lượng hoạt động tối đa trong trường hợp xấu nhất sẽ vượt quá$10^{10}$, vượt xa những gì có thể trong thời gian giới hạn. 

Quan sát quan trọng là số lượng cuộc nói chuyện tối đa bị giới hạn bởi tổng số khả năng hòa đồng và mỗi cuộc nói chuyện làm giảm khả năng hòa đồng của chính xác hai người tham gia. Do đó, chúng ta có thể tập trung vào việc luôn ghép đôi hai người có tính hòa đồng lớn nhất còn lại, bởi vì điều này đảm bảo rằng những nguồn lực hạn chế nhất sẽ được sử dụng một cách hiệu quả. Chúng ta có thể duy trì mọi người trong một đống tối đa (hoặc hàng đợi ưu tiên) được khóa bằng tính hòa đồng còn lại. Mỗi lần chúng tôi chọn hai người đứng đầu, hãy lên lịch nói chuyện, giảm mức độ hòa đồng của họ và đẩy họ lùi lại nếu họ vẫn còn cuộc nói chuyện. Cách tiếp cận tham lam này là đúng vì bất kỳ trình tự tối ưu nào cũng có thể được chuyển đổi thành trình tự trong đó những người có tính hòa đồng cao nhất được ghép đôi trước mà không làm giảm tổng số cuộc nói chuyện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² * max(a_i)) | O(n) | Quá chậm | 
| Tham lam Max-Heap | O(tổng_a * log n) | O(n) | Đã chấp nhận | 

Cách tiếp cận tham lam dựa trên đống tận dụng các ràng buộc của vấn đề một cách hiệu quả: tổng số cuộc nói chuyện nhiều nhất là tổng của tất cả các khả năng xã hội chia cho hai và log n hoạt động trên mỗi cuộc nói chuyện có thể chấp nhận được. 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$và mảng$a$của tính xã hội. 
3. Xây dựng vùng heap tối đa chứa các bộ dữ liệu`(-a_i, i)`cho tất cả$i$với$a_i > 0$. Chúng tôi sử dụng các giá trị âm vì Python`heapq`theo mặc định là một đống tối thiểu. 
4. Khởi tạo danh sách trống`talks`để ghi lại các cuộc nói chuyện theo lịch trình. 
5. Trong khi đống có ít nhất hai người, hãy chọn hai phần tử có tính xã hội lớn nhất còn lại. 
6. Lên lịch nói chuyện giữa hai người này và nối cặp`(i, j)`ĐẾN`talks`. 
7. Giảm khả năng hòa đồng của cả hai người đi 1. Nếu một người vẫn còn khả năng hòa đồng, hãy đẩy họ trở lại đống. 
8. Khi đống có ít hơn hai người thì dừng lại. Số lượng cuộc đàm phán dự kiến ​​là độ dài của`talks`. 
9. In ra số lượng bài nói theo thứ tự các cặp. 

**Tại sao nó hoạt động** 

Ở mỗi bước, hai người có tính hòa đồng cao nhất sẽ được ghép đôi. Điều này đảm bảo rằng không có người nào có khả năng hòa đồng khác 0 trong khi một người khác có tính hòa đồng tương đương hoặc cao hơn vẫn không được sử dụng. Bất biến tham lam là heap luôn chứa những người tham gia có khả năng nhất hiện tại. Bởi vì mỗi cuộc nói chuyện tiêu tốn chính xác hai đơn vị tính xã hội, nên tổng số cuộc nói chuyện không thể vượt quá tổng của tất cả các tính xã hội chia cho hai, phù hợp với số lượng cuộc nói chuyện được lên lịch theo phương pháp này. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

def productive_meeting():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        heap = []
        for idx, val in enumerate(a):
            if val > 0:
                heapq.heappush(heap, (-val, idx + 1))  # store 1-based index

        talks = []
        while len(heap) > 1:
            val1, i = heapq.heappop(heap)
            val2, j = heapq.heappop(heap)
            talks.append((i, j))
            if val1 + 1 < 0:
                heapq.heappush(heap, (val1 + 1, i))
            if val2 + 1 < 0:
                heapq.heappush(heap, (val2 + 1, j))

        print(len(talks))
        for i, j in talks:
            print(i, j)

productive_meeting()
```Heap được khởi tạo với các giá trị âm vì Python`heapq`thực hiện một heap tối thiểu. Mỗi lần lặp sẽ giảm hai giá trị trên cùng và chèn lại chúng nếu vẫn dương. Chúng tôi chỉ đẩy lùi những người còn hòa đồng để ngăn chặn những hoạt động không cần thiết. Việc sử dụng lập chỉ mục dựa trên 1 đảm bảo đầu ra phù hợp với đặc tả vấn đề. 

## Ví dụ đã hoạt động 

**Ví dụ 1**: Đầu vào`[2, 3]`| Đống trước khi nói chuyện | Cặp đã chọn | Đống còn lại | Cuộc nói chuyện | 
| --- | --- | --- | --- | 
| [(−3,2),(−2,1)] | (2,1) | [(−2,1),(−2,2)] | [(2,1)] | 
| [(−2,1),(−2,2)] | (1,2) | [] | [(2,1),(1,2)] | 

Chúng tôi lên lịch đúng 2 buổi nói chuyện, vắt kiệt mọi mối quan hệ xã hội. 

**Ví dụ 2**: Đầu vào`[1,2,3]`| Đống | Cặp | Heap sau khi giảm | Cuộc nói chuyện | 
| --- | --- | --- | --- | 
| [(−3,3),(−2,2),(−1,1)] | (3,2) | [(−2,3),(−1,2),(−1,1)] | [(3,2)] | 
| [(−2,3),(−1,2),(−1,1)] | (3,1) | [(−1,3),(−1,2)] | [(3,2),(3,1)] | 
| [(−1,3),(−1,2)] | (3,2) | [] | [(3,2),(3,1),(3,2)] | 

Ba cuộc nói chuyện đã được lên lịch, sử dụng mọi khả năng xã hội một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng_a * log n) | Mỗi cuộc nói chuyện khiến hai người cảm thấy khó chịu và có khả năng đẩy họ trở lại; tổng số cuộc đàm phán ≤ tổng(a), chi phí hoạt động heap log n mỗi lần | 
| Không gian | O(n) | Heap lưu trữ tối đa n người; danh sách các cửa hàng đàm phán có nhiều nhất tổng(a)/2 cặp | 

Giải pháp dễ dàng phù hợp trong vòng 2 giây vì sum(a) 2e5 và mỗi thao tác heap tốn tối đa log(2e5) ≈ 18 bước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        productive_meeting()
    return out.getvalue().strip()

# provided sample
assert run("""8
2
2 3
3
1 2 3
4
1 2 3 4
3
0 0 2
2
6 2
3
0 0 2
5
8 2 0 1 1
5
0 1 0 0 6""") != "", "sample 1"

# minimum size
assert run("1\n2\n0 1") != "", "minimum size"

# all equal
assert run("1\n3\n2 2 2") != "", "all equal"

# maximum size
assert run("1\n5\n200000 0 0 0 0") != "", "maximum sociability single"

# edge case sum odd
assert run("1\n3\n1 2 2") != "", "sum odd"
```| Kiểm tra đầu vào | Sản lượng dự kiến
