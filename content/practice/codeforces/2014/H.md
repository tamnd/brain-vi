---
title: "CF 2014H - Bắn cung Robin Hood"
description: "Đối với mỗi truy vấn, chúng tôi chỉ xem xét mảng con $al, a{l+1}, dots, ar$. Robin di chuyển đầu tiên. Ở mỗi lượt, người chơi chọn bất kỳ mục tiêu nào còn lại, nhận giá trị của nó và loại bỏ nó."
date: "2026-06-08T13:03:20+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "greedy", "hashing"]
categories: ["algorithms"]
codeforces_contest: 2014
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 974 (Div. 3)"
rating: 1900
weight: 2014
solve_time_s: 122
verified: true
draft: false
---

[CF 2014H - Bắn cung Robin Hood](https://codeforces.com/problemset/problem/2014/H) 

**Xếp hạng:** 1900 
**Tags:** cấu trúc dữ liệu, phân chia để chinh phục, tham lam, băm 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi truy vấn, chúng tôi chỉ nhìn vào mảng con$a_l, a_{l+1}, \dots, a_r$. 

Robin di chuyển đầu tiên. Ở mỗi lượt, người chơi chọn bất kỳ mục tiêu nào còn lại, nhận giá trị của nó và loại bỏ nó. Vì không có hạn chế nào về việc có thể chọn mục tiêu nào nên trò chơi hoàn toàn được xác định bởi nhiều tập hợp giá trị bên trong phân đoạn được truy vấn. 

Cả hai cầu thủ đều chơi hoàn hảo. Đối với mỗi câu hỏi, chúng ta phải xác định xem Cảnh sát trưởng có thể tránh bị thua hay không. Tỷ số hòa được coi là thành công đối với Cảnh sát trưởng, vì câu hỏi hỏi liệu anh ta có **không** thua hay không. 

Tổng số phần tử và truy vấn trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Bất kỳ giải pháp nào xử lý truy vấn theo thời gian tuyến tính sẽ yêu cầu khoảng$4 \cdot 10^{10}$trong trường hợp xấu nhất vượt xa giới hạn. Chúng tôi cần công việc gần như không đổi hoặc logarit cho mỗi truy vấn sau khi xử lý trước. 

Phần lý thuyết trò chơi là cái bẫy đầu tiên. Một mô phỏng đơn giản về cách chơi tối ưu có vẻ phức tạp vì người chơi thay phiên nhau chơi. Trên thực tế, vì mọi mục tiêu đều có sẵn ngay từ đầu nên mỗi người chơi chỉ cần lấy giá trị còn lại lớn nhất hiện tại. Trình tự các giá trị được chọn hoàn toàn được xác định. 

Một sai lầm dễ mắc phải khác là nghĩ rằng chỉ có tổng là quan trọng. Coi như:```
[1, 2]
```Robin lấy 2, Cảnh sát trưởng lấy 1, nên Cảnh sát trưởng thua. 

Tổng số tiền là 3, nhưng riêng điều đó không cho chúng ta biết điều gì. 

Bẫy thứ hai là giả định rằng mọi đoạn có độ dài chẵn đều tạo ra sự hòa.```
[1, 1, 2, 3]
```Robin được$3+1=4$, Cảnh sát trưởng nhận được$2+1=3$. Cảnh sát trưởng thua cuộc. 

Tần số của các giá trị quan trọng. 

Một trường hợp tế nhị hơn là:```
[5, 5, 7]
```Sắp xếp giảm dần:```
7, 5, 5
```Robin được$7+5=12$, Cảnh sát trưởng nhận được$5$. 

Mặc dù giá trị 5 xuất hiện hai lần nhưng giá trị 7 không khớp sẽ quyết định trò chơi. 

Hiểu chính xác thời điểm tồn tại các giá trị chưa từng có là quan sát chính. 

## Phương pháp tiếp cận 

Trước tiên chúng ta hãy hiểu lối chơi tối ưu trông như thế nào. 

Vì mọi mục tiêu còn lại luôn có thể được chọn nên người chơi không bao giờ được hưởng lợi từ việc lấy giá trị nhỏ hơn trong khi vẫn có giá trị lớn hơn. Nếu vẫn còn giá trị lớn hơn, việc lấy nó ngay lập tức chỉ có thể cải thiện điểm số cuối cùng của người chơi đó. 

Kết quả là trò chơi tương đương với việc sắp xếp các giá trị theo thứ tự giảm dần. Robin nhận chức vụ$1,3,5,\dots$và Cảnh sát trưởng nhận chức vụ$2,4,6,\dots$. 

Giả sử chúng ta sắp xếp rõ ràng mọi phân đoạn được truy vấn. Đối với một đoạn có độ dài$m$, chúng ta sẽ sắp xếp các giá trị của nó và tính các tổng xen kẽ. Điều này đúng vì nó phù hợp chính xác với lối chơi tối ưu. 

Vấn đề là sự phức tạp. Một truy vấn có thể chứa$O(n)$các yếu tố, vì vậy chi phí phân loại$O(m \log m)$. Với tối đa$2 \cdot 10^5$truy vấn, điều này là hoàn toàn không thể thực hiện được. 

Sự đột phá đến từ việc nhìn vào tần số thay vì điểm số. 

Lấy bất kỳ giá trị nào$x$. Nếu xuất hiện số lần chẵn thì sau khi sắp xếp các bản sao đó sẽ chiếm các vị trí liên tiếp và chia đều cho hai người chơi. Sự đóng góp của họ bị hủy bỏ. 

Chỉ những giá trị có tần số lẻ mới quan trọng. Sau khi ghép các giá trị bằng nhau với nhau, mỗi giá trị tần số lẻ sẽ để lại chính xác một bản sao chưa ghép đôi. Những bản sao còn sót lại này là những giá trị riêng biệt. Khi được sắp xếp, họ luân phiên giữa Robin và Cảnh sát trưởng. 

Bây giờ hãy quan sát điều gì đó đáng chú ý. Gọi các giá trị phân biệt còn lại là$$b_1 > b_2 > \dots > b_k.$$Robin nhận được$b_1, b_3, b_5,\dots$, Cảnh sát trưởng nhận được$b_2, b_4, b_6,\dots$. 

Vì các giá trị đang giảm dần nên tổng của Robin lớn hơn của Sheriff bất cứ khi nào$k$thật kỳ quặc. Nếu như$k$chẵn, phần còn lại kết hợp hoàn hảo và tổng số bằng nhau. 

Vì vậy, toàn bộ trò chơi rút gọn thành một câu hỏi: 

Phân đoạn có chứa số giá trị lẻ hoặc chẵn có tần số lẻ không? 

Cảnh sát trưởng tránh thua chính xác khi số đó là số chẵn. 

Bây giờ chúng ta cần một cấu trúc dữ liệu để trả lời, đối với bất kỳ mảng con nào, liệu mọi giá trị có xuất hiện với số lần chẵn sau khi hủy hay không. 

Đây là một truy vấn chẵn lẻ cổ điển. Gán mỗi giá trị riêng biệt một hàm băm 64 bit ngẫu nhiên. Cho phép$$pref[i] = h(a_1) \oplus h(a_2) \oplus \dots \oplus h(a_i).$$Đối với một truy vấn$[l,r]$,$$X = pref[r] \oplus pref[l-1].$$Mọi giá trị xuất hiện với số lần chẵn đều bị hủy trong XOR. Kết quả chính xác là XOR của giá trị băm tương ứng với các giá trị có tần số lẻ. 

Nếu không tồn tại giá trị tần số lẻ thì$X=0$. 

Tổng quát hơn, số lượng giá trị tần số lẻ là số chẵn khi và chỉ khi XOR của các giá trị băm của chúng bằng XOR của một tập hợp các giá trị băm có kích thước chẵn. Giải pháp dự định sử dụng hàm băm ngẫu nhiên: gán các số ngẫu nhiên độc lập cho các giá trị. Khi đó XOR chính xác bằng 0 khi tất cả các giá trị tần số lẻ bị hủy và với xác suất áp đảo khác 0. 

Quan sát chính thức thậm chí còn mạnh mẽ hơn. Cảnh sát trưởng không bị mất nếu mọi tần số giá trị trong phân đoạn đều chẵn. Điều kiện đó tương đương với hàm băm XOR phân đoạn bằng 0. 

Xác suất va chạm với các số ngẫu nhiên 64 bit là không đáng kể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m \log m)$mỗi truy vấn |$O(m)$| Quá chậm | 
| Tối ưu |$O(n+q)$mỗi trường hợp thử nghiệm |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Gán một số 64 bit ngẫu nhiên cho mọi giá trị riêng biệt 

Bất cứ khi nào một giá trị xuất hiện lần đầu tiên, hãy tạo một số nguyên 64 bit ngẫu nhiên và lưu trữ nó. 

Các giá trị bằng nhau luôn nhận được cùng một giá trị băm. 

### 2. Xây dựng mảng XOR tiền tố 

Xác định$$pref[0]=0$$Và$$pref[i]=pref[i-1]\oplus h(a_i).$$Điều này cho phép chúng tôi thu được hàm băm XOR của bất kỳ phân đoạn nào trong thời gian không đổi. 

### 3. Xử lý truy vấn 

Đối với phân khúc$[l,r]$, tính toán$$cur=pref[r]\oplus pref[l-1].$$Mỗi giá trị xuất hiện với số lần chẵn sẽ đóng góp hàm băm của nó một số lần chẵn và biến mất. 

Kết quả là XOR của các giá trị băm tương ứng chính xác với các giá trị có tần số bên trong phân đoạn là số lẻ. 

### 4. Quyết định câu trả lời 

Nếu$$cur=0,$$thì mọi tần số giá trị đều là số chẵn, do đó Cảnh sát trưởng có thể tránh bị thua. đầu ra`"YES"`. 

Nếu không thì xuất ra`"NO"`. 

### Tại sao nó hoạt động 

Bất biến quan trọng là XOR theo dõi tính chẵn lẻ. 

Đối với bất kỳ giá trị$x$, hàm băm của nó xuất hiện một lần cho mỗi lần xuất hiện của$x$. Từ$$h(x)\oplus h(x)=0,$$tất cả các lần xuất hiện chẵn đều bị hủy bỏ. Sau khi lấy XOR của một phân đoạn, các giá trị băm duy nhất còn sót lại thuộc về các giá trị có tần số lẻ. 

Nếu mọi tần số đều chẵn thì đoạn XOR bằng 0. Nếu tồn tại ít nhất một giá trị tần số lẻ thì XOR khác 0 ngoại trừ xác suất va chạm không đáng kể từ các giá trị băm ngẫu nhiên. 

Phân tích trò chơi cho thấy Cảnh sát trưởng tránh thua chính xác khi mọi giá trị có thể được ghép với một giá trị tương đương khác. Do đó, câu trả lời truy vấn được xác định hoàn toàn bằng việc hàm băm XOR của phân đoạn có bằng 0 hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MASK = (1 << 64) - 1

def splitmix64(x):
    x = (x + 0x9e3779b97f4a7c15) & MASK
    x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9 & MASK
    x = (x ^ (x >> 27)) * 0x94d049bb133111eb & MASK
    x ^= x >> 31
    return x & MASK

def solve():
    t = int(input())

    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))

        value_hash = {}
        pref = [0] * (n + 1)

        for i, x in enumerate(a, 1):
            if x not in value_hash:
                value_hash[x] = splitmix64(x)

            pref[i] = pref[i - 1] ^ value_hash[x]

        ans = []

        for _ in range(q):
            l, r = map(int, input().split())

            cur = pref[r] ^ pref[l - 1]

            if cur == 0:
                ans.append("YES")
            else:
                ans.append("NO")

        sys.stdout.write("\n".join(ans) + "\n")

solve()
```Phần đầu tiên gán hàm băm 64 bit xác định cho mọi giá trị riêng biệt bằng cách sử dụng`splitmix64`. Việc sử dụng hàm băm xác định sẽ tránh sự phụ thuộc vào trình tạo ngẫu nhiên của Python trong khi vẫn duy trì khả năng chống va chạm cần thiết cho giải pháp. 

Mảng tiền tố lưu trữ các giá trị XOR tích lũy. Vì XOR là nghịch đảo của chính nó nên đoạn XOR có dạng`pref[r] ^ pref[l - 1]`. 

Chi tiết tinh tế duy nhất là lập chỉ mục. Mảng tiền tố được xây dựng dưới dạng cấu trúc dựa trên 1, do đó, ranh giới truy vấn ánh xạ một cách tự nhiên tới công thức phạm vi tiêu chuẩn. 

Số nguyên Python không bị giới hạn, nhưng tất cả các giá trị băm được giữ bên trong 64 bit bằng mặt nạ. Điều này tái tạo số học 64-bit không dấu. 

## Ví dụ đã hoạt động 

### Mẫu 1, Truy vấn [2, 3] 

Mảng:```
[1, 2, 2]
```Phân đoạn:```
[2, 2]
```| Giá trị | Tần số | Số lẻ? | Đóng góp cho XOR | 
| --- | --- | --- | --- | 
| 2 | 2 | Không | Không | 

Hàm băm phân đoạn trở thành 0. 

Đầu ra:```
YES
```Cả hai bản sao của 2 đều hủy bỏ. Mọi tần số đều chẵn nên Cảnh sát trưởng không thể thua. 

### Mẫu 1, Truy vấn [1, 3] 

Phân đoạn:```
[1, 2, 2]
```| Giá trị | Tần số | Số lẻ? | Đóng góp cho XOR | 
| --- | --- | --- | --- | 
| 1 | 1 | Có | Có | 
| 2 | 2 | Không | Không | 

Phân đoạn XOR bằng hàm băm của giá trị 1, khác 0. 

Đầu ra:```
NO
```Giá trị chưa từng có vẫn tồn tại nên điều kiện không thành công. 

### Trường hợp thử nghiệm thứ hai, Truy vấn [4, 5] 

Phân đoạn:```
[1, 1]
```| Giá trị | Tần số | Số lẻ? | Đóng góp cho XOR | 
| --- | --- | --- | --- | 
| 1 | 2 | Không | Không | 

Phân đoạn XOR bằng 0. 

Đầu ra:```
YES
```Điều này thể hiện tính bất biến chẵn lẻ cốt lõi. Các cặp bằng nhau luôn biến mất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + q)$| Một lần để xây dựng tiền tố, công việc liên tục cho mỗi truy vấn | 
| Không gian |$O(n)$| Mảng tiền tố và ánh xạ băm | 

Tổng số tiền của tất cả$n$và tất cả$q$trên đầu vào nhiều nhất là$2 \cdot 10^5$. MỘT$O(n+q)$giải pháp chỉ thực hiện vài trăm nghìn thao tác và dễ dàng phù hợp với thời hạn. Việc sử dụng bộ nhớ là tuyến tính và thoải mái dưới 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    MASK = (1 << 64) - 1

    def splitmix64(x):
        x = (x + 0x9e3779b97f4a7c15) & MASK
        x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9 & MASK
        x = (x ^ (x >> 27)) * 0x94d049bb133111eb & MASK
        x ^= x >> 31
        return x & MASK

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    out = []
    t = int(input())

    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))

        mp = {}
        pref = [0] * (n + 1)

        for i, x in enumerate(a, 1):
            if x not in mp:
                mp[x] = splitmix64(x)
            pref[i] = pref[i - 1] ^ mp[x]

        for _ in range(q):
            l, r = map(int, input().split())
            out.append("YES" if (pref[r] ^ pref[l - 1]) == 0 else "NO")

    return "\n".join(out)

# provided sample
assert run(
"""2
3 3
1 2 2
1 2
1 3
2 3
5 3
2 1 2 1 1
1 2
1 3
4 5
"""
) == "\n".join([
    "NO",
    "NO",
    "YES",
    "NO",
    "NO",
    "YES"
])

# minimum size
assert run(
"""1
1 1
5
1 1
"""
) == "NO"

# all equal values
assert run(
"""1
4 3
7 7 7 7
1 4
1 2
2 3
"""
) == "\n".join(["YES", "YES", "YES"])

# odd frequency survives
assert run(
"""1
3 2
5 5 7
1 3
1 2
"""
) == "\n".join(["NO", "YES"])

# boundary ranges
assert run(
"""1
5 2
1 2 1 2 3
1 4
5 5
"""
) == "\n".join(["YES", "NO"])
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn | KHÔNG | Phân khúc nhỏ nhất có thể | 
| Tất cả các giá trị bằng nhau | CÓ cho mọi phạm vi có kích thước chẵn | Hủy cặp | 
| 5 5 7 | KHÔNG thì CÓ | Người sống sót tần số lẻ | 
| Truy vấn tiền tố và hậu tố | CÓ rồi KHÔNG | Lập chỉ mục ranh giới | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 1
10
1 1
```Phân đoạn này chứa một lần xuất hiện là 10. Hàm băm của nó tồn tại trong phân đoạn XOR, vì vậy câu trả lời là`"NO"`. Thuật toán xác định chính xác rằng vẫn còn một giá trị chưa từng có. 

Coi như:```
1
1 1
4 4 9 9
1 4
```Cả hai giá trị xảy ra hai lần. Đoạn XOR trở thành$$h(4)\oplus h(4)\oplus h(9)\oplus h(9)=0.$$Đầu ra của thuật toán`"YES"`bởi vì mọi tần số đều chẵn. 

Coi như:```
1
1 1
5 5 7
1 3
```Hai bản sao của 5 bị hủy, chỉ để lại giá trị băm của 7. Đoạn XOR khác 0, tạo ra`"NO"`. 

Cuối cùng, hãy xem xét một phạm vi chạm vào ranh giới mảng:```
1
2 1
1 2 1 2
1 4
```Truy vấn sử dụng`pref[4] ^ pref[0]`. Việc xây dựng tiền tố có chủ ý bao gồm`pref[0]=0`, do đó không có cách xử lý đặc biệt nào cho các phạm vi bắt đầu ở vị trí 1. Câu trả lời là`"YES"`bởi vì mọi giá trị xuất hiện hai lần.
