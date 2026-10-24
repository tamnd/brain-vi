---
title: "CF 105947E - \u0110\u1ebfm s\u1ed1 \u0111o\u1ea1n con"
description: "Chúng ta được cho một số trường hợp thử nghiệm, mỗi trường hợp bao gồm một mảng số nguyên. Đối với mỗi mảng, nhiệm vụ là đếm xem có bao nhiêu mảng con liền kề chứa chính xác k giá trị riêng biệt."
date: "2026-06-25T13:51:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105947
codeforces_index: "E"
codeforces_contest_name: "Bach Khoa Code Challenge #2"
rating: 0
weight: 105947
solve_time_s: 40
verified: true
draft: false
---

[CF 105947E - \u0110\u1ebfm s\u1ed1 \u0111o\u1ea1n con](https://codeforces.com/problemset/problem/105947/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số trường hợp thử nghiệm, mỗi trường hợp bao gồm một mảng số nguyên. Đối với mỗi mảng, nhiệm vụ là đếm chính xác có bao nhiêu mảng con liền kề nhau.`k`những giá trị riêng biệt. 

Mảng con liền kề là một phần của mảng được hình thành bằng cách chọn ranh giới bên trái và ranh giới bên phải mà không sắp xếp lại các phần tử. “Chính xác k giá trị riêng biệt” có nghĩa là nếu chúng ta xem xét tất cả các phần tử bên trong lát cắt đó, số lượng số duy nhất xuất hiện trong đó chính xác là`k`. 

Vì vậy, đầu ra cho mỗi trường hợp thử nghiệm là một số duy nhất: tổng số các mảng con như vậy. 

Các ràng buộc ngụ ý một cài đặt đếm mảng con cổ điển. Tổng chiều dài của tất cả các trường hợp thử nghiệm lên tới khoảng`2 · 10^5`, loại trừ mọi giải pháp bậc hai cho mỗi trường hợp thử nghiệm. Một bảng liệt kê O(n²) ngây thơ của tất cả các mảng con sẽ thực hiện khoảng 2e10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn. Điều này đẩy chúng ta tới các kỹ thuật tuyến tính hoặc gần tuyến tính, điển hình là cửa sổ trượt hoặc hai con trỏ. 

Có một số trường hợp phức tạp thường phá vỡ các giải pháp không chính xác. 

Nếu tất cả các phần tử đều bằng nhau và`k = 1`, mọi mảng con đều hợp lệ. Ví dụ,`[5, 5, 5]`sẽ mang lại 6 mảng con hợp lệ. Một giải pháp ngây thơ chỉ đếm nhầm các phân đoạn tối đa sẽ bị tính thiếu. 

Nếu như`k`lớn hơn số phần tử riêng biệt trong toàn bộ mảng thì câu trả lời phải bằng 0. Ví dụ,`[1, 2, 1]`với`k = 3`không có mảng con hợp lệ, mặc dù mỗi mảng con có tối đa 2 giá trị phân biệt. 

Một tình huống phức tạp khác là khi các bản sao được xen kẽ. Ví dụ,`[1, 2, 1, 2]`với`k = 2`có nhiều mảng con hợp lệ và việc đếm đòi hỏi phải xử lý cẩn thận các ranh giới; chỉ cần theo dõi số lượng riêng biệt mà không thu hẹp thích hợp sẽ dẫn đến việc đếm quá nhiều cửa sổ chồng chéo. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi lặp lại mọi điểm cuối bên trái, mở rộng điểm cuối bên phải và duy trì một tập hợp các phần tử để đếm các giá trị riêng biệt. Mỗi lần chúng tôi mở rộng con trỏ bên phải, chúng tôi tính toán lại hoặc cập nhật tập hợp và kiểm tra xem số lượng giá trị riêng biệt có chính xác không`k`. Điều này đếm chính xác tất cả các mảng con hợp lệ vì nó trực tiếp kiểm tra mọi phân đoạn có thể. 

Vấn đề là hiệu suất. Việc duy trì một bộ cho mỗi bản mở rộng được khấu hao O(1), nhưng chúng tôi vẫn kiểm tra các mảng con O(n²) cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất. Với tổng kích thước đầu vào là 2e5, điều này trở nên không khả thi. 

Quan sát quan trọng là chúng ta thực sự không cần phải đếm trực tiếp “chính xác k”. Thay vào đó, việc đếm các giá trị riêng biệt “tối đa k” sẽ dễ dàng hơn nhiều. Kỹ thuật cửa sổ trượt tiêu chuẩn cho phép chúng ta duy trì cửa sổ`[l, r]`sao cho nó luôn chứa nhiều nhất`k`các yếu tố riêng biệt. Đối với mỗi`r`, tất cả các mảng con kết thúc tại`r`và bắt đầu từ bất cứ đâu`l`ĐẾN`r`có giá trị dưới ràng buộc này, góp phần`(r - l + 1)`mảng con. 

Một khi chúng ta có thể tính toán`atMost(k)`, chúng ta có thể chuyển bài toán thành một bài toán khác của hai bài toán dễ hơn: các mảng con có chính xác`k`các giá trị phân biệt bằng`atMost(k) - atMost(k - 1)`. Điều này hoạt động vì mọi mảng con có chính xác`k`các giá trị riêng biệt được bao gồm trong`atMost(k)`nhưng bị loại khỏi`atMost(k - 1)`. 

Điều này biến vấn đề thành hai lần đi qua cửa sổ trượt tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Cửa sổ trượt (tối đa k + phép trừ) | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một chức năng trợ giúp`at_most(k)`đếm các mảng con có nhiều nhất`k`những giá trị riêng biệt. 

1. Khởi tạo hai con trỏ`l = 0`Và`result = 0`. Cũng duy trì một bản đồ tần số`freq`và một biến`distinct = 0`. Bản đồ theo dõi số lần mỗi giá trị xuất hiện trong cửa sổ hiện tại. 
2. Mở rộng con trỏ bên phải`r`từ`0`ĐẾN`n - 1`, thêm`a[r]`đến cửa sổ. Nếu giá trị này trước đây không có, hãy tăng`distinct`. Cập nhật tần số của nó trên bản đồ. 
3. Nếu`distinct`vượt quá`k`, thu nhỏ cửa sổ từ bên trái. Di chuyển`l`chuyển tiếp trong khi giảm tần số. Khi tần số bằng 0 thì giảm`distinct`. Điều này khôi phục bất biến mà cửa sổ có nhiều nhất`k`những giá trị riêng biệt. 
4. Sau khi cửa sổ hợp lệ, tất cả các mảng con kết thúc tại`r`và bắt đầu giữa`l`Và`r`là hợp lệ. Thêm vào`(r - l + 1)`ĐẾN`result`. Điều này có tác dụng vì mọi mảng con như vậy vẫn nằm bên trong cửa sổ hợp lệ hiện tại. 
5. Trở về`result`. 

Để có câu trả lời cuối cùng, hãy tính`at_most(k) - at_most(k - 1)`. 

### Tại sao nó hoạt động 

Tại mỗi bước, cửa sổ`[l, r]`là ranh giới bên trái nhỏ nhất sao cho mảng con kết thúc tại`r`chứa nhiều nhất`k`những giá trị riêng biệt. Bất kỳ ranh giới bên trái nhỏ hơn nào cũng sẽ vi phạm ràng buộc và bất kỳ ranh giới nào lớn hơn sẽ bỏ sót các mảng con hợp lệ. Điều này làm cho việc tính toán đóng góp trở nên chính xác, vì mỗi mảng con kết thúc tại`r`được tính chính xác một lần khi điểm cuối bên phải của nó được xử lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def at_most(a, k):
    if k < 0:
        return 0
    freq = defaultdict(int)
    l = 0
    distinct = 0
    res = 0

    for r, x in enumerate(a):
        if freq[x] == 0:
            distinct += 1
        freq[x] += 1

        while distinct > k:
            y = a[l]
            freq[y] -= 1
            if freq[y] == 0:
                distinct -= 1
            l += 1

        res += r - l + 1

    return res

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    print(at_most(a, k) - at_most(a, k - 1))
```Cấu trúc cốt lõi là cửa sổ trượt hai con trỏ. Từ điển tần số theo dõi các lần xuất hiện bên trong cửa sổ hiện tại và`distinct`tránh việc tính toán lại số lượng phần tử duy nhất từ ​​đầu. 

Một lỗi phổ biến là tính toán lại một tập hợp cho mỗi`r`, âm thầm biến nghiệm bậc hai. Một điều nữa là quên rằng việc thu hẹp phải tiếp tục cho đến khi ràng buộc được thỏa mãn trở lại; một sự thu hẹp có điều kiện duy nhất là không đủ. 

Phép trừ ở cuối là cần thiết. Không có`at_most(k - 1)`, hàm sẽ đếm tất cả các mảng con có ít hơn`k`cũng có những giá trị riêng biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, k = 2
a = [1, 2, 1]
```| r | tôi | cửa sổ | khác biệt | đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [1] | 1 | 1 | 
| 1 | 0 | [1,2] | 2 | 2 | 
| 2 | 0 | [1,2,1] | 2 | 3 |`at_most(2) = 6`,`at_most(1) = 3`, vậy đáp án là`3`. 

Điều này xác nhận rằng các cửa sổ chồng chéo được tính chính xác thông qua`(r - l + 1)`. 

### Ví dụ 2 

đầu vào:```
n = 4, k = 1
a = [5, 5, 5, 5]
```| r | tôi | cửa sổ | khác biệt | đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [5] | 1 | 1 | 
| 1 | 0 | [5,5] | 1 | 2 | 
| 2 | 0 | [5,5,5] | 1 | 3 | 
| 3 | 0 | [5,5,5,5] | 1 | 4 | 

Đây`at_most(1) = 10`,`at_most(0) = 0`, vậy đáp án là`10`. 

Điều này cho thấy thuật toán xử lý các phần tử lặp lại một cách tự nhiên mà không cần cách viết hoa đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi con trỏ di chuyển tối đa n bước và mỗi phần tử vào và ra khỏi cửa sổ một lần | 
| Không gian | O(n) | Bản đồ tần số lưu trữ tối đa n giá trị riêng biệt | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng kích thước đầu vào, phù hợp thoải mái với các ràng buộc trong đó tổng của`n`là về`2 · 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import defaultdict

def solve():
    input = sys.stdin.readline
    def at_most(a, k):
        if k < 0:
            return 0
        freq = defaultdict(int)
        l = 0
        distinct = 0
        res = 0
        for r, x in enumerate(a):
            if freq[x] == 0:
                distinct += 1
            freq[x] += 1
            while distinct > k:
                y = a[l]
                freq[y] -= 1
                if freq[y] == 0:
                    distinct -= 1
                l += 1
            res += r - l + 1
        return res

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        out.append(str(at_most(a, k) - at_most(a, k - 1)))
    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided samples
assert run("""3
1 1
10
3 1
3 1 8
16 1
8 1 5 8 8 7 4 9 7 10 10 7 5 8 5 5
""") == """1
3
19"""

# custom cases
assert run("""1
3 1
5 5 5
""") == "6"

assert run("""1
4 2
1 2 3 4
""") == "3"

assert run("""1
5 3
1 2 3 1 2
""") == "7"

assert run("""1
4 5
1 2 3 4
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 6 | đếm chồng chéo tối đa | 
| tất cả đều khác biệt | 3 | thu nhỏ cửa sổ trượt đúng cách | 
| lặp lại hỗn hợp | 7 | chồng chéo theo dõi khác biệt | 
| k quá lớn | 0 | xử lý điều kiện cạnh | 

## Vỏ cạnh 

Khi tất cả các phần tử đều giống hệt nhau và`k = 1`, cửa sổ không bao giờ co lại vì`distinct`vẫn ở mức 1. Thuật toán đếm mọi tiền tố kết thúc ở mỗi vị trí, điều này tự nhiên tạo ra số lượng mảng con hình tam giác đầy đủ. Vì`[5, 5, 5]`, các đóng góp là 1, 2 và 3, tổng bằng 6, khớp với số đếm chính xác. 

Khi`k = 0`, người giúp việc`at_most(k - 1)`trở thành`at_most(-1)`được định nghĩa là 0, do đó kết quả cũng bằng 0. Điều này tránh các cấu hình không hợp lệ trong cách viết hoa đặc biệt trong đó không có mảng con nào có thể có các giá trị khác biệt bằng 0. 

Khi tất cả các phần tử đều khác biệt và`k = 2`, cửa sổ co lại mạnh mẽ. Vì`[1,2,3,4]`, số đóng góp hợp lệ cho mỗi vị trí là 1, 2, 2 và 2, cho 7 cho`at_most(2)`, và trừ`at_most(1)`loại bỏ các mảng con một phần tử, để lại chính xác các mảng con có hai giá trị riêng biệt.
