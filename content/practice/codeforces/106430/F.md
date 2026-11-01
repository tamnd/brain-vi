---
title: "CF 106430F - Bessie tại Ngân hàng"
description: "Chúng tôi đang duy trì mã bảo mật được biểu thị dưới dạng một mảng số nguyên thay đổi theo thời gian. Sau mỗi lần thay đổi, chúng ta cần đánh giá một điều kiện dựa trên danh sách cố định các “số yêu thích” $d1, d2, dots, dD$, trong đó $D le 30$."
date: "2026-06-20T23:12:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106430
codeforces_index: "F"
codeforces_contest_name: "2026 USACO.Guide Informatics Tournament"
rating: 0
weight: 106430
solve_time_s: 52
verified: true
draft: false
---

[CF 106430F - Bessie tại Ngân hàng](https://codeforces.com/problemset/problem/106430/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì mã bảo mật được biểu thị dưới dạng một mảng số nguyên thay đổi theo thời gian. Sau mỗi lần thay đổi, chúng ta cần đánh giá một điều kiện dựa trên danh sách cố định các “số yêu thích”$d_1, d_2, \dots, d_D$, Ở đâu$D \le 30$. 

Đối với bất kỳ phiên bản nào của mảng, chúng tôi quan tâm đến số yêu thích nào chia mọi phần tử trong mảng. Nhiệm vụ không phải là liệt kê chúng mà là tính tổng của tất cả những thứ đó.$d_i$. 

Vì vậy, mỗi truy vấn sẽ hỏi: sau khi áp dụng một số cập nhật cho phiên bản trước của mảng, hãy xem xét mảng hiện tại, tìm tất cả các giá trị yêu thích chia mọi phần tử và xuất ra tổng của chúng. 

Một cách trực tiếp để nghĩ về điều kiện là một số$d_i$chia tất cả các phần tử nếu nó chia gcd của toàn bộ mảng. Tuy nhiên, việc duy trì gcd trên nhiều phiên bản liên tục là điều khó khăn vì mỗi bản cập nhật đều được áp dụng cho phiên bản trước đó, không nhất thiết phải là phiên bản mới nhất. 

Các ràng buộc ngụ ý kích thước mảng và số lượng thao tác đủ lớn để không thể tính toán lại toàn bộ mảng cho mỗi truy vấn. Một lần tính toán lại cho mỗi truy vấn sẽ tốn$O(n)$, dẫn đến$O(nq)$, quá chậm khi cả hai đều lớn. 

Trường hợp khó nhận biết xuất hiện khi cập nhật nhánh từ các phiên bản trước. Ví dụ: nếu phiên bản 3 sửa đổi chỉ số 5 của phiên bản 1 thì phiên bản 3 không được phụ thuộc vào phiên bản 2. Bất kỳ giải pháp nào chỉ lưu trữ “mảng mới nhất” hoặc áp dụng các bản cập nhật triệt để sẽ thất bại ở đây. 

Một cạm bẫy khác là cho rằng chỉ theo dõi gcd là đủ. Ngay cả khi gcd được duy trì, lịch sử phiên bản phân nhánh khiến việc cập nhật hiệu quả mà không cần tính toán lại từ đầu là không tầm thường. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ tính toán lại toàn bộ mảng sau mỗi truy vấn, sau đó kiểm tra từng số yêu thích theo gcd hoặc trực tiếp với mọi phần tử. Đối với mỗi phiên bản, chúng tôi quét toàn bộ mảng để tính gcd hoặc xác minh tính chia hết. Điều này đúng vì nó trực tiếp thực thi định nghĩa, nhưng mỗi truy vấn đều tốn$O(n)$và các bản cập nhật có thể lên tới$10^5$, làm cho tổng công là bậc hai trong trường hợp xấu nhất. 

Quan sát quan trọng là số lượng ước số yêu thích là cực kỳ nhỏ, nhiều nhất là 30. Thay vì nghĩ về các giá trị gcd, chúng ta có thể mã hóa thông tin chia hết trực tiếp dưới dạng mặt nạ bit cho mỗi phần tử mảng. Mỗi phần tử lưu trữ một mặt nạ 30 bit trong đó bit$i$cho biết liệu$d_i$chia phần tử đó. 

Khi mọi phần tử được biểu diễn dưới dạng mặt nạ, điều kiện “$d_i$chia tất cả các phần tử” trở thành AND theo từng bit trên toàn bộ mảng: chúng tôi tính toán AND của tất cả các mặt nạ phần tử và bất kỳ bit nào còn sót lại đều tương ứng chính xác với một ước số hợp lệ. 

Thách thức còn lại là hỗ trợ cập nhật các phiên bản trước một cách hiệu quả. Vì mỗi bản cập nhật tạo ra một phiên bản mới bắt nguồn từ phiên bản cũ nên chúng tôi cần một cấu trúc bảo tồn lịch sử đồng thời cho phép sửa đổi điểm. Cây phân đoạn cố định thực hiện chính xác điều này: mỗi phiên bản có chung cấu trúc không thay đổi với các phiên bản trước đó và chỉ$O(\log n)$các nút được sao chép mỗi lần cập nhật. 

Mỗi nút cây phân đoạn lưu trữ bit AND của phân đoạn đó. Do đó, phần gốc của mỗi phiên bản ngay lập tức biểu thị AND trên toàn bộ mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây phân đoạn liên tục + bitmask | O((n + q) log n + qD) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi từng số yêu thích$d_i$thành một chỉ mục cố định và liên kết nó với một bit trong mặt nạ. Điều này cho phép tất cả các phép kiểm tra tính chia hết được lưu trữ gọn gàng dưới dạng số nguyên. 
2. Đối với mỗi phần tử mảng, hãy tính mặt nạ của nó bằng cách kiểm tra khả năng chia hết cho tất cả$d_i$. Nếu như$d_i \mid a[j]$, đặt bit$i$. Điều này xử lý trước từng giá trị thành biểu diễn 30 bit. 
3. Xây dựng cây phân đoạn trên các mặt nạ này, trong đó mỗi lá lưu trữ một mặt nạ phần tử. Các nút bên trong lưu trữ AND theo bit của các nút con của chúng, bởi vì một giá trị chia tất cả các phần tử trong một phân đoạn một cách chính xác khi nó chia cả hai nửa. 
4. Duy trì tính bền vững bằng cách chỉ sao chép các nút dọc theo đường dẫn cập nhật. Khi cập nhật chỉ mục$idx$, tạo các nút mới dọc theo đường dẫn từ gốc tới lá trong khi sử dụng lại các cây con không thay đổi từ phiên bản trước. 
5. Đối với mỗi truy vấn, hãy đi theo con trỏ tới thư mục gốc của phiên bản được yêu cầu. Mặt nạ gốc bây giờ đại diện cho AND của tất cả các phần tử trong phiên bản đó. 
6. Tính toán câu trả lời bằng cách lặp lại tất cả các bit trong mặt nạ gốc. Nếu bit$i$được thiết lập, thêm$d_i$đến kết quả. 

### Tại sao nó hoạt động 

Mỗi nút cây phân đoạn lưu trữ AND theo bit của tất cả các mặt nạ trong phân đoạn của nó. Bitwise AND có tính kết hợp và giao hoán, do đó việc hợp nhất các phân đoạn theo bất kỳ thứ tự nào sẽ tạo ra kết quả tương tự như tính toán trên toàn bộ mảng. Vì tính bền vững đảm bảo gốc của mỗi phiên bản tương ứng chính xác với trạng thái mảng của nó nên mặt nạ gốc luôn là AND toàn cục chính xác. Một bit vẫn được đặt khi và chỉ khi mọi phần tử trong mảng có tập hợp bit đó, tương đương với việc mọi phần tử đều chia hết cho số tương ứng$d_i$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("l", "r", "mask")
    def __init__(self, l=None, r=None, mask=0):
        self.l = l
        self.r = r
        self.mask = mask

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.arr = arr
        self.root = self.build(0, self.n - 1)

    def build(self, l, r):
        if l == r:
            return Node(None, None, self.arr[l])
        m = (l + r) // 2
        left = self.build(l, m)
        right = self.build(m + 1, r)
        return Node(left, right, left.mask & right.mask)

    def update(self, node, l, r, idx, val):
        if l == r:
            return Node(None, None, val)
        m = (l + r) // 2
        if idx <= m:
            new_left = self.update(node.l, l, m, idx, val)
            return Node(new_left, node.r, new_left.mask & node.r.mask)
        else:
            new_right = self.update(node.r, m + 1, r, idx, val)
            return Node(node.l, new_right, node.l.mask & new_right.mask)

def main():
    n, q, d = map(int, input().split())
    divisors = list(map(int, input().split()))
    idx = {v: i for i, v in enumerate(divisors)}

    def to_mask(x):
        m = 0
        for i, v in enumerate(divisors):
            if x % v == 0:
                m |= (1 << i)
        return m

    arr = list(map(int, input().split()))
    arr = [to_mask(x) for x in arr]

    st = SegTree(arr)
    versions = [st.root]

    out = []
    for _ in range(q):
        line = input().split()
        prev, pos, val = int(line[0]), int(line[1]) - 1, int(line[2])
        new_mask = to_mask(val)
        new_root = st.update(versions[prev], 0, n - 1, pos, new_mask)
        versions.append(new_root)

        root_mask = new_root.mask
        ans = 0
        for i, v in enumerate(divisors):
            if root_mask & (1 << i):
                ans += v
        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Cây phân đoạn lưu trữ các mặt nạ thay vì các giá trị thô, vì vậy mỗi lần hợp nhất chỉ là AND theo bit. Lớp kiên trì tiềm ẩn trong thực tế là mỗi bản cập nhật trả về một gốc mới mà không sửa đổi các nút hiện có. Mảng phiên bản rất quan trọng vì các truy vấn tham chiếu rõ ràng đến các phiên bản cũ hơn thay vì luôn mở rộng trạng thái mới nhất. 

Phần tinh tế duy nhất là hãy nhớ rằng bản cập nhật sử dụng phiên bản gốc trước đó chứ không phải phiên bản được xây dựng cuối cùng. 

## Ví dụ đã hoạt động 

Xét ước số$[2, 3, 5]$và một mảng ban đầu$[6, 10]$. 

### Ví dụ 1 

Chúng tôi xây dựng mặt nạ: 

- 6 → 110 (chia 2 và 3) 
- 10 → 101 (chia 2 và 5) 

Root AND là 100 nên chỉ còn 2 sống sót, đáp án là 2. 

| Bước | Mặt nạ mảng | VÀ kết quả | Ước số hoạt động | 
| --- | --- | --- | --- | 
| Xây dựng | [110, 101] | 100 | {2} | 

Bây giờ giả sử chúng ta cập nhật chỉ mục 1 lên giá trị 15: 

- 15 → 010 

Mảng mới là [6, 15], mặt nạ [110, 010], AND là 010, câu trả lời là 3. 

| Bước | Mặt nạ mảng | VÀ kết quả | Ước số hoạt động | 
| --- | --- | --- | --- | 
| Cập nhật | [110, 010] | 010 | {3} | 

Điều này cho thấy một bản cập nhật duy nhất có thể thay đổi hoàn toàn những ước số nào tồn tại. 

### Ví dụ 2 

Ước số$[4, 6]$, mảng$[12, 18, 24]$. 

Mặt nạ: 

- 12 → 11 
- 18 → 10 
- 24 → 11 

AND là 10 nên đáp án là 6. 

Bây giờ cập nhật chỉ số 2 lên 8: 

- 8 → 01 

Mảng trở thành [12, 18, 8], mặt nạ [11, 10, 01], AND là 00, câu trả lời là 0. 

Điều này chứng tỏ rằng ngay cả khi hầu hết các phần tử đều hỗ trợ ước số, một bản cập nhật không tương thích sẽ loại bỏ hoàn toàn nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\log n + qD)$| Cập nhật cây phân đoạn cho mỗi truy vấn cộng với quét tới 30 bit | 
| Không gian |$O(n \log n)$| Các nút liên tục được tạo dọc theo đường dẫn cập nhật | 

Hệ số logarit xuất phát từ việc sao chép từ gốc sang lá trong cây phân đoạn cố định và hệ số không đổi cho các ước số được giới hạn bằng 30, giúp giải pháp nhanh chóng một cách thoải mái đối với đầu vào lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from subprocess import run as r
    return "not executed"

# These are illustrative placeholders since full integration requires main() refactor

# minimal case
# n=1, single divisor always works

# boundary case
# all elements identical

# full break case
# update removes all valid divisors
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn tối thiểu | tổng tầm thường | độ đúng cơ sở | 
| tất cả các mảng bằng nhau | bộ chia đầy đủ | ổn định không thay đổi | 
| cập nhật phá hoại | 0 | trường hợp vô hiệu hoàn toàn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi các bản cập nhật đề cập đến các phiên bản rất cũ thay vì phiên bản mới nhất. Cấu trúc bền vững đảm bảo tính chính xác vì mỗi phiên bản gốc đều độc lập. Nếu phiên bản 0 có mảng [6, 10] và phiên bản 1 sửa đổi chỉ số 0 thành 15 thì phiên bản 2 vẫn có thể phân nhánh từ phiên bản 0. Chức năng cập nhật luôn sử dụng gốc được cung cấp, do đó cấu trúc trước đó được giữ nguyên mà không bị can thiệp. 

Một trường hợp cạnh khác là khi một giá trị chia hết cho không có số yêu thích nào. Mặt nạ của nó là 0 và AND trên bất kỳ phân đoạn nào chứa nó trở thành 0. Điều này ngay lập tức buộc tất cả các câu trả lời trong phiên bản đó là 0, điều này đúng vì không$d_i$có thể chia tất cả các phần tử nếu một phần tử không đạt tất cả các lần kiểm tra.
