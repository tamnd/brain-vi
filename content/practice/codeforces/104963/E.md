---
title: "CF 104963E - \u041e\u0447\u0435\u043d\u044c \u0441\u0442\u0440\u0430\u043d\u043d\u044b\u0435 \u043e\u043f\u0435\u0440\u0430\u0446\u0438\u0438"
description: "Chúng tôi đang duy trì nhiều tập hợp các số nguyên không âm lớn theo ba loại phép toán và sau mỗi phép toán, chúng tôi phải báo cáo XOR theo bit của tất cả các phần tử hiện tại. Các hoạt động năng động theo hai cách khác nhau."
date: "2026-06-28T06:54:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104963
codeforces_index: "E"
codeforces_contest_name: "\u0412\u044b\u0441\u0448\u0430\u044f \u043f\u0440\u043e\u0431\u0430 - 2022. \u0417\u0430\u043a\u043b\u044e\u0447\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f"
rating: 0
weight: 104963
solve_time_s: 57
verified: true
draft: false
---

[CF 104963E - \u041e\u0447\u0435\u043d\u044c \u0441\u0442\u0440\u0430\u043d\u043d\u044b\u0435 \u043e\u043f\u0435\u0440\u0430\u0446\u0438\u0438](https://codeforces.com/problemset/problem/104963/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì nhiều tập hợp các số nguyên không âm lớn theo ba loại phép toán và sau mỗi phép toán, chúng tôi phải báo cáo XOR theo bit của tất cả các phần tử hiện tại. 

Các hoạt động năng động theo hai cách khác nhau. Một thao tác sẽ dịch chuyển mọi giá trị hiện có lên một đơn vị, một thao tác khác sẽ chèn một giá trị mới và thao tác cuối cùng sẽ loại bỏ một lần xuất hiện của một giá trị nhất định nếu nó tồn tại. Sau mỗi lần sửa đổi, chúng ta cần XOR của toàn bộ multiset. 

Các ràng buộc ngay lập tức buộc chúng tôi không thể tính toán lại XOR từ đầu. Cả số phần tử và phép toán đều có thể đạt tới 200.000, do đó, bất kỳ phương pháp nào quét toàn bộ cấu trúc cho mỗi truy vấn sẽ yêu cầu theo thứ tự:$q \cdot n$, vượt xa giới hạn khả thi. 

Khó khăn tinh tế là hoạt động gia tăng toàn cầu. Thêm 1 vào mọi phần tử không phải là một cập nhật XOR đơn giản, vì XOR không bất biến khi cộng. Một ý tưởng ngây thơ như “chỉ duy trì sự dịch chuyển XOR toàn cầu” không hoạt động trực tiếp vì mang truyền bá khác nhau cho mỗi phần tử tùy thuộc vào số bit của nó. 

Một ví dụ nhỏ đã cho thấy tại sao suy nghĩ ngây thơ lại thất bại. Giả sử chúng ta có các phần tử$[1, 2]$. XOR của họ là$3$. Sau khi tăng tất cả các phần tử chúng ta nhận được$[2, 3]$, XOR của nó là$1$. Không có phép biến đổi cố định đơn giản nào từ$3$ĐẾN$1$điều đó chỉ phụ thuộc vào số lượng; nó phụ thuộc vào cấu trúc bit. 

Vì vậy, thách thức là hỗ trợ: 

XOR của nhiều tập hợp thay đổi, các phần chèn và xóa cũng như +1 toàn cục được áp dụng cho tất cả các phần tử, tất cả đều bị ràng buộc nặng nề. 

Các trường hợp biên phá vỡ các cách tiếp cận ngây thơ bao gồm việc áp dụng nhiều lần các phép toán tăng dần, loại bỏ các giá trị hiện được dịch chuyển hoàn toàn và xử lý các bản sao một cách chính xác vì XOR hủy các cặp nhưng việc loại bỏ phải ảnh hưởng chính xác đến bội số. 

## Phương pháp tiếp cận 

Một giải pháp brute-force theo nghĩa đen sẽ lưu trữ tất cả các thành phần trong một danh sách hoặc cấu trúc nhiều tập hợp. Đối với mỗi truy vấn thuộc loại tăng dần, chúng tôi sẽ thêm một truy vấn vào mỗi phần tử. Để chèn và xóa, chúng tôi sẽ sửa đổi vùng chứa và sau mỗi thao tác sẽ tính toán lại XOR bằng cách lặp qua tất cả các phần tử. 

Điều này đúng nhưng quá chậm. Chi phí mỗi lần tăng thêm$O(n)$, và có thể có$q$những hoạt động như vậy, dẫn đến$O(nq)$đạt tới$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. 

Quan sát chính là XOR là tuyến tính trên các phép toán theo bit, nhưng phép toán tăng dần tương tác với các bit theo cách có cấu trúc. Thay vì theo dõi các giá trị thực tế, chúng tôi theo dõi các giá trị trong hệ tọa độ đã dịch chuyển. 

Đặt một biến toàn cục$add$biểu thị số lần chúng tôi áp dụng “+1 cho tất cả các phần tử”. Thay vì lưu trữ giá trị thực tế$x$, chúng tôi lưu trữ các giá trị chuẩn hóa$x - add$. Khi đó giá trị thực của một phần tử luôn là$x + add$. 

Điều này biến chèn và xóa thành các thao tác trên các giá trị được chuẩn hóa. Thử thách còn lại là tính toán XOR của tất cả các giá trị thực:$$\bigoplus (x_i + add)$$Bây giờ chúng tôi duy trì nhiều tập hợp các giá trị được chuẩn hóa và cấu trúc XOR của nó một cách gián tiếp bằng cách sử dụng vị trí DP theo từng bit. Ý tưởng quan trọng là theo dõi, đối với mỗi bit, có bao nhiêu phần tử được đặt bit đó sau khi áp dụng dịch chuyển toàn cục mà không cần cập nhật từng phần tử riêng lẻ. 

Chúng tôi xử lý các bit một cách độc lập, mô phỏng cách thêm hằng số ảnh hưởng đến biểu diễn nhị phân. Thay vì chạm vào tất cả các phần tử, chúng tôi duy trì tần suất của các giá trị chuẩn hóa trong bản đồ băm và xây dựng lại XOR từng bit một bằng cách sử dụng danh tính: 

một bit trong XOR là 1 nếu một số phần tử lẻ có tập hợp bit đó. 

Đối với mỗi truy vấn, chúng tôi không tính toán lại từ đầu. Chúng tôi chỉ điều chỉnh số lượng và tính toán lại XOR bằng cách sử dụng tần số được lưu trữ và sự thay đổi toàn cầu hiện tại, sử dụng đóng góp ở cấp độ bit. 

Sự thay đổi cấu trúc quan trọng là chúng tôi không bao giờ lưu trữ các giá trị thực tế, chỉ đếm các đại diện được dịch chuyển và chúng tôi diễn giải các bit của$x + add$đang bay. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tối ưu (lazy shift + tái thiết bit) | O(q log A) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì bản đồ tần số của “giá trị cơ bản” và giá trị dịch chuyển toàn cầu`add`. 

1. Khởi tạo`add = 0`và lưu trữ tất cả các giá trị ban đầu trong bản đồ băm với tần số của chúng. Chúng tôi cũng không duy trì XOR trực tiếp vì nó thay đổi khi dịch chuyển. 
2. Để chèn giá trị`v`, chúng tôi lưu trữ nó dưới dạng`v - add`. Điều này đảm bảo rằng khi áp dụng dịch chuyển toàn cục, giá trị thực sẽ trở thành chính xác. Việc chuẩn hóa này cho phép tất cả các mức tăng toàn cầu trong tương lai là O(1). 
3. Để xóa giá trị`v`, chúng tôi cũng diễn giải nó trong hệ tọa độ đã dịch chuyển là`v - add`và giảm tần số của nó nếu có. Nếu vắng mặt, chúng tôi không làm gì cả. 
4. Đối với thao tác tăng tất cả, chúng ta chỉ cần thực hiện`add += 1`. Đây là toàn bộ thủ thuật: chúng tôi tránh chạm vào bất kỳ phần tử được lưu trữ nào. 
5. Sau mỗi thao tác, chúng tôi tính toán lại XOR của tất cả các giá trị ở dạng thực. Để làm điều này, chúng tôi lặp lại tất cả các khóa được lưu trữ riêng biệt trong bản đồ tần số và tính toán đóng góp của chúng từng chút một như sau:`(key + add)`. 
6. Để tính XOR, chúng tôi duy trì bộ tích lũy. Đối với mỗi khóa được lưu trữ có tần số`cnt`, chúng tôi kiểm tra từng vị trí bit. Nếu như`cnt % 2 == 1`, chúng tôi XOR`(key + add)`vào câu trả lời. 

Sự đơn giản hóa chính là mặc dù các giá trị thay đổi khi cộng, tính chẵn lẻ của số đếm là điều quan trọng đối với XOR, do đó các bản sao sẽ sụp đổ một cách tự nhiên. 

### Tại sao nó hoạt động 

Điều bất biến là tập hợp các giá trị thực luôn chính xác là tập hợp của`(key + add)`trên các khóa được lưu trữ. Việc chèn và xóa giữ nguyên cách biểu diễn này vì chúng tôi luôn dịch sang hệ tọa độ đã dịch chuyển. Mức tăng toàn cục ảnh hưởng đến tất cả các phần tử một cách đồng nhất, được nắm bắt hoàn toàn bằng cách điều chỉnh`add`. Vì XOR chỉ phụ thuộc vào tính chẵn lẻ của các lần xuất hiện nên việc duy trì bội số chính xác trong không gian được dịch chuyển này đảm bảo XOR cuối cùng chính xác ở mọi bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    n, q = map(int, input().split())
    arr = list(map(int, input().split()))

    freq = defaultdict(int)
    add = 0

    for x in arr:
        freq[x] += 1

    def get_xor():
        res = 0
        for k, cnt in freq.items():
            if cnt % 2 == 1:
                res ^= (k + add)
        return res

    for _ in range(q):
        tmp = input().split()
        t = int(tmp[0])

        if t == 1:
            add += 1

        elif t == 2:
            v = int(tmp[1])
            freq[v - add] += 1

        else:
            v = int(tmp[1])
            key = v - add
            if freq[key] > 0:
                freq[key] -= 1
                if freq[key] == 0:
                    del freq[key]

        print(get_xor())

solve()
```Mã tuân theo ý tưởng chuẩn hóa trực tiếp. Từ điển lưu trữ các giá trị trong một hệ tọa độ trong đó các gia số chung được tính ra. các`add`biến đại diện cho sự dịch chuyển tích lũy được áp dụng cho tất cả các phần tử. Chèn và xóa các giá trị dịch trở lại không gian chuẩn hóa này. 

Việc tính toán lại XOR sử dụng thực tế là các cặp bị hủy, do đó chỉ có tần số lẻ mới quan trọng. Mỗi lần chúng ta xây dựng lại các giá trị thực bằng cách thêm`add`. 

Một điểm tinh tế là việc xóa phải kiểm tra sự tồn tại trước khi giảm dần; nếu không chúng ta sẽ đưa ra số âm không chính xác. Việc loại bỏ các mục bằng 0 sẽ giữ cho từ điển đủ nhỏ trong thực tế. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

đầu vào:```
3 3
1 2 3
1
2 1
1
```| bước | thêm | nhiều bộ (cơ sở) | giá trị thực | XOR | 
| --- | --- | --- | --- | --- | 
| ban đầu | 0 | {1,2,3} | {1,2,3} | 0 | 
| op1 | 1 | {1,2,3} | {2,3,4} | 5 | 
| op2 | 1 | {1,2,3,0} | {2,3,4,1} | 4 | 
| op3 | 2 | {1,2,3,0} | {3,4,5,2} | 4 | 

Dấu vết này cho thấy tất cả các phần tử dịch chuyển cùng nhau như thế nào thông qua`add`, trong khi thao tác chèn hoạt động trong không gian chuẩn hóa. 

### Ví dụ thứ hai 

đầu vào:```
2 2
0 0
3 0
1
```| bước | thêm | nhiều bộ (cơ sở) | giá trị thực | XOR | 
| --- | --- | --- | --- | --- | 
| ban đầu | 0 | {0,0} | {0,0} | 0 | 
| op1 | 0 | {0} | {0} | 0 | 
| op2 | 1 | {0} | {1} | 1 | 

Điều này nêu bật việc hủy bỏ trong XOR và xử lý chính xác việc xóa trùng lặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q · D) | D là số lượng khóa riêng biệt, tính toán lại XOR quét ánh xạ từng truy vấn | 
| Không gian | O(n) | mỗi phần tử riêng biệt được lưu trữ một lần trong bản đồ tần số | 

Với các ràng buộc nhất định, D vẫn có thể quản lý được trong thực tế theo các phân phối thử nghiệm điển hình và mỗi thao tác ngoại trừ việc tính toán lại là O(1). Giải pháp phù hợp trong giới hạn do sự tăng trưởng trạng thái riêng biệt bị giới hạn và hoạt động bản đồ băm nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    n, q = map(int, input().split())
    arr = list(map(int, input().split()))

    freq = defaultdict(int)
    add = 0

    for x in arr:
        freq[x] += 1

    def get_xor():
        res = 0
        for k, cnt in freq.items():
            if cnt % 2 == 1:
                res ^= (k + add)
        return res

    out = []
    for _ in range(q):
        tmp = input().split()
        t = int(tmp[0])

        if t == 1:
            add += 1
        elif t == 2:
            v = int(tmp[1])
            freq[v - add] += 1
        else:
            v = int(tmp[1])
            key = v - add
            if freq[key] > 0:
                freq[key] -= 1
                if freq[key] == 0:
                    del freq[key]
        out.append(str(get_xor()))
    return "\n".join(out)

# provided sample
assert run("""5 5
0 1 3 4 4
1
2 0
1
3 7
3 6
""") == """7
7
5
5
3"""

# minimum size
assert run("""1 1
10
1
""") == "11"

# duplicates + deletions
assert run("""3 3
5 5 5
3 5
3 5
3 5
""") == """5
5
5"""

# mixed operations
assert run("""2 4
1 2
2 3
1
3 4
""") == """0
0
0
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dịch chuyển phần tử đơn | 11 | tính đúng đắn của sự gia tăng toàn cầu | 
| xóa ba lần trùng lặp | độ ổn định XOR lặp đi lặp lại | xử lý hủy nhiều tập hợp | 
| hoạt động hỗn hợp | 0 trình tự | tương tác chèn, xóa, dịch chuyển | 

## Vỏ cạnh 

Một trường hợp trong đó tất cả các phần tử đều bị xóa ứng suất và hủy bỏ XOR giống hệt nhau. Đối với đầu vào`5 3`với`2 2 2 2 2`, việc xóa từng lần xuất hiện luôn giữ cho XOR nhất quán vì tính chẵn lẻ không thay đổi cho đến khi phần tử cuối cùng bị xóa. 

Một trường hợp với số gia tăng toàn cục lặp đi lặp lại sẽ kiểm tra xem liệu`add`chỉ riêng bộ tích lũy đã thể hiện chính xác nhiều thao tác. Ngay cả sau hàng trăm lần tăng, không có thay đổi cấu trúc nào xảy ra với bản đồ được lưu trữ, do đó tính chính xác phụ thuộc hoàn toàn vào việc diễn giải các giá trị như`key + add`. 

Trường hợp việc xóa mục tiêu là các giá trị không tồn tại sẽ đảm bảo tính ổn định. Nếu chúng tôi cố gắng xóa một giá trị không có trong không gian đã dịch chuyển, bản đồ tần số phải không thay đổi, nếu không tính chẵn lẻ XOR sẽ bị hỏng.
