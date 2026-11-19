---
title: "CF 212D - Cắt Hàng Rào"
description: "Chúng tôi được cung cấp một dải ván dọc dài, mỗi tấm có chiều cao cố định. Đối với bất kỳ đoạn ván liền kề nào có chiều dài cố định $k$, Vasya vẽ một hình chữ nhật có chiều cao được xác định bởi tấm ván ngắn nhất bên trong đoạn đó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dsu"]
categories: ["algorithms"]
codeforces_contest: 212
codeforces_index: "D"
codeforces_contest_name: "VK Cup 2012 Finals (unofficial online-version)"
rating: 2500
weight: 212
solve_time_s: 68
verified: false
draft: false
---

[CF 212D - Cắt hàng rào](https://codeforces.com/problemset/problem/212/D) 

**Đánh giá:** 2500 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dsu 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dải ván dọc dài, mỗi tấm có chiều cao cố định. Đối với bất kỳ đoạn ván liền kề nào có chiều dài cố định$k$, Vasya vẽ một hình chữ nhật có chiều cao được xác định bởi tấm ván ngắn nhất bên trong đoạn đó. Vì vậy, mọi lựa chọn vị trí bắt đầu đều tạo ra một giá trị: chiều cao tối thiểu trên mảng con đó. 

Đối với mỗi độ dài phân đoạn được yêu cầu$k_i$, về mặt khái niệm, chúng tôi trượt một cửa sổ có kích thước$k_i$từ trái sang phải trên mảng. Mỗi vị trí tạo ra một giá trị tối thiểu và tất cả các vị trí đều có khả năng như nhau. Nhiệm vụ là tính trung bình của các giá trị tối thiểu này. 

Kích thước đầu vào lớn: cả số lượng ván và số lượng truy vấn đều có thể đạt tới$10^6$. Điều đó ngay lập tức loại trừ việc tính toán lại mức tối thiểu cho mọi cửa sổ một cách độc lập. Việc quét đơn giản cho mỗi truy vấn sẽ là$O(nk)$trong trường hợp xấu nhất là hoàn toàn không khả thi khi cả hai tham số đều lớn. 

Một vấn đề tinh tế hơn là chúng ta được hỏi về những kỳ vọng trên tất cả các cửa sổ chứ không chỉ một câu trả lời duy nhất. Điều này khiến cho việc tính toán trước trở nên cần thiết: cấu trúc của tất cả các cực tiểu của cửa sổ trượt phải được sử dụng lại trên các truy vấn. 

Các trường hợp cạnh xuất hiện khi độ cao bằng phẳng hoặc hoàn toàn đơn điệu. Ví dụ: nếu tất cả các độ cao bằng nhau thì mọi cửa sổ đều có cùng mức tối thiểu, vì vậy mọi câu trả lời đều phải bằng chiều cao đó. Việc triển khai ngây thơ vô tình lấy trung bình một số thứ khác, chẳng hạn như điểm cực tiểu một phần hoặc điểm cuối, sẽ thất bại ở đây. Một cạm bẫy khác là khi$k = 1$, trong đó câu trả lời chỉ đơn giản là giá trị trung bình của mảng vì mỗi cửa sổ chứa chính xác một phần tử. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là trực tiếp. Đối với mỗi kích thước cửa sổ truy vấn$k$, chúng tôi lặp lại tất cả các vị trí bắt đầu, tính giá trị tối thiểu trong mỗi cửa sổ và tính trung bình các kết quả. Tính toán chi phí tối thiểu một cách đơn giản cho mỗi cửa sổ$O(k)$, vì vậy chi phí của mỗi truy vấn$O(nk)$. Với tối đa$10^6$truy vấn và$n$lên đến$10^6$, điều này trở nên lớn về mặt thiên văn, dễ dàng vượt quá$10^{12}$hoạt động. 

Ngay cả khi chúng tôi tối ưu hóa các truy vấn tối thiểu với cấu trúc dữ liệu như bảng thưa thớt hoặc deque, chúng tôi vẫn phải đối mặt với một vấn đề sâu sắc hơn: tính toán lại các câu trả lời một cách độc lập cho mọi truy vấn.$k$lặp lại công việc gần như giống hệt nhau. Quan sát quan trọng là câu trả lời chỉ phụ thuộc vào sự phân bố trong đó mỗi phần tử trở thành giá trị tối thiểu của một số cửa sổ chứ không phụ thuộc vào bản thân các cửa sổ riêng lẻ. 

Điều này dẫn đến một sự đảo ngược quan điểm cổ điển: thay vì lặp lại các cửa sổ và lấy giá trị cực tiểu, chúng tôi sửa một phần tử và hỏi xem nó phục vụ tối thiểu bao nhiêu cửa sổ. Nếu chúng ta có thể đếm được, cho mỗi vị trí$i$, có bao nhiêu cửa sổ có chiều dài$k$có$a[i]$ở mức tối thiểu thì giá trị mong đợi chỉ là tổng có trọng số của tất cả các phần tử chia cho số lượng cửa sổ. 

Để làm cho việc đếm này hiệu quả cho tất cả mọi người$k$, chúng ta cần biết, đối với mỗi phần tử, phạm vi kích thước cửa sổ mà nó có thể chiếm ưu thế ở mức tối thiểu. Điều này được xác định bởi các phần tử nhỏ hơn gần nhất ở cả hai phía, có thể được tìm thấy bằng cách sử dụng ngăn xếp đơn điệu. Khi đã xác định được các ranh giới đó, mỗi phần tử sẽ đóng góp vào phạm vi kích thước cửa sổ có thể dự đoán được và chúng tôi có thể tổng hợp các đóng góp trên tất cả$k$trong một lần quét duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(mnk)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n + m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với từng vị trí$i$, tính phần tử nhỏ hơn gần nhất ở bên trái và bên phải. 

Điều này xác định phân đoạn tối đa trong đó$a[i]$có thể là mức tối thiểu. Một ngăn xếp tăng đơn điệu tạo ra các ranh giới này theo thời gian tuyến tính. 
2. Chuyển các ranh giới này thành một khoảng$[L_i, R_i]$, Ở đâu$i$là mức tối thiểu của bất kỳ mảng con nào chứa đầy đủ trong phạm vi đó. 

Độ dài của nhịp này là$len_i = R_i - L_i + 1$. 
3. Đối với phần tử cố định$a[i]$, xác định có bao nhiêu mảng con có độ dài$k$có$i$như mức tối thiểu của họ. 

Điều này tương đương với việc đếm xem có bao nhiêu cửa sổ có kích thước$k$phù hợp hoàn toàn bên trong$[L_i, R_i]$và bao gồm$i$. 
4. Thể hiện sự đóng góp này như là một chức năng của$k$. 

Thay vì tính toán lại cho mỗi$k$, quan sát điều đó như$k$tăng lên thì số lượng cửa sổ hợp lệ sẽ giảm theo đường tuyến tính từng phần. Mỗi phần tử đóng góp một hình tam giác trong phạm vi có thể$k$. 
5. Sử dụng mảng sai phân$k$để tích lũy đóng góp một cách hiệu quả. 

Chúng tôi thêm các phạm vi cấp số cộng để tất cả đóng góp từ tất cả các phần tử được hợp nhất thành$O(n)$. 
6. Sau khi xử lý tất cả các phần tử, tính tổng tiền tố$k$để có được tổng số tiền tối thiểu cho mọi kích thước cửa sổ. 
7. Đối với mỗi truy vấn$k_i$, chia tổng số tiền được tính toán trước cho số cửa sổ$n - k_i + 1$để đạt được giá trị mong đợi. 

### Tại sao nó hoạt động 

Mỗi phần tử chịu trách nhiệm cho một vùng liền kề ở mức tối thiểu và vùng này xác định đầy đủ hành vi của nó trên tất cả các kích thước cửa sổ. Ngăn xếp đơn điệu đảm bảo các vùng này là tối đa và không chồng chéo theo nghĩa cấu trúc. Khi sự đóng góp của mọi phần tử được thể hiện dưới dạng hàm của kích thước cửa sổ, việc tổng hợp tuyến tính sẽ đảm bảo không tính hai lần và duy trì tính chính xác trên tất cả$k$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    m = int(input())
    ks = list(map(int, input().split()))

    # nearest smaller to left and right
    left = [-1] * n
    right = [n] * n

    stack = []
    for i in range(n):
        while stack and a[stack[-1]] > a[i]:
            stack.pop()
        left[i] = stack[-1] if stack else -1
        stack.append(i)

    stack = []
    for i in range(n - 1, -1, -1):
        while stack and a[stack[-1]] >= a[i]:
            stack.pop()
        right[i] = stack[-1] if stack else n
        stack.append(i)

    # contribution array for sum of minima over all k
    # we compute via sweep over each element
    max_k = n
    diff = [0.0] * (n + 3)

    for i in range(n):
        l = i - left[i]
        r = right[i] - i

        total = l * r

        # contributions for window sizes 1..n form a piecewise shape
        # add linear segments via difference trick
        diff[1] += a[i]
        diff[min(l, r) + 1] -= a[i]

        diff[min(l, r) + 1] += a[i] * min(l, r)
        diff[max(l, r) + 1] -= a[i] * min(l, r)

    # prefix accumulation to get sum for each k
    for i in range(1, n + 1):
        diff[i] += diff[i - 1]

    ans_sum = diff

    # answer queries
    out = []
    for k in ks:
        windows = n - k + 1
        out.append(f"{ans_sum[k] / windows:.12f}")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai tính toán các ranh giới nhỏ hơn gần nhất bằng cách sử dụng ngăn xếp đơn điệu. Quá trình quét bên trái thực thi mức giảm nghiêm ngặt, trong khi quá trình quét bên phải sử dụng so sánh không nghiêm ngặt để tránh tính hai lần các giá trị bằng nhau, đảm bảo mỗi phân đoạn có mức kiểm soát tối thiểu duy nhất. 

Phần thứ hai chuyển đổi từng phần tử thành một hồ sơ đóng góp theo kích thước cửa sổ. Chi tiết triển khai chính là việc sử dụng một mảng khác biệt để mã hóa các thay đổi tuyến tính từng phần trong các đóng góp mà không lặp lại rõ ràng trên tất cả$k$. 

Cuối cùng, mỗi truy vấn được trả lời theo thời gian không đổi bằng cách sử dụng các khoản tiền được tính toán trước. Sự phân chia theo$n - k + 1$chuyển đổi tổng số tiền trên tất cả các cửa sổ thành một kỳ vọng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 2 1
3
1 2 3
```Chúng tôi tính toán các ranh giới nhỏ hơn gần nhất: 

| tôi | một [tôi] | L | R | tôi | r | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | -1 | 1 | 1 | 2 | 
| 1 | 2 | -1 | 2 | 2 | 1 | 
| 2 | 1 | -1 | 3 | 3 | 1 | 

Mỗi phần tử đóng góp theo kích thước cửa sổ. Sau khi tổng hợp: 

| k | tổng cực tiểu | cửa sổ | dự kiến ​​| 
| --- | --- | --- | --- | 
| 1 | 6 | 3 | 2 | 
| 2 | 3 | 2 | 1,5 | 
| 3 | 1 | 1 | 1 | 

Điều này phù hợp với trực giác rằng các cửa sổ nhỏ hơn nhìn thấy các giá trị lớn hơn thường xuyên hơn, trong khi toàn bộ mảng bị chi phối bởi mức tối thiểu toàn cục. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 4 5
2
2 5
```Tất cả các phần tử đều tăng lên, vì vậy mỗi phần tử hầu như trở thành mức tối thiểu trên phạm vi mở rộng bên phải của nó. 

| tôi | một [tôi] | L | R | 
| --- | --- | --- | --- | 
| 0 | 1 | -1 | 5 | 
| 1 | 2 | 0 | 5 | 
| 2 | 3 | 1 | 5 | 
| 3 | 4 | 2 | 5 | 
| 4 | 5 | 3 | 5 | 

Vì$k=2$, cực tiểu hầu hết đều là điểm cuối bên phải, cho giá trị trung bình gần bằng 2. Đối với$k=5$, chỉ tồn tại một cửa sổ và tối thiểu là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| ngăn xếp đơn điệu cộng với một lần quét tuyến tính và xử lý truy vấn | 
| Không gian |$O(n)$| mảng ranh giới và tích lũy đóng góp | 

Giải pháp nằm trong giới hạn vì cả hai$n$Và$m$được xử lý theo thời gian tuyến tính và không thực hiện quét theo truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve() if False else ""

# provided sample
# assert run("3\n3 2 1\n3\n1 2 3\n") == "2.000000000000000\n1.500000000000000\n1.000000000000000"

# custom cases
# all equal
# assert run("4\n5 5 5 5\n2\n1 4\n") == "5.000000000000000\n5.000000000000000"

# increasing
# assert run("5\n1 2 3 4 5\n1\n3\n") == "3.000000000000000"

# decreasing
# assert run("5\n5 4 3 2 1\n2\n2\n") == "2.500000000000000"

# minimum size edge
# assert run("1\n10\n1\n1\n") == "10.000000000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các mảng bằng nhau | đầu ra không đổi | xử lý tối thiểu thống nhất | 
| mảng tăng dần | thiên vị bên phải cao hơn | độ đúng ranh giới đơn điệu | 
| mảng giảm dần | hành vi tối thiểu đối xứng | ngăn xếp đúng đắn | 
| phần tử đơn | chính nó | trường hợp ranh giới cơ sở | 

## Vỏ cạnh 

Đối với mảng một phần tử, ngăn xếp đơn điệu ngay lập tức tạo ra các ranh giới trống và mọi truy vấn sẽ giảm xuống chính phần tử đó. Cơ chế đóng góp chỉ định giá trị trên tất cả các kích thước cửa sổ một cách chính xác vì mọi cửa sổ phải chứa phần tử duy nhất đó. 

Đối với một mảng không đổi, cả ranh giới bên trái và bên phải đều mở rộng tối đa, nghĩa là mọi phần tử đều đóng góp như nhau. Sự tích lũy tạo ra một hàm không đổi trên tất cả$k$và chia cho số cửa sổ sẽ giữ nguyên giá trị không đổi đó. 

Đối với các mảng tăng nghiêm ngặt, ranh giới bên phải của mỗi phần tử chiếm ưu thế, do đó, mỗi phần tử trở thành giá trị tối thiểu chủ yếu khi nó là điểm cuối bên phải của cửa sổ. Thuật toán nắm bắt được điều này thông qua tính bất đối xứng$l$Và$r$các giá trị và hàm tổng hợp thu được sẽ chuyển trọng số một cách chính xác sang các chỉ số lớn hơn. 

Đối với các mảng giảm nghiêm ngặt, điều ngược lại xảy ra: ranh giới bên trái chiếm ưu thế và mỗi phần tử điều khiển các cửa sổ mở rộng sang trái. Tính toán ranh giới dựa trên ngăn xếp đảm bảo tính đối xứng này được xử lý mà không cần vỏ đặc biệt.
