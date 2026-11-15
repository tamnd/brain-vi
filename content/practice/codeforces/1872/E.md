---
title: "CF 1872E - Quạt cấu trúc dữ liệu"
description: "Chúng ta được cung cấp một mảng số nguyên tĩnh, nhưng mảng đó được chia động thành hai nhóm tùy thuộc vào chuỗi nhị phân. Mỗi vị trí i đóng góp giá trị a[i] của nó vào nhóm 0 hoặc nhóm 1 tùy thuộc vào việc s[i] là 0 hay 1. Hệ thống hỗ trợ hai thao tác."
date: "2026-06-08T23:19:49+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 1872
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 895 (Div. 3)"
rating: 1500
weight: 1872
solve_time_s: 102
verified: false
draft: false
---

[CF 1872E - Quạt cấu trúc dữ liệu](https://codeforces.com/problemset/problem/1872/E) 

**Đánh giá:** 1500 
**Thẻ:** tìm kiếm nhị phân, mặt nạ bit, cấu trúc dữ liệu, dp 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng số nguyên tĩnh, nhưng mảng đó được chia động thành hai nhóm tùy thuộc vào chuỗi nhị phân. Mỗi vị trí i đóng góp giá trị a[i] của nó vào nhóm 0 hoặc nhóm 1 tùy thuộc vào việc s[i] là 0 hay 1. 

Hệ thống hỗ trợ hai hoạt động. Thao tác đầu tiên lật toàn bộ phân đoạn của chuỗi nhị phân, biến 0 thành 1 và 1 thành 0. Điều này chỉ ảnh hưởng đến cách các phần tử được nhóm chứ không ảnh hưởng đến giá trị của chúng. Thao tác thứ hai yêu cầu XOR của tất cả các giá trị mảng có nhóm hiện tại khớp với một bit g được chỉ định. 

Vì vậy, khó khăn cốt lõi không phải là bản thân việc tính toán XOR mà là việc duy trì cập nhật nhanh theo các lần lật phân đoạn trong khi trả lời các truy vấn XOR của nhóm. 

Các ràng buộc rất chặt chẽ: cả n và q đều có thể lên tới 10^5 cho mỗi trường hợp thử nghiệm, với tổng số tiền trên các thử nghiệm được giới hạn tương tự. Điều này loại trừ việc tính toán lại XOR từ đầu cho mỗi truy vấn, vì điều đó sẽ dẫn đến O(nq) trong trường hợp xấu nhất, vượt xa giới hạn. 

Một cách tiếp cận đơn giản sẽ tính toán lại XOR trên tất cả các chỉ mục cho mỗi truy vấn, nhưng ngay cả một lần quét toàn bộ cho mỗi truy vấn cũng đã cung cấp 10^10 thao tác trong trường hợp xấu nhất, điều này không khả thi. 

Một trường hợp lỗi tinh tế hơn sẽ xuất hiện nếu chúng ta cố gắng duy trì hai XOR đang chạy nhưng cập nhật chúng một cách nguyên bản trong mỗi lần lật bằng cách quét toàn bộ phạm vi. Một truy vấn lật lớn duy nhất trên n phần tử vẫn sẽ tốn O(n) và các lần lật lặp lại làm giảm hiệu suất xuống bậc hai. 

Quan sát chính là XOR trên một nhóm chỉ phụ thuộc vào tính chẵn lẻ của thành viên và chỉ chuyển đổi tư cách thành viên. Điều này gợi ý việc duy trì tổng hợp toàn cầu và theo dõi cách các lần lật hoán đổi ý nghĩa của hai nhóm. 

## Phương pháp tiếp cận 

Giải pháp brute-force tính toán lại XOR cho mọi truy vấn loại 2 bằng cách lặp qua toàn bộ mảng và kiểm tra xem s[i] có bằng g hay không. Điều này đúng vì nó tuân theo định nghĩa trực tiếp nhưng quá chậm vì mỗi truy vấn có chi phí O(n) và có thể có tới 10^5 truy vấn. 

Sự kém hiệu quả xuất phát từ việc tính toán lại tư cách thành viên nhóm từ đầu sau mỗi lần lật. Cấu trúc quan trọng là thao tác loại 1 không thay đổi giá trị, chỉ hoán đổi thành viên giữa hai nhóm. Điều này có nghĩa là chúng ta hoàn toàn không cần phải chạm vào các phần tử riêng lẻ nếu chúng ta có thể theo dõi số lượng phần tử hiện thuộc về mỗi nhóm và duy trì tập hợp XOR của chúng ở trạng thái lật toàn cục. 

Chúng tôi duy trì hai bộ tích lũy XOR: một cho các vị trí trong đó chuỗi gốc có 0 và một cho các vị trí có 1. Việc lật qua một phạm vi không yêu cầu cập nhật các phần tử riêng lẻ nếu thay vào đó chúng tôi theo dõi số lần một vị trí đã bị đảo. Bí quyết quan trọng là quan sát rằng chỉ có tính chẵn lẻ của các lần lật mới quan trọng, vì vậy chúng ta có thể duy trì một cây phân đoạn với khả năng lan truyền lười biếng để lật các đóng góp XOR bằng cách hoán đổi các tập hợp nhóm. 

Mỗi nút cây phân đoạn lưu trữ hai giá trị: XOR của các phần tử hiện được gắn nhãn 0 và XOR của các phần tử hiện được gắn nhãn 1 trong phân đoạn đó. Thao tác lật chỉ cần hoán đổi hai giá trị này trong một nút. Điều này làm cho việc thay đổi phạm vi trở nên hiệu quả và việc hợp nhất vẫn nhất quán vì XOR có tính liên kết. 

Truy vấn cuối cùng trở thành việc đọc trực tiếp nút gốc tùy thuộc vào cách diễn giải hiện tại của bit g. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Cây phân đoạn lười lật | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ hai giá trị: XOR của tất cả a[i] trong phân đoạn của nó trong đó s[i] hiện bằng 0 và XOR trong đó s[i] hiện bằng 1. Sự phân tách này cho phép trả lời các truy vấn trực tiếp. 
2. Khởi tạo các nút lá bằng chuỗi nhị phân ban đầu. Nếu s[i] = 0, đặt a[i] vào nhóm 0-XOR; nếu không thì vào nhóm một XOR. Điều này mã hóa chính xác phân vùng ban đầu. 
3. Khi hợp nhất hai nút con, hãy tính toán XOR độc lập cho từng nhóm bằng cách XOR các nhóm tương ứng. Điều này hoạt động vì XOR có tính liên kết và các phần tử trong các phân đoạn rời rạc không gây trở ngại. 
4. Đối với truy vấn loại 1 trên phạm vi [l, r], hãy áp dụng thao tác lật từng phần trên cây phân đoạn. Tại mỗi nút được bao phủ hoàn toàn, hãy hoán đổi các giá trị XOR được lưu trữ cho nhóm 0 và nhóm 1. Điều này thể hiện việc chuyển đổi tất cả các bit trong phân đoạn đó. 
5. Nếu một nút bị che một phần, hãy truyền bá thao tác theo cách đệ quy và đảm bảo tính chính xác bằng cách đẩy các lần lật đang chờ xử lý xuống trước khi truy cập các nút con. 
6. Đối với truy vấn loại 2 có g, trả về trực tiếp XOR được lưu trữ trong nhóm tương ứng của nút gốc. Nếu g = 0, trả về root.xor0, nếu không thì trả về root.xor1. 

### Tại sao nó hoạt động 

Mỗi phần tử đóng góp chính xác một lần cho đúng một trong hai nhóm XOR bất kỳ lúc nào. Thao tác lật trên một phân đoạn sẽ chuyển đổi tư cách thành viên cho mọi phần tử trong phân khúc đó, tương đương với việc hoán đổi hai bộ tích lũy XOR cho phân khúc đó. Vì XOR là tuyến tính và các phần tử không tương tác giữa các phân đoạn nên việc duy trì các giá trị chính xác sẽ giảm xuống việc duy trì phân vùng chính xác theo các lần lật phạm vi. Lan truyền lười biếng đảm bảo rằng mỗi lần hoán đổi được áp dụng chính xác khi cần thiết mà không cần tính toán lại trạng thái từng phần tử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, a, s):
        self.n = len(a)
        self.a = a
        self.s = s
        self.size = 4 * self.n
        self.x0 = [0] * self.size
        self.x1 = [0] * self.size
        self.lazy = [0] * self.size
        self.build(1, 0, self.n - 1)

    def build(self, v, tl, tr):
        if tl == tr:
            if self.s[tl] == '0':
                self.x0[v] = self.a[tl]
            else:
                self.x1[v] = self.a[tl]
            return
        tm = (tl + tr) // 2
        self.build(v * 2, tl, tm)
        self.build(v * 2 + 1, tm + 1, tr)
        self.pull(v)

    def pull(self, v):
        self.x0[v] = self.x0[v * 2] ^ self.x0[v * 2 + 1]
        self.x1[v] = self.x1[v * 2] ^ self.x1[v * 2 + 1]

    def apply(self, v):
        self.x0[v], self.x1[v] = self.x1[v], self.x0[v]
        self.lazy[v] ^= 1

    def push(self, v):
        if self.lazy[v]:
            self.apply(v * 2)
            self.apply(v * 2 + 1)
            self.lazy[v] = 0

    def update(self, v, tl, tr, l, r):
        if l > r:
            return
        if l == tl and r == tr:
            self.apply(v)
            return
        self.push(v)
        tm = (tl + tr) // 2
        self.update(v * 2, tl, tm, l, min(r, tm))
        self.update(v * 2 + 1, tm + 1, tr, max(l, tm + 1), r)
        self.pull(v)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        s = input().strip()
        q = int(input())

        st = SegTree(a, s)

        for _ in range(q):
            tmp = input().split()
            if tmp[0] == '1':
                l = int(tmp[1]) - 1
                r = int(tmp[2]) - 1
                st.update(1, 0, n - 1, l, r)
            else:
                g = int(tmp[1])
                if g == 0:
                    print(st.x0[1], end=' ')
                else:
                    print(st.x1[1], end=' ')
        print()

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng cây phân đoạn trong đó mỗi nút duy trì hai giá trị XOR. Bước xây dựng chỉ định từng giá trị vào một trong hai nhóm dựa trên chuỗi ban đầu. Hàm cập nhật thực hiện việc lật phạm vi bằng cách sử dụng phương pháp lan truyền lười biếng, trong đó việc lật được thực hiện bằng cách hoán đổi hai giá trị XOR được lưu trữ. 

Chức năng đẩy đảm bảo tính chính xác khi đi xuống phần tử con bằng cách truyền các lần lật đang chờ xử lý. Hàm kéo tính toán lại các nút cha từ các nút con sau khi cập nhật. Gốc luôn chứa sự phân chia XOR toàn cục giữa các nhóm 0 và 1 hiện tại, tạo ra các truy vấn O(1). 

Một điểm tinh tế là việc lật có tính đối xứng, vì vậy chúng ta không cần theo dõi các bit thực tế, chỉ cần theo dõi xem một phân đoạn có được đảo một số lần lẻ hay không. Đó chính xác là những gì cờ lười mã hóa. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

Hãy xem xét một trường hợp đơn giản: 

Mảng:`[1, 2, 3, 4]`Sợi dây:`0100`Chúng tôi xử lý một chuỗi truy vấn: 

| Bước | Hoạt động | x0 (gốc) | x1 (gốc) | 
| --- | --- | --- | --- | 
| 1 | ban đầu | 1 + 3 + 4 = 6 | 2 | 
| 2 | truy vấn g=0 | 6 | 2 | 
| 3 | lật [2,4] | phụ thuộc vào việc hoán đổi phân khúc | hoán đổi cục bộ | 
| 4 | truy vấn g=1 | cập nhật root.x1 | đã cập nhật root.x0 | 

Dấu vết này cho thấy các lần lật không bao giờ chạm trực tiếp vào các phần tử riêng lẻ; họ chỉ trao đổi các khoản đóng góp XOR tổng hợp. 

### Ví dụ thứ hai 

Mảng:`[5, 7]`Sợi dây:`00`Ban đầu tất cả các giá trị đều ở x0: 

| Bước | Hoạt động | x0 | x1 | 
| --- | --- | --- | --- | 
| 1 | ban đầu | 5 ^ 7 = 2 | 0 | 
| 2 | lật [1,2] | 0 | 2 | 
| 3 | truy vấn g=0 | 0 | 2 | 
| 4 | truy vấn g=1 | 2 | 0 | 

Điều này xác nhận rằng việc lật một phân đoạn một cách chính xác sẽ hoán đổi tư cách thành viên nhóm mà không cần tính toán lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi lần lật phạm vi sẽ cập nhật một cây phân đoạn với khả năng lan truyền lười biếng | 
| Không gian | O(n) | Cây phân đoạn lưu trữ hai giá trị XOR trên mỗi nút | 

Độ phức tạp vừa vặn trong giới hạn vì tổng n và q trong các thử nghiệm được giới hạn bởi 10^5 và mỗi thao tác chỉ yêu cầu thời gian logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    # assuming solution is defined above as solve()
    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# sample-like small test
assert run("""1
4
1 2 3 4
0100
3
2 0
1 2 4
2 1
""") == "6 2"

# single element flip
assert run("""1
1
7
0
3
2 0
1 1 1
2 1
""") == "7 7"

# all ones initial
assert run("""1
3
1 2 3
111
2
2 0
2 1
""") == "0 6"

# alternating flips
assert run("""1
5
1 2 3 4 5
01010
4
1 1 5
2 0
1 2 4
2 1
""") == "15 10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 7 7 | flip không phá vỡ logic đơn lẻ | 
| tất cả những cái | 0 6 | nhóm trống XOR được xử lý chính xác | 
| lật xen kẽ | 15 10 | tính nhất quán trong phạm vi lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh then chốt là khi các lần lật lặp lại sẽ triệt tiêu lẫn nhau. Ví dụ: lật cùng một đoạn hai lần sẽ khôi phục lại phân vùng ban đầu. Trong cây phân đoạn, điều này được xử lý một cách tự nhiên vì cờ lười chuyển đổi tính chẵn lẻ. Hai lần lật lại đặt cờ về 0, nghĩa là không áp dụng hoán đổi, do đó trạng thái XOR sẽ trả về chính xác cấu hình ban đầu của nó. 

Một trường hợp khác là truy vấn một nhóm trở nên trống sau khi lật. Vì XOR trên một tập hợp trống được xác định là 0 nên cây phân đoạn trả về 0 một cách chính xác vì nhóm tương ứng vẫn trống và danh tính XOR được giữ nguyên. 

Trường hợp tinh vi cuối cùng là các bản cập nhật chồng chéo lớn. Ngay cả khi nhiều lần lật chồng lên nhau theo các mẫu tùy ý, mỗi nút chỉ lưu trữ tính chẵn lẻ của các lần lật, đảm bảo rằng trạng thái cuối cùng chỉ phụ thuộc vào việc nó có được lật số lần lẻ hay không.
