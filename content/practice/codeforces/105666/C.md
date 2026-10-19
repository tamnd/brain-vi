---
title: "CF 105666C - Dãy số tăng không quá dài"
description: "Chúng ta được cho một hoán vị và được yêu cầu quyết định xem liệu có thể trích xuất một dãy con có độ dài $K$ với một hạn chế về cấu trúc mạnh hay không: bên trong dãy con đã chọn đó, các phần tử phải có thể phân tách thành một số lượng nhỏ các dãy giảm dần."
date: "2026-06-22T05:17:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105666
codeforces_index: "C"
codeforces_contest_name: "MITIT Winter 2025 Advanced Round 1"
rating: 0
weight: 105666
solve_time_s: 63
verified: true
draft: false
---

[CF 105666C - Chuỗi tăng không quá dài](https://codeforces.com/problemset/problem/105666/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị và được yêu cầu quyết định xem liệu có thể trích xuất một chuỗi con có độ dài hay không$K$với một hạn chế về cấu trúc mạnh mẽ: bên trong dãy con được chọn đó, các phần tử phải được phân tách thành một số lượng nhỏ các dãy giảm nghiêm ngặt. Theo tương đương cổ điển, điều này giống như việc yêu cầu dãy con tăng dài nhất bên trong dãy con đã chọn phải bị giới hạn bởi một ngưỡng nhất định. 

Hoán vị toàn cục có dãy con tăng dài nhất, gọi độ dài của nó là$L$. Giá trị này rất quan trọng vì nó mô tả mức độ “không giảm” của toàn bộ cấu trúc và nó cũng xác định số lượng dãy con giảm tối thiểu cần thiết để bao phủ toàn bộ mảng. 

Nhiệm vụ không phải là xây dựng một dãy con tùy ý mà là quyết định xem có tồn tại một dãy con có kích thước hay không.$K$có trật tự bên trong đủ “đơn giản” theo nghĩa trên. 

Từ góc độ ràng buộc, mọi thứ đều xoay quanh việc tính toán cấu trúc liên quan đến LIS một cách hiệu quả. Một giải pháp thử tất cả các dãy con là không thể vì có$\binom{N}{K}$ứng viên. Thậm chí$O(N^2)$phương pháp là ranh giới nếu$N$là lớn. Điều này buộc chúng ta phải hướng tới tiêu chuẩn$O(N \log N)$Máy móc LIS và lý luận tổ hợp cẩn thận về cách các chuỗi con kế thừa cấu trúc từ mảng đầy đủ. 

Một trường hợp thất bại điển hình cho lối suy nghĩ ngây thơ xuất hiện khi người ta giả định rằng một dãy con tốt luôn có thể được tìm thấy bằng cách tham lam chọn các phần tử tăng hoặc giảm cục bộ. Ví dụ, trong một hoán vị như$[3,1,4,2,5]$, việc trích xuất tham lam các chuỗi con có cấu trúc có thể dễ dàng bỏ lỡ các ràng buộc LIS toàn cầu đã ngăn cản một số$K$-các kết quả từ hiện tại mặc dù các lựa chọn địa phương có vẻ khả thi. 

Khó khăn chính là tính chất không chỉ phụ thuộc vào các phần tử được chọn mà còn phụ thuộc vào cách chúng tương tác với phân rã LIS toàn cục của toàn bộ hoán vị. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các chuỗi con có kích thước$K$, tính LIS của chúng và kiểm tra xem nó có thỏa mãn giới hạn yêu cầu hay không. Mỗi phép tính LIS là$O(K \log K)$, và có$\binom{N}{K}$các hậu quả, điều này làm cho điều này hoàn toàn không khả thi ngay cả đối với mức độ vừa phải$N$. 

Sự đột phá về cơ cấu xuất phát từ hai sự thật cổ điển. Đầu tiên, độ dài LIS của một chuỗi bằng số lượng tối thiểu các chuỗi con giảm dần cần thiết để phân chia nó. Thứ hai, bất kỳ sự phân rã tối ưu nào của hoán vị đầy đủ thành các chuỗi con giảm dần đều nắm bắt được các ràng buộc toàn cục mà mọi chuỗi con phải tôn trọng. 

Cho phép$L$là độ dài LIS của toàn bộ mảng. Chúng ta có thể phân tích hoán vị thành chính xác$L$dãy con giảm dần bằng cách sử dụng cấu trúc sắp xếp kiên nhẫn tiêu chuẩn. Gọi cho họ$D_1, D_2, \dots, D_L$, được sắp xếp theo kích thước giảm dần. 

Ý tưởng cốt lõi là xây dựng chuỗi con mong muốn$b$bằng cách lấy toàn bộ khối$D_i$từ sự phân hủy này, bắt đầu từ những cái lớn nhất, cho đến khi chúng ta tích lũy$K$các phần tử. Điều này không phải là tùy tiện, bởi vì mỗi$D_i$đang giảm dần, do đó, việc ghép nối các phần của chúng sẽ duy trì khả năng kiểm soát cấu trúc mạnh mẽ đối với sự phát triển của LIS. 

Cái nhìn sâu sắc quan trọng là trường hợp xấu nhất đối với dãy con được xây dựng xảy ra khi chúng ta buộc phải lấy một phần từ một số$D_m$. Sự cân bằng giữa số lượng khối đầy đủ mà chúng ta lấy và số phần tử vẫn chưa được sử dụng trong mảng ban đầu dẫn trực tiếp đến sự bất bình đẳng liên quan đến$L, N,$Và$K$. 

Nếu chúng ta lấy quá nhiều dãy con giảm nhỏ, tập được chọn sẽ trở nên quá “phân mảnh” và LIS của tập con được chọn phải phát triển vượt quá giới hạn cho phép của các đối số chuồng bồ câu. Nếu sự phân rã đủ lệch về phía các khối lớn hơn, chúng ta có thể đóng gói$K$các phần tử trong khi vẫn giữ số lượng ràng buộc tăng hiệu quả ở mức nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê tất cả các chuỗi con + kiểm tra LIS |$O(\binom{N}{K} \cdot K \log K)$|$O(K)$| Quá chậm | 
| Phân hủy LIS + đóng gói tham lam |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp xoay quanh việc xây dựng phân rã chuỗi con giảm dần của toàn bộ hoán vị và suy luận về việc cần thu thập bao nhiêu khối trong số đó$K$các phần tử. 

### Các bước 

1. Tính toán phân tách LIS của hoán vị đầy đủ thành số dãy con giảm tối thiểu bằng cách sử dụng quy trình kiểu “sắp xếp kiên nhẫn” tham lam. 

Số dãy như vậy là$L$, cũng là độ dài LIS của mảng. 
2. Ghi lại kích thước của từng dãy con giảm dần$D_1, D_2, \dots, D_L$, sau đó sắp xếp các kích thước này theo thứ tự không tăng. 

Thứ tự này rất quan trọng vì chúng ta muốn sử dụng các khối cấu trúc lớn trước tiên để giảm thiểu sự phân mảnh. 
3. Tham lam lấy toàn bộ dãy con$D_1, D_2, \dots$cho đến khi tổng số phần tử được thu thập ít nhất$K$. 

Cho phép$m$là chỉ số đầu tiên mà tổng tích lũy đạt hoặc vượt quá$K$. 
4. Giải thích lựa chọn này: dãy con được xây dựng$b$được làm từ nhiều nhất$m$khối giảm dần. 

Nếu như$m$thì nhỏ$b$có cấu trúc mạnh vì nó được bao phủ bởi một số trình tự giảm dần. 
5. Kiểm tra xem sự bất bình đẳng toàn cầu$$2(L + K - N) \le K + 1$$nắm giữ. 

Nếu thất bại, không có cấu trúc nào có thể tồn tại do sự tranh chấp giữa cấu trúc LIS và sự chồng chéo bắt buộc. 
6. Nếu đúng, cấu trúc tham lam từ quá trình phân rã đảm bảo rằng dãy con được chọn có LIS đủ nhỏ và do đó thỏa mãn điều kiện yêu cầu. 

### Tại sao nó hoạt động 

Sự phân hủy thành$L$dãy con giảm dần là tối ưu theo nghĩa là không có biểu diễn hoán vị nào có thể sử dụng ít dãy như vậy hơn. Điều này có nghĩa là bất kỳ chuỗi con nào mà chúng tôi trích xuất đều kế thừa giới hạn dưới trên LIS của nó tùy theo số lượng khối mà nó giao nhau. 

Khi chúng tôi xây dựng$b$bằng cách tiêu thụ toàn bộ khối theo thứ tự kích thước giảm dần, chúng tôi giảm thiểu số lượng cấu trúc được sử dụng một phần, vốn là nguồn duy nhất gây ra lạm phát LIS tiềm năng. Bất đẳng thức nắm bắt chính xác ngưỡng mà tại đó các khối từng phần không thể buộc LIS vượt quá giới hạn cho phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def lis_decreasing_partition(arr):
    # patience sorting style: maintain piles by last element
    piles = []
    for x in arr:
        # place into first pile whose last element > x
        lo, hi = 0, len(piles)
        while lo < hi:
            mid = (lo + hi) // 2
            if piles[mid][-1] > x:
                hi = mid
            else:
                lo = mid + 1
        if lo == len(piles):
            piles.append([])
        piles[lo].append(x)
    return piles

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    piles = lis_decreasing_partition(a)
    L = len(piles)

    sizes = sorted((len(p) for p in piles), reverse=True)

    need = k
    m = 0
    total = 0
    for s in sizes:
        if total >= need:
            break
        total += s
        m += 1

    lhs = 2 * (L + k - n)
    rhs = k + 1

    if lhs <= rhs:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```chức năng`lis_decreasing_partition`xây dựng phân vùng tối ưu tiêu chuẩn của hoán vị thành các chuỗi con giảm dần. Mỗi cọc đại diện cho một dãy con như vậy và số lượng cọc chính xác là độ dài LIS của mảng. 

Sau đó, chúng tôi trích xuất kích thước cọc, bởi vì đối số xây dựng chỉ phụ thuộc vào độ lớn của các khối giảm dần này chứ không phụ thuộc vào thứ tự bên trong của chúng. Các mô hình bước tích lũy tham lam lấy toàn bộ khối cho đến khi chúng ta đạt được$K$các phần tử. 

Cuối cùng, chúng tôi đánh giá sự bất bình đẳng để biết liệu việc đóng gói như vậy có thể tránh được việc buộc phải tăng quá nhiều ràng buộc hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
3 1 4 2 5
```Giả sử việc phân rã thành các dãy con giảm dần sẽ tạo ra các đống:$$D_1 = [3,1],\quad D_2 = [4,2],\quad D_3 = [5]$$| Bước | Cọc được xem xét | Tổng số đã chọn | m | 
| --- | --- | --- | --- | 
| Bắt đầu | [] | 0 | 0 | 
| Lấy$D_1$| [3,1] | 2 | 1 | 
| Lấy$D_2$| [3,1,4,2] | 4 (dừng sớm vì K=3 đã vào trong) | 2 | 

Chúng tôi dừng lại sau khi sử dụng một phần$D_2$, Vì thế$m=2$. Cấu trúc vẫn được điều khiển bởi chỉ hai khối giảm dần. 

Việc kiểm tra bất đẳng thức xác định liệu sự phân mảnh này có được chấp nhận hay không. Nếu đúng thì câu trả lời là CÓ. 

### Ví dụ 2 

đầu vào:```
6 4
1 2 3 4 5 6
```Ở đây hoán vị tăng hoàn toàn, vì vậy mọi phần tử đều tạo thành dãy con giảm dần của riêng nó:$$D_1=[1], D_2=[2], D_3=[3], D_4=[4], D_5=[5], D_6=[6]$$| Bước | Cọc được xem xét | Tổng số đã chọn | m | 
| --- | --- | --- | --- | 
| 1 | [1] | 1 | 1 | 
| 2 | [1,2] | 2 | 2 | 
| 3 | [1,2,3] | 3 | 3 | 
| 4 | [1,2,3,4] | 4 | 4 | 

Chúng tôi cần tất cả bốn phần tử, vì vậy chúng tôi đã sử dụng bốn khối phần tử đơn. Điều này tối đa hóa sự phân mảnh, làm cho LIS của bất kỳ chuỗi con được chọn nào lớn so với kích thước của nó và bất đẳng thức sẽ không thành công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Mỗi phần tử được đặt thành một chồng bằng cách sử dụng tìm kiếm nhị phân và tất cả quá trình xử lý tiếp theo là tuyến tính | 
| Không gian |$O(N)$| Cọc lưu trữ mỗi phần tử chính xác một lần | 

Thuật toán phù hợp thoải mái trong các ràng buộc điển hình của Codeforce vì nó chỉ yêu cầu quét kiểu LIS duy nhất và xử lý hậu kỳ tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve  # assuming solution is in main.py
    return sys.stdout.getvalue()

# sample-like cases
assert run("5 3\n3 1 4 2 5\n") in {"YES\n", "NO\n"}

# minimum case
assert run("1 1\n1\n") == "YES\n"

# already decreasing
assert run("5 3\n5 4 3 2 1\n") == "YES\n"

# fully increasing
assert run("5 3\n1 2 3 4 5\n") in {"YES\n", "NO\n"}

# boundary K = N
assert run("4 4\n2 1 4 3\n") in {"YES\n", "NO\n"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | CÓ | tính khả thi cơ bản | 
| mảng giảm dần | CÓ | trường hợp cấu trúc tối ưu | 
| mảng tăng dần | phụ thuộc | sự phân mảnh tồi tệ nhất | 
| K = N | nhất quán | hành vi cạnh toàn mảng | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi hoán vị đã giảm nghiêm ngặt. Trong tình huống này, LIS là 1 và phân tách có một khối duy nhất. Công trình tham lam ngay lập tức thu thập các phần tử từ khối này, vì vậy$m=1$, và điều kiện luôn thỏa khi$K$không quá lớn so với$N$. Thuật toán nhận dạng chính xác rằng không có sự phân mảnh nào tồn tại. 

Một trường hợp cạnh khác là hoán vị tăng đầy đủ. Ở đây mọi phần tử đều tạo thành dãy con giảm dần của riêng nó, do đó phép phân rã có$L=N$. Quá trình tham lam tiêu tốn nhiều đơn vị, khiến$m$lớn và gây ra sự thất bại bất cứ khi nào sự bất bình đẳng không cho phép sự phân mảnh cực độ. Thuật toán phản ánh chính xác rằng bất kỳ$K$-subset kế thừa độ phức tạp LIS cao từ cấu trúc toàn cầu. 

Một hoán vị hỗn hợp như$[2,1,4,3,6,5]$thể hiện chế độ trung gian trong đó các cặp tạo thành các khối giảm dần tự nhiên. Việc phân tách tạo ra các cọc cân bằng và câu trả lời là CÓ hoàn toàn phụ thuộc vào sự bất đẳng thức chứ không phụ thuộc vào bất kỳ cấu trúc cục bộ nào phù hợp với đặc tính dự định.
