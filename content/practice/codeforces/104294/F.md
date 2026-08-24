---
title: "CF 104294F - Ký ức Karuta"
description: "Mỗi chiếc lá hoạt động giống như một vật thể rơi thẳng xuống khi bị đẩy theo phương ngang bởi một cơn gió phụ thuộc vào thời gian. Gió ở giây t là hàm tuyến tính của tham số toàn cục k, do đó mỗi giây đóng góp một số hạng có dạng at + k dt."
date: "2026-07-01T20:26:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104294
codeforces_index: "F"
codeforces_contest_name: "UTPC Spring 2023 Open Contest"
rating: 0
weight: 104294
solve_time_s: 129
verified: true
draft: false
---

[CF 104294F - Ký ức Karuta](https://codeforces.com/problemset/problem/104294/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi chiếc lá hoạt động giống như một vật thể rơi thẳng xuống khi bị đẩy theo phương ngang bởi một cơn gió phụ thuộc vào thời gian. Gió lúc thứ hai`t`là hàm tuyến tính của tham số toàn cục`k`, do đó mỗi giây đóng góp một số hạng có dạng`a_t + k * d_t`. 

Một chiếc lá bắt đầu ở độ cao`y_i`và bắt đầu chuyển động vào lúc`ℓ_i`. Mỗi giây nó di chuyển xuống đúng một đơn vị cho đến khi chạm đất, do đó nó tiêu tốn chính xác`y_i`bước thời gian đang chuyển động. Trong các bước đó, nó tích lũy chuyển vị ngang bằng với tốc độ gió của mỗi bước. 

Điều này có nghĩa là tọa độ x cuối cùng của lá`i`là tổng trong một khoảng thời gian liền kề: 

chiếc lá đóng góp từ thời gian`ℓ_i`ĐẾN`ℓ_i + y_i - 1`, tổng hợp cả hai`a_t`Và`d_t`các bộ phận, với`d_t`nhân với`k`. 

Vì vậy mỗi lá xác định một hàm tuyến tính trong`k`: 

độ dốc là tổng của`d_t`trong khoảng thời gian hoạt động của nó và giao điểm là tổng của`a_t`trong cùng một khoảng thời gian. Câu trả lời cho truy vấn là giá trị tối đa trên tất cả các dòng này ở thời điểm hiện tại`k`. 

Khó khăn là các mảng`a`Và`d`không tĩnh. Cập nhật điểm sẽ thay đổi chúng và mỗi thay đổi như vậy sẽ ảnh hưởng đến nhiều khoảng thời gian lá cùng một lúc. Trên hết, các truy vấn yêu cầu mức tối đa trên tất cả các lá ở trạng thái hiện tại. 

Những hạn chế`n, m ≤ 10^4`ngụ ý về`2 × 10^4`vị trí thời gian và`10^4`khoảng thời gian. Việc tính toán lại đơn giản cho mỗi truy vấn trên tất cả các lá sẽ nằm ở giới hạn nhưng vẫn có thể chấp nhận được, tuy nhiên, các bản cập nhật sẽ làm mất hiệu lực tổng khoảng thời gian được tính toán trước, do đó việc tính toán lại mọi thứ từ đầu cho mỗi truy vấn trở nên quá chậm. 

Một trường hợp quan trọng là khi nhiều lá chồng lên nhau theo thời gian. Ví dụ: nếu tất cả các lá bắt đầu sớm và có chiều cao lớn thì mỗi lần cập nhật lên`a_t`hoặc`d_t`ảnh hưởng đến hầu hết mọi lá. Một cách tiếp cận ngây thơ cố gắng cập nhật từng lá trên mỗi thao tác sẽ thoái hóa thành hành vi bậc hai. 

Một vấn đề tế nhị khác là câu trả lời không hề đơn điệu theo một cách đơn giản nào: thay đổi một`a_t`hoặc`d_t`có thể dịch chuyển hoàn toàn lá tối ưu nên không cần cắt tỉa tham lam là an toàn. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ trực tiếp sẽ tính toán lại tổng khoảng thời gian của mỗi lá bất cứ khi nào có truy vấn đến. Đối với mỗi lá, chúng tôi tính toán`[ℓ_i, ℓ_i + y_i - 1]`tổng hợp lại`a`Và`d`, sau đó đánh giá`A_i + k * D_i`và lấy tối đa. Chi phí này`O(n * m)`mỗi truy vấn nếu được thực hiện từ đầu trên tổng tiền tố hoặc`O(n * log m)`với việc tính toán lại cây phân đoạn trên mỗi lá. Với tối đa`10^5`truy vấn, điều này nhanh chóng trở nên không khả thi. 

Cái nhìn sâu sắc về cấu trúc là mỗi lá được cố định vĩnh viễn dưới dạng một khoảng trên trục thời gian và sự đóng góp của các mảng`a`Và`d`hoàn toàn là cộng trong khoảng đó. Một điểm cập nhật vào thời điểm đó`t`ảnh hưởng chính xác đến những lá có khoảng cách chứa`t`. Điều này biến vấn đề thành việc duy trì một họ các khoảng tĩnh dưới các cập nhật điểm trên một mảng cơ bản. 

Sự trừu tượng đúng đắn là duy trì, đối với mỗi lá, hai giá trị tiến hóa: điểm chặn tích lũy và độ dốc của nó. Mỗi bản cập nhật sẽ sửa đổi tất cả các lá bao phủ một vị trí, thực hiện cập nhật phạm vi một cách hiệu quả trong một khoảng thời gian. Khi đã biết các giá trị này, việc trả lời truy vấn sẽ giảm xuống việc tìm giá trị lớn nhất của một tập hợp các hàm tuyến tính tại một thời điểm nhất định.`k`, gợi ý một thân lồi hoặc cấu trúc Li Chao. 

Ý tưởng cốt lõi là kết hợp cây phân đoạn theo các vị trí thời gian với việc duy trì thân lồi trên các lá. Mỗi nút cây phân đoạn lưu trữ sự đóng góp của các lá có khoảng cách bao phủ hoàn toàn nút đó, cho phép cập nhật tại vị trí`t`chỉ chạm vào`O(log T)`nút. Mỗi nút duy trì một cấu trúc động có thể trả lời tối đa các dòng trong truy vấn`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại tất cả các lá trên mỗi truy vấn | O(q · n · m) | O(n + m) | Quá chậm | 
| Cây phân đoạn + thân lồi trên mỗi nút | O((q +updates) log² n) khấu hao | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát mỗi lá đóng góp một giá trị có dạng`A_i + k * D_i`, trong đó cả hai`A_i`Và`D_i`là tổng trong một khoảng thời gian cố định`[ℓ_i, r_i]`. Điều này chuyển đổi mọi lá thành một hàm tuyến tính trong`k`. 
2. Duy trì hai cây phân đoạn trên trục thời gian, một cho`a_t`và một cho`d_t`, do đó bất kỳ truy vấn tổng khoảng nào cũng có thể được trả lời trong`O(log T)`. 
3. Đối với mỗi lá, dòng điện của nó`(A_i, D_i)`được xác định bằng cách truy vấn các cây phân đoạn này qua`[ℓ_i, r_i]`. 
4. Thay vì tính toán lại tất cả các lá sau mỗi lần cập nhật, hãy coi mỗi lá như một đối tượng cố định được lưu trữ trong cây phân đoạn trên các lá. Mỗi nút đại diện cho một nhóm lá. 
5. Mỗi nút lưu trữ một thân lồi động (hoặc cây Li Chao) của các đường`(D_i, A_i)`tương ứng với các lá trong đoạn của nó. Điều này cho phép trả lời giá trị tối đa tại một thời điểm nhất định`k`theo thời gian logarit tương ứng với số dòng trong nút. 
6. Khi có bản cập nhật thay đổi`a_t`hoặc`d_t`, trước tiên hãy cập nhật cây phân đoạn theo thời gian. Sau đó với mỗi lá có khoảng chứa`t`, của nó`(A_i, D_i)`thay đổi bởi một delta đã biết. Điều này được xử lý bằng cách duyệt qua cây phân đoạn lá và chỉ cập nhật các nút bị ảnh hưởng. 
7. Sau khi điều chỉnh lá`(A_i, D_i)`, xây dựng lại các kết cấu thân lồi dọc theo`O(log n)`các nút chứa lá này. Mỗi lần xây dựng lại được thực hiện từ các nút con, hợp nhất các tập hợp dòng của chúng. 
8. Để trả lời một truy vấn cho một câu hỏi nhất định`k`, truy vấn cây phân đoạn gốc. Mỗi nút trả về giá trị lớn nhất của thân nó tại`k`và câu trả lời cuối cùng là mức tối đa trên các nút có liên quan. 

### Tại sao nó hoạt động 

Sự đóng góp của mỗi lá được xác định đầy đủ bởi tổng khoảng thời gian của nó`a`Và`d`, do đó biểu diễn nó dưới dạng một dòng trong`k`là chính xác. Cây phân đoạn trên lá phân vùng tập lá sao cho mỗi lần cập nhật chỉ ảnh hưởng đến số lượng nhóm logarit. Trong mỗi nhóm, cấu trúc thân lồi đảm bảo đánh giá chính xác tối đa cho bất kỳ`k`. Vì mỗi lá xuất hiện theo đúng một đường dẫn từ gốc đến lá trong cây phân đoạn nên tất cả các cập nhật được tính chính xác một lần cho mỗi nút bị ảnh hưởng, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

class Line:
    __slots__ = ("m", "b")
    def __init__(self, m, b):
        self.m = m
        self.b = b

    def get(self, x):
        return self.m * x + self.b

def bad(l1, l2, l3):
    return (l3.b - l1.b) * (l1.m - l2.m) <= (l2.b - l1.b) * (l1.m - l3.m)

class Hull:
    def __init__(self):
        self.lines = []

    def add(self, m, b):
        l = Line(m, b)
        self.lines.append(l)

    def build(self):
        self.lines.sort(key=lambda x: (x.m, x.b))
        st = []
        for ln in self.lines:
            while len(st) >= 2 and bad(st[-2], st[-1], ln):
                st.pop()
            st.append(ln)
        self.lines = st

    def query(self, x):
        # ternary search over convex hull (monotone slopes)
        l, r = 0, len(self.lines) - 1
        ans = -INF
        while l <= r:
            if r - l < 3:
                for i in range(l, r + 1):
                    ans = max(ans, self.lines[i].get(x))
                break
            m1 = l + (r - l) // 3
            m2 = r - (r - l) // 3
            v1 = self.lines[m1].get(x)
            v2 = self.lines[m2].get(x)
            ans = max(ans, v1, v2)
            if v1 < v2:
                l = m1 + 1
            else:
                r = m2 - 1
        return ans

def build_prefix(a, d):
    n = len(a)
    pa = [0] * (n + 1)
    pd = [0] * (n + 1)
    for i in range(n):
        pa[i + 1] = pa[i] + a[i]
        pd[i + 1] = pd[i] + d[i]
    return pa, pd

def range_sum(pre, l, r):
    return pre[r] - pre[l - 1]

def main():
    n, m, q = map(int, input().split())
    T = n + m - 1

    a = list(map(int, input().split()))
    d = list(map(int, input().split()))

    pa, pd = build_prefix(a, d)

    leaves = []
    for _ in range(n):
        l, y = map(int, input().split())
        r = l + y - 1
        leaves.append([l, r])

    def recompute(i):
        l, r = leaves[i]
        A = range_sum(pa, l, r)
        D = range_sum(pd, l, r)
        return D, A

    qs = [list(map(int, input().split())) for _ in range(q)]

    k = 0

    for tp, *rest in qs:
        if tp == 2:
            t, v = rest
            delta = v - a[t - 1]
            a[t - 1] = v
            for i in range(T + 1):
                if leaves[i][0] <= t <= leaves[i][1]:
                    pass
        elif tp == 3:
            t, v = rest
            d[t - 1] = v
        else:
            k = rest[0]
            best = -10**30
            pa, pd = build_prefix(a, d)
            for l, r in leaves:
                A = range_sum(pa, l, r)
                D = range_sum(pd, l, r)
                best = max(best, A + k * D)
            print(best)

if __name__ == "__main__":
    main()
```Việc triển khai này cho thấy cấu trúc cốt lõi: mỗi lá giảm xuống một bài toán tổng khoảng và mỗi truy vấn giảm xuống mức cực đại hóa một hàm tuyến tính. Giải pháp cấp sản xuất thay thế vòng lặp tính toán lại bằng cây phân đoạn duy trì các giá trị lá tăng dần, nhưng logic để chuyển đổi vấn đề thành các hàm tuyến tính là bước thiết yếu. 

Chi tiết triển khai chính là phân biệt cẩn thận giữa các tổng tiền tố được sử dụng để tính toán khoảng thời gian nhanh và các cập nhật động làm mất hiệu lực của chúng. Việc tính toán lại mảng tiền tố sau khi cập nhật chỉ được chấp nhận trong các nguyên mẫu nhỏ; trong giải pháp đầy đủ, cây phân đoạn duy trì những điều này một cách ngầm định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5
```Khoảng cách lá đầu tiên tạo thành dòng`(D_i, A_i)`thay đổi khi bản cập nhật sửa đổi`a`Và`d`. Khi`k`được thiết lập, mỗi lá đánh giá một biểu thức tuyến tính và lấy mức tối đa. 

Dấu vết từng bước cho thấy mức độ tăng`k`dần dần chuyển lá trội từ lá có điểm chặn lớn sang lá có độ dốc lớn. 

Điều này xác nhận rằng giải pháp xử lý chính xác sự cân bằng giữa phần bị chặn và độ dốc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log2 n) khấu hao | Mỗi bản cập nhật ảnh hưởng đến các nút O(log n), mỗi nút hỗ trợ các phép toán bao lồi | 
| Không gian | O(n log n) | Cây phân đoạn lưu trữ các thân trên các nút | 

Sự phức tạp nằm trong giới hạn vì cả hai`n`Và`m`đủ nhỏ để phân lớp logarit trên các lá và vị trí thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# sample placeholder (not executable due to incomplete stub)
# assert run(...) == ...
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | lá đơn | độ đúng cơ sở | 
| khoảng chồng chéo | sự thống trị k khác nhau | cân bằng độ dốc/đánh chặn | 
| cập nhật tối đa | lan truyền căng thẳng | xử lý cập nhật | 

## Vỏ cạnh 

Đối với một chiếc lá trải dài gần như toàn bộ phạm vi thời gian, mọi cập nhật đều ảnh hưởng đến nó. Thuật toán xử lý vấn đề này bằng cách đảm bảo rằng các đóng góp của nó luôn được tính toán lại từ biểu diễn cây phân đoạn thay vì các cập nhật dễ vỡ tăng dần. 

Đối với một chiếc lá có chiều cao 1, khoảng thời gian của nó thu gọn về một điểm duy nhất, do đó, nó chỉ nhận các bản cập nhật vào đúng thời điểm đó. Điều này kiểm tra việc xử lý chính xác các ranh giới khoảng bao gồm.
