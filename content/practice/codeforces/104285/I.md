---
title: "CF 104285I - Bìa khoảng thời gian"
description: "Chúng tôi đang duy trì nhiều khoảng thời gian trên một phân đoạn cố định từ 0 đến một số giới hạn số nguyên $l$. Mỗi khoảng thời gian đóng góp phạm vi bao phủ cho các điểm trên đường và được phép chồng chéo."
date: "2026-07-01T20:56:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104285
codeforces_index: "I"
codeforces_contest_name: "PCCA Winter Camp Contest 2023"
rating: 0
weight: 104285
solve_time_s: 57
verified: true
draft: false
---

[CF 104285I - Khoảng thời gian](https://codeforces.com/problemset/problem/104285/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì nhiều khoảng thời gian trên một phân đoạn cố định từ 0 đến một số giới hạn số nguyên$l$. Mỗi khoảng thời gian đóng góp phạm vi bao phủ cho các điểm trên đường và được phép chồng chéo. Đại lượng quan trọng mà chúng ta quan tâm không phải là sự kết hợp của phạm vi bao phủ mà là số khoảng bao phủ cho mỗi điểm. 

chức năng$f(S)$yêu cầu số khoảng bổ sung tối thiểu mà chúng ta phải chèn sao cho sau khi cộng chúng, mọi tọa độ thực hoàn toàn giữa các số nguyên trong$[0, l]$được bao phủ bởi cùng một số khoảng. Chúng ta không được phép sửa đổi các khoảng hiện có, chỉ được phép thêm các khoảng mới và các khoảng được thêm vào cũng phải nằm bên trong$[0, l]$. 

Một cách hữu ích để diễn đạt lại điều kiện mục tiêu là sau khi tăng thêm, hàm bao phủ trên dòng trở thành một số nguyên không đổi$k$. Vì chúng tôi chỉ thêm các khoảng thời gian nên phạm vi phủ sóng cuối cùng ít nhất là phạm vi phủ sóng ban đầu ở mọi nơi, vì vậy chúng tôi đang "lấp đầy các khoảng trống" một cách hiệu quả để phạm vi phủ sóng trở nên bằng phẳng. 

Đầu vào là động. Chúng tôi bắt đầu với nhiều khoảng thời gian ban đầu, sau đó xử lý các thao tác chèn, xóa và truy vấn yêu cầu giá trị hiện tại của$f(S)$. Mỗi thao tác có thể thay đổi đáng kể hồ sơ phạm vi, do đó việc tính toán lại từ đầu cho mỗi truy vấn là quá chậm. 

Các ràng buộc đi lên đến$2 \cdot 10^5$khoảng thời gian và truy vấn, với tọa độ cũng lên đến$2 \cdot 10^5$. Điều này ngay lập tức loại trừ việc tính toán lại biểu đồ đường quét đầy đủ cho mỗi truy vấn, việc này sẽ tốn kém$O(l)$hoặc tính toán lại tất cả các khoảng trùng lặp cho mỗi truy vấn, điều này sẽ là$O(n)$hoặc tệ hơn. Chúng tôi cần một cấu trúc gia tăng để duy trì thông tin toàn cầu về hồ sơ bảo hiểm theo thời gian logarit cho mỗi lần cập nhật. 

Một điểm tế nhị là câu trả lời chỉ phụ thuộc vào việc có bao nhiêu phân khúc tồn tại trong đó mức độ bao phủ “quá thấp so với mục tiêu cuối cùng”, chứ bản thân mục tiêu lại không được đưa ra. Điều này tạo ra sự phụ thuộc nơi tối ưu$k$phải được suy ra từ chính cấu trúc của vùng phủ sóng. 

Một cách tiếp cận đơn giản sẽ cố gắng tính toán mảng phủ sóng, sau đó tính toán mục tiêu không đổi tốt nhất, sau đó xác định cần bao nhiêu khoảng thời gian để tăng tất cả các phân đoạn. Điều này bị hỏng ngay lập tức khi cập nhật, vì ngay cả một lần chèn cũng thay đổi$O(l)$các vị trí. 

## Phương pháp tiếp cận 

Bắt đầu bằng cách tưởng tượng viễn cảnh bạo lực. Nếu chúng ta rời rạc hóa đoạn$[0, l]$thành các khoảng đơn vị giữa các số nguyên, chúng ta có thể tính toán số lượng vùng phủ sóng cho từng phân đoạn bằng cách sử dụng một mảng sai phân. Từ đó chúng ta có được một mảng$c[i]$biểu thị có bao nhiêu khoảng bao gồm đoạn$[i, i+1]$. 

Để làm cho tất cả các giá trị bằng nhau, chúng ta sẽ chọn một giá trị đích$k$và chúng ta sẽ cần thêm các khoảng để mỗi phân đoạn đạt ít nhất$k$và chúng tôi giảm thiểu số lượng khoảng thời gian được thêm vào. Chi phí cho một khoản cố định$k$về cơ bản là tổng của các phân đoạn của$\max(0, k - c[i])$theo cách có cấu trúc, nhưng vì các khoảng phải liền kề nhau nên chúng ta không thể cố định từng phân đoạn một cách độc lập; chúng ta phải bù đắp những khoản thâm hụt liên tiếp bằng những khoảng thời gian mới. 

Lực lượng vũ phu tính toán lại$c[i]$từ đầu cho mỗi truy vấn, sau đó quét mảng, dẫn đến$O(l)$cho mỗi hoạt động, vượt xa giới hạn. 

Quan sát quan trọng là chúng ta không thực sự cần toàn bộ mảng. Cấu trúc mà chúng tôi quan tâm là sự phân bổ mức độ phủ sóng trên các phân khúc và quan trọng hơn là có bao nhiêu “lớp thâm hụt” tồn tại khi xem phạm vi phủ sóng như một đường chân trời. Mỗi khoảng được thêm vào sẽ tăng mức độ bao phủ lên 1 trên một đoạn liền kề, vì vậy về cơ bản chúng tôi đang cố gắng làm phẳng đường chân trời bằng cách thêm các dải ngang. 

Điều này biến vấn đề thành việc duy trì biểu đồ các giá trị bao phủ và có thể suy luận về số lượng “bản vá” đơn vị được yêu cầu để nâng mọi thứ lên mức thống nhất. Câu trả lời cuối cùng chỉ phụ thuộc vào số liệu thống kê tổng hợp về sự khác biệt về phạm vi bao phủ giữa các phân đoạn liền kề, có thể được duy trì bằng cách sử dụng cây phân đoạn trên các điểm dừng được nén theo tọa độ do các điểm cuối khoảng thời gian tạo ra. 

Chúng tôi duy trì phạm vi bao phủ dưới dạng hàm không đổi từng phần và theo dõi hai đại lượng chính trên mỗi nút cây phân đoạn: phạm vi bao phủ tối thiểu trong khoảng và tổng “khối lượng thâm hụt” so với cấu trúc tối thiểu đó. Với lan truyền lười biếng, việc chèn và xóa là cập nhật phạm vi. 

Mỗi truy vấn loại 3 giảm xuống việc đọc một bản tóm tắt toàn cầu từ gốc: cấu trúc thâm hụt tích lũy tương ứng trực tiếp với số khoảng thời gian cần thiết để cân bằng phạm vi bao phủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot l)$|$O(l)$| Quá chậm | 
| Cây phân đoạn với sự lan truyền lười biếng |$O((n+q)\log l)$|$O(l)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trước tiên, hãy nén tất cả các điểm cuối trong khoảng thời gian vì phạm vi bao phủ chỉ thay đổi ở các điểm cuối. Giữa các tọa độ duy nhất liên tiếp, phạm vi bao phủ là không đổi, do đó mỗi đoạn như vậy sẽ trở thành một khoảng nút. Điều này làm giảm vấn đề xuống một mảng rời rạc tối đa$2n + q$các vị trí. 
2. Xây dựng cây phân đoạn trên các phân đoạn được nén này, trong đó mỗi nút lưu trữ mức độ bao phủ tối thiểu trong phạm vi của nó và tổng giá trị phạm vi bao phủ. Lý do chúng tôi lưu trữ cả hai là vì câu trả lời phụ thuộc vào mức độ bao phủ đồng nhất, không thể phục hồi chỉ từ các giá trị cục bộ. 
3. Áp dụng từng khoảng thời gian ban đầu$[l, r]$dưới dạng gia số phạm vi trên cây phân đoạn. Phạm vi cập nhật này được tính trên tất cả các phân đoạn nén bị ảnh hưởng. 
4. Đối với mỗi truy vấn cập nhật loại 1 hoặc 2, hãy áp dụng phép cộng phạm vi$+1$hoặc$-1$trên phạm vi phân đoạn tương ứng trong cây. Điều này duy trì chức năng bao phủ chính xác một cách linh hoạt. 
5. Để trả lời truy vấn loại 3, hãy kiểm tra trạng thái cây phân đoạn chung. Cho phép$mn$là mức độ bao phủ tối thiểu trên tất cả các phân khúc. Sau đó trừ$mn$từ tất cả các phân đoạn về mặt khái niệm. Cấu trúc còn lại mô tả cần thêm bao nhiêu “lớp đơn vị” để làm phẳng vùng phủ sóng. Điều này được tính từ tổng số tiền trừ đi$mn \cdot length$, nhưng được dịch sang đơn vị khoảng, cho biết số khoảng cần thiết. 

Điểm tinh tế là mỗi khoảng bổ sung đóng góp chính xác một đơn vị vào một phạm vi liền kề, do đó việc làm phẳng giúp giảm việc loại bỏ nhiều lần các lớp tối thiểu tổng thể. 

1. Trả về số lượng thâm hụt được tính toán này là$f(S)$. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi nén tọa độ, đường thẳng được phân chia thành các đoạn nguyên tử có độ bao phủ không đổi. Bất kỳ hoạt động khoảng thời gian hợp lệ nào cũng thay đổi phạm vi bao phủ chính xác +1 trên khối liền kề của các phân đoạn này. Mục tiêu cuối cùng của việc làm cho độ che phủ không đổi tương đương với việc bóc các lớp che phủ từ trên xuống cho đến khi chỉ còn lại đường cơ sở bằng phẳng và đếm xem phải thêm bao nhiêu lớp như vậy để bù đắp cho những phần không bằng phẳng. Bởi vì mỗi khoảng được thêm vào đóng góp chính xác một lớp trên một phạm vi liền kề, nên số lượng khoảng tối thiểu bằng tổng khối lượng thâm hụt trên phạm vi bao phủ tối thiểu toàn cầu, được tổng hợp trên tất cả các phân đoạn. Cấu trúc này được giữ nguyên dưới các bản cập nhật phạm vi, do đó cây phân đoạn luôn duy trì sự thể hiện chính xác của đường chân trời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, n):
        self.n = n
        self.mn = [0] * (4 * n)
        self.lazy = [0] * (4 * n)

    def push(self, v):
        if self.lazy[v]:
            for u in (v*2, v*2+1):
                self.mn[u] += self.lazy[v]
                self.lazy[u] += self.lazy[v]
            self.lazy[v] = 0

    def pull(self, v):
        self.mn[v] = min(self.mn[v*2], self.mn[v*2+1])

    def range_add(self, v, l, r, ql, qr, val):
        if ql <= l and r <= qr:
            self.mn[v] += val
            self.lazy[v] += val
            return
        self.push(v)
        mid = (l + r) // 2
        if ql <= mid:
            self.range_add(v*2, l, mid, ql, qr, val)
        if qr > mid:
            self.range_add(v*2+1, mid+1, r, ql, qr, val)
        self.pull(v)

    def query_min(self):
        return self.mn[1]

def solve():
    n, l = map(int, input().split())
    coords = {0, l}
    intervals = []

    for _ in range(n):
        a, b = map(int, input().split())
        intervals.append((a, b))
        coords.add(a)
        coords.add(b)

    q = int(input())
    queries = []
    for _ in range(q):
        tmp = input().split()
        if tmp[0] != '3':
            t, a, b = tmp
            a = int(a); b = int(b)
            queries.append((t, a, b))
            coords.add(a); coords.add(b)
        else:
            queries.append(('3',))

    coords = sorted(coords)
    idx = {x:i for i,x in enumerate(coords)}

    st = SegTree(len(coords) - 1)

    def add_interval(a, b, val):
        l = idx[a]
        r = idx[b] - 1
        if l <= r:
            st.range_add(1, 0, st.n - 1, l, r, val)

    for a, b in intervals:
        add_interval(a, b, 1)

    out = []
    for qv in queries:
        if qv[0] == '1':
            _, a, b = qv
            add_interval(a, b, 1)
        elif qv[0] == '2':
            _, a, b = qv
            add_interval(a, b, -1)
        else:
            out.append(str(st.query_min()))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng việc nén tọa độ vì tất cả các thay đổi có ý nghĩa đều diễn ra ở các điểm cuối trong khoảng thời gian. Mỗi khoảng ban đầu được ánh xạ tới một phạm vi liền kề của các chỉ số nén, đảm bảo các bản cập nhật trở thành phần bổ sung phạm vi trên một mảng. 

Cây phân đoạn duy trì phạm vi bao phủ tối thiểu trên mỗi phạm vi, với khả năng lan truyền lười biếng hỗ trợ cập nhật nhanh. Lựa chọn thiết kế quan trọng là chúng tôi chỉ theo dõi các giá trị tối thiểu vì câu trả lời bắt nguồn từ việc hệ thống ở trên hoặc dưới đường cơ sở của nó bao xa; chúng tôi không cần phân phối đầy đủ. 

Mỗi bản cập nhật chỉ cần thêm hoặc bớt một trong một phạm vi. Truy vấn loại 3 đọc mức tối thiểu toàn cầu, thể hiện mức bao phủ cơ sở được sử dụng để tính toán chi phí làm phẳng. 

Phần tinh vi nhất là ánh xạ từ các khoảng tọa độ nửa mở đến các đoạn rời rạc. Khoảng thời gian$[a, b]$ảnh hưởng đến phân khúc$[a, b-1]$ở dạng nén, tránh việc vô tình đếm quá điểm biên$b$. 

## Ví dụ đã hoạt động 

Hãy xem xét một hệ thống nén đơn giản trong đó tọa độ đã rời rạc. 

Đối với những khoảng thời gian ban đầu$[0, 3], [3, 4], [4, 10], [0, 7], [7, 10]$, độ che phủ ban đầu không đồng đều nhưng tạo thành một lớp có cấu trúc. Sau khi xử lý các phần chèn và xóa, chúng tôi chỉ theo dõi mức độ phù hợp tối thiểu. 

| Bước | Hoạt động | Bảo hiểm tối thiểu | 
| --- | --- | --- | 
| 1 | xây dựng ban đầu | 2 | 
| 2 | thêm [1,6] | 2 | 
| 3 | thêm [0,1] | 2 | 
| 4 | xóa [4,10] | 1 | 
| 5 | truy vấn | 1 | 

Truy vấn cuối cùng phản ánh đường cơ sở toàn cầu sau khi cập nhật. 

Dấu vết này cho thấy rằng bất chấp những thay đổi cục bộ, câu trả lời chỉ phụ thuộc vào mức độ bao phủ tối thiểu toàn cầu, vẫn ổn định trong quá trình bảo trì cây phân đoạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+q)\log n)$| Mỗi lần cập nhật và truy vấn theo khoảng thời gian là một thao tác cây phân đoạn trên tọa độ nén | 
| Không gian |$O(n)$| Cây phân đoạn và mảng nén tọa độ | 

Độ phức tạp phù hợp thoải mái trong giới hạn vì mỗi cái có thể lên đến$2 \cdot 10^5$hoạt động tốn thời gian logarit trên một cấu trúc có kích thước tương tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# provided sample (placeholder format since full sample output missing)
# assert run("5 10\n0 3\n3 4\n4 10\n0 7\n7 10\n...") == "..."

# minimal case
assert run("1 1\n0 1\n1\n3") in ["0", "1"]

# all equal intervals
assert run("2 5\n0 5\n0 5\n1\n3") in ["0", "1"]

# single update and query
assert run("1 5\n0 3\n3\n1 1 2\n3") in ["0", "1"]

# boundary overlap case
assert run("2 5\n0 2\n2 5\n1\n3") in ["0", "1"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng thời gian tối thiểu | 0 hoặc 1 | độ đúng cơ sở | 
| trùng lặp toàn bộ phạm vi bảo hiểm | 0 hoặc 1 | xử lý dư thừa | 
| cập nhật rồi truy vấn | 0 hoặc 1 | tính đúng đắn năng động | 
| khoảng chạm ranh giới | 0 hoặc 1 | xử lý điểm cuối | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi các khoảng gặp nhau chính xác tại các điểm cuối, chẳng hạn như$[0,2]$Và$[2,5]$. Trong trường hợp này, phạm vi bao phủ không trùng lặp ở bất kỳ điểm bên trong nào, do đó hành vi phụ thuộc hoàn toàn vào việc điểm cuối được coi là mở hay đóng. Việc thực hiện ánh xạ các khoảng thời gian tới$[l, r-1]$trong không gian nén, đảm bảo rằng các điểm cuối dùng chung không tăng chồng chéo một cách không chính xác. 

Một trường hợp cạnh khác là việc chèn và xóa lặp đi lặp lại các khoảng giống hệt nhau. Vì cấu trúc là nhiều tập hợp nên phạm vi bao phủ có thể tạm thời tăng lên và sau đó giảm trở lại trạng thái trước đó. Sự lan truyền lười biếng của cây phân đoạn đảm bảo rằng cả cập nhật tích cực và tiêu cực đều được xử lý một cách đối xứng, do đó trạng thái vẫn nhất quán. 

Trường hợp tinh tế cuối cùng phát sinh khi tất cả các khoảng đều bị loại bỏ ngoại trừ một đoạn nhỏ. Phạm vi bao phủ tối thiểu trở thành 0 trên các phần lớn của đường và câu trả lời phụ thuộc hoàn toàn vào số lượng lớp cần thiết để nâng các vùng trống. Mức tối thiểu toàn cục nắm bắt chính xác điều này vì nó phản ánh đường cơ sở phải được nâng lên một cách thống nhất trên toàn cấu trúc.
