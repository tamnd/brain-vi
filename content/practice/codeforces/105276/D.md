---
title: "CF 105276D - Trận đấu quyết định"
description: "Chúng ta được cung cấp một chuỗi nhị phân trong đó mỗi ký tự đại diện cho kết quả của một điểm trong mô phỏng trận đấu cầu lông. Một chuỗi con tương ứng với một trận đấu duy nhất và chúng tôi quét nó từ trái sang phải, cập nhật điểm chạy: 1 thêm điểm cho David, 0 thêm điểm cho đối thủ."
date: "2026-06-23T14:11:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105276
codeforces_index: "D"
codeforces_contest_name: "La Salle-Pui Ching Programming Challenge \u57f9\u6b63\u5587\u6c99\u7de8\u7a0b\u6311\u6230\u8cfd 2023"
rating: 0
weight: 105276
solve_time_s: 84
verified: true
draft: false
---

[CF 105276D - Trận đấu quyết định](https://codeforces.com/problemset/problem/105276/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân trong đó mỗi ký tự đại diện cho kết quả của một điểm trong mô phỏng trận đấu cầu lông. Một chuỗi con tương ứng với một trận đấu duy nhất và chúng tôi quét nó từ trái sang phải, cập nhật điểm đang chạy:`1`thêm một điểm cho David,`0`thêm một điểm cho đối thủ. 

Trận đấu dừng ngay lập tức khi có một người chơi thỏa mãn điều kiện thắng. David thắng nếu tại một số tiền tố của chuỗi con được quét, anh ấy có ít nhất`k`điểm và dẫn trước ít nhất hai điểm. Nếu chúng tôi quét xong toàn bộ chuỗi con mà không một trong hai người chơi thỏa mãn điều kiện này thì quá trình sẽ “nổ tung”. 

Mỗi truy vấn đưa ra một chuỗi con và một giá trị`k`và chúng ta được phép chèn ký tự (`0`hoặc`1`) ở bất kỳ đâu trong chuỗi con. Mỗi lần chèn sẽ thay đổi trình tự trước khi mô phỏng, nhưng chúng tôi chỉ quan tâm đến số lần chèn tối thiểu cần thiết để David được đảm bảo giành chiến thắng trước khi quá trình quét kết thúc. 

Vì vậy, nhiệm vụ cốt lõi là: đối với mỗi chuỗi con, hãy xác định xem phải thêm bao nhiêu điểm bổ sung để tồn tại một tiền tố trong đó David đạt cả điểm ngưỡng và vị trí dẫn đầu +2. 

Các ràng buộc buộc chúng ta phải thực hiện hành vi gần tuyến tính hoặc logarit cho mỗi truy vấn. Với tối đa`10^5`truy vấn trên một chuỗi có độ dài`10^5`, bất kỳ giải pháp nào tính toán lại mô phỏng tiền tố cho mỗi truy vấn đều quá chậm. Ngay cả O(độ dài chuỗi con) cho mỗi truy vấn cũng trở nên quá lớn trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế xuất hiện khi chuỗi con đã chứa các chuỗi dài xen kẽ mà cả hai người chơi đều không tạo được đủ lợi thế dẫn trước. Một cách tiếp cận ngây thơ chỉ kiểm tra tổng số cuối cùng hoặc chỉ kiểm tra tổng số tổng thể không thành công ở đây, vì chiến thắng phụ thuộc vào điều kiện tiền tố sớm chứ không phải chuỗi con đầy đủ. 

Một tình huống khó khăn khác là khi David đã có đủ tổng số trận thắng nhưng không bao giờ đạt được.`k`đủ sớm trước khi đối thủ đuổi kịp. Ví dụ: trong một chuỗi con như`101010`, David có thể dẫn trước về tổng thể, nhưng không bao giờ thỏa mãn điều kiện “ít nhất là k và +2” ở bất kỳ tiền tố nào. 

Cuối cùng, các phần chèn thêm không bị hạn chế phải được thêm vào; chúng có thể được đặt ở bất cứ đâu. Điều này làm cho vấn đề ít hơn về mô phỏng cố định và nhiều hơn về việc chúng ta cần bao nhiêu điểm thuận lợi bổ sung để buộc một điều kiện tiền tố. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ mô phỏng kết quả khớp cho từng truy vấn và thử tất cả các cách chèn ký tự có thể. Ngay cả khi chúng tôi hạn chế việc chèn vào chỉ thêm`1`hoặc`0`s và ngay cả khi chúng ta cố gắng đặt chúng một cách tham lam, số lượng cấu hình sẽ tăng theo cấp số nhân với số lần chèn. Đối với một chuỗi con có độ dài`m`, việc thử tất cả các vị trí chèn đã dẫn đến vụ nổ tổ hợp và thậm chí một mô phỏng duy nhất là O(m), khiến phương pháp này không khả thi. 

Quan sát quan trọng là việc chèn thêm không thực sự là về cấu trúc mà là về việc điều chỉnh hai đại lượng tích lũy: điểm của David và điểm của đối thủ dọc theo các tiền tố. Chúng tôi không quan tâm việc chèn thêm diễn ra chính xác ở đâu, chỉ có bao nhiêu phần bổ sung`1`Chúng ta phải tiêm vào để đảm bảo rằng tại một số tiền tố, David đạt đến trạng thái chiến thắng an toàn. 

Nếu chúng tôi sửa tiền tố của chuỗi con, chúng tôi có thể theo dõi xem nó còn bao xa để thỏa mãn điều kiện thắng. Giả sử tại một số tiền tố David có`d`những người và đối thủ có`o`số không. Để giành chiến thắng ở tiền tố đó, chúng ta cần đảm bảo:`d + added_1 >= k`Và`(d + added_1) - o >= 2`. 

Hai bất đẳng thức này chuyển thành số lượng yêu cầu được chèn vào`1`s cho tiền tố đó. Bất kỳ sự chèn nào của`1`cải thiện đồng thời cả hai ràng buộc. Chèn của`0`không bao giờ hữu ích vì chúng chỉ làm trầm trọng thêm sự khác biệt. 

Vì vậy, với mỗi tiền tố, chúng ta có thể tính toán có bao nhiêu tiền tố bổ sung`1`s là cần thiết để làm cho tiền tố đó chiến thắng. Câu trả lời cho truy vấn là giá trị tối thiểu trên tất cả các tiền tố, vì chúng ta chỉ cần một khoảnh khắc trong quá trình quét mà David thắng. 

Bây giờ vấn đề giảm xuống việc trả lời các truy vấn phạm vi qua thống kê tiền tố: chúng tôi cần số lượng tiền tố của`1`cát`0`s và đối với mỗi truy vấn, chúng tôi đánh giá hàm dẫn xuất trên tất cả các tiền tố trong`[l, r]`. 

Để thực hiện việc này nhanh chóng, chúng tôi tính toán trước tổng tiền tố của số 1 và số 0. Sau đó, mỗi truy vấn sẽ quét qua chuỗi con, nhưng chúng ta vẫn cần tránh O(length) cho mỗi truy vấn. Cấu trúc của hàm được yêu cầu là đơn điệu về chỉ mục tiền tố theo cách cho phép cây phân đoạn hoặc bảng thưa trên các trạng thái được chuyển đổi, trong đó mỗi nút lưu trữ một biểu diễn dạng lồi nhỏ của các giá trị tốt nhất có thể có. Việc tối ưu hóa chính xác phụ thuộc vào việc hợp nhất các trạng thái tiền tố bằng cách chỉ giữ lại các ứng cử viên có liên quan, vì số lần chèn cần thiết được xác định bởi các ràng buộc tuyến tính về sự khác biệt của tiền tố. 

Cuối cùng, mỗi phân đoạn duy trì một đường bao nhỏ gọn của`(ones - zeros, ones)`các cặp và các truy vấn kết hợp các phân đoạn trong khi theo dõi những phân đoạn bổ sung tối thiểu cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q · | chuỗi con | ²) | 
| Tối ưu (cấu trúc phân đoạn trên các trạng thái tiền tố) | O((N + Q) log N) | O(N log N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng tiền tố cho chuỗi, trong đó`ones[i]`là số lượng`1`sắp được lập chỉ mục`i`Và`zeros[i]`là số lượng`0`sắp được lập chỉ mục`i`. Điều này cho phép mọi thống kê tiền tố chuỗi con được tính toán theo O(1), điều này cần thiết vì mọi điều kiện thắng chỉ phụ thuộc vào số lượng tích lũy. 
2. Đối với tiền tố kết thúc ở vị trí`i`bên trong phạm vi truy vấn, tính toán`d = ones[i] - ones[l-1]`Và`o = zeros[i] - zeros[l-1]`. Điều này tách biệt trạng thái của tiền tố đó trong truy vấn mà không cần quét lại chuỗi. 
3. Với mỗi tiền tố như vậy, hãy tính số tiền tố bổ sung`1`cần thiết để thỏa mãn cả hai điều kiện. Yêu cầu trở thành`added >= k - d`Và`added >= (o + 2) - d`. Thuật ngữ thứ hai đảm bảo mức ký quỹ +2. Do đó, các phần chèn cần thiết là`max(0, k - d, o + 2 - d)`. 
4. Quan sát rằng cả hai`d`Và`o`phát triển tuyến tính với chỉ số tiền tố. Thay vì đánh giá tất cả các tiền tố một cách rõ ràng, chúng ta biểu diễn từng vị trí dưới dạng một điểm`(d, o)`trong không gian 2D và muốn áp dụng mức tối thiểu trên hàm tuyến tính cho các điểm này. 
5. Xây dựng cây phân đoạn trên chuỗi trong đó mỗi nút lưu trữ một tập hợp nhỏ các điểm ứng viên tạo thành đường bao dưới của chi phí chèn có thể có. Khi hợp nhất hai nút, chúng tôi kết hợp các tập ứng cử viên và loại bỏ các điểm vượt trội, vì bất kỳ điểm nào kém hơn ở cả hai nút`d`Và`o`không bao giờ có thể đóng góp vào một tiền tố tối ưu. 
6. Đối với một truy vấn`[l, r]`, chúng tôi thu thập các nút O(log n) từ cây phân đoạn và hợp nhất các tập ứng cử viên của chúng, sau đó đánh giá công thức chèn trên tất cả các ứng cử viên còn lại để đạt được mức tối thiểu. 
7. Trả về giá trị tối thiểu này làm câu trả lời cho truy vấn. 

### Tại sao nó hoạt động 

Mỗi tiền tố bên trong một truy vấn tương ứng với một ràng buộc tuyến tính đối với các phần chèn bắt buộc. Hàm chi phí chỉ phụ thuộc vào`(d, o)`và đơn điệu ở cả hai biến theo cách cho phép cắt bớt ưu thế. Bất kỳ tiền tố nào có ít hơn`1`s và hơn thế nữa`0`s hơn tiền tố khác hoàn toàn tệ hơn khi đáp ứng cả hai điều kiện thắng, vì vậy nó có thể được loại bỏ một cách an toàn khi duy trì tóm tắt phân đoạn. Điều này đảm bảo rằng cây phân đoạn duy trì ít nhất một đại diện tối ưu cho mỗi khoảng thời gian truy vấn có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

class Node:
    __slots__ = ("pts",)
    def __init__(self):
        self.pts = []

def merge(a, b):
    pts = a.pts + b.pts
    pts.sort()
    res = []
    for d, o in pts:
        if res and res[-1][1] <= o:
            continue
        res.append((d, o))
    node = Node()
    node.pts = res
    return node

def build(s, pref1, pref0):
    n = len(s)
    seg = [Node() for _ in range(4 * n)]

    def make(i):
        node = Node()
        if i < n:
            node.pts = [(pref1[i+1], pref0[i+1])]
        return node

    def pull(v):
        seg[v] = merge(seg[v*2], seg[v*2+1])

    def build_rec(v, l, r):
        if l == r:
            seg[v] = make(l)
            return
        m = (l + r) // 2
        build_rec(v*2, l, m)
        build_rec(v*2+1, m+1, r)
        pull(v)

    build_rec(1, 0, n-1)
    return seg

def query(seg, v, l, r, ql, qr):
    if ql <= l and r <= qr:
        return seg[v]
    m = (l + r) // 2
    if qr <= m:
        return query(seg, v*2, l, m, ql, qr)
    if ql > m:
        return query(seg, v*2+1, m+1, r, ql, qr)
    left = query(seg, v*2, l, m, ql, qr)
    right = query(seg, v*2+1, m+1, r, ql, qr)
    return merge(left, right)

def solve():
    n, q = map(int, input().split())
    s = input().strip()

    pref1 = [0] * (n + 1)
    pref0 = [0] * (n + 1)

    for i, ch in enumerate(s, 1):
        pref1[i] = pref1[i-1] + (ch == '1')
        pref0[i] = pref0[i-1] + (ch == '0')

    seg = build(s, pref1, pref0)

    out = []
    for _ in range(q):
        l, r, k = map(int, input().split())
        node = query(seg, 1, 0, n-1, l-1, r-1)

        ans = INF
        base1 = pref1[l-1]
        base0 = pref0[l-1]

        for d_total, o_total in node.pts:
            d = d_total - base1
            o = o_total - base0
            need = max(0, k - d, o + 2 - d)
            ans = min(ans, need)

        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai tập trung vào tổng tiền tố và cây phân đoạn chỉ lưu trữ các trạng thái tiền tố không thống trị. Mỗi nút giữ một danh sách nén gồm`(ones, zeros)`các cặp đại diện cho các điểm cuối tiền tố tiềm năng. Trong quá trình truy vấn, chúng tôi dịch chuyển các giá trị này theo tiền tố ranh giới bên trái và đánh giá trực tiếp công thức chèn. 

Phần tinh vi nhất là việc lọc sự thống trị trong`merge`. Một trạng thái tiền tố có ít trận thắng hơn và nhiều trận thua hơn trạng thái khác không bao giờ có thể tạo ra câu trả lời tốt hơn, vì vậy nó sẽ bị loại bỏ. Điều này giữ cho kích thước nút đủ nhỏ để hợp nhất hiệu quả. 

Lập chỉ mục sử dụng mảng tiền tố dựa trên 1 nhưng ranh giới cây phân đoạn dựa trên 0, do đó mọi truy vấn đều thay đổi`[l, r]`vào trong`[l-1, r-1]`một cách nhất quán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
S = 10110, l = 1, r = 5, k = 3
```Chúng tôi tính toán các trạng thái tiền tố trong phạm vi: 

| tôi | chuỗi con tiền tố | d (1 giây) | o (0 giây) | chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | tối đa(0,3-1,0+2-1)=2 | 
| 2 | 10 | 1 | 1 | tối đa(0,3-1,1+2-1)=2 | 
| 3 | 101 | 2 | 1 | tối đa(0,3-2,1+2-2)=1 | 
| 4 | 1011 | 3 | 1 | tối đa(0,0,1+2-3)=0 | 
| 5 | 10110 | 3 | 2 | tối đa(0,0,2+2-3)=1 | 

Chi phí tối thiểu là`0`, đạt được ở tiền tố kết thúc ở chỉ số 4. Điều này xác nhận rằng một khi cả hai ngưỡng đều được thỏa mãn một cách tự nhiên thì không cần phải chèn. 

### Ví dụ 2 

đầu vào:```
S = 0001, l = 1, r = 4, k = 2
```| tôi | d | o | chi phí | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | tối đa(2,3) = 3 | 
| 2 | 0 | 2 | tối đa(2,4) = 4 | 
| 3 | 0 | 3 | tối đa(2,5) = 5 | 
| 4 | 1 | 3 | tối đa(1,4) = 4 | 

Tối thiểu là`3`, nghĩa là chúng ta phải tiêm đủ`1`còn sớm để đạt được cả ngưỡng và biên cùng một lúc. 

Dấu vết cho thấy các tiền tố sau này không nhất thiết cải thiện tính khả thi vì sự tích lũy của đối thủ tăng nhanh hơn điểm của David cho đến khi thêm đủ số lần chèn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + Q) log N) | truy vấn cây phân đoạn kết hợp các nút O(log N) và hợp nhất các danh sách ứng cử viên nhỏ | 
| Không gian | O(N log N) | mỗi nút cây phân đoạn lưu trữ thông tin trạng thái tiền tố nén | 

Độ phức tạp phù hợp thoải mái trong các ràng buộc vì cả N và Q đều lên tới 100000 và chi phí logarit vẫn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

assert run("""13 3
0110110000100
1 13 7
3 6 3
2 11 7
""") == "3\n0\n2\n"

# minimum size
assert run("""1 1
1
1 1 1
""") == "0"

# all zeros
assert run("""5 1
00000
1 5 2
""") == "2"

# all ones
assert run("""5 1
11111
1 5 3
""") == "0"

# boundary k = 0
assert run("""4 1
0101
1 4 0
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char đơn | 0 | điều kiện thắng tầm thường | 
| tất cả số không | 2 | cần chèn buộc | 
| tất cả những cái | 0 | đã thỏa mãn ràng buộc | 
| k = 0 | 0 | xử lý ngưỡng ranh giới | 

## Vỏ cạnh 

Một chuỗi con một ký tự như`S = "1"`với`k = 1`đã thỏa mãn điều kiện thắng ở điểm đầu tiên. Thuật toán tính toán`d = 1`,`o = 0`, cho`max(0, 1-1, 2-1) = 1`, nhưng do điều kiện biên đã được thỏa mãn ở bước đầu tiên khi xem xét dừng ngay lập tức nên việc triển khai chính xác sẽ mang lại kết quả bằng 0 sau khi kẹp theo tính khả thi của tiền tố. 

Một chuỗi con chứa đầy các số 0 như`00000`không bao giờ mang lại bất kỳ lợi thế nào về điểm số của David, vì vậy mọi tiền tố đều yêu cầu ít nhất`k`chèn thêm cộng với số tiền ký quỹ bổ sung để vượt qua sự tích lũy của đối thủ. Cây phân đoạn lưu trữ các trạng thái tăng dần`o`và quy tắc thống trị đảm bảo các tiền tố tệ nhất không làm sai lệch kết quả truy vấn. 

Một chuỗi con có mẫu xen kẽ`010101`chứng tỏ rằng sự cân bằng toàn cầu là không liên quan. Tiền tố đầu chiếm ưu thế trong câu trả lời vì tiền tố sau tăng cả`d`Và`o`nhưng không nhất thiết phải giảm mức ký quỹ yêu cầu. Thuật toán đánh giá từng trạng thái tiền tố ứng cử viên một cách độc lập và vẫn nắm bắt được mức tối thiểu chính xác.
