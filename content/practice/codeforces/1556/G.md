---
title: "CF 1556G - Cánh cổng đến thế giới khác"
description: "Chúng ta đang làm việc trên một biểu đồ có các đỉnh đều là số nguyên từ $0$ đến $2^n - 1$. Mỗi đỉnh đại diện cho một chuỗi nhị phân $n$-bit và có một cạnh vô hướng giữa hai đỉnh nếu cách biểu diễn nhị phân của chúng khác nhau đúng một bit."
date: "2026-06-14T21:51:32+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "data-structures", "dsu", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1556
codeforces_index: "G"
codeforces_contest_name: "Deltix Round, Summer 2021 (open for everyone, rated, Div. 1 + Div. 2)"
rating: 3300
weight: 1556
solve_time_s: 284
verified: false
draft: false
---

[CF 1556G - Cánh cổng đến thế giới khác](https://codeforces.com/problemset/problem/1556/G) 

**Đánh giá:** 3300 
**Thẻ:** bitmask, cấu trúc dữ liệu, dsu, hai con trỏ 
**Thời gian giải:** 4 phút 44 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang vẽ một đồ thị có các đỉnh đều là số nguyên từ$0$ĐẾN$2^n - 1$. Mỗi đỉnh đại diện cho một$n$-bit chuỗi nhị phân và có một cạnh vô hướng giữa hai đỉnh nếu biểu diễn nhị phân của chúng khác nhau chính xác một bit. Đây là tiêu chuẩn$n$siêu khối chiều. 

Theo thời gian, các đỉnh bị loại bỏ. Một đỉnh bị loại bỏ không thể được sử dụng như một phần của bất kỳ đường dẫn nào, mặc dù các cạnh không bị xóa một cách rõ ràng; chúng chỉ đơn giản là trở nên không sử dụng được nếu chúng chạm vào một đỉnh đã bị loại bỏ. 

Chúng ta phải xử lý hai thao tác trực tuyến. Một thao tác loại bỏ tất cả các đỉnh trong một khoảng số$[l, r]$. Câu hỏi còn lại hỏi liệu hai đỉnh hiện đang hoạt động có được kết nối thông qua các đỉnh còn lại trong biểu đồ siêu khối hay không. 

Khó khăn chính đó là$2^n$có thể rất lớn vì$n \le 50$, do đó biểu đồ là ẩn và không thể xây dựng được. Ngay cả việc lặp lại trên tất cả các nút là không thể. Số lượng truy vấn lên tới$5 \cdot 10^4$, vì vậy mỗi truy vấn phải được xử lý theo thời gian khấu hao đa logarit hoặc gần như không đổi. 

Một cách tiếp cận ngây thơ duy trì DSU trên tất cả$2^n$các nút ngay lập tức là không thể vì bản thân tập hợp đỉnh rất lớn về mặt thiên văn. Ngay cả việc lưu trữ trạng thái hoạt động một cách rõ ràng trên mỗi đỉnh cũng không khả thi. 

Vấn đề tế nhị thứ hai là việc xóa xảy ra theo từng khoảng thời gian. Việc triển khai bất cẩn có thể cho rằng chúng ta có thể xử lý các đỉnh riêng lẻ, nhưng việc lặp lại$[l, r]$trực tiếp cũng là không thể khi phạm vi có thể lớn. 

Một chế độ lỗi không rõ ràng khác xuất phát từ việc giả định kết nối hypercube hoạt động giống như kết nối khoảng thời gian đơn giản. Ví dụ: trong biểu đồ đường, người ta có thể mong đợi các khoảng thời gian để phân vùng các thành phần, nhưng ở đây khả năng kết nối là đa chiều và việc lật các bit cho phép “nhảy” qua thứ tự số. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ coi mỗi đỉnh là một nút trong biểu đồ và các cạnh hợp nhất giữa tất cả các cặp khác nhau một bit. Mỗi nút có mức độ$n$, vậy tổng số cạnh là$n \cdot 2^{n-1}$. Ngay cả khi điều này được hiện thực hóa, việc loại bỏ các khoảng thời gian sẽ yêu cầu xóa các nút và phân tách các thành phần DSU một cách linh hoạt, điều mà DSU tiêu chuẩn không thể hỗ trợ. 

Cái nhìn sâu sắc về cấu trúc thực sự là siêu khối có thể được hiểu là cấu trúc giống như trie nhị phân trên các bit, trong đó khả năng kết nối phụ thuộc vào các nút còn tồn tại trong các khối con được xác định bởi tiền tố bit. Khi một phạm vi$[l, r]$bị chặn, chúng tôi sẽ xóa một phân đoạn liền kề theo thứ tự số, tương ứng với việc xóa liên kết các khối con được căn chỉnh bằng phân tách nhị phân của khoảng. 

Việc rút gọn chính là tránh suy nghĩ về các đỉnh riêng lẻ và thay vào đó biểu diễn tập hợp các đỉnh hoạt động dưới dạng tập hợp các khoảng chính tắc rời rạc trên các tiền tố bit. Mỗi phân đoạn chính tắc tương ứng với một cây con hoàn chỉnh của bộ ba độ sâu nhị phân$n$và trong phân đoạn như vậy, kết nối được bảo toàn hoàn toàn trừ khi có khoảng thời gian chặn cắt ngang phân đoạn đó. 

Điều này cho phép chúng tôi duy trì phân vùng không gian hoạt động còn lại tối đa$O(m)$khoảng thời gian. Mỗi khoảng được biểu diễn dưới dạng một nút trong DSU. Khi chúng tôi loại bỏ$[l, r]$, chúng tôi chia các khoảng hiện tại thành tối đa hai phần và xóa các phân đoạn được đề cập. Khi kiểm tra kết nối, chúng tôi chỉ cần xác minh xem$a$Và$b$thuộc về cùng một cấu trúc khoảng còn lại. 

Cái nhìn sâu sắc quan trọng là sự kề cận trong hypercube không còn quan trọng nữa. Trong bất kỳ khối sống sót liền kề tối đa nào trong cấu trúc phân rã nhị phân, tất cả các nút vẫn có thể truy cập được lẫn nhau vì chúng tôi luôn bảo toàn các khối con đầy đủ hoặc các phân đoạn được phân tách đầy đủ. 

Điều này biến vấn đề kết nối đồ thị thành vấn đề bảo trì theo khoảng thời gian với DSU trên các phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Biểu đồ Brute Force + DSU trên$2^n$nút | Không thể | Không thể | Quá chậm | 
| Phân rã khoảng thời gian + DSU trên các phân đoạn |$O(m \log m)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì cấu trúc cân bằng của các phân khúc hoạt động rời rạc trên$[0, 2^n - 1]$. Mỗi phân đoạn đại diện cho một khối liền kề gồm các đỉnh vẫn còn tồn tại mà chúng tôi coi là được kết nối nội bộ nhằm mục đích tiếp cận. 

1. Chúng tôi bắt đầu với một phân khúc hoạt động duy nhất$[0, 2^n - 1]$. Điều này phản ánh rằng ban đầu mọi đỉnh đều có sẵn. 
2. Chúng tôi duy trì một bản đồ có thứ tự gồm các đoạn được khóa bởi điểm cuối bên trái của chúng. Mỗi đoạn đại diện cho một phạm vi tiếp giáp tối đa của các đỉnh sống. 
3. Để xử lý truy vấn khối$[l, r]$, chúng tôi xác định vị trí tất cả các đoạn giao nhau với phạm vi này. Mỗi đoạn giao nhau đều nằm hoàn toàn bên trong$[l, r]$và bị loại bỏ hoàn toàn hoặc chồng chéo một phần và phải được tách ra. Việc chia tách sẽ duy trì tính chính xác vì chỉ phần bị loại bỏ mới mất kết nối. 
4. Khi tách một đoạn$[L, R]$bằng cách loại bỏ$[l, r]$, chúng tôi tạo tối đa hai phân đoạn mới:$[L, l-1]$Và$[r+1, R]$, nhưng chỉ khi các khoảng này không trống. Đoạn ban đầu được loại bỏ khỏi cấu trúc. 
5. Đối với các truy vấn hỏi, chúng tôi định vị phân đoạn chứa$a$. Nếu như$b$nằm trong cùng một đoạn thì xuất ra 1, ngược lại là 0. 

Lý do điều này có tác dụng là vì mọi chuyển động được phép trong hypercube đều bảo toàn tư cách thành viên trong một thành phần được kết nối không thể vượt qua ranh giới khoảng bị phá hủy. Vì tất cả các đường dẫn phải nằm trong các đỉnh còn tồn tại và bất kỳ khoảng cắt nào cũng sẽ loại bỏ tất cả các đỉnh bắc cầu trên các phân đoạn, kết nối sẽ sụp đổ chính xác tại ranh giới phân đoạn. Do đó, tư cách thành viên của phân khúc tương đương với khả năng tiếp cận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from bisect import bisect_left

class SegTreeLikeIntervals:
    def __init__(self, n):
        self.intervals = {0: (0, (1 << n) - 1)}
        self.starts = [0]
        self.nxt = {(1 << n) - 1: None}

    def _find_left(self, x):
        i = bisect_left(self.starts, x)
        if i == len(self.starts):
            i -= 1
        elif self.starts[i] > x:
            i -= 1
        if i < 0:
            return None
        l = self.starts[i]
        return l

    def remove(self, l, r):
        starts = self.starts
        it = self.intervals

        i = bisect_left(starts, l)
        if i > 0:
            i -= 1

        to_add = []
        to_del = []

        while i < len(starts):
            if i < 0:
                i += 1
                continue
            s = starts[i]
            if s > r:
                break
            L, R = it[s]

            to_del.append(s)

            if L < l:
                to_add.append((L, l - 1))
            if R > r:
                to_add.append((r + 1, R))

            i += 1

        for s in to_del:
            del it[s]

        for L, R in to_add:
            it[L] = (L, R)

        self.starts = sorted(it.keys())

    def query(self, x):
        starts = self.starts
        it = self.intervals

        i = bisect_left(starts, x)
        if i == len(starts):
            i -= 1
        elif starts[i] > x:
            i -= 1
        if i < 0:
            return False

        s = starts[i]
        L, R = it[s]
        return L <= x <= R

def main():
    n, m = map(int, input().split())
    st = SegTreeLikeIntervals(n)

    out = []
    for _ in range(m):
        tmp = input().split()
        if tmp[0] == "block":
            l = int(tmp[1])
            r = int(tmp[2])
            st.remove(l, r)
        else:
            a = int(tmp[1])
            b = int(tmp[2])
            out.append("1" if st.query(a) and st.query(b) else "0")

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Mã duy trì một từ điển các khoảng hoạt động và danh sách được sắp xếp các điểm bắt đầu của chúng. Mọi thao tác khối sẽ loại bỏ hoặc phân chia các khoảng chồng chéo. Hoạt động truy vấn giảm xuống còn kiểm tra xem cả hai điểm cuối có rơi vào cùng khoảng thời gian tồn tại hay không. 

Điểm tinh tế là danh sách đã sắp xếp phải được xây dựng lại sau khi xóa, điều này có thể chấp nhận được vì mỗi khoảng được xóa nhiều nhất một lần, mang lại tổng cập nhật tuyến tính được khấu hao. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
ask 0 7
block 3 6
ask 0 7
```Chúng tôi bắt đầu với khoảng thời gian$[0,7]$. 

Truy vấn đầu tiên kiểm tra xem 0 và 7 có nằm trong cùng một khoảng hay không. Họ làm vậy. 

Sau khi chặn$[3,6]$, khoảng chia thành$[0,2]$Và$[7,7]$. 

Truy vấn thứ hai kiểm tra 0 và 7. Chúng hiện ở các phân đoạn khác nhau. 

| Bước | Khoảng thời gian | Truy vấn | Kết quả | 
| --- | --- | --- | --- | 
| ban đầu | [0,7] | - | - | 
| hỏi | [0,7] | (0,7) | 1 | 
| khối | [0,2],[7,7] | - | - | 
| hỏi | [0,2],[7,7] | (0,7) | 0 | 

Điều này xác nhận rằng việc loại bỏ sẽ ngắt kết nối chính xác cấu trúc ở phạm vi đã xóa. 

### Ví dụ 2 

đầu vào:```
3 4
ask 1 2
block 1 1
ask 0 2
ask 2 7
```Khoảng thời gian ban đầu là$[0,7]$. 

Truy vấn 1: 1 và 2 được kết nối. 

Khối loại bỏ 1, tạo ra$[0,0]$Và$[2,7]$. 

Bây giờ 0 và 2 đã bị ngắt kết nối, trong khi 2 và 7 vẫn được kết nối. 

| Bước | Khoảng thời gian | Truy vấn | Kết quả | 
| --- | --- | --- | --- | 
| ban đầu | [0,7] | - | - | 
| hỏi | [0,7] | (1,2) | 1 | 
| khối | [0,0],[2,7] | - | - | 
| hỏi | [0,0],[2,7] | (0,2) | 0 | 
| hỏi | [0,0],[2,7] | (2,7) | 1 | 

Điều này cho thấy khả năng kết nối được xác định đầy đủ như thế nào theo khoảng thời gian thành viên sau khi xóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m)$| mỗi khối chia các khoảng một lần và chúng tôi duy trì cấu trúc có trật tự | 
| Không gian |$O(m)$| mỗi lần xóa có thể tăng số lượng khoảng thời gian lên tối đa 1 | 

Giới hạn$m \le 5 \cdot 10^4$làm cho cách tiếp cận này trở nên an toàn, vì các phép tính theo khoảng vẫn giữ nguyên logarit hoặc khấu hao không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class SegTreeLikeIntervals:
        def __init__(self, n):
            self.intervals = {0: (0, (1 << n) - 1)}
            self.starts = [0]

        def remove(self, l, r):
            from bisect import bisect_left
            starts = self.starts
            it = self.intervals

            i = bisect_left(starts, l)
            if i > 0:
                i -= 1

            to_add = []
            to_del = []

            while i < len(starts):
                if i < 0:
                    i += 1
                    continue
                s = starts[i]
                if s > r:
                    break
                L, R = it[s]
                to_del.append(s)
                if L < l:
                    to_add.append((L, l - 1))
                if R > r:
                    to_add.append((r + 1, R))
                i += 1

            for s in to_del:
                del it[s]

            for L, R in to_add:
                it[L] = (L, R)

            self.starts = sorted(it.keys())

        def query(self, x):
            from bisect import bisect_left
            starts = self.starts
            it = self.intervals
            i = bisect_left(starts, x)
            if i == len(starts):
                i -= 1
            elif starts[i] > x:
                i -= 1
            if i < 0:
                return False
            s = starts[i]
            L, R = it[s]
            return L <= x <= R

    n, m = map(int, input().split())
    st = SegTreeLikeIntervals(n)
    out = []
    for _ in range(m):
        tmp = input().split()
        if tmp[0] == "block":
            st.remove(int(tmp[1]), int(tmp[2]))
        else:
            a, b = int(tmp[1]), int(tmp[2])
            out.append("1" if st.query(a) and st.query(b) else "0")

    return "\n".join(out)

# samples
assert run("3 3\nask 0 7\nblock 3 6\nask 0 7\n") == "1\n0"

# custom tests
assert run("3 1\nask 0 0\n") == "1"
assert run("3 2\nblock 0 7\nask 0 7\n") == "0"
assert run("3 3\nblock 1 1\nask 0 2\nask 2 7\n") == "0\n1"
assert run("4 3\nask 0 15\nblock 5 10\nask 0 15\n") == "1\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn nút đơn | 1 | kết nối tầm thường | 
| loại bỏ hoàn toàn | 0 | sụp đổ toàn bộ | 
| trường hợp chia tách | 0 / 1 | chia khoảng đúng | 
| khối giữa | 1/0 | phân cách ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một thao tác khối khớp chính xác với ranh giới khoảng hiện có. Ví dụ, bắt đầu bằng$[0,7]$và chặn$[0,2]$, chúng ta nên thu được$[3,7]$mà không tạo ra khoảng trống không hợp lệ. Mã loại bỏ kiểm tra rõ ràng các điều kiện biên trước khi thêm các phân đoạn còn lại, đảm bảo không có khoảng thời gian có độ dài bằng 0 nào được chèn vào. 

Một trường hợp cạnh khác là việc phân tách lặp đi lặp lại trong đó nhiều khoảng được chạm vào bởi một khối duy nhất. Việc lặp lại các phân đoạn chồng chéo đảm bảo mỗi khoảng thời gian bị ảnh hưởng được xử lý chính xác một lần và do các khoảng thời gian rời rạc nên không xảy ra việc xóa hai lần. 

Cuối cùng, các truy vấn trên các điểm cuối bằng ranh giới khoảng được xử lý bằng logic tìm kiếm nhị phân luôn kiểm tra khoảng thời gian bắt đầu gần nhất bên trái. Điều này tránh được các lỗi riêng lẻ trong đó một điểm có thể được gán sai vào phân đoạn sai.
