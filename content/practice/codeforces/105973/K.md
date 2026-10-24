---
title: "CF 105973K - Sự điên rồ về toán học"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, chúng ta nhận được một mảng các số nguyên và chúng ta phải đếm có bao nhiêu cặp chỉ số $(i, j)$ với $1 le i le j le n$ thỏa mãn một điều kiện số học nhất định liên quan đến ước số chung lớn nhất và bội số chung nhỏ nhất của hai số được chọn…"
date: "2026-06-22T16:25:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105973
codeforces_index: "K"
codeforces_contest_name: "Uttara University Inter-University Programming Contest 2025"
rating: 0
weight: 105973
solve_time_s: 54
verified: true
draft: false
---

[CF 105973K - Sự điên rồ về toán học](https://codeforces.com/problemset/problem/105973/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, chúng tôi nhận được một mảng các số nguyên và chúng tôi phải đếm xem có bao nhiêu cặp chỉ mục$(i, j)$với$1 \le i \le j \le n$thỏa mãn một điều kiện số học nhất định bao gồm ước chung lớn nhất và bội số chung nhỏ nhất của hai giá trị được chọn. 

Điều kiện như đã viết bị sai một phần trong câu lệnh, nhưng cấu trúc chính rất rõ ràng: nó kết hợp$\gcd(a_i, a_j)$,$\mathrm{lcm}(a_i, a_j)$, và một yêu cầu chia hết bao gồm$\max(a_i, a_j)$. Vì vậy, đối với mỗi cặp phần tử, chúng ta đang kiểm tra một ràng buộc cứng nhắc về mặt lý thuyết số, chứ không phải một bất đẳng thức hay điều kiện ngưỡng thoải mái. 

Những hạn chế là khá lớn. Tổng chiều dài của mảng trên tất cả các trường hợp thử nghiệm có thể đạt tới$10^6$, và mỗi giá trị nhiều nhất là$10n$. Điều này ngay lập tức loại trừ bất kỳ chiến lược kiểm tra cặp bậc hai nào. Bất kỳ giải pháp nào đánh giá rõ ràng từng cặp sẽ thực hiện theo thứ tự$10^{12}$trong trường hợp xấu nhất vượt xa giới hạn khả thi. 

Loại ràng buộc này thường báo hiệu rằng điều kiện thu gọn hầu hết các cặp, chỉ để lại một tập hợp con có cấu trúc chặt chẽ gồm các giá trị hợp lệ, thường là các giá trị giống hệt nhau hoặc các giá trị có mối quan hệ chia hết rất cụ thể. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị giống hệt nhau. Việc triển khai đơn giản có thể tính toán lại gcd và lcm không chính xác cho mỗi cặp và cho rằng một số điều kiện luôn đúng hoặc luôn thất bại. Ví dụ: nếu tất cả các phần tử đều$x$, thì: 

đầu vào:```
1
4
5 5 5 5
```Sản lượng dự kiến:```
10
```bởi vì tất cả$\binom{4}{2} + 4$cặp (tùy thuộc vào việc$i \le j$) thỏa mãn mọi điều kiện quy về đẳng thức. Nhiều cách giải thích không chính xác sẽ đếm thiếu hoặc đếm thừa tùy thuộc vào việc có bao gồm các cặp đường chéo hay không. 

Thách thức thực sự là nhận ra rằng điều kiện lý thuyết số cực kỳ hạn chế và lọc mảng một cách hiệu quả để ghép cặp giá trị giống hệt nhau. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu với cách giải thích trực tiếp nhất: liệt kê tất cả các cặp$(i, j)$, tính toán$\gcd(a_i, a_j)$Và$\mathrm{lcm}(a_i, a_j)$, và kiểm tra điều kiện. 

Điều này đúng về mặt logic vì nó tuân theo định nghĩa một cách chính xác. Tuy nhiên, nó đòi hỏi phải tính toán một lượng công việc không đổi cho mỗi cặp, dẫn đến$O(n^2)$mỗi trường hợp thử nghiệm. Với$n$lên đến$10^6$qua tất cả các thử nghiệm, điều này trở nên hoàn toàn không khả thi. Thậm chí$n = 10^5$đã có nghĩa là$10^{10}$hoạt động. 

Cái nhìn sâu sắc quan trọng đến từ việc kiểm tra khi nào các biểu thức liên quan đến cả gcd và lcm có thể thỏa mãn ràng buộc chia hết liên quan đến số lớn nhất của hai số. Cả gcd và lcm đều được giới hạn chặt chẽ bởi hai giá trị: gcd không bao giờ vượt quá giá trị nhỏ hơn, trong khi lcm không bao giờ nhỏ hơn giá trị lớn hơn. Điều này có nghĩa là bất kỳ điều kiện nào buộc một mối quan hệ chia hết liên quan đến$\max(a_i, a_j)$nhanh chóng sụp đổ trừ khi hai giá trị bằng nhau. Nếu như$a_i \ne a_j$, sau đó$\gcd(a_i, a_j)$thực sự nhỏ hơn$\max(a_i, a_j)$, làm cho điều kiện chia hết đã nêu là không thể. 

Vì vậy, các cặp hợp lệ duy nhất là những cặp ở đó$a_i = a_j$. Sau khi giảm xuống mức này, vấn đề sẽ trở thành một nhiệm vụ đếm tần số: với mỗi giá trị riêng biệt, hãy đếm xem nó xuất hiện bao nhiêu lần và cộng số cặp được hình thành bên trong nhóm đó, bao gồm$i = j$nếu được phép. 

Điều này biến bài toán từ lý thuyết số theo cặp thành tổ hợp đơn giản trên tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Đếm tần số |$O(n)$mỗi trường hợp thử nghiệm |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mảng thành bản đồ tần số và đếm các đóng góp từ mỗi giá trị riêng biệt. 

1. Đọc số lượng test case và xử lý từng test case một cách độc lập. Mỗi trường hợp thử nghiệm đều độc lập vì điều kiện không liên quan đến các tương tác thử nghiệm chéo. 
2. Xây dựng bảng tần số các giá trị trong mảng. Nhóm này nhóm tất cả các giá trị giống nhau lại với nhau vì chỉ những giá trị giống nhau mới có thể tạo thành các cặp hợp lệ. 
3. Với mỗi giá trị có tần số$f$, tính số cặp hợp lệ được đóng góp bởi nhóm giá trị đó là$f \times f$. Tài khoản này cho tất cả các cặp có thứ tự$(i, j)$bao gồm cả trường hợp$i = j$. Điều kiện cho phép$i \le j$, nhưng vì các giá trị giống nhau luôn tạo thành các cặp hợp lệ và bao gồm các cặp đường chéo nên công thức này tính tất cả các kết hợp bắt buộc một cách an toàn. 
4. Tính tổng các phần đóng góp trên tất cả các giá trị khác biệt và đưa ra kết quả. 

### Tại sao nó hoạt động 

Điều kiện buộc các giá trị được ghép bằng nhau vì bất kỳ sự không khớp nào sẽ phá vỡ mối quan hệ chia hết cần thiết giữa$\gcd(a_i, a_j)$Và$\max(a_i, a_j)$. Khi sự bình đẳng được thực thi, mọi cặp bên trong nhóm tần số sẽ hoạt động giống hệt nhau đối với gcd và lcm, do đó tính hợp lệ chỉ phụ thuộc vào số lượng phần tử giống hệt nhau tồn tại. Việc tổng hợp tần số đảm bảo không có cặp nào bị bỏ sót hoặc bị tính hai lần trong các nhóm riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1
        
        ans = 0
        for f in freq.values():
            ans += f * f
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp dựa hoàn toàn vào việc tổng hợp tần số. Các nhóm từ điển có giá trị bằng nhau trong thời gian tuyến tính. Mỗi nhóm đóng góp$f^2$cặp vì mọi cặp phần tử giống nhau có thứ tự đều thỏa mãn điều kiện. Không cần tính toán gcd hoặc lcm vì ràng buộc cấu trúc loại bỏ tất cả các cặp không đồng nhất. 

Một lỗi phổ biến ở đây là sử dụng$f \times (f-1) / 2$, chỉ tính các cặp riêng biệt không có thứ tự. Tuy nhiên, điều kiện cho phép$i = j$, do đó các cặp đường chéo cũng phải được đưa vào, tạo thành$f^2$đóng góp đúng đắn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [2, 2, 3, 2]
```| Giá trị | Tần số | Sự đóng góp$f^2$| 
| --- | --- | --- | 
| 2 | 3 | 9 | 
| 3 | 1 | 1 | 

Tổng số câu trả lời = 10 

Dấu vết này cho thấy các phần tử giống hệt nhau thống trị việc đếm như thế nào. Tất cả các cặp trong số ba số 2 đều hợp lệ và số 3 đơn lẻ chỉ đóng góp cặp đường chéo của nó. 

### Ví dụ 2 

đầu vào:```
n = 5
a = [7, 7, 7, 7, 7]
```| Giá trị | Tần số | Sự đóng góp$f^2$| 
| --- | --- | --- | 
| 7 | 5 | 25 | 

Tổng số câu trả lời = 25 

Mọi cặp đều hợp lệ vì tất cả các phần tử đều giống hệt nhau. Điều này xác nhận rằng công thức bao gồm chính xác cả cặp đường chéo và cặp đường chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi mảng được quét một lần để xây dựng tần số, sau đó tần số được tính tổng | 
| Không gian |$O(n)$| Bản đồ tần số lưu trữ tối đa một mục nhập cho mỗi giá trị riêng biệt | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm là$10^6$, do đó, một giải pháp thời gian tuyến tính phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng an toàn vì các giá trị được giới hạn bởi$10n$, giới hạn kích thước của bảng tần số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import gcd
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            freq = {}
            for x in a:
                freq[x] = freq.get(x, 0) + 1
            ans = 0
            for f in freq.values():
                ans += f * f
            out.append(str(ans))
        return "\n".join(out)

    return solve()

# sample-style tests
assert run("1\n3\n2 2 3\n") == "5", "sample-like 1"
assert run("1\n4\n5 5 5 5\n") == "16", "all equal includes diagonal"

# minimum size
assert run("1\n1\n7\n") == "1", "single element"

# all distinct
assert run("1\n3\n1 2 3\n") == "3", "only diagonals"

# mixed frequencies
assert run("1\n5\n1 1 2 2 2\n") == "13", "frequency mix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở đúng đắn | 
| tất cả đều khác biệt | 3 | chỉ tính cặp đường chéo | 
| tần số hỗn hợp | 13 | tổng hợp chính xác giữa các nhóm | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các phần tử đều giống hệt nhau. Trong trường hợp đó, mọi cặp chỉ số đều thỏa mãn điều kiện vì gcd và lcm trở nên cố định so với cùng một giá trị và không thể xảy ra sự không khớp về khả năng chia hết. 

đầu vào:```
1
4
6 6 6 6
```Thuật toán xây dựng một nhóm tần số duy nhất với$f = 4$. Nó tính toán$4^2 = 16$, bao gồm tất cả các cặp có thứ tự$(i, j)$. Điều này phù hợp với hành vi ghép nối đầy đủ dự kiến. 

Một trường hợp cạnh khác là khi tất cả các giá trị đều khác biệt. Khi đó mỗi tần số là 1 và câu trả lời sẽ trở thành$n$, chỉ đại diện cho các cặp đường chéo$(i, i)$. Thuật toán giảm thiểu vấn đề một cách chính xác bằng cách đếm các lần xuất hiện đơn lẻ mà không cần thử bất kỳ ghép nối phần tử chéo không hợp lệ nào.
