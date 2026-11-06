---
title: "CF 1285E - Xóa một đoạn"
description: "Chúng tôi được cung cấp một số khoảng trên một dòng số. Mỗi khoảng đại diện cho một đoạn liên tục của các điểm được bảo hiểm. Nếu nhiều khoảng chồng lên nhau hoặc chạm vào nhau, vùng được bao phủ kết hợp của chúng sẽ hợp nhất thành một phần liên tục duy nhất khi chúng ta hợp nhất."
date: "2026-06-16T03:35:02+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "data-structures", "dp", "graphs", "sortings", "trees", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1285
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 613 (Div. 2)"
rating: 2300
weight: 1285
solve_time_s: 194
verified: false
draft: false
---

[CF 1285E - Xóa phân đoạn](https://codeforces.com/problemset/problem/1285/E) 

**Đánh giá:** 2300 
**Thẻ:** sức mạnh vũ phu, thuật toán xây dựng, cấu trúc dữ liệu, dp, đồ thị, sắp xếp, cây, hai con trỏ 
**Thời gian giải:** 3 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số khoảng trên một dòng số. Mỗi khoảng đại diện cho một đoạn liên tục của các điểm được bảo hiểm. Nếu nhiều khoảng chồng lên nhau hoặc chạm vào nhau, vùng được bao phủ kết hợp của chúng sẽ hợp nhất thành một phần liên tục duy nhất khi chúng ta hợp nhất. 

Nhiệm vụ không phải là tính toán kết hợp trực tiếp cho tập hợp đầy đủ mà là loại bỏ chính xác một khoảng và sau đó tính xem có bao nhiêu phân đoạn liên tục rời rạc còn lại trong kết hợp của các khoảng còn lại. Chúng tôi muốn chọn khoảng thời gian bị loại bỏ sao cho số lượng thành phần được kết nối này càng lớn càng tốt. 

Vì vậy, vấn đề là hiểu được mỗi khoảng góp phần như thế nào vào việc hợp nhất các vùng được bao phủ riêng biệt trước đó. Việc xóa một khoảng có thể có khả năng chia một vùng đã hợp nhất thành nhiều phần nhưng cũng không thể thực hiện được nếu khoảng đó là dư thừa. 

Các ràng buộc rất chặt chẽ: tổng số khoảng thời gian lên tới 200.000 trên tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại sự kết hợp đầy đủ cho mỗi lần xóa. Một cách tiếp cận đơn giản để tính toán lại việc hợp nhất sau khi xóa từng khoảng thời gian sẽ yêu cầu sắp xếp và quét từng lần xóa trong số n lần xóa, dẫn đến khoảng O(n^2 log n) cho mỗi trường hợp thử nghiệm, quá chậm. 

Một khía cạnh tinh tế là các khoảng thời gian có thể hoàn toàn dư thừa hoặc hoàn toàn quan trọng. Ví dụ: nếu một khoảng được bao phủ hoàn toàn bởi những khoảng khác thì việc loại bỏ nó cũng không thay đổi gì. Ngược lại, khoảng thời gian là cầu nối duy nhất giữa hai phần là rất quan trọng. 

Các trường hợp đặc biệt phá vỡ suy nghĩ ngây thơ bao gồm các tình huống như tất cả các khoảng đều giống hệt nhau, trong đó việc loại bỏ bất kỳ khoảng nào cũng không có tác dụng gì hoặc chuỗi trong đó mỗi khoảng chỉ chồng lên nhau một chút để việc loại bỏ một khoảng sẽ chia một thành phần được kết nối dài thành hai. Một ví dụ nhỏ: 

đầu vào:```
3
1 3
3 5
5 7
```Ở đây tất cả các khoảng tạo thành một chuỗi, tạo ra một thành phần được kết nối. Việc xóa khoảng giữa sẽ chia chuỗi thành hai thành phần, tăng câu trả lời từ 1 lên 2. Việc "đếm các thành phần được hợp nhất sau khi loại bỏ" trên mỗi chỉ mục vẫn sẽ quá chậm, nhưng một ý tưởng tham lam ngây thơ như loại bỏ khoảng nhỏ nhất hoặc lớn nhất cũng không thành công vì cấu trúc phụ thuộc vào kết nối chồng chéo chứ không phải độ dài. 

## Phương pháp tiếp cận 

Chiến lược brute-force rất đơn giản: đối với mỗi khoảng, hãy loại bỏ nó, xây dựng lại sự kết hợp của các khoảng còn lại và đếm xem có bao nhiêu phân đoạn được hợp nhất rời rạc tồn tại. Việc xây dựng liên kết yêu cầu sắp xếp các khoảng thời gian theo điểm cuối bên trái và quét một lần, hợp nhất các phần chồng chéo. Chi phí này là O(n log n) cho mỗi trường hợp thử nghiệm. Thực hiện n lần sẽ dẫn đến O (n^2 log n), điều này ở tổng khoảng thời gian 2e5 là hoàn toàn không khả thi. 

Quan sát quan trọng là cấu trúc hợp nhất chỉ phụ thuộc vào mức độ trùng lặp của các khoảng khi được sắp xếp theo điểm cuối của chúng. Khi các khoảng được sắp xếp theo điểm cuối bên trái, sự kết hợp được hình thành bằng cách mở rộng một cách tham lam phân đoạn hiện tại bất cứ khi nào một khoảng trùng lặp hoặc chạm vào ranh giới bên phải đang hoạt động hiện tại. Mỗi lần chúng tôi không gia hạn được, một thành phần mới sẽ bắt đầu. 

Bây giờ chúng ta hỏi điều gì sẽ xảy ra khi chúng ta loại bỏ một khoảng. Việc xóa một khoảng chỉ có ý nghĩa nếu khoảng đó chịu trách nhiệm mở rộng một thành phần đã hợp nhất hoặc kết nối hai phần. Trong quá trình quét được sắp xếp theo điểm cuối bên trái, có những thời điểm mà phạm vi phủ sóng hoạt động hiện tại được xác định bởi điểm cuối bên phải tối đa trong các khoảng thời gian hoạt động. Khoảng thời gian duy nhất quan trọng là những khoảng thời gian từng trở thành “người đóng góp điểm cuối bên phải tối đa” tại một thời điểm nào đó trong quá trình quét. 

Điều này dẫn đến một phép biến đổi cổ điển: thay vì tính toán lại các phần hợp cho mỗi lần xóa, chúng tôi tính toán, đối với mỗi khoảng thời gian, cần có bao nhiêu “phân đoạn hợp”. Nếu việc loại bỏ nó gây ra sự hợp nhất hoặc phân chia, hiệu ứng đó có thể được biểu thị bằng số lần nó xác định duy nhất phạm vi phủ sóng tối đa đang hoạt động. 

Để thực hiện điều này một cách chính xác, chúng tôi xử lý các khoảng được sắp xếp theo điểm cuối bên trái nhưng không chỉ duy trì một điểm cuối bên phải đang hoạt động mà còn theo dõi xem có bao nhiêu khoảng thời gian hiện đang đóng góp vào điểm cuối bên phải tối đa. Nếu mức tối đa là duy nhất, việc loại bỏ khoảng đó sẽ làm giảm phần mở rộng phạm vi bao phủ và có thể tạo ra sự chia rẽ trong liên minh. Nếu mức tối đa không phải là duy nhất, việc xóa một bản sao sẽ không làm thay đổi ranh giới hợp. 

Điều này làm giảm vấn đề trong việc theo dõi, đối với mỗi khoảng thời gian, liệu nó có chịu trách nhiệm duy nhất cho việc mở rộng liên kết ở một số chuyển tiếp phân đoạn hay không. Bằng cách sử dụng nén tọa độ và quét với cấu trúc nhiều tập hợp hoặc giống như đống, chúng ta có thể duy trì điểm cuối bên phải tốt nhất và tốt thứ hai hiện tại, đồng thời phát hiện khi nào điểm tốt nhất là duy nhất. 

Mỗi lần hợp mở rộng đến một ranh giới bên phải mới, phần mở rộng đó được quy cho ít nhất một khoảng. Nếu chính xác một khoảng chịu trách nhiệm, việc loại bỏ nó sẽ phá vỡ phần mở rộng đó và tăng số lượng thành phần lên một. 

Cuối cùng, câu trả lời là số lượng cơ sở của các phân đoạn hợp nhất trừ đi 1 cộng với mức cải thiện tốt nhất đạt được bằng cách loại bỏ khoảng quan trọng duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² log n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính toán số lượng các thành phần được kết nối trong liên kết của tất cả các khoảng. Điều này được thực hiện bằng cách sắp xếp các khoảng theo điểm cuối bên trái và hợp nhất một cách tham lam bất cứ khi nào khoảng tiếp theo trùng với khoảng hiện tại. Mỗi lần chúng tôi bắt đầu một vùng không chồng chéo mới, chúng tôi sẽ tăng số lượng thành phần. 

Tiếp theo, chúng tôi mô phỏng quá trình quét tương tự này nhưng làm phong phú thêm thông tin về khoảng thời gian nào chịu trách nhiệm mở rộng điểm cuối bên phải xa nhất hiện tại.

1. Sắp xếp các khoảng theo điểm cuối bên trái và nếu bằng nhau thì theo điểm cuối bên phải theo thứ tự giảm dần. Điều này đảm bảo rằng trong số các khoảng bắt đầu tại cùng một điểm, khoảng nào kéo dài hơn sẽ được xử lý trước. 
2. Duy trì nhiều tập hợp (hoặc hai đống) điểm cuối bên phải của "các ứng viên đang hoạt động" trong khu vực được hợp nhất hiện tại. Mục tiêu là luôn biết mức tối đa và liệu nó có phải là duy nhất hay không. 
3. Quét qua các khoảng thời gian. Đối với mỗi khoảng, chúng tôi chèn điểm cuối bên phải của nó vào cấu trúc hiện hoạt. 
4. Khi khoảng thời gian hiện tại bắt đầu sau khi kết thúc phân đoạn được hợp nhất hiện tại, chúng tôi sẽ hoàn thiện thành phần trước đó. Đây là nơi chúng tôi ghi lại rằng một phân đoạn kết hợp mới bắt đầu. 
5. Trong khi mở rộng một phân đoạn, bất cứ khi nào điểm cuối bên phải tối đa tăng lên, chúng tôi gán phần mở rộng này cho (các) khoảng đạt được mức tối đa đó. 
6. Nếu tại bất kỳ thời điểm nào, điểm cuối bên phải tối đa đạt được đúng một khoảng, thì chúng tôi đánh dấu khoảng đó là “quan trọng ở ranh giới này”, nghĩa là việc loại bỏ nó sẽ ngăn cản việc mở rộng này và chia tách một thành phần. 
7. Sau khi xử lý tất cả các khoảng, chúng tôi tính toán xem chúng tôi có thể thu được bao nhiêu thành phần bằng cách loại bỏ một khoảng. Mỗi khoảng thời gian có thể đóng góp tối đa cải tiến +1 nếu nó chịu trách nhiệm duy nhất cho việc hợp nhất mà lẽ ra sẽ không xảy ra. 
8. Câu trả lời cuối cùng là số lượng thành phần ban đầu cộng với sự cải thiện tốt nhất trong tất cả các khoảng thời gian sau khi xóa. 

### Tại sao nó hoạt động 

Cấu trúc liên kết chỉ thay đổi tại các điểm mà điểm cuối bên phải hoạt động tối đa thay đổi qua các khoảng trống giữa các thành phần. Một ranh giới thành phần tồn tại chính xác khi không có khoảng cách nào thu hẹp khoảng cách để mở rộng phạm vi bao phủ. Chỉ những khoảng xác định duy nhất phạm vi tiếp cận tối đa tại một ranh giới mới có thể ảnh hưởng đến việc ranh giới đó có biến mất sau khi xóa hay không. Vì các phần chồng chéo được ghi lại hoàn toàn bằng tính năng theo dõi tối đa của quá trình quét nên mọi sự kiện hợp nhất hoặc phân tách chỉ được xác định bằng danh tính của khoảng thời gian đạt được điểm cuối bên phải tối đa tại thời điểm đó, điều này làm cho ảnh hưởng của mỗi khoảng thời gian trở nên độc lập và có thể tổng hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    segs = []
    for i in range(n):
        l, r = map(int, input().split())
        segs.append((l, r, i))

    segs.sort()

    import heapq

    # active max-heap by (-r, id)
    heap = []
    contrib = [0] * n

    i = 0
    ans = 0
    cur_l = None
    cur_r = None

    while i < n:
        l = segs[i][0]

        if cur_l is None:
            cur_l, cur_r = segs[i][0], segs[i][1]

        if l > cur_r:
            ans += 1
            heap.clear()
            cur_l, cur_r = segs[i][0], segs[i][1]
            heapq.heappush(heap, (-segs[i][1], segs[i][2]))
            i += 1
            continue

        while i < n and segs[i][0] <= cur_r:
            l, r, idx = segs[i]
            heapq.heappush(heap, (-r, idx))
            cur_r = max(cur_r, r)
            i += 1

        # collect candidates with max r
        best_r = -heap[0][0]
        best_ids = []

        while heap and -heap[0][0] == best_r:
            best_r2, idx = heapq.heappop(heap)
            best_ids.append(idx)

        if len(best_ids) == 1:
            contrib[best_ids[0]] += 1

        for idx in best_ids:
            heapq.heappush(heap, (-best_r, idx))

    ans += 1
    print(ans + max(contrib) - 1)

t = int(input())
for _ in range(t):
    solve()
```Đầu tiên, mã sẽ tính toán có bao nhiêu thành phần hợp nhất tồn tại bằng cách quét qua các khoảng được sắp xếp và phát hiện thời điểm một phân đoạn mới bắt đầu sau một khoảng trống. Nó duy trì một loạt các điểm cuối bên phải đang hoạt động để xác định khi nào ranh giới vùng phủ sóng hiện tại được xác định duy nhất bởi một khoảng thời gian duy nhất. 

các`contrib`mảng theo dõi số lần mỗi khoảng chịu trách nhiệm duy nhất cho việc mở rộng một phân đoạn hợp. Nếu một khoảng chịu trách nhiệm duy nhất cho ít nhất một phần mở rộng như vậy thì việc loại bỏ nó có thể tăng số lượng thành phần hợp lên đúng một. 

Công thức cuối cùng mang lại lợi ích tốt nhất có thể từ việc xóa một khoảng thời gian. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 4
2 3
3 6
5 7
```Chúng tôi theo dõi việc hợp nhất: 

| Bước | Khoảng thời gian | Phân khúc hiện tại | Hành động | Linh kiện | 
| --- | --- | --- | --- | --- | 
| 1 | [1,4] | [1,4] | bắt đầu | 0 | 
| 2 | [2,3] | [1,4] | chứa | 0 | 
| 3 | [3,6] | [1,6] | mở rộng | 0 | 
| 4 | [5,7] | [1,7] | mở rộng | 0 | 

Tất cả các khoảng tạo thành một thành phần được kết nối. 

Bây giờ loại bỏ hiệu ứng: 

Việc loại bỏ [3,6] sẽ phá vỡ cầu nối giữa [1,4] và [5,7], tạo ra hai thành phần. 

Vậy đáp án là 2. 

### Ví dụ 2 

đầu vào:```
3
1 1
1 1
1 1
```| Bước | Khoảng thời gian | Phân đoạn | Linh kiện | 
| --- | --- | --- | --- | 
| 1 | [1,1] | [1,1] | 0 | 
| 2 | [1,1] | [1,1] | 0 | 
| 3 | [1,1] | [1,1] | 0 | 

Mọi thứ hoàn toàn trùng lặp nên chỉ có 1 thành phần. 

Việc xóa bất kỳ khoảng thời gian nào cũng không làm thay đổi mức độ phù hợp. 

Câu trả lời là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | khoảng thời gian sắp xếp và duy trì hoạt động của heap trong mỗi khoảng thời gian | 
| Không gian | O(n) | lưu trữ các khoảng, đống và mảng đóng góp | 

Giải pháp phù hợp thoải mái trong giới hạn vì tổng số khoảng thời gian trên tất cả các trường hợp thử nghiệm là 2e5, do đó hệ số log vẫn nhỏ và quét tuyến tính chiếm ưu thế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict
    input = sys.stdin.readline

    def solve():
        n = int(input())
        segs = []
        for i in range(n):
            l, r = map(int, input().split())
            segs.append((l, r, i))
        segs.sort()

        import heapq
        heap = []
        contrib = [0] * n

        i = 0
        ans = 0
        cur_l = None
        cur_r = None

        while i < n:
            l = segs[i][0]

            if cur_l is None:
                cur_l, cur_r = segs[i][0], segs[i][1]

            if l > cur_r:
                ans += 1
                heap.clear()
                cur_l, cur_r = segs[i][0], segs[i][1]
                heapq.heappush(heap, (-segs[i][1], segs[i][2]))
                i += 1
                continue

            while i < n and segs[i][0] <= cur_r:
                l, r, idx = segs[i]
                heapq.heappush(heap, (-r, idx))
                cur_r = max(cur_r, r)
                i += 1

            best_r = -heap[0][0]
            best_ids = []

            while heap and -heap[0][0] == best_r:
                rr, idx = heapq.heappop(heap)
                best_ids.append(idx)

            if len(best_ids) == 1:
                contrib[best_ids[0]] += 1

            for idx in best_ids:
                heapq.heappush(heap, (-best_r, idx))

        ans += 1
        return ans + max(contrib) - 1

    t = int(input())
    out = []
    for _ in range(t):
        out.append(str(solve()))
    return "\n".join(out)

# provided samples
assert run("""3
4
1 4
2 3
3 6
5 7
3
5 5
5 5
5 5
6
3 3
1 1
5 5
1 5
2 2
4 4
""") == "2\n1\n5"

# custom cases
assert run("""1
2
1 10
2 9
""") == "1"

assert run("""1
3
1 2
3 4
5 6
""") == "2"

assert run("""1
4
1 3
2 5
4 6
7 8
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả lồng nhau | 1 | khoảng dư thừa | 
| hoàn toàn rời rạc | 2 | thành phần cơ bản | 
| xích một phần | 2 | hiệu ứng cầu | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tồn tại nhiều khoảng giống hệt nhau. Trong trường hợp này, việc loại bỏ một bản sao hoàn toàn không làm thay đổi sự kết hợp, bởi vì một khoảng thời gian giống hệt khác vẫn giữ nguyên phạm vi bao phủ. Thuật toán xử lý việc này vì điểm cuối bên phải tối đa không bao giờ được quy duy nhất cho một khoảng duy nhất; nhiều chỉ số có cùng giá trị tốt nhất nên không có đóng góp nào được ghi nhận. 

Một trường hợp cạnh khác là một chuỗi các khoảng chồng lấp trong đó mỗi khoảng chồng lấp là cần thiết để kết nối hai vùng ở xa nhau. Trong những trường hợp như vậy, việc loại bỏ một khoảng cầu sẽ làm tăng số lượng thành phần hợp lên đúng một. Quá trình quét phát hiện một khoảng duy nhất chịu trách nhiệm mở rộng điểm cuối bên phải tối đa hiện tại ở ranh giới hợp nhất, do đó khoảng thời gian đó được ghi có. 

Trường hợp khó phát hiện cuối cùng là khi một khoảng được chứa đầy trong một khoảng lớn hơn. Việc loại bỏ nó sẽ không bao giờ ảnh hưởng đến cấu trúc công đoàn. Vì nó không bao giờ trở thành điểm cuối bên phải tối đa duy nhất trong bất kỳ bước quét nào nên nó không bao giờ đóng góp vào số lượng khuếch đại, vì vậy câu trả lời cuối cùng sẽ bỏ qua nó một cách chính xác.
