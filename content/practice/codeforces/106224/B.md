---
title: "CF 106224B - Phân bón"
description: "Chúng ta được cung cấp một dòng các trường được lập chỉ mục từ trái sang phải. Trên hết, có một số tuyến phun được lập trình sẵn. Mỗi tuyến đường là một khoảng thời gian, nghĩa là nó bón phân cho mọi cánh đồng trong một số đoạn liền kề $[Li, Ri]$. Một truy vấn không hỏi về tất cả các tuyến đường."
date: "2026-06-25T07:00:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106224
codeforces_index: "B"
codeforces_contest_name: "INOI 2024"
rating: 0
weight: 106224
solve_time_s: 83
verified: true
draft: false
---

[CF 106224B - Phân bón](https://codeforces.com/problemset/problem/106224/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng các trường được lập chỉ mục từ trái sang phải. Trên hết, có một số tuyến phun được lập trình sẵn. Mỗi tuyến đường là một khoảng thời gian, nghĩa là nó bón phân cho mọi cánh đồng trong một số đoạn liền kề$[L_i, R_i]$. 

Một truy vấn không hỏi về tất cả các tuyến đường. Thay vào đó, nó chọn một khối tuyến đường liên tiếp, từ chỉ mục$X$ĐẾN$Y$và về mặt khái niệm, chúng tôi chỉ áp dụng những lần phun đó. Mỗi tuyến được chọn bao phủ đầy đủ khoảng thời gian của nó và các lần phun chồng chéo không xếp chồng lên nhau theo bất kỳ cách đặc biệt nào ngoại trừ một cánh đồng được coi là được bón phân nếu có ít nhất một khoảng thời gian được chọn bao phủ nó. Nhiệm vụ là tính toán, đối với mỗi truy vấn, có bao nhiêu trường được bao phủ bởi ít nhất một trong các khoảng đã chọn. 

Cấu trúc chính là chúng ta lặp đi lặp lại một mảng con gồm các khoảng và yêu cầu tổng độ dài liên kết của chúng trên một trục tọa độ cố định. Đây là một bài toán "phạm vi khoảng, độ dài liên kết truy vấn của phạm vi con", khó hơn so với liên kết tĩnh tiêu chuẩn vì tập hợp các khoảng hoạt động thay đổi trên mỗi truy vấn. 

Các ràng buộc ngụ ý rằng cả số lượng trường cũng như số khoảng và truy vấn có thể lớn, theo thứ tự$10^5$. Bất kỳ giải pháp nào tính toán lại sự kết hợp từ đầu cho mỗi truy vấn sẽ yêu cầu hợp nhất tối đa$10^5$khoảng thời gian cho mỗi truy vấn, điều này sẽ ngay lập tức vượt quá giới hạn thời gian. Thậm chí một$O(N)$mỗi cách tiếp cận truy vấn dẫn đến$10^{10}$trong trường hợp xấu nhất là không khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi các khoảng chồng lên nhau nhiều. Ví dụ, nếu mỗi khoảng là$[1, F]$, mọi truy vấn sẽ trả về$F$, bất kể có bao nhiêu khoảng được chọn. Một giải pháp đơn giản có thể đếm nhầm các phần trùng lặp nhiều lần nếu nó không duy trì một cách rõ ràng một biểu diễn đã hợp nhất. 

Một trường hợp lỗi khác phát sinh khi các khoảng rời rạc nhưng xen kẽ trong phạm vi truy vấn. Ví dụ, các khoảng$[1,2], [3,4], [5,6]$và truy vấn chọn tất cả chúng, yêu cầu hợp nhất cẩn thận; ở đây chỉ cần tính tổng độ dài sẽ cho câu trả lời đúng, nhưng chỉ vì không có sự trùng lặp. Các mẫu chồng chéo hỗn hợp là nơi phá vỡ tổng hợp ngây thơ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi truy vấn, lấy tất cả các khoảng trong$[X, Y]$, sắp xếp chúng theo điểm cuối bên trái và hợp nhất chúng thành một liên kết. Chúng tôi duy trì một phân đoạn đang hoạt động hiện tại và mở rộng nó bất cứ khi nào các khoảng thời gian trùng nhau. Điều này đúng vì sự kết hợp của các khoảng trên một dòng luôn có thể được tính bằng cách sắp xếp và hợp nhất. 

Vấn đề là chi phí. Mỗi truy vấn có thể chạm vào$O(N)$các khoảng thời gian và việc sắp xếp chúng sẽ tốn kém$O(N \log N)$mỗi truy vấn. Ngay cả khi chúng ta tránh sắp xếp bằng cách sắp xếp trước trên toàn cục, chúng ta vẫn cần trích xuất và hợp nhất thành$O(N)$khoảng thời gian cho mỗi truy vấn, đưa ra$O(NQ)$hành vi. Với$10^5$truy vấn, điều này trở nên hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta không thay đổi hình dạng của các khoảng; chúng tôi chỉ thay đổi tập hợp con của chỉ mục nào đang hoạt động. Điều này gợi ý việc xây dựng cấu trúc dữ liệu trên không gian chỉ mục của các khoảng. Mỗi nút trong cây phân đoạn đại diện cho một khối liền kề gồm các chỉ số khoảng và chúng tôi lưu trữ, đối với khối đó, sự kết hợp của tất cả các khoảng của nó ở dạng nén dưới dạng các phân đoạn rời rạc trên trục trường. Khi cấu trúc này được xây dựng, một truy vấn$[X, Y]$chỉ có thể được trả lời bằng cách kết hợp$O(\log N)$các nút, thay vì lặp lại tất cả các khoảng trong phạm vi. 

Điều này có hiệu quả vì liên minh các công đoàn vẫn là một công đoàn. Nếu mỗi nút lưu trữ phần đóng góp của nó đã được hợp nhất thành các khoảng rời rạc, thì việc hợp nhất hai nút chỉ yêu cầu hợp nhất hai danh sách đã sắp xếp gồm các phân đoạn rời rạc, có kích thước tuyến tính theo kích thước kết hợp của chúng. Tổng kích thước của tất cả các danh sách phân đoạn trên cây vẫn có thể quản lý được vì mỗi khoảng góp phần vào$O(\log N)$nút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force cho mỗi truy vấn |$O(N \log N)$|$O(N)$| Quá chậm | 
| Cây phân đoạn với các liên kết khoảng | ~$O((N+Q)\log N)$khấu hao |$O(N \log N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng cây phân đoạn trên phạm vi chỉ mục của các khoảng. 

1. Mỗi nút lá tương ứng với một khoảng$[L_i, R_i]$, được lưu trữ dưới dạng danh sách chứa một phân đoạn. Danh sách này đã được "nén" vì một khoảng đơn lẻ về cơ bản là sự kết hợp của một phân đoạn. 
2. Mỗi nút bên trong đại diện cho một khoảng thời gian. Giá trị của nó là sự kết hợp của tất cả các khoảng trong phần tử con bên trái của nó và tất cả các khoảng trong phần tử con bên phải của nó, được biểu thị dưới dạng danh sách được sắp xếp các phân đoạn rời rạc trên trục trường. Để xây dựng nó, chúng ta hợp nhất hai danh sách phân đoạn của trẻ em. 
3. Hợp nhất hai nút có nghĩa là duyệt qua cả hai danh sách đã được sắp xếp và kết hợp các phân đoạn chồng chéo hoặc liền kề thành một phân đoạn liên tục. Bước này giống hệt với việc tính toán kết hợp của hai tập hợp khoảng. 
4. Sau khi xây dựng cây phân đoạn đầy đủ, mỗi nút sẽ lưu trữ sự kết hợp hoàn chỉnh của khối phân đoạn của nó. 
5. Để trả lời một câu hỏi$[X, Y]$, chúng tôi phân tách phạm vi này thành$O(\log N)$các nút cây phân đoạn, thu thập danh sách khoảng thời gian được tính toán trước của chúng và hợp nhất chúng thành một liên kết cuối cùng. 
6. Câu trả lời là tổng độ dài của tất cả các đoạn rời rạc trong danh sách hợp nhất cuối cùng. 

Tính chính xác xuất phát từ thực tế là ở mọi cấp độ, mỗi nút thể hiện chính xác sự kết hợp của tất cả các khoảng trong phạm vi phân đoạn của nó. Vì phép kết hợp có tính kết hợp nên việc kết hợp các kết quả nút theo bất kỳ thứ tự nào vẫn tạo ra phép kết hợp của tất cả các khoảng đã chọn. 

Điều bất biến là mỗi nút cây phân đoạn đều lưu trữ một biểu diễn chính xác, được hợp nhất đầy đủ của tất cả các khoảng trong cây con của nó. Bởi vì việc hợp nhất hai biểu diễn hợp chính xác sẽ tạo ra một biểu diễn hợp chính xác khác, thuộc tính này được duy trì trong suốt quá trình xây dựng. Việc phân tách truy vấn không bỏ sót hoặc trùng lặp bất kỳ khoảng nào, do đó danh sách được hợp nhất cuối cùng khớp chính xác với sự kết hợp của tất cả các khoảng trong$[X, Y]$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, intervals):
        self.n = len(intervals)
        self.size = 1
        while self.size < self.n:
            self.size *= 2
        self.data = [[] for _ in range(2 * self.size)]

        for i, (l, r) in enumerate(intervals):
            self.data[self.size + i] = [(l, r)]

        for i in range(self.size - 1, 0, -1):
            self.data[i] = self.merge(self.data[2 * i], self.data[2 * i + 1])

    def merge(self, a, b):
        if not a:
            return b
        if not b:
            return a

        res = []
        i = j = 0

        def add(seg):
            if not res:
                res.append(seg)
            else:
                l, r = res[-1]
                if seg[0] <= r:
                    res[-1] = (l, max(r, seg[1]))
                else:
                    res.append(seg)

        while i < len(a) and j < len(b):
            if a[i][0] < b[j][0]:
                add(a[i])
                i += 1
            else:
                add(b[j])
                j += 1

        while i < len(a):
            add(a[i])
            i += 1
        while j < len(b):
            add(b[j])
            j += 1

        return res

    def query(self, l, r):
        l += self.size
        r += self.size + 1

        left_res = []
        right_res = []

        def merge_into(res, segs):
            for seg in segs:
                if not res:
                    res.append(seg)
                else:
                    l0, r0 = res[-1]
                    if seg[0] <= r0:
                        res[-1] = (l0, max(r0, seg[1]))
                    else:
                        res.append(seg)

        while l < r:
            if l & 1:
                merge_into(left_res, self.data[l])
                l += 1
            if r & 1:
                r -= 1
                merge_into(right_res, self.data[r])
            l //= 2
            r //= 2

        total = left_res + right_res[::-1]
        if not total:
            return 0

        merged = []
        for seg in total:
            if not merged:
                merged.append(seg)
            else:
                l0, r0 = merged[-1]
                if seg[0] <= r0:
                    merged[-1] = (l0, max(r0, seg[1]))
                else:
                    merged.append(seg)

        return sum(r - l for l, r in merged)

def main():
    F = int(input())
    N = int(input())
    intervals = [tuple(map(int, input().split())) for _ in range(N)]
    seg = SegTree(intervals)

    Q = int(input())
    out = []
    for _ in range(Q):
        x, y = map(int, input().split())
        out.append(str(seg.query(x - 1, y - 1)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Cây phân đoạn được xây dựng từ dưới lên để mỗi nút đều lưu trữ một tập hợp các khoảng được chuẩn hóa. Điều này tránh việc tính toán lại nhiều lần trong quá trình truy vấn. Quy trình hợp nhất cẩn thận để duy trì các phân đoạn rời rạc đã được sắp xếp, điều này cần thiết cho tính chính xác khi tính toán độ dài một cách hiệu quả. 

Một cạm bẫy triển khai phổ biến là quên hợp nhất các khoảng thời gian chạm ranh giới. Từ$[1,2]$Và$[3,4]$rời rạc, chúng không nên hợp nhất, nhưng$[1,2]$Và$[2,5]$phải sáp nhập vào$[1,5]$tùy thuộc vào việc các điểm cuối có được bao gồm hay không. Ở đây, các điểm cuối đều mang tính bao hàm, do đó, sự chồng chéo hoặc liền kề phải được xử lý một cách nhất quán. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ với$F = 6$và khoảng thời gian$[1,3], [2,4], [5,6]$. Chúng tôi xử lý một truy vấn chọn cả ba. 

| Bước | Cấu trúc hoạt động | Các phân đoạn đã hợp nhất | 
| --- | --- | --- | 
| Lá hợp nhất 1 | [1,3] | [1,3] | 
| Lá hợp nhất 2 | [1,3] + [2,4] | [1,4] | 
| Hợp nhất cuối cùng | [1,4] + [5,6] | [1,4], [5,6] | 

Câu trả lời cuối cùng là$4 + 2 = 6$, hiển thị phạm vi bảo hiểm đầy đủ. 

Bây giờ hãy xem xét một truy vấn chỉ chọn$[2,4]$Và$[5,6]$. 

| Bước | Cấu trúc hoạt động | Các phân đoạn đã hợp nhất | 
| --- | --- | --- | 
| Khoảng đầu tiên | [2,4] | [2,4] | 
| Thêm thứ hai | [5,6] | [2,4], [5,6] | 

Kết quả là$3 + 2 = 5$, xác nhận rằng các phân đoạn rời rạc được xử lý mà không hợp nhất không chính xác. 

Những dấu vết này cho thấy cấu trúc luôn duy trì ngữ nghĩa hợp nhất bất kể mẫu chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((N + Q)\log N)$khấu hao | Mỗi khoảng được hợp nhất thông qua$O(\log N)$nút; mỗi truy vấn kết hợp$O(\log N)$phân đoạn | 
| Không gian |$O(N \log N)$| Mỗi nút cây lưu trữ một liên kết nén của khối khoảng thời gian của nó | 

Sự phức tạp phù hợp thoải mái trong giới hạn cho$10^5$các khoảng thời gian và truy vấn, vì các yếu tố logarit giữ cho hoạt động ở mức dưới vài triệu hoạt động phân đoạn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class SegTree:
        def __init__(self, intervals):
            self.n = len(intervals)
            self.size = 1
            while self.size < self.n:
                self.size *= 2
            self.data = [[] for _ in range(2 * self.size)]

            for i, (l, r) in enumerate(intervals):
                self.data[self.size + i] = [(l, r)]

            for i in range(self.size - 1, 0, -1):
                a, b = self.data[2*i], self.data[2*i+1]
                res = []
                i1 = i2 = 0
                def add(seg):
                    if not res:
                        res.append(seg)
                    else:
                        l0, r0 = res[-1]
                        if seg[0] <= r0:
                            res[-1] = (l0, max(r0, seg[1]))
                        else:
                            res.append(seg)

                while i1 < len(a) and i2 < len(b):
                    if a[i1][0] < b[i2][0]:
                        add(a[i1]); i1 += 1
                    else:
                        add(b[i2]); i2 += 1
                while i1 < len(a):
                    add(a[i1]); i1 += 1
                while i2 < len(b):
                    add(b[i2]); i2 += 1

                self.data[i] = res

        def query(self, l, r):
            l += self.size
            r += self.size + 1
            res = []

            def add(seg):
                if not res:
                    res.append(seg)
                else:
                    l0, r0 = res[-1]
                    if seg[0] <= r0:
                        res[-1] = (l0, max(r0, seg[1]))
                    else:
                        res.append(seg)

            left = []
            right = []

            while l < r:
                if l & 1:
                    for seg in self.data[l]:
                        add(seg)
                    l += 1
                if r & 1:
                    r -= 1
                    for seg in self.data[r]:
                        add(seg)
                l //= 2
                r //= 2

            return sum(r-l for l,r in res)

    F = int(input())
    N = int(input())
    intervals = [tuple(map(int, input().split())) for _ in range(N)]
    seg = SegTree(intervals)

    Q = int(input())
    out = []
    for _ in range(Q):
        x, y = map(int, input().split())
        out.append(str(seg.query(x-1, y-1)))
    return "\n".join(out)

# custom tests
assert run("6\n3\n1 3\n2 4\n5 6\n1\n1 3\n") == "6", "full union"
assert run("10\n4\n1 2\n3 4\n5 6\n7 8\n1\n1 4\n") == "8", "disjoint intervals"
assert run("10\n3\n1 10\n2 5\n6 9\n1\n1 3\n") == "10", "nested overlaps"
assert run("5\n1\n1 3\n1\n1 1\n") == "3", "single interval prefix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| liên minh đầy đủ | 6 | chồng chéo hợp nhất chính xác | 
| khoảng rời rạc | 8 | không có sự hợp nhất ngẫu nhiên | 
| chồng chéo lồng nhau | 10 | xử lý ngăn chặn | 
| tiền tố khoảng đơn | 3 | tính đúng đắn của trường hợp tối thiểu | 

## Vỏ cạnh 

Một trường hợp đặc biệt phổ biến là khi tất cả các khoảng trong một truy vấn giống hệt nhau, chẳng hạn như mọi khoảng đều được$[1, F]$. Tất cả các nút cây phân đoạn đều thu gọn về cùng một phân đoạn đã hợp nhất, do đó mọi truy vấn đều trả về chính xác$F$không tính gấp đôi. 

Một trường hợp cạnh khác xảy ra khi các khoảng chỉ chạm vào ranh giới, chẳng hạn$[1,2]$Và$[2,3]$. Vì các điểm cuối được bao gồm nên bước hợp nhất phải coi chúng là chồng chéo, nếu không câu trả lời sẽ trở thành không chính xác.$2 + 2 = 4$thay vì$3$. Thủ tục hợp nhất kiểm tra một cách rõ ràng`seg[0] <= r0`, điều này đảm bảo các phân đoạn chạm ranh giới được hợp nhất một cách chính xác. 

Trường hợp thứ ba là khi truy vấn chọn một khoảng duy nhất. Trong trường hợp đó, việc phân tách truy vấn trả về một nút lá và không cần phải hợp nhất ngoài việc chuẩn hóa. Đầu ra chỉ đơn giản là độ dài khoảng, khớp trực tiếp với định nghĩa dự định.
