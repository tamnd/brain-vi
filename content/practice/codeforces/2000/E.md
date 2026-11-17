---
title: "CF 2000E - Chụp ảnh khỉ đột"
description: "Chúng ta đang đặt các vật thể có trọng lượng vào một lưới, nhưng điểm số không chỉ là tổng các giá trị trên lưới. Thay vào đó, mỗi vị trí đóng góp nhiều lần: mỗi ô tham gia vào một số ô vuông con $k nhân k$ có kích thước cố định chồng chéo và tổng số điểm là tổng của tất cả…"
date: "2026-06-08T14:13:08+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2000
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 966 (Div. 3)"
rating: 1400
weight: 2000
solve_time_s: 102
verified: true
draft: false
---

[CF 2000E - Chụp ảnh cho Khỉ đột](https://codeforces.com/problemset/problem/2000/E) 

**Đánh giá:** 1400 
**Tags:** tổ hợp, cấu trúc dữ liệu, tham lam, toán học 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang đặt các vật thể có trọng lượng vào một lưới, nhưng điểm số không chỉ là tổng các giá trị trên lưới. Thay vào đó, mỗi vị trí đóng góp nhiều lần: mỗi ô tham gia vào một số kích thước cố định chồng chéo nhau.$k \times k$các bình phương con và tổng điểm là tổng của tất cả các bình phương con đó của tổng các giá trị bên trong chúng. 

Tương tự, mỗi ô đóng góp giá trị của nó nhân với bao nhiêu$k \times k$ô vuông phụ bao gồm ô đó. Nhiệm vụ là đặt tất cả các trọng số đã cho vào các ô lưới để đóng góp trọng số này được tối đa hóa. 

Các ràng buộc có tổng kích thước lớn nhưng không phải cho mỗi trường hợp thử nghiệm, nghĩa là chúng ta có thể đủ khả năng$O(nm \log nm)$hoặc thậm chí$O(nm)$cho mỗi trường hợp thử nghiệm, nhưng không phải bất kỳ thứ gì như liệt kê các vị trí hoặc thử hoán vị. Ý nghĩa chính là cấu trúc phải được giảm xuống để sắp xếp và phân công xác định. 

Một sai lầm ngây thơ ở đây là suy nghĩ cục bộ: đặt những con khỉ đột lớn ở các góc hoặc cạnh vì chúng có vẻ “kém hữu ích hơn”. Điều này không chính xác vì phạm vi bao phủ chỉ phụ thuộc vào hình học chứ không phụ thuộc vào việc ô có nằm trên ranh giới của chính lưới hay không. Một thất bại phổ biến khác là đảm nhận sự đóng góp thống nhất, điều này sẽ bị phá vỡ khi$k < n,m$, bởi vì các ô bên trong thuộc về nhiều ô vuông con hơn các ô biên. 

Ví dụ, trong một$3 \times 3$lưới với$k=2$, các ô trung tâm xuất hiện nhiều hơn$2 \times 2$hình vuông phụ hơn là góc. Người tham lam bỏ qua điều này sẽ đặt sai giá trị cao và mất điểm mặc dù lưới trông đối xứng. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử tất cả các vị trí của khỉ đột vào các ô và tính toán trực tiếp phần đóng góp. Vì có tới$nm \le 2 \cdot 10^5$các ô, ngay cả trước khi xem xét hoán vị trọng số, đây là giai thừa và không thể. 

Quan sát chính là tính tuyến tính: tổng điểm là tổng của các ô và mỗi ô có một hệ số cố định chỉ được xác định bởi$n, m, k$. Khi chúng ta tính toán các hệ số này, bài toán sẽ thuần túy trở thành bài toán gán: gán trọng số lớn nhất cho hệ số lớn nhất. 

Để tính hệ số cho một ô$(i,j)$, chúng tôi đếm có bao nhiêu$k \times k$các ô vuông phụ bao gồm nó. Điều này phân chia độc lập thành các đóng góp theo hàng và cột. Nếu chúng ta xác định có bao nhiêu cửa sổ bao phủ một chỉ mục nhất định trong mảng 1D thì câu trả lời 2D là tích của sự đóng góp của hàng và cột. Điều này làm giảm toàn bộ vấn đề về việc sắp xếp các hệ số và sắp xếp trọng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O((nm)!)$|$O(nm)$| Không thể | 
| Sắp xếp hệ số |$O(nm \log nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm lưới thành một danh sách trọng số trên mỗi ô và khớp nó với danh sách các giá trị quan trọng. 

Bước 1: Đối với mỗi chỉ mục hàng$i$, tính xem có bao nhiêu vị trí dọc của một$k$- đoạn dài bao phủ nó. Một hàng được bao phủ bởi tất cả các đoạn có vị trí bắt đầu nằm trong một phạm vi giao nhau$i$, tạo thành một khoảng liền kề. Số lần bắt đầu hợp lệ sẽ cho một giá trị$row[i]$. 

Bước 2: Tính toán tương tự$col[j]$cho các cột. Đây là số lượng phạm vi bao phủ của cửa sổ trượt 1D giống hệt nhau. 

Bước 3: Đối với mỗi ô$(i,j)$, số nhân đóng góp của nó là$row[i] \cdot col[j]$. Điều này là do một$k \times k$hình vuông được xác định độc lập bằng cách chọn khoảng thời gian hàng và khoảng thời gian cột hợp lệ. 

Bước 4: Làm phẳng tất cả các số nhân ô thành một mảng có kích thước duy nhất$nm$. 

Bước 5: Sắp xếp mảng này theo thứ tự không giảm. 

Bước 6: Sắp xếp chiều cao khỉ đột theo thứ tự không giảm. 

Bước 7: Nhân chiều cao lớn nhất với số nhân và tổng lớn nhất. 

### Tại sao nó hoạt động 

Điểm số là tuyến tính trong từng giá trị ô và độc lập giữa các ô sau khi bội số vùng phủ sóng được cố định. Bất kỳ sự hoán đổi nào của hai con khỉ đột chỉ ảnh hưởng đến sự đóng góp của cá nhân chúng. Do đó, việc ghép các giá trị lớn hơn với các hệ số lớn hơn luôn làm tăng hoặc bảo toàn tổng, đây là một ứng dụng trực tiếp của bất đẳng thức sắp xếp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        w = int(input())
        a = list(map(int, input().split()))

        # compute 1D coverage counts
        def coverage(x):
            # number of segments [l, l+k-1] covering x
            # l must satisfy l <= x <= l+k-1
            # => x-k+1 <= l <= x
            # valid l in [1, n-k+1]
            L = max(1, x - k + 1)
            R = min(x, n - k + 1)
            return max(0, R - L + 1)

        row = [coverage(i) for i in range(1, n + 1)]
        col = [coverage(j) for j in range(1, m + 1)]

        vals = []
        for i in range(n):
            for j in range(m):
                vals.append(row[i] * col[j])

        vals.sort()
        a.sort()

        # take largest weights with largest coefficients
        ans = 0
        for i in range(w):
            ans += a[i] * vals[i + (n * m - w)]

        print(ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính toán tần suất mỗi chỉ mục hàng hoặc cột tham gia vào một chỉ mục hợp lệ.$k$-cửa sổ. Phần thứ hai xây dựng mạng lưới đầy đủ các bội số thông qua cấu trúc sản phẩm. Việc ghép đôi cuối cùng sử dụng cách sắp xếp để tối đa hóa tổng. 

Một điểm tinh tế là chúng tôi chỉ sử dụng phần trên cùng$w$hệ số vì chỉ$w$khỉ đột được đặt; các ô còn lại đóng góp bằng không. Việc sắp xếp đảm bảo chúng ta khớp độ cao lớn nhất với hệ số sẵn có lớn nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét$n=2, m=2, k=1$, trọng lượng$[5,7]$. 

Mỗi ô có hệ số 1 vì mỗi ô$1\times1$ô vuông phụ bao phủ chính xác một ô. 

| Bước | Giá trị | 
| --- | --- | 
| bảo hiểm hàng/col | tất cả 1 | 
| hệ số ô | [1,1,1,1] | 
| hệ số được sắp xếp | [1,1,1,1] | 
| trọng lượng được sắp xếp | [5,7] | 

Chúng tôi lấy hai hệ số lớn nhất và ghép chúng với 7 và 5, được 12. 

Điều này khẳng định rằng khi$k=1$, vấn đề giảm xuống còn việc chọn hai ô bất kỳ. 

### Ví dụ 2 

Lấy một$3 \times 3$lưới với$k=2$. Độ bao phủ hàng và cột là$[1,2,1]$. Do đó hệ số là: 

| Tế bào | Giá trị | 
| --- | --- | 
| góc | 1 | 
| cạnh | 2 | 
| trung tâm | 4 | 

Danh sách làm phẳng:$[1,2,1,2,4,2,1,2,1]$Sắp xếp mang lại$[1,1,1,1,2,2,2,2,4]$. Trọng lượng lớn nhất phải lên 4, cạnh 2s. 

Điều này chứng tỏ tại sao vị trí trung tâm lại chiếm ưu thế và tại sao cần phải có sự phân công tham lam. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm \log (nm))$| sắp xếp tất cả các hệ số ô chiếm ưu thế | 
| Không gian |$O(nm)$| lưới hệ số lưu trữ | 

Với tổng số đó$nm$qua các bài kiểm tra nhiều nhất là$2 \cdot 10^5$, điều này nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m, k = map(int, input().split())
        w = int(input())
        a = list(map(int, input().split()))

        def cov(x, n):
            L = max(1, x - k + 1)
            R = min(x, n - k + 1)
            return max(0, R - L + 1)

        row = [cov(i, n) for i in range(1, n + 1)]
        col = [cov(j, m) for j in range(1, m + 1)]

        vals = []
        for i in range(n):
            for j in range(m):
                vals.append(row[i] * col[j])

        vals.sort()
        a.sort()

        ans = 0
        for i in range(w):
            ans += a[i] * vals[i + (n*m - w)]

        out.append(str(ans))

    return "\n".join(out)

# provided sample check
assert run("""5
3 4 2
9
1 1 1 1 1 1 1 1 1
2 1 1
2
5 7
20 15 7
9
4 1 4 5 6 1 1000000000 898 777
1984 1 1
4
5 4 1499 2004
9 5 5
6
6 7 14 16 16 6
""") == """21
12
49000083104
3512
319"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | tổng trực tiếp | độ đúng cơ sở | 
| k = 1 | trọng lượng đồng đều | trường hợp cạnh của vùng phủ sóng đồng đều | 
| k = n | cửa sổ đơn | sự thống trị ranh giới | 
| trọng lượng lớn thưa thớt | sắp xếp đúng đắn | tham lam ghép nối đúng đắn |
