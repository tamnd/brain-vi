---
title: "CF 106511C - Truy vấn LCM"
description: "Chúng ta được cung cấp một dãy số nguyên và một tập hợp các truy vấn. Mỗi truy vấn yêu cầu bội số chung nhỏ nhất của tất cả các số bên trong một phân đoạn liền kề của mảng. Nhiệm vụ là trả lời từng truy vấn một cách độc lập và xuất ra giá trị kết quả cho phân đoạn đó."
date: "2026-06-18T19:06:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106511
codeforces_index: "C"
codeforces_contest_name: "Columbia University Local Contest (CULC) Spring 2026"
rating: 0
weight: 106511
solve_time_s: 53
verified: true
draft: false
---

[CF 106511C - Truy vấn LCM](https://codeforces.com/problemset/problem/106511/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số nguyên và một tập hợp các truy vấn. Mỗi truy vấn yêu cầu bội số chung nhỏ nhất của tất cả các số bên trong một phân đoạn liền kề của mảng. Nhiệm vụ là trả lời từng truy vấn một cách độc lập và xuất ra giá trị kết quả cho phân đoạn đó. 

Khó khăn cốt lõi không phải là xác định chính LCM mà là xử lý nhiều truy vấn phạm vi một cách hiệu quả. Việc tính toán lại LCM trực tiếp cho mọi truy vấn sẽ quét liên tục các phần lớn của mảng và tính toán lại các phép nhân và phép tính gcd từ đầu, việc này trở nên quá chậm khi cả kích thước mảng và số lượng truy vấn đều lớn. 

Các ràng buộc thường liên quan đến loại vấn đề này ngụ ý rằng mảng có thể đủ lớn đến mức không thể giải được phương trình bậc hai. Ngay cả một giải pháp tính toán lại từng truy vấn theo thời gian tuyến tính cũng sẽ dẫn đến khoảng n lần q thao tác, vượt quá giới hạn hai giây có thể xử lý. Điều này đẩy chúng ta tới một cấu trúc tiền xử lý trong đó mỗi truy vấn có thể được trả lời theo logarit hoặc theo thời gian gần như không đổi. 

Một vấn đề khó nhận thấy trong bài toán này là giá trị LCM tăng cực kỳ nhanh chóng. Ngay cả đầu vào vừa phải cũng có thể tạo ra số vượt quá phạm vi số nguyên 64 bit tiêu chuẩn. Ví dụ: kết hợp các giá trị như 30, 42 và 70 đã tạo ra kết quả trung gian lớn. Việc triển khai ngây thơ nhân trước rồi chia sau mà không quan tâm đến tình trạng tràn có thể âm thầm tạo ra các câu trả lời sai. Một trường hợp cạnh khác phát sinh khi các giá trị LCM trung gian vượt quá giới hạn thực tế mặc dù câu trả lời cuối cùng có thể vẫn nằm trong các ràng buộc, điều này làm cho phép nhân không được kiểm tra không an toàn. 

## Phương pháp tiếp cận 

Giải pháp brute-force xử lý từng truy vấn bằng cách lặp lại phân đoạn được yêu cầu và liên tục gấp các giá trị vào LCM đang chạy. Điều này rất đơn giản: bắt đầu từ 1 và tính toán cho mỗi phần tử trong phân đoạn`lcm(current, a[i]) = current * a[i] // gcd(current, a[i])`. Điều này đúng vì LCM có tính kết hợp theo cặp. 

Tuy nhiên, cách tiếp cận này tính toán lại từ đầu cho mọi truy vấn. Nếu có q truy vấn trên một mảng có kích thước n, trường hợp xấu nhất liên quan đến các phép toán gcd O(nq). Với cả n và q đều lớn, điều này nhanh chóng trở nên không khả thi. 

Quan sát quan trọng là việc tổng hợp LCM trên các phân đoạn có tính kết hợp, cho phép chúng ta tính toán trước các kết quả từng phần trong một cấu trúc cân bằng. Cây phân đoạn phù hợp một cách tự nhiên vì mỗi nút lưu trữ LCM của khoảng của nó và việc kết hợp hai nút con sẽ tạo ra LCM của khoảng cha. 

Điều phức tạp duy nhất là giá trị LCM có thể tăng lên không giới hạn. Trong thực tế, khi giá trị vượt quá ngưỡng an toàn chẳng hạn như 10^18, việc phân biệt các giá trị chính xác sẽ không còn hữu ích nữa vì bất kỳ phép nhân nào nữa sẽ không khôi phục được độ nhỏ. Điều này cho phép chúng tôi giới hạn các giá trị ở một ngưỡng cố định và vẫn duy trì tính chính xác cho các phép so sánh và câu trả lời cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Cây phân đoạn (LCM) | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng cây phân đoạn trong đó mỗi nút đại diện cho LCM của một phân đoạn trong mảng. 

1. Xây dựng cây phân đoạn trên mảng, trong đó mỗi lá lưu trữ giá trị mảng tương ứng. Mỗi nút bên trong lưu trữ LCM của hai nút con của nó. Điều này đảm bảo rằng mọi phân khúc đều được tổng hợp trước. 
2. Khi tính LCM tại một nút, hãy sử dụng công thức`lcm(a, b) = a // gcd(a, b) * b`. Chúng tôi tính toán theo thứ tự này để giảm nguy cơ tràn trung gian. 
3. Nếu LCM được tính toán vượt quá giới hạn cố định chẳng hạn như 10^18, chúng tôi sẽ giới hạn nó ở giới hạn đó. Điều này ngăn ngừa tràn trong khi vẫn duy trì tính chính xác, bởi vì bất kỳ sự kết hợp nào nữa sẽ không làm giảm giá trị. 
4. Với mỗi truy vấn [l, r], duyệt cây phân đoạn. Nếu phân đoạn của nút nằm hoàn toàn bên trong phạm vi truy vấn, hãy sử dụng trực tiếp giá trị được lưu trữ của nó. Nếu nó nằm bên ngoài, hãy bỏ qua nó. Ngược lại, kết hợp kết quả của cả hai trẻ sử dụng LCM. 
5. Xuất giá trị tổng hợp cuối cùng cho mỗi truy vấn. 

Lý do cấu trúc này hoạt động là vì LCM trên các số nguyên tạo thành một phép toán kết hợp, nghĩa là việc nhóm các phép tính không làm thay đổi kết quả. Điều này cho phép chúng tôi tính toán trước một phần LCM cho tất cả các phân đoạn và sử dụng lại chúng trong các truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LIMIT = 10**18

def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

def lcm(a, b):
    if a == 0 or b == 0:
        return 0
    g = gcd(a, b)
    res = a // g * b
    return res if res <= LIMIT else LIMIT

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.size = 1
        while self.size < self.n:
            self.size *= 2
        self.t = [1] * (2 * self.size)

        for i in range(self.n):
            self.t[self.size + i] = arr[i]
        for i in range(self.size - 1, 0, -1):
            self.t[i] = lcm(self.t[2 * i], self.t[2 * i + 1])

    def query(self, l, r):
        l += self.size
        r += self.size
        res_left = 1
        res_right = 1

        while l <= r:
            if l % 2 == 1:
                res_left = lcm(res_left, self.t[l])
                l += 1
            if r % 2 == 0:
                res_right = lcm(self.t[r], res_right)
                r -= 1
            l //= 2
            r //= 2

        return lcm(res_left, res_right)

def main():
    n, q = map(int, input().split())
    arr = list(map(int, input().split()))
    st = SegTree(arr)

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        l -= 1
        r -= 1
        out.append(str(st.query(l, r)))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Cây phân đoạn được lưu trữ trong một mảng phẳng trong đó các lá bắt đầu ở độ lệch lũy thừa hai. Điều này đơn giản hóa cả việc xây dựng và truyền tải truy vấn. Hàm truy vấn sử dụng kiểu đi lên hai con trỏ lặp đi lặp lại, hợp nhất một phần kết quả từ ranh giới bên trái và bên phải. Việc chia thành`res_left`Và`res_right`tránh đảo ngược các vấn đề trật tự trong khi vẫn duy trì tính kết hợp. 

Chi tiết triển khai tinh vi nhất là tính toán LCM. Việc chia trước khi nhân là cần thiết để giảm nguy cơ tràn và việc kẹp đảm bảo chúng ta không bao giờ truyền các số nguyên không giới hạn qua cấu trúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
2 3 4 5 6
1 3
2 5
```| Bước | Phân đoạn | Giá trị kết hợp | 
| --- | --- | --- | 
| Truy vấn 1 | [2, 3, 4] | lcm(2,3)=6 → lcm(6,4)=12 | 
| Truy vấn 2 | [3, 4, 5, 6] | 60 | 

Truy vấn đầu tiên xây dựng LCM tăng dần trên một tiền tố nhỏ, cho thấy cách tổng hợp lặp lại nén phân đoạn thành một giá trị duy nhất. Truy vấn thứ hai thể hiện việc sử dụng lại cấu trúc được tính toán trước thay vì tính toán lại từ đầu. 

### Ví dụ 2 

đầu vào:```
4 1
6 10 15 21
1 4
```| Bước | Phân đoạn | Giá trị kết hợp | 
| --- | --- | --- | 
| Đầy đủ | [6,10,15,21] | 210 | 

Dấu vết này nêu bật tốc độ LCM ổn định thành một sản phẩm duy nhất gồm các thừa số nguyên tố riêng biệt, vì sự chồng chéo được loại bỏ thông qua các phép chia gcd. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi bản cập nhật không có mặt, cấu trúc là O(n), mỗi truy vấn kết hợp các nút O(log n) | 
| Không gian | O(n) | Lưu trữ cây phân đoạn tỷ lệ thuận với kích thước mảng | 

Thời gian truy vấn logarit đảm bảo rằng ngay cả các đầu vào lớn với nhiều truy vấn vẫn nằm trong giới hạn, vì mỗi truy vấn chỉ chạm vào một số lượng nhỏ nút cây thay vì quét toàn bộ phân đoạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    LIMIT = 10**18

    def gcd(a, b):
        while b:
            a, b = b, a % b
        return a

    def lcm(a, b):
        if a == 0 or b == 0:
            return 0
        g = gcd(a, b)
        res = a // g * b
        return res if res <= LIMIT else LIMIT

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.size = 1
            while self.size < self.n:
                self.size *= 2
            self.t = [1] * (2 * self.size)
            for i in range(self.n):
                self.t[self.size + i] = arr[i]
            for i in range(self.size - 1, 0, -1):
                self.t[i] = lcm(self.t[2*i], self.t[2*i+1])

        def query(self, l, r):
            l += self.size
            r += self.size
            res_left, res_right = 1, 1
            while l <= r:
                if l % 2:
                    res_left = lcm(res_left, self.t[l])
                    l += 1
                if not r % 2:
                    res_right = lcm(self.t[r], res_right)
                    r -= 1
                l //= 2
                r //= 2
            return lcm(res_left, res_right)

    n, q = map(int, input().split())
    arr = list(map(int, input().split()))
    st = SegTree(arr)

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        out.append(str(st.query(l-1, r-1)))
    return "\n".join(out)

# custom cases

assert run("""5 2
2 3 4 5 6
1 3
2 5
""") == "12\n60"

assert run("""4 1
6 10 15 21
1 4
""") == "210"

assert run("""1 1
7
1 1
""") == "7"

assert run("""3 2
2 2 2
1 3
2 3
""") == "2\n2"

assert run("""6 1
1 2 3 4 5 6
1 6
""") == "60"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn | 7 | tính đúng đắn của trường hợp cơ sở | 
| Giá trị lặp lại | 2, 2 | hành vi LCM bình thường | 
| Đầy đủ | 60 | tổng hợp đa phần tử | 

## Vỏ cạnh 

Một truy vấn một phần tử như`[7]`chứng minh rằng cây phân đoạn trả về chính xác các giá trị lá mà không cần sửa đổi. Truy vấn không nhập bất kỳ logic kết hợp nào ngoài một nút duy nhất, do đó hành vi nhận dạng của LCM được giữ nguyên. 

Một mảng giá trị lặp lại như`[2, 2, 2]`cho thấy các yếu tố dư thừa sẽ sụp đổ đúng cách. Mặc dù nhiều nút đóng góp cùng một giá trị, các thao tác LCM lặp đi lặp lại không làm tăng kết quả, vì`lcm(2, 2) = 2`và điều này lan truyền một cách nhất quán qua cây. 

Truy vấn toàn dải thực hiện việc truyền tải theo chiều sâu tối đa. Thuật toán kết hợp nhiều phân đoạn từ cả hai đầu của cây và tính chính xác phụ thuộc vào khả năng kết hợp của LCM, đảm bảo rằng mọi nhóm hợp nhất đều mang lại cùng một giá trị cuối cùng.
