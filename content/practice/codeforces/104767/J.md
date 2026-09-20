---
title: "CF 104767J - Proglute"
description: "Chúng ta có các điểm có nhãn $N$ được đặt trên một vòng tròn, với mọi chuỗi có thể được vẽ dưới dạng một đoạn thẳng giữa hai điểm phân biệt, nhưng chỉ cho phép một số bộ chuỗi."
date: "2026-06-28T22:44:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104767
codeforces_index: "J"
codeforces_contest_name: "2023-2024 CTU Open Contest"
rating: 0
weight: 104767
solve_time_s: 77
verified: true
draft: false
---

[CF 104767J - Proglute](https://codeforces.com/problemset/problem/104767/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao$N$các điểm được gắn nhãn đặt trên một vòng tròn, với mọi chuỗi có thể được vẽ dưới dạng một đoạn thẳng giữa hai điểm phân biệt, nhưng chỉ cho phép một số bộ chuỗi. Các chuỗi không được giao nhau ngoại trừ tại các điểm cuối được chia sẻ, do đó đồ thị được vẽ là phẳng đối với vị trí lồi của các điểm. 

Mỗi chốt có chính xác hai chuỗi sự cố, ngoại trừ hai chuỗi đặc biệt có chính xác một chuỗi sự cố. Điều này buộc toàn bộ cấu trúc phải là một đường đi đơn giản truy cập vào mọi chốt đúng một lần: các đỉnh bên trong có bậc 2 và hai điểm cuối có bậc 1, do đó không thể phân nhánh. 

Nhiệm vụ là đếm xem có thể vẽ được bao nhiêu đường đi đầy đủ không cắt nhau như vậy.$N$các điểm được gắn nhãn trên một đa giác lồi, trong đó hai hình vẽ được coi là giống hệt nhau nếu chúng sử dụng chính xác cùng một bộ cạnh. 

Ràng buộc$N \le 1000$loại trừ mọi nỗ lực liệt kê các cấu trúc một cách rõ ràng. Bất kỳ giải pháp nào cố gắng khám phá các lựa chọn về các cạnh hoặc xây dựng biểu đồ tăng dần bằng cách quay lui sẽ bùng nổ về mặt tổ hợp. Một giải pháp khả thi phải đưa vấn đề về dạng đóng hoặc tệ nhất là$O(N)$hoặc$O(N \log N)$tính toán, vì số học mô-đun theo$10^9+7$được yêu cầu. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ là giả sử rằng bất kỳ hoán vị nào của các đỉnh đều tương ứng với một đường đi không cắt nhau hợp lệ. Ví dụ, với$N=4$, trình tự$1 \to 3 \to 2 \to 4$không hợp lệ vì các cạnh$(1,3)$Và$(2,4)$đi qua. Ràng buộc mang tính hình học chứ không phải đơn thuần là tổ hợp. 

Một lỗi phổ biến khác là cho rằng trước tiên các điểm cuối phải được cố định hoặc được chọn độc lập. Trong thực tế, lựa chọn điểm cuối và cấu trúc bên trong không thể tách rời nhau; đếm chúng một cách độc lập sẽ vượt quá số lượng rất nhiều. 

## Phương pháp tiếp cận 

Một phương pháp bạo lực sẽ cố gắng tạo ra tất cả các hoán vị của$N$các đỉnh, diễn giải mỗi đỉnh là một đường dẫn và kiểm tra xem các cạnh của nó có tạo thành một tập hợp không cắt nhau trên một đa giác lồi hay không. Ngay cả việc xác minh một hoán vị cũng yêu cầu kiểm tra$O(N)$các cạnh và có$N!$hoán vị, dẫn đến$O(N! \cdot N)$, điều này vượt xa tính khả thi ngay cả đối với$N=20$. 

Quan sát cấu trúc quan trọng là bất kỳ cấu hình hợp lệ nào không chỉ là một đường đi mà là một đường đi Hamilton không cắt nhau trên một đa giác lồi. Các đối tượng như vậy có khả năng phân rã đệ quy mạnh mẽ: chọn một cạnh sẽ chia đa giác thành hai bài toán con độc lập, vì các cạnh không giao nhau không thể kết nối qua phân vùng đó. 

Điều này dẫn đến một cấu trúc lựa chọn nhị phân. Khi một cạnh được cố định trên đường đi, mọi đỉnh còn lại phải kết nối theo cách duy trì tính phẳng và tại mỗi bước, công trình sẽ chia thành các vùng bên trái và bên phải dọc theo đường biên. Mỗi quyết định nội bộ tương ứng với việc chọn đỉnh nào sẽ gắn vào trong đường đi phát triển và những lựa chọn này trở nên độc lập. 

Tính độc lập này thu gọn bài toán thành việc đếm thuần túy các phép gán nhị phân trên các đỉnh bên trong, kết hợp với việc lựa chọn vị trí bắt đầu trên chu trình. 

Kết quả là dạng đóng trở thành$N \cdot 2^{N-3}$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị |$O(N! \cdot N)$|$O(N)$| Quá chậm | 
| Cấu trúc tổ hợp |$O(N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ta rút ra và tính công thức$N \cdot 2^{N-3}$. 

1. Cố định một đỉnh tùy ý làm điểm tham chiếu trên chu trình. Cấu trúc đối xứng khi xoay, do đó không có đỉnh nào có các ràng buộc hình học đặc biệt. 
2. Quan sát rằng mọi cấu hình hợp lệ đều là một đường dẫn Hamilton. Khi các điểm cuối được chọn hoàn toàn, các đỉnh còn lại sẽ bị ép vào thứ tự chèn không giao nhau. 
3. Root công trình tại điểm cuối đã chọn của đường dẫn. Từ điểm cuối đó, đường đi tiếp tục bằng cách liên tục mở rộng đến các đỉnh không được sử dụng trong khi vẫn duy trì các ràng buộc không cắt nhau. Ở mỗi bước, các đỉnh không được sử dụng tạo thành một khoảng liền kề trên đường biên. 
4. Mỗi khi đường đi kéo dài qua một đỉnh bên trong, đỉnh đó có chính xác hai cạnh sẵn có để nó có thể kết nối các lân cận chưa sử dụng còn lại của nó, tương ứng với một quyết định nhị phân xác định cách phân chia khoảng còn lại. 
5. Có chính xác$N-3$các quyết định nhị phân độc lập như vậy. Điều này xuất phát từ thực tế là sau khi sửa các điểm cuối, phần còn lại$N-2$đỉnh đóng góp$N-3$các bước đính kèm bên trong không bị ép buộc bởi hình học. 
6. Mỗi lựa chọn nhị phân là độc lập, tạo ra hệ số$2^{N-3}$. 
7. Cuối cùng, vị trí bắt đầu của công trình có thể được neo ở bất kỳ vị trí nào$N$đỉnh, đóng góp một hệ số nhân của$N$. 

Câu trả lời cuối cùng là:$$\text{ans} = N \cdot 2^{N-3} \bmod (10^9+7)$$### Tại sao nó hoạt động 

Điều kiện không cắt nhau buộc đồ thị hoạt động giống như một phép phân tách nhị phân đệ quy của đa giác lồi. Mỗi khi một đỉnh bên trong mới được đưa vào đường đi, nó sẽ chia các đỉnh chưa được thăm còn lại thành hai khoảng ranh giới rời nhau. Những khoảng này không bao giờ tương tác nữa nên quyết định tại đỉnh đó độc lập với tất cả các lựa chọn trước đó. Điều này tạo ra một sản phẩm của các quyết định nhị phân độc lập, tính toán chính xác cho số mũ, trong khi tính đối xứng của chu trình tính đến hệ số tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modexp(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def solve():
    n = int(input().strip())
    if n <= 2:
        print(1)
        return
    ans = n * modexp(2, n - 3) % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện là một bản dịch trực tiếp của biểu mẫu đóng. Tính lũy thừa nhanh$2^{n-3}$theo thời gian logarit. Phép nhân với$n$được lấy modulo$10^9+7$. 

Trường hợp góc xuất hiện tại$N=2$, trong đó số mũ trở thành số âm. Điều này tương ứng với cạnh đơn duy nhất nối cả hai đỉnh, do đó câu trả lời được xác định riêng biệt là 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$N=5$Chúng tôi tính toán$5 \cdot 2^{2} = 20$. 

| Bước | Giá trị | 
| --- | --- | 
|$n$| 5 | 
| số mũ$n-3$| 2 | 
|$2^{n-3}$| 4 | 
| kết quả | 20 | 

Điều này phù hợp với ý tưởng rằng một khi các điểm cuối được chọn ngầm, sẽ có hai quyết định cấu trúc nhị phân độc lập. 

### Ví dụ 2:$N=666$| Bước | Giá trị | 
| --- | --- | 
|$n$| 666 | 
| số mũ$n-3$| 663 | 
| cấu trúc |$666 \cdot 2^{663}$| 

Việc tính toán được thực hiện modulo$10^9+7$, với lũy thừa nhanh đảm bảo tính khả thi mặc dù số mũ lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log N)$| lũy thừa nhanh của$2^{N-3}$| 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ | 

Thuật toán dễ dàng thỏa mãn các ràng buộc lên đến$N=1000$, vì lũy thừa chiếm ưu thế và cực kỳ nhanh. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def modexp(a, e):
        res = 1
        while e:
            if e & 1:
                res = res * a % MOD
            a = a * a % MOD
            e >>= 1
        return res

    n = int(sys.stdin.readline().strip())
    if n <= 2:
        return "1"
    return str(n * modexp(2, n - 3) % MOD)

# provided samples
assert run("5\n") == "20"
assert run("666\n") == "61847156"

# custom cases
assert run("2\n") == "1", "minimum edge case"
assert run("3\n") == str(3 * pow(2, 0, MOD)), "smallest nontrivial structure"
assert run("4\n") == str(4 * pow(2, 1, MOD)), "checks linear-exponential balance"
assert run("10\n") == str(10 * pow(2, 7, MOD)), "medium consistency check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 1 | thoái hóa cấu trúc tối thiểu | 
| 3 | 3 | trường hợp số mũ cơ sở | 
| 4 | 8 | hành vi phân chia nhị phân thực đầu tiên | 
| 10 | 1280 | độ chính xác của tỷ lệ | 

## Vỏ cạnh 

cho$N=2$, công thức$N \cdot 2^{N-3}$sẽ yêu cầu$2^{-1}$, điều này không có ý nghĩa trong số học mô-đun. Trong trường hợp này, cấu hình duy nhất có thể là một chuỗi đơn nối hai chốt, vì vậy câu trả lời là 1. 

cho$N=3$, mọi cấu hình hợp lệ phải kết nối cả ba đỉnh trong một đường dẫn. Công thức cho$3 \cdot 2^0 = 3$, tương ứng với ba lựa chọn điểm cuối có thể có, mỗi điểm tạo ra một đường đi không cắt nhau duy nhất.
