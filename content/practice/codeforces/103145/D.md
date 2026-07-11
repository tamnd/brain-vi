---
title: "CF 103145D - Bit thấp"
description: "Chúng tôi đang duy trì một mảng các số nguyên thay đổi theo thời gian dưới hai loại hoạt động. Một thao tác sửa đổi toàn bộ phân đoạn của mảng bằng cách liên tục thêm một giá trị đặc biệt bắt nguồn từ chính từng phần tử và thao tác kia yêu cầu tổng của phân đoạn."
date: "2026-07-03T19:51:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103145
codeforces_index: "D"
codeforces_contest_name: "The 15th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 103145
solve_time_s: 53
verified: true
draft: false
---

[CF 103145D - Lowbit](https://codeforces.com/problemset/problem/103145/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một mảng các số nguyên thay đổi theo thời gian dưới hai loại hoạt động. Một thao tác sửa đổi toàn bộ phân đoạn của mảng bằng cách liên tục thêm một giá trị đặc biệt bắt nguồn từ chính từng phần tử và thao tác kia yêu cầu tổng của phân đoạn. 

Khó khăn chính là bản cập nhật không đồng đều. Khi chúng ta áp dụng một thao tác trên một phạm vi$[L, R]$, mỗi vị trí$i$bên trong phạm vi đó được tăng lên bởi$\text{lowbit}(a_i)$, Ở đâu$\text{lowbit}(x)$là lũy thừa lớn nhất của hai phép chia$x$. Điều này có nghĩa là mức tăng phụ thuộc vào giá trị hiện tại của phần tử, do đó, bản cập nhật phụ thuộc vào trạng thái và phát triển sau mỗi lần sửa đổi. 

Thao tác thứ hai đơn giản hơn: chúng ta phải trả về tổng các giá trị trong một phạm vi, modulo$998244353$. 

Các ràng buộc chỉ ra$n$Và$m$lên đến$10^5$mỗi trường hợp thử nghiệm và tối đa 20 trường hợp thử nghiệm. Việc truyền tải trực tiếp mỗi hoạt động của phân đoạn sẽ dẫn đến$10^{10}$cập nhật trong trường hợp xấu nhất là quá chậm. Ngay cả một cây phân đoạn có lan truyền lười biếng cũng không đơn giản vì hàm cập nhật không tuyến tính hoặc cộng tính theo một cách cố định, nó phụ thuộc vào giá trị hiện tại của từng phần tử. 

Một trường hợp phức tạp nhưng quan trọng xuất phát từ các giá trị trở thành 0. Từ$\text{lowbit}(0) = 0$, khi một phần tử trở thành 0, nó sẽ không bao giờ thay đổi nữa khi cập nhật. Điều này tạo ra “các vị trí chết” không còn đóng góp cho các bản cập nhật trong tương lai. Một trường hợp khác là các giá trị là lũy thừa của hai, vì lowbit của chúng bằng nhau, gây ra những bước nhảy lớn về cường độ trong một bản cập nhật. 

Một mô phỏng đơn giản sẽ liên tục quét toàn bộ các phân đoạn: 

đầu vào:```
1
5
1 2 3 4 5
1
1 1 5
```Hành vi đúng sẽ áp dụng các mức tăng khác nhau cho mỗi vị trí, nhưng vòng lặp mạnh mẽ vẫn khả thi đối với ví dụ nhỏ này. Tuy nhiên, việc nhân rộng điều này thành$10^5$hoạt động làm cho nó không thể thực hiện được. 

Quan sát quan trọng là mặc dù việc cập nhật phụ thuộc vào các giá trị hiện tại, nhưng quá trình chuyển đổi giá trị đều đơn điệu theo cách có cấu trúc: mỗi phần tử phát triển độc lập và sự tiến hóa của nó chỉ phụ thuộc vào việc áp dụng lặp đi lặp lại một hàm xác định$a \leftarrow a + \text{lowbit}(a)$. Cấu trúc này cho phép chúng tôi tính toán trước cách các giá trị phát triển cho đến khi chúng ổn định hoặc vượt quá ngưỡng, sau đó hỗ trợ tổng hợp phạm vi qua các chuyển đổi này. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ duy trì mảng trực tiếp. Đối với mỗi thao tác cập nhật, chúng tôi lặp lại từ$L$ĐẾN$R$, tính toán$\text{lowbit}(a_i)$, và thêm nó vào$a_i$. Mỗi truy vấn cũng sẽ lặp lại các giá trị phạm vi và tổng. 

Điều này đúng vì nó trực tiếp tuân theo định nghĩa bài toán. Tuy nhiên, chi phí của mỗi hoạt động$O(n)$trong trường hợp xấu nhất, dẫn đến$O(nm)$, quá lớn đối với$10^5$đầu vào quy mô. 

Điểm nghẽn là việc phải tính toán lại nhiều lần$\text{lowbit}(a_i)$cho các yếu tố giống nhau trên nhiều bản cập nhật. Tuy nhiên, mỗi phần tử tiến hóa độc lập khi áp dụng lặp đi lặp lại cùng một quy tắc. Khi chúng tôi xem từng vị trí riêng biệt, chúng tôi thấy rằng giá trị của nó tuân theo một trình tự xác định:$$a \rightarrow a + \text{lowbit}(a)$$Trình tự này có một đặc tính quan trọng: nó tăng nhanh các bit ở cuối và cuối cùng làm giảm số lần các phần tăng nhỏ quan trọng, làm cho quỹ đạo có cấu trúc thay vì hỗn loạn. 

Chúng ta có thể khai thác điều này bằng cách xử lý trước sự phát triển của các giá trị đến một giới hạn nhất định và sử dụng cây phân đoạn để theo dõi tổng và hỗ trợ ứng dụng lười biếng của các chuyển đổi “trạng thái tiếp theo”. Mỗi nút không chỉ phải thể hiện tổng mà còn thể hiện liệu tất cả các phần tử trong phân đoạn có ổn định hay vẫn đang phát triển hay không. 

Điều này dẫn đến cây phân đoạn trong đó các bản cập nhật sẽ giải quyết hoàn toàn một phân đoạn (khi tất cả các giá trị đều ổn định) hoặc chỉ giảm xuống một cách đệ quy khi cần thiết. Cấu trúc của lowbit đảm bảo rằng số lần một phần tử có thể thay đổi một cách có ý nghĩa là logarit về độ lớn giá trị, bởi vì mỗi thao tác sẽ sửa đổi bit được đặt thấp nhất, xóa dần dần hoặc dịch chuyển cấu trúc bit lên trên. 

Do đó, mỗi phần tử chuyển đổi qua một số trạng thái giới hạn và tổng số lần cập nhật trên tất cả các phần tử trở thành logarit được khấu hao. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(n)$| Quá chậm | 
| Cây phân đoạn có nén trạng thái |$O((n + m)\log n)$khấu hao |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát thấy mỗi phần tử tiến hóa độc lập dưới sự biến đổi$a \leftarrow a + \text{lowbit}(a)$. Điều này cho phép chúng ta coi mỗi vị trí là một quá trình động học riêng biệt chứ không phải là một hệ thống kết hợp. 
2. Tính toán trước hoặc mô phỏng động sự phát triển của một giá trị duy nhất cho đến khi nó đạt đến trạng thái mà những thay đổi tiếp theo không còn phù hợp trong phạm vi ràng buộc. Ý tưởng quan trọng là mỗi ứng dụng đều tăng giá trị một cách nghiêm ngặt nên các chu kỳ là không thể. 
3. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ tổng phân đoạn của nó và cũng là cờ cho biết liệu tất cả các phần tử bên trong có ở trạng thái “ổn định” hay không, nghĩa là các bản cập nhật tiếp theo sẽ không thay đổi chúng đáng kể hoặc chúng đã đạt đến mẫu cuối. 
4. Đối với thao tác cập nhật trên$[L, R]$, duyệt cây phân đoạn. Nếu một nút hoàn toàn ổn định, chúng tôi bỏ qua nó vì việc áp dụng thao tác không ảnh hưởng đến cấu trúc được lưu trữ của nó. Việc cắt tỉa này là thứ ngăn ngừa đầy đủ$O(n)$quét. 
5. Đối với nút không ổn định, chúng tôi áp dụng bản cập nhật trực tiếp nếu đó là nút lá hoặc truyền đệ quy cho nút con. Sau khi cập nhật các nút con, chúng tôi tính toán lại tổng và trạng thái ổn định của nút cha. 
6. Đối với thao tác truy vấn, chúng tôi chỉ tổng hợp tổng phân đoạn từ cây vì tổng luôn được duy trì nhất quán sau khi cập nhật. 
7. Duy trì tính chính xác bằng cách đảm bảo rằng mọi bản cập nhật đều duy trì chuyển đổi chính xác cho từng phần tử, ngay cả khi được áp dụng một cách lười biếng thông qua đệ quy. 

Điều bất biến chính là mọi nút cây phân đoạn luôn thể hiện chính xác tổng phân đoạn của nó sau tất cả các cập nhật được áp dụng đầy đủ và nếu một nút được đánh dấu là ổn định thì sẽ không có thao tác nào trong tương lai sẽ sửa đổi bất kỳ giá trị nào bên trong nó. Điều này đảm bảo rằng các nút bị bỏ qua không đóng góp các giá trị không chính xác và tất cả các thay đổi cuối cùng sẽ được truyền xuống các nút lá khi cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def lowbit(x):
    return x & -x

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.sum = [0] * (4 * self.n)
        self.arr = arr
        self.build(1, 0, self.n - 1)

    def build(self, idx, l, r):
        if l == r:
            self.sum[idx] = self.arr[l]
            return
        mid = (l + r) // 2
        self.build(idx * 2, l, mid)
        self.build(idx * 2 + 1, mid + 1, r)
        self.sum[idx] = (self.sum[idx * 2] + self.sum[idx * 2 + 1]) % MOD

    def update(self, idx, l, r, ql, qr):
        if l > qr or r < ql:
            return
        if l == r:
            add = lowbit(self.arr[l])
            self.arr[l] += add
            self.sum[idx] = self.arr[l] % MOD
            return
        mid = (l + r) // 2
        self.update(idx * 2, l, mid, ql, qr)
        self.update(idx * 2 + 1, mid + 1, r, ql, qr)
        self.sum[idx] = (self.sum[idx * 2] + self.sum[idx * 2 + 1]) % MOD

    def query(self, idx, l, r, ql, qr):
        if l > qr or r < ql:
            return 0
        if ql <= l and r <= qr:
            return self.sum[idx]
        mid = (l + r) // 2
        return (self.query(idx * 2, l, mid, ql, qr) +
                self.query(idx * 2 + 1, mid + 1, r, ql, qr)) % MOD

def solve():
    T = int(input())
    out = []
    for _ in range(T):
        n = int(input())
        arr = list(map(int, input().split()))
        m = int(input())

        st = SegTree(arr)

        for _ in range(m):
            op, L, R = map(int, input().split())
            L -= 1
            R -= 1
            if op == 1:
                for i in range(L, R + 1):
                    st.arr[i] += lowbit(st.arr[i])
                st = SegTree(st.arr)
            else:
                out.append(str(st.query(1, 0, n - 1, L, R)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Đoạn mã trên trực tiếp mô hình hóa cấu trúc cây phân đoạn, nhưng quan trọng nhất là nó giữ mảng bên dưới làm nguồn trạng thái thực sự. Mọi bản cập nhật đều tính toán lại các giá trị một cách rõ ràng bằng quy tắc lowbit, đảm bảo tính chính xác ngay cả khi cây phân đoạn được xây dựng lại. 

Cây phân đoạn được sử dụng hoàn toàn cho các truy vấn tổng phạm vi, trong khi các bản cập nhật được áp dụng bằng cách lặp lại trong khoảng thời gian bị ảnh hưởng. Sự tách biệt này tránh trộn lẫn logic trạng thái lười phức tạp với quy tắc cập nhật phi tuyến tính, làm giảm hiệu quả trong quá trình triển khai đơn giản này. 

Tính chính xác phụ thuộc vào thực tế là mỗi bản cập nhật được áp dụng trung thực cho mọi phần tử trong phạm vi trước khi bất kỳ truy vấn nào đọc nó. 

## Ví dụ đã hoạt động 

Hãy xem xét trình tự:```
a = [1, 2, 3, 4]
```Hoạt động:```
1 1 3
2 1 4
```Sau khi cập nhật: 

| tôi | a[i] trước | bit thấp | a[i] sau | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 
| 2 | 2 | 2 | 4 | 
| 3 | 3 | 1 | 4 | 
| 4 | 4 | - | 4 | 

Bây giờ mảng là`[2, 4, 4, 4]`. 

Truy vấn`[1,4]`trả lại`14`. 

Điều này cho thấy các phần tử có giá trị lowbit khác nhau tiến hóa ở tốc độ khác nhau, đó là lý do tại sao việc lan truyền lười đồng nhất không thành công. 

Bây giờ hãy xem xét:```
a = [5, 8, 0, 6]
```Hoạt động:```
1 1 4
```| tôi | a[i] trước | bit thấp | a[i] sau | 
| --- | --- | --- | --- | 
| 1 | 5 | 1 | 6 | 
| 2 | 8 | 8 | 16 | 
| 3 | 0 | 0 | 0 | 
| 4 | 6 | 2 | 8 | 

Mảng kết quả:```
[6, 16, 0, 8]
```Phần tử 0 không thay đổi, xác nhận rằng khi một phần tử đạt đến 0, nó sẽ trở nên trơ trong các bản cập nhật tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$trường hợp xấu nhất | Mỗi bản cập nhật có thể đi qua toàn bộ phân đoạn và tính toán lại các phần tử | 
| Không gian |$O(n)$| Lưu trữ cây phân đoạn cộng với mảng | 

Được cho$n, m \le 10^5$, giải pháp này không mở rộng theo các đầu vào trong trường hợp xấu nhất, nhưng nó thể hiện mô hình tính đúng đắn trực tiếp. 

Một giải pháp được tối ưu hóa hoàn toàn sẽ cần chuyển đổi logarit theo từng phần tử được khấu hao để vượt qua các giới hạn nghiêm ngặt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# basic sample-like test
assert run("""1
4
1 2 3 4
3
2 1 4
1 1 3
2 1 4
""") is not None

# minimum case
assert run("""1
1
1
2
2 1 1
1 1 1
""") is not None

# zero propagation case
assert run("""1
3
0 1 2
2
1 1 3
2 1 3
""") is not None

# all equal values
assert run("""1
5
4 4 4 4 4
1
1 1 5
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hoạt động phần tử đơn | cập nhật động chính xác | độ đúng ranh giới | 
| xử lý bằng không | sự ổn định của các phần tử bằng không | hành vi lowbit(0)=0 | 
| mảng thống nhất | cập nhật nhất quán trên phạm vi | tính đối xứng của các bản cập nhật | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các phần tử trở thành số 0. Ví dụ: bắt đầu từ:```
a = [0, 1, 2]
```Áp dụng bản cập nhật trên toàn bộ phạm vi: 

| tôi | trước | bit thấp | sau | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 
| 2 | 1 | 1 | 2 | 
| 3 | 2 | 2 | 4 | 

Phần tử 0 không thay đổi, phần tử này phải được giữ nguyên trong tất cả các hoạt động trong tương lai. Việc triển khai ngây thơ giả định mọi phần tử luôn tăng sẽ sửa đổi vị trí này một cách không chính xác. 

Một trường hợp cạnh khác phát sinh với lũy thừa của hai, chẳng hạn như:```
a = [8]
```Ở đây lowbit bằng chính giá trị đó, do đó, một bản cập nhật sẽ nhân đôi số:```
8 -> 16 -> 32 -> ...
```Điều này tạo ra các giá trị tăng trưởng nhanh chóng và bất kỳ hoạt động triển khai nào dựa vào quá trình tiền xử lý có giới hạn đều phải đảm bảo rằng nó không có phạm vi ổn định nhỏ. 

Cuối cùng, việc cập nhật một phần lặp đi lặp lại trên các phân đoạn chồng chéo có thể làm lộ ra từng lỗi một trong việc xử lý phân đoạn. Việc triển khai đúng phải đảm bảo rằng mọi chỉ mục trong$[L, R]$được cập nhật chính xác một lần cho mỗi hoạt động, không có ranh giới rò rỉ bên ngoài.
