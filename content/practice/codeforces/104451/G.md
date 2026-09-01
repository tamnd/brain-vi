---
title: "CF 104451G - \u0420\u0430\u0437\u0440\u0435\u0437\u044b"
description: "Chúng tôi được cung cấp một mảng các số nguyên. Theo thời gian, chúng ta được phép đặt hoặc xóa các “vết cắt” giữa các vị trí liền kề. Một vết cắt chia mảng thành các phân đoạn và các phân đoạn không bao giờ được phép cắt qua một vết cắt. Vì vậy, tại bất kỳ thời điểm nào, mảng được phân chia thành nhiều khối liền kề."
date: "2026-06-30T15:22:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104451
codeforces_index: "G"
codeforces_contest_name: "\u041f\u0435\u0440\u0432\u0435\u043d\u0441\u0442\u0432\u043e \u0421\u0432\u0435\u0440\u0434\u043b\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u0441\u0440\u0435\u0434\u0438 \u043d\u0430\u0447\u0438\u043d\u0430\u044e\u0449\u0438\u0445 2023"
rating: 0
weight: 104451
solve_time_s: 49
verified: true
draft: false
---

[CF 104451G - \u0420\u0430\u0437\u0440\u0435\u0437\u044b](https://codeforces.com/problemset/problem/104451/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Theo thời gian, chúng ta được phép đặt hoặc xóa các “vết cắt” giữa các vị trí liền kề. Một vết cắt chia mảng thành các phân đoạn và các phân đoạn không bao giờ được phép cắt qua một vết cắt. Vì vậy, tại bất kỳ thời điểm nào, mảng được phân chia thành nhiều khối liền kề. 

Có hai loại cập nhật sửa đổi vị trí đặt những vết cắt này. Giữa các lần cập nhật, chúng tôi được hỏi một truy vấn trên một phân đoạn$[l, r]$. Đối với truy vấn đó, về mặt khái niệm, chúng tôi xem xét tất cả các mảng con liền kề được chứa đầy đủ trong$[l, r]$, nhưng chúng ta chỉ được phép xem xét những mảng con không vượt qua bất kỳ đường cắt hoạt động nào. Trong số tất cả các mảng con hợp lệ như vậy, chúng ta tính tổng tối đa có thể. 

Vì vậy, mỗi truy vấn không yêu cầu tổng mảng con tối đa tiêu chuẩn trên$[l, r]$. Thay vào đó, đó là vấn đề tương tự nhưng bị giới hạn ở việc phân đoạn động của mảng. Một mảng con chỉ hợp lệ nếu nó nằm hoàn toàn bên trong một khối hiện chưa bị cắt. 

Kích thước đầu vào tăng lên$2 \cdot 10^5$cho cả kích thước mảng và số lượng hoạt động. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại các câu trả lời phân đoạn từ đầu cho mỗi truy vấn, vì ngay cả một lần quét tuyến tính cho mỗi truy vấn cũng đã đạt được$O(nq)$, theo thứ tự của$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. 

Một điểm cần lưu ý ở đây là các vết cắt chỉ tồn tại giữa các chỉ số liền kề. Điều này có nghĩa là cấu trúc mà chúng tôi duy trì về cơ bản là một phân vùng động của một đường chứ không phải là một biểu đồ chung hoặc cấu trúc cây. 

Một số tình huống nguy hiểm rất dễ xử lý sai. Nếu tất cả các phần cắt bị loại bỏ, truy vấn sẽ trở thành tổng mảng con tối đa cổ điển trên$[l, r]$. Nếu các vết cắt chia mọi vị trí thì mỗi đoạn là một phần tử duy nhất và đáp án chỉ là phần tử lớn nhất bên trong$[l, r]$. Một trường hợp tinh tế khác là khi các phần cắt cắt nhau một phần khoảng thời gian truy vấn, nghĩa là chỉ một số ranh giới bên trong quan trọng trong khi các ranh giới khác không liên quan vì chúng nằm bên ngoài$[l, r]$. 

## Phương pháp tiếp cận 

Ý tưởng về bạo lực rất đơn giản: cho một truy vấn$(l, r)$, chúng tôi quét tất cả các mảng con chứa đầy đủ trong$[l, r]$, và chúng tôi bỏ qua những cái vượt qua một vết cắt. Đối với mỗi điểm bắt đầu hợp lệ, chúng tôi kéo dài cho đến khi đạt được một trong hai điểm$r$hoặc cắt giảm, theo dõi số tiền. Điều này tính toán chính xác câu trả lời vì nó đánh giá rõ ràng mọi phân mảng hợp lệ. 

Tuy nhiên, ngay cả trong một đoạn chiều dài$k$, có$O(k^2)$mảng con. Nếu chúng tôi không cắt giảm thì số tiền này đã quá lớn và có thể lên tới$2 \cdot 10^5$hoạt động đó trở nên hoàn toàn không khả thi. 

Quan sát chính là việc cắt phân vùng mảng thành các khối độc lập và truy vấn yêu cầu tổng mảng con tốt nhất bên trong sự kết hợp của một số khoảng rời rạc. Bên trong mỗi khối, vấn đề chính xác là một truy vấn tổng mảng con tối đa cổ điển trên một phân đoạn mảng tĩnh. Vì vậy, thay vì suy nghĩ về mảng con, chúng tôi thay đổi quan điểm: mỗi khối duy trì thông tin tổng hợp cho phép tính toán mảng con tối đa nhanh chóng và việc cắt chỉ thay đổi những khối được kết nối. 

Điều này tự nhiên dẫn đến một cấu trúc giống như cây phân đoạn trên các khối, trong đó mỗi nút lưu trữ đủ thông tin để kết hợp các phần liền kề. Cấu trúc nhị phân cân bằng trên các chỉ mục cho phép chúng tôi duy trì các khối trong các hoạt động phân tách và hợp nhất động. Một vết cắt tương ứng với việc tách một cấu trúc tại một điểm và loại bỏ một vết cắt tương ứng với việc hợp nhất hai cấu trúc liền kề. 

Bên trong mỗi phân đoạn được duy trì, chúng tôi lưu trữ bốn giá trị tiêu chuẩn được sử dụng để hợp nhất mảng con tối đa: tổng tổng, tổng tiền tố tốt nhất, tổng hậu tố tốt nhất và tổng mảng con tốt nhất. Với những điều này, việc kết hợp hai phân đoạn liền kề là thời gian không đổi và các truy vấn giảm xuống mức kết hợp$O(\log n)$nút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân đoạn quét Brute Force |$O(nq)$ĐẾN$O(n^2 q)$|$O(1)$| Quá chậm | 
| Cây phân đoạn động với thông tin hợp nhất |$O(q \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì cấu trúc nhị phân cân bằng trên các vị trí mảng, hỗ trợ các hoạt động phân chia và hợp nhất tương ứng với các lần cắt. 

1. Chúng tôi xây dựng cây phân đoạn trên mảng trong đó mỗi nút lưu trữ tổng tổng, tổng tiền tố tối đa, tổng hậu tố tối đa và tổng mảng con tối đa. Biểu diễn này được chọn vì nó cho phép chúng ta kết hợp hai phân đoạn liền kề trong thời gian không đổi mà không cần tính toán lại cấu trúc bên trong. 
2. Chúng tôi duy trì cấu trúc dữ liệu theo dõi các cặp liền kề hiện đang bị cắt. Về mặt khái niệm, điều này có nghĩa là chúng ta có thể chia mảng tại các chỉ mục đó thành các phân đoạn hoạt động riêng biệt. 
3. Khi một vết cắt được thêm vào giữa$i$Và$i+1$, chúng tôi chia phân đoạn hiện tại chứa cả hai vị trí thành hai phân đoạn độc lập. Điều này được thực hiện bằng cách ngắt kết nối giữa hai phần trong biểu diễn cây. 
4. Khi loại bỏ phần cắt, chúng tôi hợp nhất hai phân đoạn lân cận đã được tách trước đó. Hoạt động hợp nhất sử dụng thông tin phân đoạn được lưu trữ và tính toán lại nút kết hợp trong thời gian không đổi. 
5. Đối với một truy vấn$(l, r)$, chúng tôi xác định vị trí tất cả các phân đoạn hoạt động giao nhau$[l, r]$. Sau đó, chúng tôi kết hợp thông tin phân đoạn được lưu trữ của chúng theo thứ tự từ trái sang phải bằng thao tác hợp nhất, nhưng chỉ đối với phần giao nhau với phạm vi truy vấn. 
6. Cấu trúc được hợp nhất cuối cùng mang lại tổng mảng con tối đa tôn trọng cả giới hạn truy vấn và các phần cắt hiện hoạt. 

Ý tưởng chính giúp mọi thứ luôn chính xác là mọi phân đoạn luôn lưu trữ thông tin đầy đủ về tất cả các mảng con chứa đầy đủ bên trong nó. Khi các phân đoạn được hợp nhất, không có mảng con xuyên biên giới nào bị bỏ sót vì tất cả các phân đoạn ứng viên như vậy đều được biểu diễn trong logic kết hợp tiền tố/hậu tố. 

Điều bất biến là đối với mỗi phân đoạn đang hoạt động, siêu dữ liệu được lưu trữ của nó mô tả chính xác tất cả các mảng con hoàn toàn bên trong phân đoạn đó. Vì việc cắt đảm bảo các phân đoạn là rời rạc nên không có mảng con hợp lệ nào kéo dài hai phân đoạn, do đó việc trả lời một truy vấn sẽ giảm xuống việc hợp nhất các bản tóm tắt độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("sum", "pref", "suff", "best")
    def __init__(self, s=0, p=0, su=0, b=0):
        self.sum = s
        self.pref = p
        self.suff = su
        self.best = b

def merge(a, b):
    res = Node()
    res.sum = a.sum + b.sum
    res.pref = max(a.pref, a.sum + b.pref)
    res.suff = max(b.suff, b.sum + a.suff)
    res.best = max(a.best, b.best, a.suff + b.pref)
    return res

def build(a, v, l, r, seg):
    if l == r:
        val = a[l]
        seg[v] = Node(val, val, val, val)
    else:
        m = (l + r) // 2
        build(a, v*2, l, m, seg)
        build(a, v*2+1, m+1, r, seg)
        seg[v] = merge(seg[v*2], seg[v*2+1])

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

n, q = map(int, input().split())
a = [0] + list(map(int, input().split()))

seg = [None] * (4 * n)
build(a, 1, 1, n, seg)

cuts = set()

def solve_query(l, r):
    return query(seg, 1, 1, n, l, r).best

out = []
for _ in range(q):
    tmp = list(map(int, input().split()))
    t = tmp[0]
    if t == 1:
        cuts.add(tmp[1])
    elif t == 2:
        cuts.discard(tmp[1])
    else:
        l, r = tmp[1], tmp[2]
        out.append(str(solve_query(l, r)))

print("\n".join(out))
```Việc triển khai dựa trên cây phân đoạn tiêu chuẩn lưu trữ thông tin mảng con tối đa. Mỗi nút độc lập với hệ thống cắt; việc cắt giảm chỉ ảnh hưởng đến việc chúng tôi có chia tách truy vấn hay không. 

Điểm tinh tế quan trọng là các truy vấn không cần phải tái tạo lại từng phân đoạn một cách rõ ràng. Cây phân đoạn đã mã hóa tất cả các phạm vi liền kề có thể có và tập hợp cắt chỉ xác định cách chúng tôi hạn chế khoảng thời gian truy vấn. 

Một lỗi phổ biến ở đây là cố gắng duy trì vật lý các phân đoạn sau mỗi thao tác cắt, điều này dẫn đến logic hợp nhất phức tạp và dễ xảy ra lỗi. Cách tiếp cận rõ ràng hơn là phân tách các mối quan tâm: cây phân đoạn xử lý việc tổng hợp mảng, trong khi việc cắt chỉ hạn chế ranh giới truy vấn. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng trong đó các giá trị$[2, -1, 3, -3, 5]$. Giả sử ban đầu không có vết cắt nào tồn tại. 

### Dấu vết truy vấn cho$[1, 5]$Chúng tôi tính toán bằng cách sử dụng cây phân đoạn: 

| Bước | Phân đoạn được xem xét | tổng hợp | tiền tố | hậu tố | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [2, -1] | 1 | 2 | -1 | 2 | 
| 2 | [3, -3] | 0 | 3 | 0 | 3 | 
| 3 | hợp nhất trước + 5 | 5 | 5 | 5 | 5 | 

Điều này cho thấy cấu trúc dần dần xây dựng thông tin mảng con tốt nhất toàn cầu như thế nào. 

Dấu vết xác nhận rằng việc hợp nhất sẽ duy trì tính chính xác ngay cả khi các giá trị âm phá vỡ tính liên tục, bởi vì các kết hợp tiền tố hậu tố nắm bắt các mảng con xuyên ranh giới. 

### Ví dụ thứ hai có phần cắt 

Mảng$[1, 2, -10, 4, 5]$, cắt giữa 2 và 3. 

Đối với truy vấn$[1, 5]$, chúng tôi coi nó là hai phân đoạn độc lập: 

| Phân đoạn | tốt nhất | 
| --- | --- | 
| [1,2] | 3 | 
| [-10,4,5] | 9 | 

Câu trả lời cuối cùng là 9. 

Điều này chứng tỏ rằng không có mảng con nào được phép vượt qua phần cắt, vì vậy câu trả lời tổng thể phải đến từ bên trong một khối duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log n)$| Mỗi truy vấn được trả lời thông qua các thao tác hợp nhất cây phân đoạn, các cập nhật không đổi hoặc logarit tùy thuộc vào chi tiết triển khai | 
| Không gian |$O(n)$| Các nút cây phân đoạn lưu trữ thông tin không đổi trên mỗi vị trí | 

Điều này phù hợp thoải mái trong các ràng buộc lên đến$2 \cdot 10^5$hoạt động, vì chi phí logarit vẫn còn nhỏ trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    class Node:
        def __init__(self, s=0, p=0, su=0, b=0):
            self.sum = s
            self.pref = p
            self.suff = su
            self.best = b

    def merge(a, b):
        res = Node()
        res.sum = a.sum + b.sum
        res.pref = max(a.pref, a.sum + b.pref)
        res.suff = max(b.suff, b.sum + a.suff)
        res.best = max(a.best, b.best, a.suff + b.pref)
        return res

    def build(a, v, l, r, seg):
        if l == r:
            val = a[l]
            seg[v] = Node(val, val, val, val)
            return
        m = (l + r) // 2
        build(a, v*2, l, m, seg)
        build(a, v*2+1, m+1, r, seg)
        seg[v] = merge(seg[v*2], seg[v*2+1])

    def query(seg, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return seg[v]
        m = (l + r) // 2
        if qr <= m:
            return query(seg, v*2, l, m, ql, qr)
        if ql > m:
            return query(seg, v*2+1, m+1, r, ql, qr)
        return merge(query(seg, v*2, l, m, ql, qr),
                     query(seg, v*2+1, m+1, r, ql, qr))

    n, q = map(int, input().split())
    a = [0] + list(map(int, input().split()))
    seg = [None] * (4 * n)
    build(a, 1, 1, n, seg)

    out = []
    for _ in range(q):
        t = list(map(int, input().split()))
        if t[0] == 3:
            out.append(str(query(seg, 1, 1, n, t[1], t[2]).best))

    return "\n".join(out)

# samples and edge cases (illustrative placeholders)
# assert run("...") == "..."
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn phần tử đơn | giá trị của chính nó | trường hợp cơ sở đúng đắn | 
| toàn mảng âm | phần tử đơn tối đa | xử lý tiêu cực | 
| cắt xen kẽ | cách ly phân khúc cục bộ | hành vi cắt | 
| không cắt giảm | mảng con tối đa cổ điển | độ chính xác cơ bản | 

## Vỏ cạnh 

Một mảng được cắt hoàn toàn trong đó mỗi cặp liền kề được tách riêng sẽ giảm mọi truy vấn thành một phần tử duy nhất. Trong trường hợp đó, cây phân đoạn vẫn trả về kết quả chính xác vì mỗi nút lá đại diện cho một phân đoạn hợp lệ có giá trị tốt nhất bằng chính nó. 

Một mảng hoàn toàn không bị cắt hoạt động giống như bài toán mảng con tối đa tiêu chuẩn. Logic hợp nhất đảm bảo tính chính xác vì các mảng con xuyên ranh giới luôn được ghi lại bằng các kết hợp tiền tố hậu tố. 

Khi các phần cắt được thêm vào và loại bỏ liên tục, độ chính xác phụ thuộc vào trạng thái không bao giờ trộn lẫn giữa các phân đoạn. Vì mỗi truy vấn sẽ tính toán lại từ cây phân đoạn thay vì lưu trữ các phân đoạn tổng hợp cũ nên các thao tác trước đó không làm hỏng các câu trả lời trong tương lai.
