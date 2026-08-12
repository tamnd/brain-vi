---
title: "CF 102420E - \u041b\u0435\u043d\u0438\u0432\u044b\u0435 \u043b\u0435\u0441\u043e\u0440\u0443\u0431\u044b"
description: "Chúng ta có một dãy có thứ tự gồm (n) thợ rừng. Thợ rừng (i) làm việc trong một khoảng ([li,ri]), và trong khoảng đó anh ta hạ bức tường xuống đúng nửa mét."
date: "2026-08-12T00:42:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102420
codeforces_index: "E"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102420
solve_time_s: 122
verified: true
draft: false
---

[CF 102420E - \u041b\u0435\u043d\u0438\u0432\u044b\u0435 \u043b\u0435\u0441\u043e\u0440\u0443\u0431\u044b](https://codeforces.com/problemset/problem/102420/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy có thứ tự gồm (n) thợ rừng. Thợ rừng (i) làm việc trong một khoảng thời gian ([l_i,r_i]), và trong khoảng thời gian đó anh ta hạ thấp bức tường xuống chính xác nửa mét. Đối với một nhóm thợ rừng liền kề đã chọn (a,a+1,\ldots,b), chúng ta cần tổng mức giảm phải là số nguyên tại mọi tọa độ trên tường. 

Vì mọi đóng góp đều là (0,5), điều duy nhất quan trọng là tính chẵn lẻ. Tại mỗi tọa độ, số khoảng được chọn bao trùm tọa độ đó phải là số chẵn. Câu trả lời là số lượng các mảng con liền kề của thợ rừng thỏa mãn điều kiện chẵn lẻ này. 

Các ràng buộc chính thức cho phép (n=200.000), trong khi trang cuộc thi đưa ra giới hạn thời gian là 2 giây và bộ nhớ 512 MB. Một bảng liệt kê (O(n^2)) thực hiện khoảng (n(n+1)/2), đại khái là (2\cdot10^{10}), kiểm tra mảng con trong trường hợp xấu nhất. Ngay cả khi mỗi tấm séc cực kỳ rẻ, điều đó vẫn vượt xa những gì có thể đáp ứng được trong thời hạn. Chúng ta cần một thuật toán tuyến tính hoặc gần tuyến tính. 

Tọa độ có thể nhỏ đến (-10^9) và lớn đến (10^9), do đó, việc coi mọi tọa độ nguyên là chỉ mục mảng là không thể. Chỉ có điểm cuối khoảng là quan trọng, cung cấp cho chúng ta tối đa (2n) tọa độ phù hợp. 

Có một số trường hợp khó xử lý. Ví dụ: với một người thợ rừng, câu trả lời là 0:```
1
1 2
```Một khoảng đóng góp nửa mét trên ([1,2]), vì vậy nó không bao giờ có thể tự tạo thành một thay đổi số nguyên. Việc triển khai tính từng khoảng thời gian vì các điểm cuối khác nhau sẽ sai. 

Sai lầm phổ biến thứ hai là quên rằng thay đổi trống được cho phép khi một nhóm khoảng thời gian bị hủy hoàn toàn. Ví dụ:```
2
1 3
1 3
```Câu trả lời là (1), bởi vì việc chọn cả hai thợ rừng sẽ làm giảm chính xác một mét trên ([1,3]). Tổng quát hơn, các khoảng giống nhau sẽ triệt tiêu theo cặp khi chúng ta chỉ nhìn vào tính chẵn lẻ. 

Tọa độ âm cũng phải hoạt động mà không cần xử lý đặc biệt:```
2
-5 -2
-5 -2
```Một lần nữa câu trả lời là (1). Bất kỳ giải pháp nào dựa trên việc lập chỉ mục mảng trực tiếp theo tọa độ sẽ không thành công ở đây trừ khi nó thực hiện nén tọa độ. 

Cuối cùng, một khoảng có thể có cùng điểm cuối với điểm cuối của khoảng khác mà không có các khoảng giống hệt nhau. Ví dụ:```
3
1 2
2 3
1 3
```Câu trả lời là (1). Ba khoảng cùng nhau hủy bỏ ở mức chẵn lẻ, trong khi không có tiền tố không trống thích hợp nào làm được. Một giải pháp chỉ lý giải về độ dài khoảng thời gian hoặc chỉ về số lượng điểm cuối được chia sẻ sẽ bỏ lỡ cấu trúc chẵn lẻ thực tế. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra từng cặp ((a,b)). Đối với mỗi mảng con được chọn, chúng tôi có thể duy trì số lượng khoảng đã chọn bao gồm mọi tọa độ có liên quan và kiểm tra xem tất cả các số đó có chẵn hay không. Điều này đúng vì điều kiện vật lý chính xác là cứ mỗi nửa mét đóng góp sẽ xảy ra số lần chẵn. Ngay cả với việc nén tọa độ và cập nhật hiệu quả, vẫn có thể có (\Theta(n^2)) các mảng con. Với (n=200.000), có (20.000.100.000) trong số chúng, vì vậy phương pháp bậc hai là không khả thi. 

Quan sát hữu ích xuất phát từ việc xem xét một khoảng thông qua các điểm cuối của nó thay vì qua mọi điểm bên trong nó. Hãy tưởng tượng bạn đang đi dọc theo trục số. Tính chẵn lẻ của số khoảng thời gian hoạt động thay đổi chính xác khi chúng ta vượt qua điểm cuối. Một khoảng ([l,r]) chuyển đổi tính chẵn lẻ tại (l) và chuyển đổi nó trở lại (r). Do đó, nếu chúng ta biểu diễn tập hợp tọa độ điểm cuối với bội số lẻ, thì một khoảng được biểu thị bằng cách chuyển đổi chính xác hai tọa độ (l) và (r). 

Đối với một tập hợp các khoảng, tính chẵn lẻ của phạm vi bao phủ cuối cùng bằng 0 ở mọi nơi chính xác khi mọi tọa độ điểm cuối xảy ra một số lần chẵn. Theo thuật ngữ đại số, mọi khoảng là một vectơ trên (\mathrm{GF}(2)), với (1) tại hai điểm cuối của nó và nhóm được chọn là hợp lệ chính xác khi XOR của tất cả các vectơ này bằng 0. 

Bây giờ hãy xem xét các nhóm tiền tố. Đặt (P_i) là XOR của các vectơ điểm cuối của thợ rừng (1) đến (i), với (P_0=0). Các khoảng từ (a) đến (b) có XOR 

[ 
P_b \oplus P_{a-1}. 
] 

XOR này bằng 0 chính xác khi 

[ 
P_b=P_{a-1}. 
] 

Vì vậy, bài toán ban đầu trở thành bài toán đếm trạng thái tiền tố quen thuộc: tính trạng thái sau mỗi người thợ rừng và đếm các trạng thái bằng nhau. Nếu một trạng thái đã xuất hiện (k) lần thì lần xuất hiện tiếp theo sẽ tạo ra (k) mảng con hợp lệ mới. 

Các trạng thái thực sự là các tập hợp chẵn lẻ trên tọa độ lên tới (2n), vì vậy việc lưu trữ chúng theo nghĩa đen sẽ quá tốn kém. Chúng tôi có thể cung cấp cho mọi tọa độ một dấu vân tay 128 bit ngẫu nhiên và XOR dấu vân tay của các điểm cuối. Với hai thành phần 64-bit độc lập, các bộ chẵn lẻ bằng nhau luôn tạo ra các dấu vân tay giống nhau, trong khi xác suất để hai bộ chẵn lẻ khác nhau va chạm nhau là không đáng kể. Một hạt giống ngẫu nhiên mới làm cho việc xây dựng dữ liệu đầu vào cụ thể để gây ra xung đột là không thực tế. 

Cách tiếp cận vũ phu hoạt động vì nó kiểm tra rõ ràng mọi nhóm khoảng, nhưng không thành công vì có nhiều nhóm bậc hai. Quan sát rằng một khoảng được đặc trưng hoàn toàn, vì mục đích chẵn lẻ, bởi hai điểm cuối của nó cho phép chúng ta biến mọi nhóm thành một so sánh XOR tiền tố, giảm toàn bộ nhiệm vụ xuống một lần bằng bản đồ băm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) hoặc tệ hơn | (O(n)) | Quá chậm | 
| Tiền tố XOR với dấu vân tay 128 bit | (O(n)) dự kiến ​​| (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Gán cho mỗi tọa độ một dấu vân tay 128 bit ngẫu nhiên, được biểu thị dưới dạng hai số nguyên 64 bit độc lập. Cùng một tọa độ phải luôn nhận được cùng một cặp, trong khi các tọa độ khác nhau gần như chắc chắn sẽ nhận được các dấu vân tay khác nhau. 
2. Bắt đầu với trạng thái tiền tố ((0,0)). Điều này đại diện cho bộ sưu tập thợ rừng trống rỗng. Đặt trạng thái này vào bản đồ tần số có số đếm (1), vì mọi tiền tố sau bằng với nó sẽ tạo thành một mảng con hợp lệ bắt đầu từ thợ rừng (1). 
3. Xử lý thợ rừng từ trái sang phải. Đối với khoảng ([l_i,r_i]), XOR dấu vân tay của (l_i) và dấu vân tay của (r_i) vào trạng thái tiền tố hiện tại. Cả hai điểm cuối đều được chuyển đổi vì một khoảng thời gian sẽ thay đổi tính chẵn lẻ của vùng phủ sóng hoạt động khi vào và ra khỏi nó. 
4. Tra cứu trạng thái tiền tố mới trong bản đồ tần số. Nếu nó đã xuất hiện (k) lần trước đó, hãy thêm (k) vào câu trả lời. Mỗi lần xuất hiện trước đó tương ứng với một tiền tố (P_j) với (P_j=P_i) và do đó, thợ rừng (j+1,\ldots,i) không có XOR. 
5. Tăng tần suất của trạng thái hiện tại. Trạng thái hiện có sẵn để tạo thành các mảng con hợp lệ kết thúc ở một số vị trí sau đó. 
6. Sau khi tất cả (n) thợ rừng đã được xử lý, hãy xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Đối với mọi tọa độ (x), tính chẵn lẻ của vùng phủ sóng đã chọn chỉ thay đổi khi một khoảng bắt đầu hoặc kết thúc tại (x). Do đó, một khoảng đóng góp chính xác hai nút chuyển đổi chẵn lẻ, một nút ở mỗi điểm cuối. Một nhóm các khoảng có chiều cao nguyên thay đổi chính xác ở mọi nơi khi mỗi tọa độ được chuyển đổi một số lần chẵn, đó chính xác là khi XOR của các vectơ điểm cuối của chúng bằng 0. 

Trạng thái tiền tố (P_i) lưu trữ XOR của khoảng (i) đầu tiên. Một mảng con (a,\ldots,b) có điểm chẵn lẻ điểm cuối bằng 0 chính xác khi (P_{a-1}\oplus P_b=0) hoặc tương đương (P_{a-1}=P_b). Bản đồ tần số đếm chính xác có bao nhiêu lựa chọn về (a-1) mang lại sự bằng nhau này. Do đó, mỗi mảng con hợp lệ được tính một lần và không có mảng con không hợp lệ nào được tính. 

Phép tính gần đúng duy nhất là thay thế vectơ chẵn lẻ đầy đủ bằng dấu vân tay 128 bit ngẫu nhiên. Về mặt lý thuyết, một sự va chạm giữa hai vectơ chẵn lẻ khác nhau có thể tạo ra một câu trả lời sai, nhưng với hai giá trị 64 bit độc lập thì xác suất là không đáng kể đối với kích thước đầu vào này. 

## Giải pháp Python```python
import sys
import random

input = sys.stdin.readline

MASK = (1 << 64) - 1
SM_CONST = 0x9E3779B97F4A7C15

def splitmix64(x):
    x = (x + SM_CONST) & MASK
    x = (x ^ (x >> 30)) * 0xBF58476D1CE4E5B9 & MASK
    x = (x ^ (x >> 27)) * 0x94D049BB133111EB & MASK
    return (x ^ (x >> 31)) & MASK

def solve():
    n = int(input())

    seed1 = random.SystemRandom().getrandbits(64)
    seed2 = random.SystemRandom().getrandbits(64)

    state1 = 0
    state2 = 0

    freq = {(0, 0): 1}
    answer = 0

    def fingerprint(x):
        # x is allowed to be negative, so normalize it to 64 bits.
        ux = x & MASK
        h1 = splitmix64(ux ^ seed1)
        h2 = splitmix64(ux ^ seed2)
        return h1, h2

    for _ in range(n):
        l, r = map(int, input().split())

        l1, l2 = fingerprint(l)
        r1, r2 = fingerprint(r)

        state1 ^= l1 ^ r1
        state2 ^= l2 ^ r2

        state = (state1, state2)
        answer += freq.get(state, 0)
        freq[state] = freq.get(state, 0) + 1

    print(answer)

if __name__ == "__main__":
    solve()
```các`splitmix64`hàm biến tọa độ thành giá trị 64 bit được trộn đều. Hai hạt giống độc lập tạo ra hai dấu vân tay độc lập, tạo ra trạng thái tiền tố tổng cộng là 128 bit. Việc sử dụng`& MASK`là cần thiết vì số nguyên Python không tự nhiên bao bọc ở 64 bit, trong khi hàm trộn được xác định theo số học 64 bit không dấu. 

Trạng thái tiền tố bắt đầu từ 0 và được chèn vào`freq`trước khi xử lý bất kỳ khoảng thời gian nào. Sự xuất hiện ban đầu đó đại diện cho (P_0), cần thiết cho các mảng con bắt đầu bằng thợ rừng (1). Quên nó sẽ mất mọi tiền tố hợp lệ. 

Đối với mỗi khoảng thời gian, mã XOR sẽ đưa cả dấu vân tay điểm cuối vào trạng thái. XOR chính xác là thao tác cần thiết cho tính chẵn lẻ: áp dụng cùng một tọa độ hai lần sẽ hủy nó vì (x\oplus x=0). Thứ tự của hai bản cập nhật điểm cuối không quan trọng. 

Câu trả lời được cập nhật trước khi tăng tần suất của trạng thái hiện tại. Điều này làm cho bản đồ chỉ đại diện cho các tiền tố trước đó. Việc thêm trạng thái hiện tại trước tiên sẽ cho phép một mảng con có độ dài bằng 0 không chính xác, không nằm trong số các lựa chọn (a\le b). 

Số nguyên Python có độ chính xác tùy ý, nhưng tất cả số học dấu vân tay được giảm rõ ràng xuống còn 64 bit bên trong`splitmix64`. Bản thân câu trả lời cuối cùng có thể lớn bằng (n(n+1)/2), khoảng (2\cdot10^{10}), mà Python xử lý trực tiếp. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, các khoảng là ([1,3]), ([2,3]), ([2,3]) và ([1,3]). Chúng ta có thể viết (A_x) cho dấu vân tay tọa độ (x). Trạng thái tiền tố được biểu diễn một cách tượng trưng dưới dạng XOR của các dấu vân tay này. 

| tôi | Khoảng thời gian | Trạng thái tiền tố | Trạng thái bình đẳng trước đó | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | không | (0) | 1 | 0 | 
| 1 | ([1,3]) | (A_1\oplus A_3) | 0 | 0 | 
| 2 | ([2,3]) | (A_1\oplus A_2) | 0 | 0 | 
| 3 | ([2,3]) | (A_1\oplus A_3) | 1 | 1 | 
| 4 | ([1,3]) | (0) | 1 | 2 | 

Tại (i=3), trạng thái bằng với trạng thái sau người thợ rừng đầu tiên, do đó những người thợ rừng (2) đến (3) tạo thành một nhóm hợp lệ. Tại (i=4), trạng thái trở về 0, khớp với tiền tố trống, do đó cả bốn thợ rừng tạo thành một nhóm hợp lệ khác. Câu trả lời là (2). 

Đối với Mẫu 2, các khoảng là ([1,2]), ([2,3]) và ([1,3]). 

| tôi | Khoảng thời gian | Trạng thái tiền tố | Trạng thái bình đẳng trước đó | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | không | (0) | 1 | 0 | 
| 1 | ([1,2]) | (A_1\oplus A_2) | 0 | 0 | 
| 2 | ([2,3]) | (A_1\oplus A_3) | 0 | 0 | 
| 3 | ([1,3]) | (0) | 1 | 1 | 

Chỉ có chuỗi hoàn chỉnh mới trở về trạng thái ban đầu. Ba khoảng của nó có nhiều điểm cuối (1,2,2,3,1,3), do đó mỗi tọa độ xảy ra chính xác hai lần. Câu trả lời là (1). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) dự kiến ​​| Mỗi thợ rừng thực hiện một số lượng không đổi các phép toán băm 64 bit và các phép toán bản đồ băm dự kiến ​​(O(1)). | 
| Không gian | (O(n)) | Bản đồ tần số chứa tối đa (n+1) trạng thái tiền tố riêng biệt. | 

Với (n\le200.000), một đường chuyền tuyến tính duy nhất phù hợp với giới hạn 2 giây, 512 MB do trang cuộc thi đưa ra. Việc triển khai chỉ thực hiện công việc liên tục trên mỗi thợ rừng và lưu trữ tối đa một mục nhập cho mỗi trạng thái tiền tố. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới sử dụng ý tưởng dấu vân tay giống như giải pháp đã gửi nhưng hiển thị thuật toán thông qua`solve_text`vì vậy mỗi trường hợp có thể được kiểm tra bằng một khẳng định.```python
import sys
import io
import random

MASK = (1 << 64) - 1
SM_CONST = 0x9E3779B97F4A7C15

def splitmix64(x):
    x = (x + SM_CONST) & MASK
    x = (x ^ (x >> 30)) * 0xBF58476D1CE4E5B9 & MASK
    x = (x ^ (x >> 27)) * 0x94D049BB133111EB & MASK
    return (x ^ (x >> 31)) & MASK

def solve_text(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))

    seed1 = 0x123456789ABCDEF0
    seed2 = 0x0FEDCBA987654321

    def fingerprint(x):
        ux = x & MASK
        return (
            splitmix64(ux ^ seed1),
            splitmix64(ux ^ seed2),
        )

    s1 = 0
    s2 = 0
    freq = {(0, 0): 1}
    ans = 0

    for _ in range(n):
        l = int(next(it))
        r = int(next(it))

        l1, l2 = fingerprint(l)
        r1, r2 = fingerprint(r)

        s1 ^= l1 ^ r1
        s2 ^= l2 ^ r2

        state = (s1, s2)
        ans += freq.get(state, 0)
        freq[state] = freq.get(state, 0) + 1

    return str(ans)

def run(inp: str) -> str:
    return solve_text(inp)

assert run("""4
1 3
2 3
2 3
1 3
""") == "2", "sample 1"

assert run("""3
1 2
2 3
1 3
""") == "1", "sample 2"

assert run("""1
1 2
""") == "0", "single interval can never be valid"

assert run("""4
1 3
1 3
1 3
1 3
""") == "4", "four equal intervals, every even-length subarray"

assert run("""2
-1000000000 1000000000
-1000000000 1000000000
""") == "1", "coordinate boundaries and negative values"

n = 200000
max_case = str(n) + "\n" + "\n".join(["-1 1"] * n) + "\n"
assert run(max_case) == str((n // 2) * ((n + 1) // 2)), "maximum-size all-equal case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 2`|`0`| Kích thước tối thiểu và thực tế là một khoảng không trống không thể tự hủy. | 
|`4 / 1 3`lặp đi lặp lại bốn lần |`4`| Lặp lại các khoảng giống hệt nhau và đếm mọi mảng con có độ dài chẵn. | 
|`2 / -1000000000 1000000000`lặp lại hai lần |`1`| Giá trị tọa độ cực trị và tọa độ âm. | 
|`200000 / -1 1`lặp đi lặp lại |`10000000000`| Tối đa (n), câu trả lời lớn và khả năng mở rộng tuyến tính. | 

## Vỏ cạnh 

Vụ án thợ rừng đơn lẻ```
1
1 2
```bắt đầu bằng trạng thái (0), sau đó XOR dấu vân tay của (1) và (2), tạo ra trạng thái khác 0 vì hai tọa độ khác nhau. Trạng thái chưa xuất hiện trước đó nên đáp án vẫn là (0). Đây chính xác là những gì chúng ta cần, vì đóng góp nửa mét không thể trở thành cả mét. 

Đối với hai khoảng thời gian giống nhau,```
2
1 3
1 3
```khoảng đầu tiên tạo ra (A_1\oplus A_3). Cái thứ hai áp dụng chính xác XOR tương tự, do đó trạng thái lại trở về 0. Tần số bằng 0 ban đầu là (1), đại diện cho (P_0), do đó tiền tố thứ hai đóng góp một mảng con hợp lệ. Đầu ra là (1). 

Đối với tọa độ cực trị,```
2
-1000000000 1000000000
-1000000000 1000000000
```tọa độ được chuẩn hóa thành giá trị 64-bit không dấu trước khi băm. Cả hai lần xuất hiện của mỗi điểm cuối đều nhận được dấu vân tay giống hệt nhau, do đó hai khoảng thời gian này sẽ hủy bỏ chính xác ở trạng thái tiền tố. Câu trả lời là (1), không có trường hợp đặc biệt nào cho tọa độ âm. 

Đối với đầu vào hoàn toàn bằng kích thước tối đa, mọi khoảng đều có cùng một vectơ điểm cuối. Một mảng con hợp lệ chính xác khi nó chứa một số khoảng chẵn. Với (n=200.000), có (100.000) lựa chọn có độ dài chẵn cho mỗi chẵn lẻ bắt đầu phù hợp, đưa ra 

[ 
100000\cdot100000=10^{10} 
] 

mảng con hợp lệ. Phương thức trạng thái tiền tố đếm chúng thông qua sự xen kẽ lặp đi lặp lại giữa hai trạng thái và kiểu số nguyên của Python lưu trữ câu trả lời một cách an toàn.
