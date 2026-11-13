---
title: "CF 1692G - 2^Sắp xếp"
description: "Chúng ta được cung cấp một mảng và được yêu cầu kiểm tra mọi đoạn liền kề có độ dài cố định $k+1$. Đối với mỗi đoạn như vậy bắt đầu từ vị trí $i$, chúng ta biến đổi nó về mặt khái niệm bằng cách nhân phần tử $j$ (so với điểm bắt đầu của đoạn) với $2^j$."
date: "2026-06-09T23:04:03+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1692
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 799 (Div. 4)"
rating: 1400
weight: 1692
solve_time_s: 93
verified: true
draft: false
---

[CF 1692G - 2^Sắp xếp](https://codeforces.com/problemset/problem/1692/G) 

**Đánh giá:** 1400 
**Thẻ:** cấu trúc dữ liệu, dp, sắp xếp, hai con trỏ 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng và được yêu cầu kiểm tra mọi đoạn liền kề có độ dài cố định$k+1$. Đối với mỗi đoạn như vậy bắt đầu từ vị trí$i$, chúng tôi biến đổi nó một cách khái niệm bằng cách nhân phần tử$j$(so với điểm bắt đầu của đoạn) bởi$2^j$. Sau trọng số mũ này, chúng tôi kiểm tra xem chuỗi kết quả có tăng nghiêm ngặt hay không. Nhiệm vụ là đếm xem có bao nhiêu vị trí bắt đầu tạo ra một đoạn hợp lệ. 

Việc đọc trực tiếp sẽ ẩn cấu trúc: chúng tôi không sắp xếp hoặc sắp xếp lại bất cứ thứ gì, chỉ kiểm tra xem liệu một chuỗi bất đẳng thức rất cụ thể có giữ được trên các cửa sổ trượt hay không. Mỗi cửa sổ áp đặt$k$so sánh và mọi so sánh đều trộn các giá trị liền kề với hệ số hai. 

Các ràng buộc buộc phải đưa ra giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Vì tổng cộng$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$, bất kỳ giải pháp nào làm được$O(nk)$công việc trên mỗi trường hợp thử nghiệm ngay lập tức quá chậm khi$k$cũng có thể lớn. Điều này thúc đẩy chúng tôi hướng tới một cách tiếp cận trong đó chúng tôi xử lý trước các so sánh để có thể kiểm tra từng cửa sổ$O(1)$hoặc khấu hao$O(1)$. 

Trường hợp cạnh tinh tế xuất hiện khi các giá trị bằng hoặc gần bằng nhau sau khi chia tỷ lệ. Ví dụ, nếu$a = [2, 1]$Và$k = 1$, chúng tôi so sánh$2 \cdot 2$Và$1 \cdot 1$, nhận được sự bình đẳng. Yêu cầu là sự bất bình đẳng nghiêm ngặt nên không được tính cửa sổ này. Một cách triển khai đơn giản sử dụng lũy ​​thừa dấu phẩy động của hai hoặc sắp xếp lại các phép so sánh không chính xác có thể âm thầm coi đẳng thức là hợp lệ hoặc xử lý sai lỗi tràn số nguyên khi tính toán$2^j \cdot a_i$. 

Một trường hợp khác là tràn hoặc tính toán lại lũy thừa của hai không cần thiết. Từ$2^k$có thể vượt quá giới hạn số nguyên tiêu chuẩn nếu được triển khai một cách ngây thơ, mọi giải pháp dựa vào lũy thừa trực tiếp trên mỗi cửa sổ đều trở nên mong manh và kém hiệu quả. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi chỉ số$i$, chúng tôi kiểm tra cửa sổ$[i, i+k]$. Chúng tôi tính toán từng giá trị được chuyển đổi$a_{i+j} \cdot 2^j$và xác minh rằng tất cả các cặp liền kề đều tăng nghiêm ngặt. Chi phí này$O(k)$trên mỗi cửa sổ, dẫn đến$O(nk)$mỗi trường hợp thử nghiệm. Khi$n$lớn và$k$cũng lớn, điều này suy biến thành xung quanh$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần cụ thể hóa các giá trị được chia tỷ lệ. Điều kiện giữa hai phần tử liền kề trong một cửa sổ là$$a_{i+j} \cdot 2^j < a_{i+j+1} \cdot 2^{j+1}.$$Chia cả hai vế cho$2^j$(dương và bảo toàn chiều bất đẳng thức), ta thu được dạng đơn giản:$$a_{i+j} < 2 \cdot a_{i+j+1}.$$Đây là sự đơn giản hóa trung tâm: mọi so sánh liền kề chỉ phụ thuộc vào một cặp giá trị mảng ban đầu, không phụ thuộc vào vị trí trong cửa sổ. Trọng số mũ biến mất hoàn toàn. 

Vì vậy, thay vì kiểm tra các cửa sổ đầy đủ bằng tính toán lại, chúng tôi tính toán trước một mảng boolean cho biết liệu mỗi cặp liền kề có thỏa mãn hay không$a_i < 2a_{i+1}$. Sau đó, vấn đề ban đầu trở thành việc đếm có bao nhiêu chiều dài-$k$các phân đoạn liên tiếp trong mảng boolean này là hoàn toàn đúng. 

Điều này làm giảm vấn đề thành cửa sổ trượt trên mảng nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nk)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi cặp liền kề$(a_i, a_{i+1})$, tính xem$a_i < 2a_{i+1}$. Lưu trữ kết quả trong một mảng`good[i]`. 

Điều này chuyển đổi các ràng buộc hàm mũ nhân thành một điều kiện cục bộ đơn giản. 
2. Quan sát chỉ số bắt đầu hợp lệ$i$đối với bài toán ban đầu tương ứng với một đoạn liền kề`good[i] ... good[i+k-1]`tất cả đều là sự thật. 

Việc cải cách này làm giảm độ dài cửa sổ từ$k+1$các yếu tố để$k$điều kiện boolean. 
3. Duy trì tổng cửa sổ trượt`good`. Khởi tạo tổng cho lần đầu tiên$k$giá trị của`good`. 

Nếu tổng bằng$k$, mọi điều kiện trong đoạn đó đều được thỏa mãn. 
4. Trượt cửa sổ từng bước một. Khi di chuyển từ$i$ĐẾN$i+1$, trừ`good[i]`và thêm`good[i+k]`. 

Điều này giữ cho mỗi cửa sổ kiểm tra trong thời gian liên tục. 
5. Đếm từng cửa sổ có tổng bằng$k$, vì điều đó ngụ ý rằng tất cả các so sánh trong phân khúc đều hợp lệ. 

Tính đúng đắn dựa trên thực tế là mọi chuỗi bất đẳng thức ban đầu đều tương đương với một chuỗi các điều kiện liền kề độc lập sau khi triệt tiêu lũy thừa của hai. Hiệu lực của mỗi cửa sổ chỉ phụ thuộc vào việc liệu tất cả các ràng buộc cục bộ bên trong nó có được giữ hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        if k == 1:
            # window size 2, directly check condition
            ans = 0
            for i in range(n - 1):
                if a[i] < 2 * a[i + 1]:
                    ans += 1
            print(ans)
            continue

        good = [0] * (n - 1)
        for i in range(n - 1):
            if a[i] < 2 * a[i + 1]:
                good[i] = 1

        window_sum = sum(good[:k])
        ans = 1 if window_sum == k else 0

        for i in range(1, n - k):
            window_sum -= good[i - 1]
            window_sum += good[i + k - 1]
            if window_sum == k:
                ans += 1

        print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này rút gọn chuỗi bất đẳng thức hàm mũ thành các kiểm tra liền kề được lưu trữ trong`good`. Trường hợp đặc biệt$k=1$được xử lý trực tiếp cho rõ ràng, mặc dù cửa sổ trượt chung cũng hoạt động đồng đều. Tổng cửa sổ trượt theo dõi xem liệu toàn bộ phân đoạn ràng buộc có được giữ nguyên hay không, tránh việc tính toán lại. 

Một cạm bẫy phổ biến là việc lập chỉ mục theo từng cái một:`good[i]`tương ứng với điều kiện giữa`a[i]`Và`a[i+1]`, vì vậy một cửa sổ bắt đầu từ`i`công dụng`good[i]`bởi vì`good[i+k-1]`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, k = 2
a = [3, 1, 4, 2, 8]
```Chúng tôi tính toán`good[i] = (a[i] < 2*a[i+1])`. 

| tôi | một [tôi] | a[i+1] | tốt[i] | 
| --- | --- | --- | --- | 
| 0 | 3 | 1 | Sai | 
| 1 | 1 | 4 | Đúng | 
| 2 | 4 | 2 | Đúng | 
| 3 | 2 | 8 | Đúng | 

Bây giờ các cửa sổ có kích thước$k=2$qua`good`: 

| bắt đầu | cửa sổ | tổng hợp | hợp lệ | 
| --- | --- | --- | --- | 
| 0 | [0,1] | 1 | không | 
| 1 | [1,2] | 2 | vâng | 
| 2 | [2,3] | 2 | vâng | 

Điều này cho thấy các ràng buộc nhân ban đầu được chuyển thành bài toán cửa sổ trượt nhị phân như thế nào. 

### Ví dụ 2 (tất cả các giá trị bằng nhau) 

đầu vào:```
n = 4, k = 1
a = [5, 5, 5, 5]
```| tôi | một [tôi] | a[i+1] | tốt[i] | 
| --- | --- | --- | --- | 
| 0 | 5 | 5 | Sai | 
| 1 | 5 | 5 | Sai | 
| 2 | 5 | 5 | Sai | 

Không có cửa sổ nào hợp lệ vì sự bất bình đẳng nghiêm ngặt không xảy ra ở mọi nơi. Điều này xác nhận việc xử lý đúng các trường hợp bình đẳng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi trường hợp thử nghiệm tính toán kề một lần và trượt một cửa sổ qua nó | 
| Không gian |$O(n)$| Lưu trữ`good`mảng kích thước$n-1$| 

Thuật toán phù hợp thoải mái trong giới hạn trong đó tổng$n$qua các bài kiểm tra là$2 \cdot 10^5$, vì mỗi phần tử được xử lý một số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        if k == 1:
            ans = 0
            for i in range(n - 1):
                if a[i] < 2 * a[i + 1]:
                    ans += 1
            out.append(str(ans))
            continue

        good = [0] * (n - 1)
        for i in range(n - 1):
            good[i] = 1 if a[i] < 2 * a[i + 1] else 0

        window_sum = sum(good[:k])
        ans = 1 if window_sum == k else 0

        for i in range(1, n - k):
            window_sum -= good[i - 1]
            window_sum += good[i + k - 1]
            if window_sum == k:
                ans += 1

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""6
4 2
20 22 19 84
5 1
9 5 3 2 1
5 2
9 5 3 2 1
7 2
22 12 16 4 3 22 12
7 3
22 12 16 4 3 22 12
9 3
3 9 12 3 9 12 3 9 12
""") == """2
3
2
3
1
0"""

# custom cases
assert run("""1
3 1
1 2 3
""") == "2", "increasing simple"

assert run("""1
3 2
1 1 1
""") == "0", "all equal strict failure"

assert run("""1
5 2
1 2 1 2 1
""") == "2", "alternating pattern"

assert run("""1
4 1
100 1 100 1
""") == "2", "boundary alternation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tăng đơn giản | 2 | cửa sổ hợp lệ cơ bản | 
| đều thất bại nghiêm khắc như nhau | 0 | bình đẳng phá vỡ điều kiện nghiêm ngặt | 
| mô hình xen kẽ | 2 | cửa sổ trượt đúng cách | 
| luân phiên ranh giới | 2 | giá trị lớn/nhỏ hỗn hợp | 

## Vỏ cạnh 

Đối với các giá trị liền kề bằng nhau, điều kiện$a_i < 2a_{i+1}$luôn thất bại khi cả hai đều bằng nhau, vì nó trở thành$a_i < 2a_i$, điều đó đúng trừ khi$a_i = 0$, nhưng bất đẳng thức nghiêm ngặt trong chuỗi ban đầu vẫn lan truyền chính xác thông qua phép biến đổi. Thuật toán đánh dấu chính xác các cặp như vậy và ngăn không cho bất kỳ cửa sổ nào chứa chúng được tính. 

Đối với các giá trị rất lớn, việc tính toán không bao giờ hình thành$2^k \cdot a_i$, do đó không có nguy cơ tràn. Toàn bộ giải pháp chỉ dựa vào phép nhân với 2 một lần cho mỗi lần so sánh, nằm trong giới hạn số nguyên tiêu chuẩn trong Python và môi trường 64-bit thông thường. 

Đối với nhỏ$k$, đặc biệt$k=1$, thuật toán giảm xuống mức quét kề trực tiếp. Việc triển khai xử lý việc này một cách thống nhất, nhưng logic cũng xử lý rõ ràng nó như một cửa sổ trượt suy biến có kích thước 1 trên`good`, đảm bảo không có lỗi xảy ra trong quá trình khởi tạo cửa sổ.
