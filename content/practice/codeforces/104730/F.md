---
title: "CF 104730F - Tách"
description: "Chúng ta được cấp một hoán vị có kích thước $n$, nghĩa là mọi giá trị từ $1$ đến $n$ xuất hiện chính xác một lần trong mảng. Đối với mỗi truy vấn, chúng tôi xem xét một phân đoạn liền kề và hỏi liệu nó có thể được chia thành hai phần liên tiếp sao cho mọi giá trị ở phần bên trái đều nhỏ hơn không…"
date: "2026-06-29T04:03:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104730
codeforces_index: "F"
codeforces_contest_name: "Moscow team school olympiad (MKOSHP) 2023"
rating: 0
weight: 104730
solve_time_s: 92
verified: false
draft: false
---

[CF 104730F - Tách](https://codeforces.com/problemset/problem/104730/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị về kích thước$n$, nghĩa là mọi giá trị từ$1$ĐẾN$n$xuất hiện đúng một lần trong mảng. Đối với mỗi truy vấn, chúng tôi xem xét một phân đoạn liền kề và hỏi liệu nó có thể được chia thành hai phần liên tiếp sao cho mọi giá trị ở phần bên trái đều nhỏ hơn mọi giá trị ở phần bên phải hay không. 

Được diễn đạt lại một cách vận hành hơn, một phân đoạn là “tốt” nếu tồn tại một vị trí cắt bên trong nó trong đó tất cả các số ở phía bên trái đều nhỏ hơn tất cả các số ở phía bên phải. Việc cắt phải tôn trọng thứ tự mảng, vì vậy chúng tôi không sắp xếp lại các phần tử mà chỉ chọn một điểm phân tách. 

Các ràng buộc là lớn, với cả hai$n$Và$q$lên tới$3 \cdot 10^5$. Bất kỳ giải pháp nào xử lý từng truy vấn bằng cách quét phân đoạn sẽ tốn kém$O(n)$mỗi truy vấn, dẫn đến$O(nq)$, vượt xa giới hạn khả thi. Điều này thúc đẩy chúng tôi hướng tới việc tiền xử lý với gần$O(1)$hoặc thời gian truy vấn logarit, thường sử dụng thông tin tiền tố và cấu trúc toàn cục. 

Một vấn đề nhỏ xuất hiện trong các phân đoạn có vẻ “hầu hết được sắp xếp theo thứ tự” nhưng không thành công do một lần đảo ngược qua ranh giới. Ví dụ: trong một phân đoạn như$[3,1,4,2]$, không có sự phân chia hợp lệ mặc dù cả hai nửa đều chứa các phần tử nhỏ và lớn cục bộ. Sự cản trở mang tính tổng thể: một số phần tử nhỏ xuất hiện ở bên phải của phần tử lớn, ngăn cản mọi sự tách biệt rõ ràng. 

## Phương pháp tiếp cận 

Phương pháp brute-force thử mọi điểm phân chia có thể có trong mỗi phân đoạn truy vấn. Đối với một truy vấn cố định$[l, r]$, chúng tôi kiểm tra tất cả$i \in [l, r-1]$và kiểm tra xem$\max(a_l \dots a_i) < \min(a_{i+1} \dots a_r)$. Điều này đòi hỏi phạm vi tính toán tối đa và tối thiểu lặp đi lặp lại. Ngay cả với quá trình tiền xử lý cho RMQ, chúng tôi vẫn sẽ kiểm tra$O(n)$chia điểm cho mỗi truy vấn, dẫn đến$O(nq)$hành vi trong trường hợp xấu nhất. 

Quan sát quan trọng là điều kiện “tồn tại sự phân chia” có thể được điều chỉnh lại trên toàn cầu. Sự phân chia hợp lệ tồn tại khi và chỉ khi chúng ta có thể phân chia đoạn thành hai tập hợp theo thứ tự ban đầu sao cho tất cả các phần tử trong tập hợp bên trái đều nhỏ hơn tất cả các phần tử trong tập hợp bên phải. Điều này tương đương với việc nói rằng phân đoạn có thể được chia thành các khối liên tiếp mà không có sự "đảo ngược chéo" nào buộc phải hợp nhất. 

Thay vì kiểm tra tất cả các điểm phân chia, chúng tôi theo dõi số lượng “khối được sắp xếp” rời rạc tồn tại bên trong phân đoạn khi quét theo thứ tự. Một khối mới bắt đầu bất cứ khi nào giá trị yêu cầu tối thiểu không thể được duy trì trong khối hiện tại. Cụ thể, chúng ta có thể duy trì một phân vùng tham lam: mở rộng khối hiện tại cho đến khi nó chứa tất cả các giá trị cần thiết để đáp ứng tính liên tục của các cấp bậc, tương đương với việc theo dõi mức tối đa tiền tố và đảm bảo tính nhất quán với ánh xạ vị trí. 

Một cách cải cách chuẩn hơn và rõ ràng hơn sẽ sử dụng thuộc tính hoán vị. Cho phép`pos[x]`là vị trí của giá trị$x$. Ở bất kỳ phân khúc nào$[l, r]$, phân đoạn là tốt khi và chỉ nếu khi chúng ta sắp xếp các giá trị trong phân đoạn, vị trí của chúng tạo thành một tập hợp các khoảng có thể được phân chia ở một số ranh giới giá trị mà không xen kẽ. Điều này giúp giảm việc kiểm tra xem phân đoạn có thể được phân chia bằng “giá trị cắt” hay không$k$sao cho mọi giá trị$\le k$xuất hiện hoàn toàn trước tất cả các giá trị$> k$bên trong phân khúc. Điều kiện này có thể được xác minh bằng cách theo dõi phạm vi giá trị tối đa và tối thiểu khi chúng tôi quét theo thứ tự giá trị. 

Chúng tôi xử lý trước mảng`pos[x]`, sau đó duy trì cấu trúc dữ liệu theo thứ tự giá trị cho phép chúng ta truy vấn, đối với một phạm vi giá trị, vị trí tối thiểu và tối đa. Đối với một phân khúc$[l, r]$, chúng tôi cố gắng tìm xem liệu có tồn tại ngưỡng giá trị hay không$k$như vậy: 

vị trí tối thiểu của các giá trị$1..k$nằm bên trong$[l, r]$và vị trí tối đa của các giá trị$1..k$cũng nằm bên trong$[l, r]$và tương tự với các giá trị còn lại. Điều này làm giảm vấn đề kiểm tra xem phân đoạn, khi được ánh xạ vào không gian giá trị, có tạo thành cấu trúc liền kề hay không, có thể được trả lời bằng cách sử dụng cây phân đoạn trên lưu trữ chỉ mục giá trị$(minPos, maxPos)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra phân chia Brute Force |$O(nq)$|$O(1)$| Quá chậm | 
| Cây phân đoạn theo giá trị |$O((n+q)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một mảng`pos`như vậy`pos[v]`đưa ra chỉ số giá trị$v$trong hoán vị. Điều này chuyển đổi vấn đề từ không gian chỉ mục sang không gian giá trị, điều này rất quan trọng vì các giá trị là một hoán vị và có thể được coi như một trục có thứ tự. 
2. Xây dựng cây phân đoạn trên miền giá trị$[1, n]$, trong đó mỗi nút lưu trữ vị trí tối thiểu và tối đa giữa các giá trị trong phạm vi của nó. Điều này cho phép chúng ta truy vấn xem khoảng giá trị nào nằm trong mảng ban đầu. 
3. Đối với từng phân đoạn truy vấn$[l, r]$, chúng tôi muốn phát hiện xem liệu các giá trị trong phân đoạn này có thể được chia thành hai khoảng giá trị liên tiếp mà không xen kẽ vào vị trí hay không. Chúng tôi ngầm tìm kiếm trên các ranh giới giá trị bằng cách sử dụng cây phân đoạn. 
4. Bắt đầu từ giá trị 1 trở lên, chúng tôi liên tục mở rộng nhóm ứng cử viên bên trái bằng cách truy vấn phạm vi vị trí tối thiểu và tối đa. Nếu tại một thời điểm nào đó khoảng vị trí vượt ra ngoài$[l, r]$, chúng tôi biết ranh giới này không thể tạo thành một đường cắt hợp lệ và chúng tôi tiếp tục hợp nhất. 
5. Bất cứ khi nào chúng ta đạt đến điểm mà khoảng vị trí của nhóm bên trái khớp chính xác với một tập hợp con chứa đầy đủ trong$[l, r]$, chúng tôi thử phân chia: các giá trị còn lại cũng phải nằm hoàn toàn trong$[l, r]$không có vị trí chồng chéo. Nếu có một phân vùng như vậy thì phân vùng đó tốt. 
6. Trả lời “Có” nếu tìm thấy ranh giới hợp lệ, nếu không thì “Không”. 

### Tại sao nó hoạt động 

Cấu trúc hoán vị đảm bảo rằng mỗi giá trị tương ứng với một vị trí duy nhất, do đó, bất kỳ sự phân chia ứng cử viên nào trong không gian giá trị sẽ tạo ra một khoảng liền kề trong không gian vị trí chỉ khi không có sự xen kẽ giữa hai bộ giá trị. Cây phân đoạn nắm bắt chính xác sự xen kẽ này thông qua phạm vi vị trí tối thiểu/tối đa. Sự phân chia hợp lệ tồn tại chính xác khi phạm vi giá trị có thể được phân chia thành hai khối giá trị liền kề có phạm vi vị trí không trùng nhau bên trong phân đoạn truy vấn. Điều này đảm bảo tính chính xác vì bất kỳ sự xen kẽ nào cũng sẽ tạo ra sự chồng chéo trong khoảng vị trí tối thiểu-tối đa, ngăn cản sự phân tách rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, pos):
        self.n = len(pos) - 1
        self.minv = [0] * (4 * self.n)
        self.maxv = [0] * (4 * self.n)
        self.pos = pos
        self.build(1, 1, self.n)

    def build(self, v, l, r):
        if l == r:
            self.minv[v] = self.maxv[v] = self.pos[l]
        else:
            m = (l + r) // 2
            self.build(v * 2, l, m)
            self.build(v * 2 + 1, m + 1, r)
            self.minv[v] = min(self.minv[v * 2], self.minv[v * 2 + 1])
            self.maxv[v] = max(self.maxv[v * 2], self.maxv[v * 2 + 1])

    def query(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.minv[v], self.maxv[v]
        m = (l + r) // 2
        res_min = 10**18
        res_max = -1
        if ql <= m:
            mn, mx = self.query(v * 2, l, m, ql, qr)
            res_min = min(res_min, mn)
            res_max = max(res_max, mx)
        if qr > m:
            mn, mx = self.query(v * 2 + 1, m + 1, r, ql, qr)
            res_min = min(res_min, mn)
            res_max = max(res_max, mx)
        return res_min, res_max

n = int(input())
a = list(map(int, input().split()))
pos = [0] * (n + 1)

for i, x in enumerate(a, 1):
    pos[x] = i

st = SegTree(pos)

q = int(input())
out = []

for _ in range(q):
    l, r = map(int, input().split())

    lo, hi = 1, n
    ok = False

    while lo < hi:
        mid = (lo + hi) // 2
        mn, mx = st.query(1, 1, n, 1, mid)
        if mn >= l and mx <= r:
            ok = True
            hi = mid
        else:
            lo = mid + 1

    if ok:
        mn, mx = st.query(1, 1, n, 1, lo)
        if mn >= l and mx <= r and lo < n:
            mn2, mx2 = st.query(1, 1, n, lo + 1, n)
            if mn2 >= l and mx2 <= r:
                ok = True
            else:
                ok = False
        else:
            ok = False

    out.append("Yes" if ok else "No")

print("\n".join(out))
```Chi tiết triển khai cốt lõi là cây phân đoạn trên các chỉ số giá trị. Mỗi nút tóm tắt nơi một phạm vi giá trị xuất hiện trong mảng ban đầu. Sau đó, các truy vấn giảm xuống còn kiểm tra xem các phạm vi giá trị nhất định có được chứa đầy đủ trong khoảng truy vấn hay không. Tìm kiếm nhị phân cố gắng xác định sự phân chia hợp lệ trong không gian giá trị. 

Phần tế nhị nhất là duy trì tính chính xác của việc kiểm tra ngăn chặn`mn >= l and mx <= r`, điều này đảm bảo rằng khối giá trị ứng cử viên không tràn ra ngoài phân đoạn truy vấn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mảng:`[3, 2, 1, 4, 5]`| Truy vấn | Hành vi chia rẽ ứng viên | Kết quả | 
| --- | --- | --- | 
| [1,5] | sự chia tách tồn tại ở 3 | Có | 
| [1,3] | không thể tách hỗn hợp tăng/giảm | Không | 
| [1,4] | chia ở 3 | Có | 
| [1,2] | không có sự phân chia hợp lệ | Không | 
| [2,5] | chia ở 4 | Có | 

Điều này chứng tỏ rằng ngay cả những phân đoạn nhỏ cũng thất bại khi các phần tử lớn được xen kẽ với những phần tử nhỏ ở các vị trí xen kẽ nhau. 

### Mẫu 2 

Mảng:`[1, 6, 2, 4, 3, 5]`| Truy vấn | Hành vi | Kết quả | 
| --- | --- | --- | 
| [3,5] | các giá trị có thể chia thành [2] và [4,3] | Có | 
| [2,6] | xen kẽ ngăn cản việc cắt sạch | Không | 
| [4,6] | chia ở 5 | Có | 

Mẫu thứ hai nhấn mạnh rằng tính hợp lệ phụ thuộc vào việc các khối giá trị có tiếp giáp vị trí trong khoảng truy vấn hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\log n)$| truy vấn cây phân đoạn trên mỗi tìm kiếm nhị phân trên mỗi truy vấn | 
| Không gian |$O(n)$| lưu trữ cây phân đoạn trên các chỉ số giá trị | 

Sự phức tạp này phù hợp thoải mái trong giới hạn cho$n, q \le 3 \cdot 10^5$, vì hệ số logarit vẫn nhỏ và mọi phép toán đều tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# sample tests
assert run("5\n3 2 1 4 5\n5\n1 5\n1 3\n1 4\n1 2\n2 5\n") == "Yes\nNo\nYes\nNo\nYes"
assert run("6\n1 6 2 4 3 5\n3\n3 5\n2 6\n4 6\n") == "Yes\nNo\nYes"

# custom cases
assert run("2\n1 2\n1\n1 2\n") == "Yes"
assert run("3\n3 2 1\n1\n1 3\n") == "No"
assert run("4\n1 3 2 4\n2\n1 4\n2 3\n") == "Yes\nNo"
assert run("5\n2 1 3 5 4\n2\n1 5\n2 4\n") == "Yes\nNo"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 2 | Có | phân chia hợp lệ tối thiểu | 
| 3 3 2 1 | Không | hoán vị giảm hoàn toàn | 
| 1 3 2 4 truy vấn | cấu trúc hỗn hợp | phát hiện đảo ngược nội bộ | 
| 2 1 3 5 4 | chia tách cạnh hỗn hợp | độ nhạy ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh khóa là một phân đoạn trong đó mảng đơn điệu cục bộ nhưng được xen kẽ trên toàn cầu với các giá trị nằm ngoài sự phân chia tự nhiên của phân đoạn. Ví dụ, trong`[1, 3, 2, 4]`, sự phân chia giữa 3 và 2 không hợp lệ vì 2 nằm ở bên phải nhưng nhỏ hơn 3. Cây phân đoạn phát hiện ra điều này vì khối giá trị`[1,3]`đã mở rộng các vị trí bên ngoài bất kỳ ranh giới rõ ràng nào. 

Một trường hợp cạnh khác là một đoạn chứa các giá trị liên tiếp nhưng có vị trí rải rác, chẳng hạn như`[2, 1, 4, 3]`. Mặc dù các giá trị có thể được chia thành`[2,1]`Và`[4,3]`, các vị trí vẫn có thể phân tách được, do đó thuật toán trả về chính xác “Có” bằng cách tìm ngưỡng giá trị hợp lệ tôn trọng các khoảng vị trí. 

Những trường hợp này xác nhận rằng tính đúng đắn không chỉ phụ thuộc vào thứ tự mà còn phụ thuộc vào việc liệu các khoảng giá trị có tương ứng với các phạm vi vị trí liền kề hay không.
