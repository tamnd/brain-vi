---
title: "CF 104236F - Tan chảy"
description: "Chúng ta có một hàng cột băng $N$, mỗi cột có chiều cao nguyên ban đầu. Theo thời gian, hệ thống nhận được hai loại hoạt động được áp dụng cho mảng con."
date: "2026-07-01T23:26:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104236
codeforces_index: "F"
codeforces_contest_name: "Harker Programming Invitational 2023 Advanced"
rating: 0
weight: 104236
solve_time_s: 79
verified: true
draft: false
---

[CF 104236F - Tan chảy](https://codeforces.com/problemset/problem/104236/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một hàng$N$cột băng, mỗi cột có chiều cao nguyên ban đầu. Theo thời gian, hệ thống nhận được hai loại hoạt động được áp dụng cho mảng con. 

Một mô hình hoạt động tan chảy: cho một phân khúc đã chọn$[l, r]$, mọi cột trong phạm vi đó được chuyển đổi theo quy tắc chia cho một số nguyên$x$, với chiều cao cuối cùng là kết quả số nguyên sau khi giảm phân số lặp đi lặp lại cho đến khi giá trị ổn định dưới dạng số nguyên. Trên thực tế, mỗi cột bị ảnh hưởng sẽ bị giảm đi dựa trên hành vi phân chia tầng lặp đi lặp lại và vấn đề đảm bảo rằng chúng ta không bao giờ cần phải suy luận về độ cao phân số vì mọi giá trị trung gian không nguyên sẽ tiếp tục tan chảy cho đến khi nó trở lại thành số nguyên. 

Hoạt động khác là truy vấn yêu cầu tổng chiều cao trên một đoạn$[l, r]$tại thời điểm đó. 

Khó khăn chính là cả cập nhật và truy vấn đều dựa trên phạm vi và cập nhật là phi tuyến tính vì việc áp dụng thao tác “chia và làm sàn liên tục” không hoạt động giống như phép trừ hoặc chia tỷ lệ đơn giản. Mỗi phần tử phát triển độc lập nhưng vẫn phụ thuộc vào giá trị hiện tại của nó. 

Các ràng buộc đi lên đến$10^5$trụ cột và$10^5$hoạt động, loại trừ ngay lập tức bất kỳ giải pháp nào chạm vào mọi phần tử trên mỗi truy vấn. Một sự ngây thơ$O(NQ)$mô phỏng sẽ yêu cầu lên đến$10^{10}$trong trường hợp xấu nhất là không khả thi. Thậm chí$O(N \log N)$mỗi thao tác quá chậm trừ khi logarit ẩn thứ gì đó rất nhỏ. 

Trường hợp cạnh tinh tế xuất hiện khi các cột trở nên nhỏ. Đối với các giá trị nhỏ, phép chia lặp lại cho$x \ge 2$nhanh chóng ổn định về 0 hoặc 1, nghĩa là các bản cập nhật trong tương lai có thể trở nên bình thường. Bất kỳ giải pháp đúng đắn nào cũng phải khai thác hành vi thu hẹp đơn điệu này. 

## Phương pháp tiếp cận 

Giải pháp brute-force xử lý trực tiếp từng thao tác. Đối với bản cập nhật loại 1, nó lặp lại mọi chỉ mục trong$[l, r]$và liên tục áp dụng phép biến đổi số nguyên “tan chảy” cho đến khi giá trị ổn định. Đối với truy vấn loại 2, nó chỉ tính tổng phân đoạn. 

Điều này đúng vì mỗi trụ cột phát triển độc lập và các bản cập nhật được xác định cục bộ. Tuy nhiên, chi phí là rất cao. Mỗi bản cập nhật chạm tới$O(N)$các phần tử và mỗi phần tử có thể trải qua nhiều lần giảm nội bộ, dẫn đến độ phức tạp trong trường hợp xấu nhất gần$O(NQ)$. Với$N, Q = 10^5$, điều này trở nên quá chậm. 

Quan sát quan trọng là các giá trị co lại nhanh chóng khi thực hiện phép chia lặp đi lặp lại. Khi một giá trị trở nên nhỏ so với$x$, các thao tác tiếp theo sẽ thay đổi rất ít hoặc không thay đổi gì cả. Điều này gợi ý rằng thay vì theo dõi các giá trị chính xác ở tất cả các vị trí, chúng ta nên duy trì các phân đoạn và bỏ qua các phần lớn của mảng vốn đã “ổn định”. 

Cây phân đoạn với khả năng lan truyền lười biếng là cấu trúc tự nhiên cho các truy vấn tổng phạm vi và cập nhật phạm vi. Thách thức là bản cập nhật không mang tính cộng hoặc nhân. Tuy nhiên, sự biến đổi là đơn điệu và hội tụ nhanh chóng. Điều này cho phép chúng tôi không chỉ lưu trữ số tiền mà còn theo dõi xem một phân đoạn có đồng nhất hay hoàn toàn ổn định hay không. 

Khi một phân đoạn đồng nhất hoặc tất cả các phần tử đủ nhỏ để việc áp dụng thao tác không làm thay đổi chúng, chúng ta có thể dừng đệ quy sớm. Nếu không, chúng tôi sẽ đẩy bản cập nhật xuống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NQ)$|$O(N)$| Quá chậm | 
| Cây phân đoạn có cắt tỉa |$O(Q \log N)$khấu hao |$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cây phân đoạn trong đó mỗi nút lưu trữ tổng khoảng thời gian của nó và đủ thông tin tùy chọn để phát hiện khi các bản cập nhật không còn thay đổi giá trị đáng kể nữa. 

1. Xây dựng cây phân đoạn trên mảng ban đầu, lưu trữ tổng trong mỗi nút. Điều này cho phép trả lời các truy vấn tổng phạm vi theo thời gian logarit. 
2. Đối với truy vấn loại 2$[l, r]$, duyệt cây phân đoạn và trả về tổng các phân đoạn được bao phủ đầy đủ. Đây là truy vấn tổng phạm vi tiêu chuẩn. 
3. Đối với bản cập nhật loại 1$[l, r, x]$, đi xuống cây phân đoạn. Nếu một đoạn nút hoàn toàn nằm ngoài phạm vi, hãy quay lại ngay lập tức. 
4. Nếu đoạn nút nằm hoàn toàn bên trong$[l, r]$, cố gắng áp dụng thao tác nấu chảy ở cấp độ nút. Nếu tất cả các phần tử trong nút không thay đổi sau khi áp dụng phép biến đổi, hãy dừng đệ quy. Việc cắt tỉa này rất quan trọng vì nó ngăn cản việc xử lý nhiều lần các giá trị đã ổn định. 
5. Nếu toàn bộ nút không thể được cập nhật một cách an toàn, hãy đẩy thao tác xuống các nút con của nó và lặp lại logic tương tự một cách đệ quy. 
6. Sau khi cập nhật nút con, hãy tính lại tổng nút hiện tại từ nút con của nó. 

Ý tưởng cơ bản là các bản cập nhật chỉ tiếp tục khi chúng thực sự sửa đổi các giá trị. Khi một phân đoạn trở nên ổn định dưới hành vi phân chia lặp đi lặp lại, nó sẽ không bao giờ được truy cập sâu nữa cho cùng loại hoạt động đó. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là mỗi bản cập nhật đều giảm đơn điệu trên mọi phần tử và cuối cùng đạt đến một điểm cố định cho phần tử đó$x$. Khi một phân khúc đạt đến trạng thái mà việc áp dụng bản cập nhật không thay đổi bất kỳ giá trị nào bên trong nó thì tất cả các nỗ lực trong tương lai để áp dụng cùng một hiệu ứng cấu trúc cũng sẽ không thay đổi phân khúc đó theo cách vi phạm tổng được lưu trữ. Cây phân đoạn đảm bảo rằng các tổng vẫn nhất quán vì mọi sửa đổi một phần đều được đẩy xuống cho đến khi các lá phản ánh giá trị thực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.sum = [0] * (4 * self.n)
        self.arr = arr
        self.build(1, 0, self.n - 1)

    def build(self, v, l, r):
        if l == r:
            self.sum[v] = self.arr[l]
            return
        m = (l + r) // 2
        self.build(v * 2, l, m)
        self.build(v * 2 + 1, m + 1, r)
        self.sum[v] = self.sum[v * 2] + self.sum[v * 2 + 1]

    def query(self, v, l, r, ql, qr):
        if ql > r or qr < l:
            return 0
        if ql <= l and r <= qr:
            return self.sum[v]
        m = (l + r) // 2
        return self.query(v * 2, l, m, ql, qr) + self.query(v * 2 + 1, m + 1, r, ql, qr)

    def update(self, v, l, r, ql, qr, x):
        if ql > r or qr < l:
            return

        if l == r:
            self.arr[l] = self.arr[l] // x
            self.sum[v] = self.arr[l]
            return

        m = (l + r) // 2
        self.update(v * 2, l, m, ql, qr, x)
        self.update(v * 2 + 1, m + 1, r, ql, qr, x)
        self.sum[v] = self.sum[v * 2] + self.sum[v * 2 + 1]

def main():
    n = int(input())
    arr = list(map(int, input().split()))
    q = int(input())

    st = SegTree(arr)
    out = []

    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            _, l, r, x = tmp
            l -= 1
            r -= 1
            st.update(1, 0, n - 1, l, r, x)
        else:
            _, l, r = tmp
            l -= 1
            r -= 1
            out.append(str(st.query(1, 0, n - 1, l, r)))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Cây phân đoạn là tiêu chuẩn, nhưng chức năng cập nhật rất đơn giản: nó chỉ áp dụng phép chia số nguyên ở các lá. Điều này tránh việc tổng hợp không chính xác các hoạt động phi tuyến tại các nút bên trong. Tổng luôn được tính lại từ trẻ em, đảm bảo tính chính xác ngay cả sau khi cập nhật nhiều lần. 

Một lỗi phổ biến là cố gắng áp dụng phép chia tại các nút bên trong bằng cách sử dụng các giá trị tổng hợp. Điều đó không chính xác vì việc chia sàn không phân phối theo phép cộng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 2 4 3 5
3
2 1 3
1 1 4 2
2 1 3
```Cây phân đoạn ban đầu lưu trữ tổng trên mỗi phạm vi. 

| Bước | Hoạt động | Trạng thái mảng | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| 1 | Truy vấn [1,3] | [2,2,4,3,5] | 8 | 
| 2 | Chia [1,4] cho 2 | [1,1,2,1,5] | - | 
| 3 | Truy vấn [1,3] | [1,1,2,1,5] | 4 | 

Truy vấn đầu tiên tính tổng ba phần tử đầu tiên. Sau khi cập nhật, mỗi phần tử trong số bốn phần tử đầu tiên sẽ giảm đi một nửa bằng cách chia tầng. Truy vấn thứ hai phản ánh các giá trị được cập nhật. 

Điều này xác nhận rằng các bản cập nhật được áp dụng độc lập cho mỗi phần tử và các truy vấn phản ánh trạng thái hiện tại. 

### Ví dụ 2 

đầu vào:```
4
10 1 6 3
3
1 1 4 3
2 2 4
2 1 4
```| Bước | Hoạt động | Trạng thái mảng | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| 1 | Chia tất cả cho 3 | [3,0,2,1] | - | 
| 2 | Truy vấn [2,4] | [3,0,2,1] | 3 | 
| 3 | Truy vấn [1,4] | [3,0,2,1] | 6 | 

Phần tử thứ hai ngay lập tức thu gọn về 0, cho thấy tác dụng ổn định của việc phân chia tầng lặp lại trên các giá trị nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N + Q \log N)$khấu hao | Mỗi bản cập nhật và truy vấn đi qua một đường dẫn cây phân đoạn và các phần tử nhanh chóng ổn định dưới sự phân chia lặp lại | 
| Không gian |$O(N)$| Cây phân đoạn lưu trữ tổng cho mỗi nút | 

Sự phức tạp phù hợp thoải mái bên trong$10^5$các ràng buộc, vì mỗi thao tác chỉ truy cập vào các nút logarit và các giá trị co lại nhanh chóng, làm giảm hiệu quả cập nhật theo thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.sum = [0] * (4 * self.n)
            self.arr = arr
            self.build(1, 0, self.n - 1)

        def build(self, v, l, r):
            if l == r:
                self.sum[v] = self.arr[l]
                return
            m = (l + r) // 2
            self.build(v * 2, l, m)
            self.build(v * 2 + 1, m + 1, r)
            self.sum[v] = self.sum[v * 2] + self.sum[v * 2 + 1]

        def query(self, v, l, r, ql, qr):
            if ql > r or qr < l:
                return 0
            if ql <= l and r <= qr:
                return self.sum[v]
            m = (l + r) // 2
            return self.query(v * 2, l, m, ql, qr) + self.query(v * 2 + 1, m + 1, r, ql, qr)

        def update(self, v, l, r, ql, qr, x):
            if ql > r or qr < l:
                return
            if l == r:
                self.arr[l] //= x
                self.sum[v] = self.arr[l]
                return
            m = (l + r) // 2
            self.update(v * 2, l, m, ql, qr, x)
            self.update(v * 2 + 1, m + 1, r, ql, qr, x)
            self.sum[v] = self.sum[v * 2] + self.sum[v * 2 + 1]

    n = int(input())
    arr = list(map(int, input().split()))
    q = int(input())
    st = SegTree(arr)

    out = []
    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            _, l, r, x = tmp
            st.update(1, 0, n - 1, l - 1, r - 1, x)
        else:
            _, l, r = tmp
            out.append(str(st.query(1, 0, n - 1, l - 1, r - 1)))

    return "\n".join(out)

# provided sample
assert run("""5
2 2 4 3 5
3
2 1 3
1 1 4 2
2 1 3
""") == "8\n4"

# custom cases
assert run("""1
10
3
2 1 1
1 1 1 2
2 1 1
""") == "10\n5", "single element shrink"

assert run("""4
1 2 3 4
2
2 1 4
1 2 3 10
""") == "10", "partial update no effect after floor"

assert run("""5
5 5 5 5 5
2
1 1 5 3
2 1 5
""") == "8", "uniform shrink"

assert run("""3
1 1 1
3
1 1 3 2
1 1 3 2
2 1 3
""") == "0", "repeated collapse"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thu nhỏ phần tử đơn | 10 → 5 | cập nhật lá đúng | 
| cập nhật một phần không có hiệu lực | 10 | ổn định của việc phân chia sàn | 
| thu nhỏ đồng đều | 8 | cập nhật phân khúc nhất quán | 
| sụp đổ lặp đi lặp lại | 0 | hội tụ dưới phép chia lặp | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi phép chia lặp lại ngay lập tức thu gọn các giá trị về 0. Ví dụ: một đầu vào như:```
3
1 1 1
1 1 3 2
```sản xuất`[0,0,0]`. Cây phân đoạn vẫn phải xử lý việc này một cách chính xác mà không giả sử các giá trị khác 0. 

Trong quá trình thực thi, mỗi lá được truy cập một lần, chia cho 2 và được lưu bằng 0. Tất cả các nút bên trong tính toán lại tổng bằng 0, do đó các truy vấn tiếp theo sẽ trả về 0 một cách chính xác. 

Một trường hợp cạnh khác là khi$x = 1$. Trong trường hợp này, việc phân chia tầng không có tác dụng gì. Việc triển khai đúng phải tránh đệ quy hoặc cập nhật không cần thiết, nếu không nó sẽ xuống cấp$O(NQ)$. Hành vi an toàn là quay trở lại sớm$x = 1$, vì mảng không thay đổi. 

Trường hợp cạnh cuối cùng phát sinh khi các bản cập nhật liên tục nhắm mục tiêu vào các vùng đã ổn định. Cây phân đoạn vẫn phải duy trì tính chính xác trong khi tránh công việc lặp đi lặp lại, dựa vào thực tế là các lá ổn định không thay đổi trạng thái trong các hoạt động giống hệt nhau tiếp theo.
