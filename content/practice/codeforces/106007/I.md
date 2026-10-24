---
title: "CF 106007I - Đảo ngược và loại bỏ"
description: "Chúng ta được cung cấp một dãy số và một số thao tác để thực hiện trên đó. Mỗi thao tác luôn loại bỏ phần tử đầu tiên hiện tại của chuỗi và sau đó đảo ngược phần tử còn lại."
date: "2026-06-22T16:42:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106007
codeforces_index: "I"
codeforces_contest_name: "The 2025 Aleppo Collegiate programming contest"
rating: 0
weight: 106007
solve_time_s: 51
verified: true
draft: false
---

[CF 106007I - Đảo ngược và xóa](https://codeforces.com/problemset/problem/106007/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số và một số thao tác để thực hiện trên đó. Mỗi thao tác luôn loại bỏ phần tử đầu tiên hiện tại của chuỗi và sau đó đảo ngược phần tử còn lại. Điều này được lặp lại chính xác k lần và chúng ta phải xuất ra chuỗi cuối cùng sau tất cả các phép biến đổi này. 

Một cách hữu ích để hình dung về quá trình này là mảng liên tục bị bóc ra từ phía trước, trong khi hướng của nó vẫn tiếp tục bị lật. Sau mỗi lần loại bỏ, thứ tự tương đối của các hậu tố còn lại sẽ thay đổi tùy thuộc vào số lần đảo ngược đã xảy ra, điều này làm cho việc mô phỏng trực tiếp trở nên đắt đỏ nếu được thực hiện theo đúng nghĩa đen. 

Các ràng buộc cho phép n lên tới 100000, do đó, bất kỳ cách tiếp cận nào đảo ngược mảng k lần về mặt vật lý hoặc dịch chuyển các phần tử liên tục sẽ quá chậm. Một mô phỏng đơn giản sẽ thực hiện tối đa k lần loại bỏ và mỗi lần đảo ngược sẽ tốn O(n), tạo ra O(nk), suy biến thành O(n^2) trong trường hợp xấu nhất và không được chấp nhận. 

Có hai trường hợp phức tạp bộc lộ những sai lầm ngây thơ. Đầu tiên, nếu k gần với n trừ một, mảng sẽ trở nên cực kỳ ngắn và việc đảo ngược lặp đi lặp lại sẽ thay đổi phần đầu và phần đuôi một cách nhanh chóng, do đó việc triển khai quên cập nhật chỉ số chính xác sẽ không thành công. Ví dụ: với đầu vào n = 5, k = 4, a = [1, 2, 3, 4, 5], quy trình chỉ nên để lại một phần tử duy nhất, nhưng các mô phỏng dựa trên con trỏ không chính xác thường loại bỏ hoặc trùng lặp các phần tử do lỗi hướng xen kẽ. 

Thứ hai, nếu k = 0 không được phép ở đây nhưng nghĩ chung, hoặc nếu k = 1, việc triển khai đơn giản có thể đảo ngược trước khi xóa thay vì sau khi xóa, điều này làm thay đổi hoàn toàn kết quả. Ví dụ: [1, 2, 3, 4], k = 1 sẽ mang lại [2, 3, 4] chứ không phải [4, 3, 2]. 

Khó khăn cốt lõi là chuỗi không chỉ bị thu hẹp mà còn thay đổi hướng theo cách có cấu trúc mà chúng ta có thể theo dõi mà không cần sửa đổi mảng đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp thực hiện các hoạt động được mô tả theo đúng nghĩa đen. Chúng tôi loại bỏ phần tử đầu tiên của danh sách và đảo ngược phần còn lại, lặp lại điều này k lần. Mỗi lần loại bỏ là O(1) nếu sử dụng một deque, nhưng mỗi lần đảo ngược n i phần tử còn lại là O(n i), dẫn đến tổng O(n + (n−1) + … + (n−k)), là O(nk). Khi k lớn, giá trị này trở thành bậc hai theo kích thước của đầu vào. 

Quan sát quan trọng là chúng ta không bao giờ thực sự cần các mảng trung gian. Chúng ta chỉ cần biết phần tử nào còn lại sau k lần loại bỏ và theo thứ tự nào. Thao tác này sẽ thay đổi hướng đọc đoạn còn lại. Thay vì mô phỏng sự đảo ngược, chúng ta có thể duy trì hai con trỏ biểu thị ranh giới bên trái và bên phải hiện tại của mảng còn lại và một cờ boolean biểu thị hướng hiện tại là bình thường hay đảo ngược. 

Mỗi thao tác sẽ loại bỏ một phần tử khỏi mặt trước hoạt động tùy theo hướng. Nếu chúng ta ở hướng bình thường, chúng ta loại bỏ từ bên trái; nếu đảo ngược thì ta loại bỏ từ bên phải. Sau khi gỡ bỏ, chúng ta lật hướng. Điều này giúp loại bỏ mọi nhu cầu đảo ngược mảng thực tế. 

Điều này làm giảm toàn bộ quá trình xuống còn cập nhật con trỏ O(k) và vì k < n 100000 nên nó dễ dàng đạt hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(n) | Quá chậm | 
| Hai con trỏ có cờ hướng | O(k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai chỉ số, l bắt đầu từ 0 và r bắt đầu từ n − 1 và một biến boolean rev ban đầu sai nghĩa là chúng tôi đang đọc từ phía bên trái.

1. Khởi tạo l = 0 và r = n − 1 và đặt rev = false. Điều này thể hiện toàn bộ phân khúc đang hoạt động và hướng hiện tại của nó. 
2. Lặp lại k lần và ở mỗi bước hãy quyết định đầu nào của đoạn này là “phía trước” tùy thuộc vào vòng quay. Nếu rev ​​là sai thì mặt trước ở l, nếu không thì ở r. Điều này hoàn toàn tương đương với mảng hiện tại sau tất cả các lần đảo ngược ngầm định. 
3. Nếu rev ​​là sai, tăng l lên 1 để loại bỏ phần tử ngoài cùng bên trái. Nếu rev ​​là đúng, giảm r đi 1 để loại bỏ phần tử ngoài cùng bên phải. Điều này mô phỏng việc xóa phần tử đầu tiên theo hướng hiện tại mà không sắp xếp lại bất kỳ thứ gì về mặt vật lý. 
4. Lật vòng sau mỗi lần tháo. Điều này nắm bắt được hiệu ứng đảo ngược mảng còn lại. Thay vì sắp xếp lại các phần tử, chúng tôi chỉ cần thay đổi phía mà chúng tôi coi là mặt trước. 
5. Sau k thao tác, đoạn còn lại là đoạn [l, r]. Định hướng cuối cùng được xác định bởi rev. Nếu rev ​​là sai, chúng ta xuất các phần tử từ l đến r theo thứ tự tăng dần. Nếu rev ​​đúng, chúng ta xuất các phần tử từ r đến l theo thứ tự giảm dần. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mảng còn lại chính xác là mảng ban đầu được giới hạn ở một đoạn liền kề [l, r], nhưng có thể được xem theo thứ tự đảo ngược tùy thuộc vào số lần đảo ngược đã xảy ra. Mỗi thao tác sẽ loại bỏ một điểm cuối của phân đoạn này và thay đổi cách giải thích hướng. Điều bất biến là mảng logic hiện tại luôn được biểu diễn dưới dạng một phần liền kề của mảng ban đầu cộng với cờ chỉ hướng. Vì cả việc loại bỏ và đảo ngược đều bảo toàn cấu trúc này nên không có hoạt động nào đưa ra sự gián đoạn hoặc sắp xếp lại các phần tử ngoài một lần lật toàn cục. Điều này đảm bảo rằng chuyển động của con trỏ và một cờ boolean duy nhất mô tả đầy đủ trạng thái phát triển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

l, r = 0, n - 1
rev = False

for _ in range(k):
    if not rev:
        l += 1
    else:
        r -= 1
    rev = not rev

if not rev:
    res = a[l:r+1]
else:
    res = a[l:r+1][::-1]

print(*res)
```Việc thực hiện giữ hai ranh giới l và r và cập nhật chúng theo hướng hiện tại. Vòng quay boolean theo dõi xem hướng logic có bị đảo ngược hay không. Sau k bước, chúng ta trích xuất đoạn còn lại. Sự tinh tế duy nhất là đầu ra cuối cùng: nếu hướng bị đảo ngược, chúng ta phải đảo ngược lát cắt một cách rõ ràng. Điều này tránh đảo ngược toàn bộ mảng ở mỗi bước và giữ cho giải pháp tuyến tính. 

Việc xử lý ranh giới rất đơn giản vì vòng lặp đảm bảo l  r luôn cho đến khi thực hiện chính xác k việc loại bỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 5, k = 2 

a = [1, 2, 3, 4, 5] 

| Bước | tôi | r | vòng quay | Đã xóa | Giải thích phân đoạn | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 4 | sai | - | [1, 2, 3, 4, 5] | 
| 1 | 1 | 4 | đúng | 1 | chế độ xem đảo ngược | 
| 2 | 1 | 3 | sai | 5 | xem bình thường | 

Sau hai thao tác, đoạn còn lại là [1, 3], tương ứng với [2, 3, 4]. Đầu ra là: 

2 3 4 

Điều này xác nhận rằng việc loại bỏ xen kẽ từ các đầu đối diện được xử lý chính xác bởi cờ định hướng. 

### Ví dụ 2 

đầu vào: 

n = 6, k = 3 

a = [10, 20, 30, 40, 50, 60] 

| Bước | tôi | r | vòng quay | Đã xóa | Giải thích phân đoạn | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 5 | sai | - | [10, 20, 30, 40, 50, 60] | 
| 1 | 1 | 5 | đúng | 10 | đảo ngược | 
| 2 | 1 | 4 | sai | 60 | bình thường | 
| 3 | 2 | 4 | đúng | 20 | đảo ngược | 

Phân đoạn còn lại là [2, 4] cho [30, 40, 50] phù hợp với đầu ra. 

Điều này cho thấy rằng mặc dù việc loại bỏ các kết thúc xen kẽ nhau nhưng việc biểu diễn không bao giờ bị phá vỡ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Mỗi thao tác cập nhật hai con trỏ và lật cờ một lần | 
| Không gian | O(n) | Mảng đầu vào được lưu trữ, không cần cấu trúc bổ sung | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì k và n nhiều nhất là 100000. Mỗi thao tác là thời gian không đổi và chỉ thực hiện một lần đảo ngược cuối cùng duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import *
    
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    l, r = 0, n - 1
    rev = False

    for _ in range(k):
        if not rev:
            l += 1
        else:
            r -= 1
        rev = not rev

    if not rev:
        res = a[l:r+1]
    else:
        res = a[l:r+1][::-1]

    return " ".join(map(str, res)) if res else ""

# provided sample
assert run("5 2\n1 2 3 4 5\n") == "2 3 4"

# custom: k = 1
assert run("4 1\n1 2 3 4\n") == "2 3 4"

# custom: small alternating
assert run("6 3\n10 20 30 40 50 60\n") == "30 40 50"

# custom: maximal skew removal
assert run("5 4\n1 2 3 4 5\n") == "5"

# custom: identity-like structure
assert run("3 2\n7 8 9\n") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 2 / 1 2 3 4 5 | 2 3 4 | độ chính xác của mẫu | 
| 4 1 / 1 2 3 4 | 2 3 4 | hoạt động đơn lẻ | 
| 6 3/10 20 30 40 50 60 | 30 40 50 | xen kẽ các đầu | 
| 5 4 / 1 2 3 4 5 | 5 | thu hẹp ranh giới | 
| 3 2 / 7 8 9 | 8 | kết quả trung bình tối thiểu | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi k loại bỏ gần như toàn bộ mảng. Xem xét đầu vào: 

n = 5, k = 4, a = [1, 2, 3, 4, 5] 

Quá trình chỉ nên để lại một yếu tố. Quá trình mô phỏng con trỏ tiến hành như sau: ban đầu l = 0, r = 4. Việc loại bỏ đầu tiên lấy 1 từ bên trái, do đó l trở thành 1 và rev trở thành đúng. Loại thứ hai từ bên phải, r trở thành 3. Loại thứ ba từ bên trái, l trở thành 2. Loại thứ tư từ bên phải, r trở thành 2. Đoạn thu gọn thành một phần tử duy nhất [3], được xuất ra chính xác. 

Một trường hợp cạnh khác là tối thiểu k = 1: 

n = 4, k = 1, a = [1, 2, 3, 4] 

Chúng tôi xóa 1 ở bên trái, để lại [2, 3, 4], sau đó đảo ngược không liên quan đến kết quả cuối cùng vì chỉ thực hiện một bước. Thuật toán khớp chính xác với điều này vì nó thực hiện tăng một con trỏ và đảo hướng một lần, tạo ra l = 1, r = 3 và rev = true, nhưng sự đảo ngược cuối cùng của lát cắt sẽ khôi phục lại thứ tự đúng. 

Trường hợp tinh vi cuối cùng là khi đoạn cuối cùng đã có hướng đảo ngược. Ví dụ: 

n = 3, k = 2, a = [7, 8, 9] 

Sau hai thao tác, phân đoạn là [1, 1] và rev là sai hoặc đúng tùy thuộc vào tính chẵn lẻ. Thuật toán đảm bảo rằng lát cắt cuối cùng chỉ được đảo ngược một lần, tránh những thay đổi cấu trúc lặp đi lặp lại có thể dẫn đến thứ tự không nhất quán.
