---
title: "CF 105457B - Quần đảo và Núi"
description: "Chúng ta có một đường đất được chia thành $n$ các đoạn liên tiếp, mỗi đoạn có chiều cao cố định. Theo thời gian, mực nước biển dâng lên theo từng bậc và sau mỗi lần dâng lên, chúng ta phải xác định xem còn lại bao nhiêu nhóm đất khô được kết nối với nhau."
date: "2026-06-23T17:46:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105457
codeforces_index: "B"
codeforces_contest_name: "XXIII Spain Olympiad in Informatics, Online Qualifier 1"
rating: 0
weight: 105457
solve_time_s: 78
verified: true
draft: false
---

[CF 105457B - Quần đảo và Núi](https://codeforces.com/problemset/problem/105457/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một mảnh đất được chia thành$n$các đoạn liên tiếp, mỗi đoạn có chiều cao cố định. Theo thời gian, mực nước biển dâng lên theo từng bậc và sau mỗi lần dâng lên, chúng ta phải xác định xem còn lại bao nhiêu nhóm đất khô được kết nối với nhau. 

Một đoạn được coi là khô nếu độ cao của nó vượt quá điều kiện ngưỡng mực nước biển hiện tại$a_i > m$, nếu không thì nó bị chìm. Các đoạn khô liền kề với đường ban đầu tạo thành một hòn đảo duy nhất, do đó, một hòn đảo chỉ đơn giản là một khối liền kề tối đa của các chỉ số có độ cao cao hơn mực nước biển hiện tại. 

Sau mỗi năm, một số đoạn bị ngập vĩnh viễn khi mực nước biển dâng cao, điều này chỉ có thể phá vỡ các hòn đảo hiện có thành những hòn đảo nhỏ hơn hoặc loại bỏ chúng hoàn toàn. 

Đầu vào bao gồm nhiều trường hợp độc lập. Mỗi trường hợp đưa ra một mảng chiều cao và trình tự mực nước biển tăng dần. Đối với mỗi mực nước biển, chúng ta phải xuất ra số lượng thành phần khô được kết nối. 

Các ràng buộc làm cho cấu trúc rõ ràng:$n$Và$k$có thể đạt được$10^5$, do đó việc tính toán lại các đảo từ đầu sau mỗi truy vấn sẽ quá chậm. Bất kỳ giải pháp nào quét liên tục mảng cho mỗi truy vấn sẽ tốn kém$O(nk)$, đạt tới$10^{10}$hoạt động trong trường hợp xấu nhất và không thể thực hiện được. 

Một trường hợp thất bại tinh tế đối với các phương pháp tiếp cận đơn giản là việc tính toán lại mà không nhớ trạng thái trước đó. Ví dụ, hãy xem xét chiều cao$[5, 1, 5]$và mực nước biển$1$sau đó$5$. Ở cấp độ$1$, ta có hai đảo: vị trí 0 và 2 khô. Ở cấp độ$5$, mọi thứ đều chìm trong nước nên câu trả lời là$0$. Ở đây, một lần quét đơn giản cho mỗi truy vấn vẫn hoạt động, nhưng nhiều tối ưu hóa không chính xác cố gắng "cập nhật số lượng cục bộ" mà không theo dõi rằng việc tràn giữa sẽ hợp nhất các sự kiện phân tách một cách chính xác theo lý luận ngược, phá vỡ tính nhất quán khi nhiều điểm tràn vào cùng một lúc. 

Một trường hợp cạnh quan trọng khác là chiều cao bằng nhau. Vì lũ lụt xảy ra khi$a_i \le m_i$, các phân đoạn biến mất chính xác ở ngưỡng độ cao của chúng và việc không xử lý bình đẳng một cách chính xác sẽ dẫn đến số lượng đảo lệch nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ tính toán lại số đảo sau mỗi mực nước biển bằng cách quét toàn bộ mảng và đếm số lần một đoạn khô bắt đầu. Điều này hiệu quả vì mỗi hòn đảo là một dãy các tế bào khô liền kề nhau, vì vậy chúng ta có thể phát hiện một hòn đảo mới bất cứ khi nào$a_i > m$và một trong hai$i = 0$hoặc$a_{i-1} \le m$. Độ chính xác rất đơn giản, nhưng việc thực hiện điều này cho mọi truy vấn sẽ lặp lại cùng một quá trình truyền tải đầy đủ$k$lần. 

Nút thắt là hiển nhiên: mỗi truy vấn đều tốn chi phí$O(n)$, do đó tổng độ phức tạp trở thành$O(nk)$. Với cả hai lên đến$10^5$, điều này vượt xa mọi giới hạn thực tế. 

Quan sát quan trọng là lũ lụt có tính chất đơn điệu. Khi một đoạn bị nhấn chìm, nó sẽ không bao giờ quay trở lại. Thay vì mô phỏng quá trình về phía trước, chúng tôi đảo ngược góc nhìn: hãy tưởng tượng toàn bộ vùng đất ban đầu bị ngập và chúng tôi “rút lũ” các đoạn theo thứ tự giảm dần của mực nước biển hoặc xử lý tương đương các đoạn bằng cách tăng độ cao. 

Khi chúng tôi sắp xếp các phân đoạn theo độ cao, mỗi lần chúng tôi “kích hoạt” một phân đoạn (có nghĩa là nó sẽ trở nên khô khi mực nước biển vượt qua nó), nó sẽ tạo thành một hòn đảo mới hoặc hợp nhất hai hòn đảo hiện có nếu cả hai hòn đảo lân cận đều đã hoạt động. Đây là một quá trình kết nối động cổ điển trên một đường dây, có thể được duy trì bằng cấu trúc rời rạc. 

Để kết nối điều này với các truy vấn, chúng tôi xử lý các kích hoạt theo thứ tự độ cao tăng dần đồng thời xử lý các truy vấn theo thứ tự mực nước biển tăng dần. Đối với mỗi ngưỡng truy vấn, chúng tôi kích hoạt tất cả các phân đoạn có chiều cao lớn hơn ngưỡng đó, duy trì các thành phần được kết nối. 

Mỗi lần kích hoạt sẽ tăng số lượng đảo lên một hoặc giảm số lượng đảo tùy thuộc vào việc hàng xóm có hoạt động hay không. Union-find trên một dòng đảm bảo mỗi lần hợp nhất là$O(\alpha(n))$, đưa ra nghiệm gần như tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nk)$|$O(1)$| Quá chậm | 
| Sắp xếp + quét DSU |$O((n+k)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển vấn đề thành một quá trình kích hoạt ngược trong đó đất xuất hiện dần dần từ cao nhất đến thấp nhất. 

1. Ghép từng chỉ số phân đoạn với chiều cao của nó và sắp xếp các cặp này theo chiều cao theo thứ tự giảm dần. Điều này cho phép chúng tôi kích hoạt vùng đất từ ​​​​cao nhất đến ngắn nhất, đảm bảo rằng khi một phân khúc hoạt động thì tất cả các phân khúc cao hơn đều đã hoạt động. Thứ tự này rất quan trọng vì cấu trúc đảo chỉ phụ thuộc vào việc các đảo lân cận có hoạt động hay không chứ không phụ thuộc vào độ cao tuyệt đối. 
2. Sắp xếp truy vấn theo mực nước biển theo thứ tự giảm dần trong khi vẫn giữ nguyên các chỉ số gốc. Mỗi truy vấn đại diện cho một ngưỡng; chúng tôi sẽ kích hoạt tất cả các phân đoạn vượt quá ngưỡng đó trước khi trả lời. 
3. Duy trì mảng boolean`active[i]`cho biết liệu phân đoạn$i$hiện đang khô. Đồng thời duy trì cấu trúc liên kết được thiết lập rời rạc trên các chỉ mục để theo dõi các thành phần được kết nối giữa các phân đoạn đang hoạt động. 
4. Giữ một quầy`islands`được khởi tạo về 0. Mỗi khi một phân khúc mới hoạt động, chúng tôi tạm coi nó như một hòn đảo mới. 
5. Khi kích hoạt vị trí$i$, kiểm tra hàng xóm bên trái của nó$i-1$và hàng xóm bên phải$i+1$. Nếu một hàng xóm đang hoạt động, chúng ta sẽ hợp nhất các tập hợp. Mỗi khi hai thành phần riêng biệt trước đó hợp nhất, chúng tôi sẽ giảm số lượng đảo đi một. Điều này đảm bảo số lượng luôn khớp với số lượng thành phần hoạt động được kết nối. 
6. Xử lý các truy vấn theo thứ tự giảm dần. Đối với mỗi giá trị truy vấn$m$, kích hoạt tất cả các đoạn có chiều cao lớn hơn$m$. Sau khi xử lý tất cả các kích hoạt như vậy, hãy lưu trữ thông tin hiện tại`islands`như câu trả lời cho truy vấn đó. 
7. Khôi phục câu trả lời về thứ tự truy vấn ban đầu. 

Bất biến cốt lõi là tại bất kỳ thời điểm nào, các thành phần DSU đều tương ứng chính xác với các khối chỉ số hoạt động liền kề. Mỗi thành phần đại diện cho một hòn đảo vì vùng lân cận là kết nối duy nhất được phép. Khi chúng tôi kích hoạt một ô mới, nó sẽ tạo ra một thành phần biệt lập mới hoặc hợp nhất với tối đa hai ô lân cận, điều này sẽ cập nhật chính xác số lượng đảo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return False
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]
        return True

def solve_case(n, k, a, m):
    segs = sorted([(a[i], i) for i in range(n)], reverse=True)
    queries = sorted([(m[i], i) for i in range(k)], reverse=True)

    dsu = DSU(n)
    active = [False] * n

    ans = [0] * k
    ptr = 0
    islands = 0

    for qval, qi in queries:
        while ptr < n and segs[ptr][0] > qval:
            h, i = segs[ptr]
            active[i] = True
            islands += 1

            if i > 0 and active[i - 1]:
                if dsu.union(i, i - 1):
                    islands -= 1
            if i + 1 < n and active[i + 1]:
                if dsu.union(i, i + 1):
                    islands -= 1

            ptr += 1

        ans[qi] = islands

    return ans

def main():
    data = sys.stdin.read().strip().split()
    idx = 0
    out = []

    while idx < len(data):
        n = int(data[idx]); k = int(data[idx + 1])
        idx += 2
        a = list(map(int, data[idx:idx + n]))
        idx += n
        m = list(map(int, data[idx:idx + k]))
        idx += k

        res = solve_case(n, k, a, m)
        out.append(" ".join(map(str, res)))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```DSU theo dõi kết nối giữa các phân đoạn hiện đang hoạt động. các`active`mảng đảm bảo chúng tôi chỉ hợp nhất những vùng đất đã không bị ngập. Mỗi liên kết tương ứng với hai hòn đảo hợp nhất, do đó việc giảm bộ đếm sẽ duy trì tính chính xác. 

Một chi tiết triển khai tinh tế là sự so sánh chặt chẽ`a[i] > m`. Đây là lý do tại sao kích hoạt sử dụng`> qval`. Nếu chúng ta sử dụng nhầm`>=`, các phân đoạn chính xác ở mực nước sẽ được xử lý không chính xác và sẽ vẫn hoạt động quá lâu, làm tăng số lượng đảo. 

Con trỏ`ptr`đảm bảo mỗi phân đoạn được kích hoạt một lần trên tất cả các truy vấn, giúp quá trình quét trở nên tuyến tính sau khi sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 7, k = 1
a = [9, 1, 6, 1, 3, 7, 2]
m = [5]
```Phân đoạn được sắp xếp theo chiều cao: 

| bước | chiều cao | chỉ mục | bộ hoạt động | đảo | 
| --- | --- | --- | --- | --- | 
| 1 | 9 | 0 | {0} | 1 | 
| 2 | 7 | 5 | {0,5} | 2 | 
| 3 | 6 | 2 | {0,2,5} | 3 | 
| 4 | 3 | 4 | {0,2,4,5} | 3 (hợp nhất nếu không liền kề) | 
| 5 | 2 | 6 | {0,2,4,5,6} | 3 | 
| 6 | 1 | 1 | {0,1,2,4,5,6} | 2 | 
| 7 | 1 | 3 | tất cả | 1 | 

Đối với ngưỡng 5, chỉ các giá trị lớn hơn 5 mới được kích hoạt: 9, 7, 6. Các thành phần biểu mẫu này {0}, {5}, {2}, tạo ra 3 hòn đảo. 

Dấu vết này cho thấy các đảo tương ứng chính xác với các thành phần được kết nối trong bộ được kích hoạt. 

### Ví dụ 2 

đầu vào:```
n = 5, k = 3
a = [2, 2, 2, 2, 2]
m = [1, 2, 3]
```| truy vấn | chỉ số kích hoạt | đảo | 
| --- | --- | --- | 
| 3 | không | 0 | 
| 2 | không có (nghiêm ngặt > 2) | 0 | 
| 1 | tất cả | 1 | 

Trường hợp này thể hiện rõ ràng hành vi bất đẳng thức nghiêm ngặt. Ở cấp độ 2, mọi thứ đều chìm trong nước nên không có hòn đảo nào tồn tại. Chỉ khi ngưỡng giảm xuống dưới 2 thì đất mới xuất hiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + k)\log n)$| sắp xếp các phân đoạn và truy vấn chiếm ưu thế; Hoạt động DSU gần như không đổi | 
| Không gian |$O(n)$| Mảng DSU và trạng thái kích hoạt | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn vì mỗi phần tử được sắp xếp một lần và các phép toán tìm hợp được khấu hao nghịch đảo với thời gian Ackermann, có hiệu quả không đổi trong thực tế ngay cả đối với$10^5$các phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import deque

    # assume solution is defined above in same runtime
    return main_capture(inp)

def main_capture(inp: str) -> str:
    import sys
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().strip().split()
    idx = 0
    out = []

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n))
            self.size = [1] * n

        def find(self, x):
            while self.parent[x] != x:
                self.parent[x] = self.parent[self.parent[x]]
                x = self.parent[x]
            return x

        def union(self, a, b):
            ra, rb = self.find(a), self.find(b)
            if ra == rb:
                return False
            if self.size[ra] < self.size[rb]:
                ra, rb = rb, ra
            self.parent[rb] = ra
            self.size[ra] += self.size[rb]
            return True

    def solve_case(n, k, a, m):
        segs = sorted([(a[i], i) for i in range(n)], reverse=True)
        queries = sorted([(m[i], i) for i in range(k)], reverse=True)

        dsu = DSU(n)
        active = [False] * n
        ans = [0] * k
        ptr = 0
        islands = 0

        for qval, qi in queries:
            while ptr < n and segs[ptr][0] > qval:
                h, i = segs[ptr]
                active[i] = True
                islands += 1

                if i > 0 and active[i - 1]:
                    if dsu.union(i, i - 1):
                        islands -= 1
                if i + 1 < n and active[i + 1]:
                    if dsu.union(i, i + 1):
                        islands -= 1

                ptr += 1

            ans[qi] = islands

        return ans

    while idx < len(data):
        n = int(data[idx]); k = int(data[idx + 1])
        idx += 2
        a = list(map(int, data[idx:idx + n]))
        idx += n
        m = list(map(int, data[idx:idx + k]))
        idx += k
        out.append(" ".join(map(str, solve_case(n, k, a, m))))

    return "\n".join(out)

# provided samples
assert run("""7 1
9 1 6 1 3 7 2
5
""") == "3", "sample 1"

assert run("""10 5
9 5 4 10 3 2 5 6 2 6
3 4 6 8 9
""") == "3 4 2 2 1", "sample 2"

# custom cases
assert run("""1 1
5
4
""") == "1", "single element"

assert run("""5 1
1 2 3 4 5
5
""") == "0", "all submerged"

assert run("""5 1
1 2 3 4 5
0
""") == "1", "all active"

assert run("""6 1
5 1 5 1 5 1
3
""") == "3", "alternating heights"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | cấu trúc tối thiểu | 
| tất cả chìm trong nước | 0 | hành vi ngưỡng nghiêm ngặt | 
| tất cả đều hoạt động | 1 | sáp nhập kết nối đầy đủ | 
| độ cao xen kẽ | 3 | hình thành nhiều hòn đảo | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi nhiều phân đoạn liền kề kích hoạt trong cùng một cửa sổ truy vấn và hợp nhất ngay lập tức. Xem xét đầu vào`a = [3, 2, 1]`với ngưỡng truy vấn`0`. Tất cả các phân đoạn kích hoạt tuần tự, nhưng số đảo phải kết thúc bằng 1 chứ không phải 3. Thuật toán xử lý vấn đề này bằng cách tăng số đảo khi kích hoạt và sau đó hợp nhất ngay lập tức với các lân cận, giảm dần cho mỗi lần kết hợp thành công, đảm bảo cấu trúc được hợp nhất cuối cùng là chính xác. 

Một trường hợp khác là khi không có phân đoạn nào được kích hoạt cho một truy vấn. Nếu mực nước biển đã cao hơn mọi độ cao, con trỏ sẽ không di chuyển và câu trả lời vẫn giữ nguyên trạng thái trước đó, phản ánh chính xác rằng không có vùng đất mới nào xuất hiện. 

Trường hợp tinh vi thứ ba là có chiều cao bằng nhau với ngưỡng khớp chính xác với chúng. Bởi vì kích hoạt sử dụng so sánh nghiêm ngặt`>`, các đoạn có độ cao bằng mực nước biển vẫn chìm trong nước, ngăn chặn sự hình thành đảo sớm và giữ số lượng đảo phù hợp với định nghĩa về lũ lụt.
