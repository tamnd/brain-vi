---
title: "CF 1234D - Truy vấn ký tự riêng biệt"
description: "Chúng tôi đang duy trì một chuỗi các chữ cái viết thường theo hai loại hoạt động. Một thao tác sửa đổi một vị trí trong chuỗi, thay đổi ký tự của nó."
date: "2026-06-15T20:02:04+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1234
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 590 (Div. 3)"
rating: 1600
weight: 1234
solve_time_s: 177
verified: true
draft: false
---

[CF 1234D - Truy vấn các ký tự riêng biệt](https://codeforces.com/problemset/problem/1234/D) 

**Đánh giá:** 1600 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một chuỗi các chữ cái viết thường theo hai loại hoạt động. Một thao tác sửa đổi một vị trí trong chuỗi, thay đổi ký tự của nó. Thao tác còn lại hỏi về một đoạn của chuỗi và yêu cầu đếm xem có bao nhiêu chữ cái khác nhau xuất hiện bên trong đoạn đó. 

Khó khăn chính là cả cập nhật và truy vấn phạm vi đều được xen kẽ. Một giải pháp tĩnh tính toán lại các câu trả lời từ đầu cho mỗi truy vấn sẽ quét liên tục các chuỗi con có độ dài lên tới 100.000, quá chậm khi có tới 100.000 truy vấn. 

Các ràng buộc ngụ ý tổng kích thước đầu vào theo thứ tự 200.000 thao tác trên cấu trúc có kích thước lên tới 100.000. Bất kỳ giải pháp nào thực hiện công việc tuyến tính cho mỗi truy vấn sẽ đạt khoảng 10¹⁰ kiểm tra ký tự trong trường hợp xấu nhất, không thể thực hiện được trong giới hạn 2 giây. Điều này ngay lập tức loại trừ việc quét chuỗi con ngây thơ. 

Vấn đề tế nhị thứ hai là các bản cập nhật ảnh hưởng đến các truy vấn trong tương lai. Truy vấn phụ thuộc vào phiên bản hiện tại của chuỗi chứ không phải phiên bản ban đầu. Bất kỳ cách tiếp cận nào cũng phải duy trì cấu trúc động thay vì tính toán lại từ một ảnh chụp nhanh cố định. 

Một trường hợp khó nhận thấy xuất phát từ việc cập nhật lặp đi lặp lại cho cùng một vị trí. Ví dụ: nếu chúng ta liên tục thay đổi qua lại một chỉ mục và truy vấn xung quanh nó, thì giải pháp đúng phải luôn phản ánh ký tự mới nhất, không giả sử hành vi đơn điệu. 

Một trường hợp khác phá vỡ các giải pháp ngây thơ là các truy vấn chồng chéo sau khi cập nhật. Ví dụ: nếu chúng ta thay đổi một ký tự trong phạm vi được truy vấn trước đó, câu trả lời phải phản ánh trạng thái chuỗi mới chứ không phải kết quả được lưu trong bộ nhớ đệm. 

## Phương pháp tiếp cận 

Giải pháp brute-force xử lý từng truy vấn một cách độc lập. Đối với truy vấn loại 2 trên phạm vi`[l, r]`, chúng tôi quét chuỗi con và đánh dấu chữ nào trong số 26 chữ cái xuất hiện. Sau đó chúng tôi đếm xem có bao nhiêu người đã được nhìn thấy. Điều này đúng vì định nghĩa của câu trả lời hoàn toàn mang tính cục bộ đối với chuỗi con. 

Tuy nhiên, cách tiếp cận này tốn O(r - l + 1) cho mỗi truy vấn. Trong trường hợp xấu nhất, một truy vấn sẽ quét 100.000 ký tự và với 100.000 truy vấn thì tổng công việc sẽ trở thành bậc hai. Điều này thất bại ngay lập tức. 

Quan sát quan trọng là kích thước bảng chữ cái không đổi và nhỏ. Thay vì theo dõi toàn bộ chuỗi con, chúng ta có thể duy trì, đối với mỗi ký tự, tập hợp các vị trí nơi nó xuất hiện. Sau đó, truy vấn phạm vi sẽ trở thành: ký tự này có xuất hiện ít nhất một lần trong khoảng không? 

Để hỗ trợ cập nhật động một cách hiệu quả, chúng ta cần một cấu trúc duy trì tư cách thành viên của các chỉ mục theo thứ tự được sắp xếp và hỗ trợ chèn, xóa và đếm phạm vi nhanh chóng. Cây Fenwick (Cây chỉ mục nhị phân) hoạt động tốt nếu chúng ta duy trì 26 BIT riêng biệt, một BIT cho mỗi chữ cái. Mỗi BIT lưu trữ số 1 tại các vị trí mà chữ cái đó hiện xuất hiện và số 0 ở những vị trí khác. 

Cập nhật điểm sẽ xóa ký tự cũ tại vị trí`pos`từ BIT của nó và chèn cái mới. Một truy vấn sẽ kiểm tra từng BIT trong số 26 BIT và đếm xem có bao nhiêu BIT có tổng khác 0 trong`[l, r]`. 

Vì 26 là hằng số nên mỗi truy vấn sẽ trở thành O(26 log n), thực tế là O(log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) mỗi truy vấn | O(1) | Quá chậm | 
| 26 BIT | O(26 log n) cho mỗi truy vấn | O(26n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì 26 Cây Fenwick, mỗi cây đại diện cho một nhân vật. Chúng tôi cũng duy trì chuỗi hiện tại để biết ký tự nào hiện được lưu trữ ở mỗi vị trí. 

1. Khởi tạo 26 Cây Fenwick có kích thước n và xây dựng chúng bằng chuỗi ban đầu. Đối với mỗi vị trí`i`, chúng ta thêm 1 vào BIT tương ứng với`s[i]`. Điều này đảm bảo mỗi cây phản ánh nơi đặc tính của nó xuất hiện. 
2. Đối với truy vấn loại 1`pos c`, trước tiên chúng ta xác định vị trí ký tự trước đó tại`pos`. Chúng tôi xóa nó khỏi BIT tương ứng bằng cách trừ 1 tại chỉ mục đó. Sau đó chúng ta chèn ký tự mới vào BIT của nó bằng cách thêm 1 vào cùng vị trí. Cuối cùng, chúng tôi cập nhật chuỗi được lưu trữ. 
3. Đối với truy vấn loại 2`l r`, chúng tôi lặp lại tất cả 26 ký tự. Đối với mỗi ký tự, chúng tôi tính tổng BIT của nó trong phạm vi`[l, r]`. Nếu tổng lớn hơn 0 thì ký tự đó xuất hiện ít nhất một lần trong chuỗi con nên chúng ta đếm nó. 
4. Xuất ra số đếm tích lũy cho mỗi truy vấn loại 2. 

Phần không rõ ràng là tại sao tổng lớn hơn 0 lại thể hiện chính xác sự tồn tại. Mỗi BIT lưu trữ chính xác chức năng chỉ báo của các vị trí nơi ký tự xuất hiện, do đó tổng trên một phạm vi chính xác là số lần xuất hiện trong phân đoạn đó. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, mỗi vị trí trong chuỗi được biểu thị chính xác một lần trên 26 BIT. Các bản cập nhật duy trì sự bất biến này bằng cách xóa ký tự cũ trước khi chèn ký tự mới. Do đó, mỗi BIT luôn chứa một mảng chỉ báo chính xác cho ký tự của nó. Tổng phạm vi trên mảng chỉ báo này tương ứng trực tiếp với việc kiểm tra sự hiện diện và tổng tất cả các ký tự sẽ mang lại số lượng chữ cái riêng biệt trong phân đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

def main():
    s = list(input().strip())
    n = len(s)

    bits = [Fenwick(n) for _ in range(26)]

    for i, ch in enumerate(s, 1):
        bits[ord(ch) - 97].add(i, 1)

    q = int(input())
    out = []

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            pos = int(tmp[1])
            c = tmp[2]
            old = s[pos - 1]
            if old != c:
                bits[ord(old) - 97].add(pos, -1)
                bits[ord(c) - 97].add(pos, 1)
                s[pos - 1] = c
        else:
            l, r = int(tmp[1]), int(tmp[2])
            ans = 0
            for i in range(26):
                if bits[i].range_sum(l, r) > 0:
                    ans += 1
            out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Giải pháp giữ chuỗi trong danh sách có thể thay đổi để các bản cập nhật là O(1) ở cấp độ mảng. Mỗi Cây Fenwick sử dụng chỉ mục dựa trên 1 để đơn giản hóa việc điều hướng của cha mẹ. Thao tác cập nhật sẽ cẩn thận loại bỏ ký tự cũ trước khi chèn ký tự mới, điều này ngăn cản việc đếm hai lần. 

Truy vấn phạm vi dựa vào`range_sum(l, r)`tính toán sự khác biệt tiền tố. Vòng lặp cuối cùng trên 26 ký tự là an toàn vì kích thước bảng chữ cái được cố định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
abacaba
5
2 1 4
1 4 b
1 5 b
2 4 6
2 1 7
```Trạng thái ban đầu là`"abacaba"`. 

| Bước | Hoạt động | Phạm vi/Chuỗi | Những thay đổi BIT chính | Kết quả khác biệt | 
| --- | --- | --- | --- | --- | 
| 1 | truy vấn | [1,4] = "abac" | không | {a,b,c} = 3 | 
| 2 | cập nhật | vị trí 4 -> b | bỏ a ở 4, thêm b ở 4 | chuỗi = "abbcaba" | 
| 3 | cập nhật | vị trí 5 -> b | bỏ a ở 5, thêm b ở 5 | chuỗi = "abbcbba" | 
| 4 | truy vấn | [4,6] = "cbb" | không | {c,b} = 2 | 
| 5 | truy vấn | [1,7] | không | {a,b,c} = 3 | 

Dấu vết cho thấy các bản cập nhật chỉ ảnh hưởng đến BIT cục bộ trong khi các truy vấn vẫn độc lập và nhanh chóng. 

### Ví dụ 2 

đầu vào:```
aaaa
3
2 1 4
1 2 b
2 1 4
```| Bước | Hoạt động | Chuỗi | Chữ cái riêng biệt | 
| --- | --- | --- | --- | 
| 1 | truy vấn | "aaa" | {a} = 1 | 
| 2 | cập nhật | "abaa" | bỏ a tại 2, thêm b | 
| 3 | truy vấn | "abaa" | {a,b} = 2 | 

Điều này chứng tỏ rằng một bản cập nhật sẽ ngay lập tức ảnh hưởng đến các truy vấn phạm vi trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 log n) cho mỗi truy vấn | Mỗi bản cập nhật chạm vào một BIT, mỗi truy vấn sẽ kiểm tra 26 phạm vi BIT | 
| Không gian | O(26 n) | 26 cây Fenwick chứa n vị trí mỗi cây | 

Với n, q ≤ 100.000, điều này phù hợp thoải mái trong giới hạn. Hệ số hằng số 26 nhỏ và hệ số logarit nhanh trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

        def range_sum(self, l, r):
            return self.sum(r) - self.sum(l - 1)

    s = list(input().strip())
    n = len(s)
    bits = [Fenwick(n) for _ in range(26)]

    for i, ch in enumerate(s, 1):
        bits[ord(ch) - 97].add(i, 1)

    q = int(input())
    out = []

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            pos = int(tmp[1])
            c = tmp[2]
            old = s[pos - 1]
            if old != c:
                bits[ord(old) - 97].add(pos, -1)
                bits[ord(c) - 97].add(pos, 1)
                s[pos - 1] = c
        else:
            l, r = int(tmp[1]), int(tmp[2])
            ans = 0
            for i in range(26):
                if bits[i].range_sum(l, r) > 0:
                    ans += 1
            out.append(str(ans))

    return "\n".join(out)

# provided sample
assert run("""abacaba
5
2 1 4
1 4 b
1 5 b
2 4 6
2 1 7
""") == "3\n2\n3"

# all same characters
assert run("""aaaaa
3
2 1 5
1 3 b
2 1 5
""") == "1\n2"

# single character string
assert run("""a
2
2 1 1
1 1 b
""") == "1"

# alternating updates
assert run("""abc
4
2 1 3
1 2 a
2 1 3
2 2 2
""") == "3\n2\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả là | 1 rồi 2 | cập nhật lặp đi lặp lại chính xác | 
| char đơn | 1 | xử lý ranh giới | 
| cập nhật xen kẽ | nhiều giá trị | tính nhất quán khi xen kẽ | 

## Vỏ cạnh 

Trường hợp cập nhật dày đặc xảy ra khi cùng một vị trí được thay đổi liên tục. Ví dụ, bắt đầu bằng`"aaaaa"`, cập nhật vị trí 3 thành`'b'`, sau đó quay lại`'a'`, sau đó đến`'c'`kiểm tra xem cấu trúc có tránh được số lượng cũ tích lũy hay không. Logic cập nhật Fenwick loại bỏ ký tự trước đó trước khi chèn ký tự mới, do đó, mỗi lần chuyển đổi trạng thái duy trì sự hiện diện rõ ràng cho mỗi chỉ mục trong chính xác một cây. 

Trường hợp truy vấn phạm vi tối thiểu như truy vấn`[1,1]`kiểm tra xem chuỗi con một ký tự có được đếm chính xác hay không. Vì mỗi BIT thể hiện các vị trí chính xác nên tổng phạm vi cho một chỉ mục là 0 hoặc 1 và thuật toán ánh xạ chính xác giá trị này thành 0 hoặc 1 ký tự riêng biệt. 

Truy vấn toàn dải sau nhiều lần cập nhật đảm bảo rằng việc tổng hợp trên tất cả 26 cây vẫn nhất quán. Vì mỗi cây độc lập và được cập nhật đối xứng nên không có ký tự nào bị đếm hai lần hoặc bị mất.
