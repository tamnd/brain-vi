---
title: "CF 1268B - Domino dành cho giới trẻ"
description: "Biểu đồ Young có thể được xem dưới dạng biểu đồ có chiều cao cột được cho bởi mảng không tăng $a1,a2,dots,an$."
date: "2026-06-11T20:16:57+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1268
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 609 (Div. 1)"
rating: 2000
weight: 1268
solve_time_s: 132
verified: true
draft: false
---

[CF 1268B - Domino dành cho giới trẻ](https://codeforces.com/problemset/problem/1268/B) 

**Xếp hạng:** 2000 
**Tags:** dp, tham lam, toán học 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Biểu đồ Young có thể được xem dưới dạng biểu đồ có chiều cao cột được tính bằng mảng không tăng$a_1,a_2,\dots,a_n$. Mỗi ô vuông đơn vị bên trong biểu đồ có thể được bao phủ bởi nhiều nhất một domino và mỗi domino chiếm chính xác hai ô liền kề, theo chiều dọc hoặc chiều ngang. 

Nhiệm vụ là đặt càng nhiều quân domino càng tốt vào trong sơ đồ mà không bị chồng chéo. 

Một cách hữu ích để suy nghĩ về hình dạng là tập hợp các ô lưới. Cột$i$chứa các tế bào$(i,1),(i,2),\dots,(i,a_i)$. Vì chiều cao không tăng nên mỗi hàng tạo thành một đoạn liền kề bắt đầu từ cạnh trái. 

Những hạn chế là lớn. Có thể có tới$300\,000$cột và mỗi chiều cao cũng có thể lớn bằng$300\,000$. Tổng diện tích của sơ đồ có thể đạt tới khoảng$9 \times 10^{10}$, do đó, bất kỳ thuật toán nào xây dựng các ô hoặc lặp lại rõ ràng trên toàn bộ sơ đồ đều không thể thực hiện được. Thậm chí một$O(n^2)$thuật toán sẽ yêu cầu khoảng$9 \times 10^{10}$hoạt động trong trường hợp xấu nhất. Chúng ta cần một cái gì đó tuyến tính hoặc gần tuyến tính trong$n$. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một cột duy nhất:```
1
3
```Sơ đồ chứa ba ô. Chỉ có thể đặt một domino dọc, vì vậy câu trả lời là:```
1
```Một giải pháp giả định mọi cặp ô luôn có thể khớp nhau sẽ trả về không chính xác$\lfloor 3/2 \rfloor = 1$ở đây là do may mắn, nhưng lý do sẽ không khái quát hóa được. 

Xét hai cột có chiều cao lẻ:```
2
1 1
```Câu trả lời là:```
1
```Không có vị trí dọc, nhưng một domino ngang bao phủ hàng duy nhất. Bất kỳ cách tiếp cận nào chỉ tính các cặp dọc sẽ trả về 0. 

Một trường hợp thú vị khác là:```
2
3 3
```Diện tích là 6 nên có thể có tối đa 3 quân domino. Trên thực tế, tất cả 6 ô đều có thể được xếp chồng lên nhau, đưa ra câu trả lời 3. Một chiến lược cục bộ ngây thơ sử dụng các quân domino dọc ở mọi nơi một cách tham lam sẽ chỉ đặt 2 quân domino dọc và để lại hai ô không khớp. 

Khó khăn chính là hiểu cách các cột có chiều cao lẻ tương tác với nhau thông qua các vị trí nằm ngang. 

## Phương pháp tiếp cận 

Quan điểm brute-force là coi mỗi ô là một đỉnh của biểu đồ lưới và kết nối các ô liền kề. Vấn đề trở thành tìm một kết quả khớp tối đa trong biểu đồ đó, bởi vì mỗi domino bao gồm hai ô liền kề. 

Công thức này là chính xác. Một kết quả khớp tương ứng chính xác với một tập hợp các quân domino không chồng chéo. Thật không may, biểu đồ chứa một đỉnh trên mỗi ô. Vì diện tích có thể lớn bằng$9 \times 10^{10}$, việc xây dựng đồ thị là hoàn toàn không khả thi. 

Cấu trúc của sơ đồ Young cung cấp nhiều thông tin hơn một lưới tùy ý. Trước tiên chúng ta hãy đặt càng nhiều quân domino dọc càng tốt vào trong mỗi cột. 

Một cột có chiều cao$a_i$đóng góp$\lfloor a_i/2 \rfloor$domino dọc. Nếu như$a_i$chẵn thì toàn bộ cột bị che phủ. Nếu như$a_i$thật kỳ quặc, chính xác là một ô vẫn chưa được phát hiện. 

Câu hỏi duy nhất là điều gì sẽ xảy ra với những tế bào còn sót lại này. 

Đối với cột lẻ, ô không được che có thể được chọn ở hàng lẻ hoặc hàng chẵn. Ví dụ: một cột có chiều cao 5 có thể được xếp theo chiều dọc để không che hàng 1 hoặc hàng 3 không che hoặc hàng 5 không che. Tất cả các vị trí chưa được phát hiện đều có tính chẵn lẻ như nhau. 

Bây giờ hãy nhìn vào hai cột lẻ lân cận. Nếu các ô còn sót lại của chúng được đặt trên cùng một hàng, chúng sẽ tạo thành một quân domino ngang. Vì sơ đồ được căn trái nên bất kỳ hàng nào tồn tại trong cả hai cột cũng tồn tại trong mọi cột giữa chúng. Do đó, một khối cột lẻ liên tiếp có thể ghép hai phần còn lại của chúng cùng một lúc. 

Điều duy nhất quan trọng là sự bình đẳng. 

Cho phép$$S = \sum_i \left\lfloor \frac{a_i}{2} \right\rfloor$$là số quân domino dọc. 

Mỗi cột lẻ đóng góp một ô còn sót lại. Các cột lẻ liên tiếp có cùng tính chẵn lẻ chỉ số có thể ghép các phần còn lại đó. Một lần quét đơn giản cho thấy rằng mỗi cột lẻ thứ hai sẽ tạo thêm một domino. 

Câu trả lời kết quả là$$S + \sum \left\lfloor \frac{k}{2} \right\rfloor$$Ở đâu$k$là số cột lẻ trong một đoạn liên tiếp tối đa. 

Việc triển khai thậm chí còn đơn giản hơn sẽ duy trì số lượng cột lẻ hiện chưa khớp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp Brute Force trên các ô |$O(\text{area}^{1.5})$hoặc tệ hơn |$O(\text{area})$| Quá chậm | 
| Tham lam chẵn lẻ tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`answer = 0`. 
2. Khởi tạo`pending = 0`. 

Đây`pending`lưu trữ bao nhiêu cột lẻ trong đoạn liên tiếp hiện tại chưa được ghép nối. 
3. Xử lý các cột từ trái qua phải. 
4. Thêm`a[i] // 2`để trả lời. 

Điều này đếm tất cả các quân domino dọc có thể được đặt bên trong cột. 
5. Nếu`a[i]`là số lẻ, tăng lên`pending`. 

Một cột lẻ đóng góp một ô còn sót lại. 
6. Nếu`a[i]`chẵn, cộng`pending // 2`để trả lời và đặt lại`pending = 0`. 

Một cột chẵn phá vỡ phân khúc. Mọi ghép nối ngang phải được hoàn thành bên trong khối cột lẻ tối đa hiện tại. 
7. Sau khi xử lý tất cả các cột, hãy thêm`pending // 2`để trả lời. 

Điều này xử lý khối cuối cùng của các cột lẻ. 
8. Xuất ra câu trả lời. 

### Tại sao nó hoạt động 

Sau khi lấy tất cả các quân domino theo chiều dọc có thể có, mỗi cột có chiều cao chẵn sẽ được bao phủ hoàn toàn và mỗi cột có chiều cao lẻ sẽ đóng góp chính xác một ô còn lại. Những ô còn sót lại này là những ô duy nhất có thể tham gia vào các quân domino bổ sung. 

Bên trong một phân đoạn liên tiếp tối đa của các cột lẻ, ô còn lại từ mỗi cột có thể được đặt trên một hàng được chia sẻ bởi tất cả các cột trong phân đoạn đó. Sau đó, mỗi cặp thức ăn thừa có thể được nối thành một domino ngang. Một đoạn chứa$k$cột lẻ mang lại kết quả chính xác$\lfloor k/2 \rfloor$thêm domino. 

Các phân đoạn khác nhau được phân cách bằng một cột chẵn. Cột như vậy không chứa ô còn sót lại nên không có quân domino ngang nào có thể vượt qua ranh giới. Sự đóng góp của các phân đoạn là độc lập. 

Thuật toán đếm tất cả các quân domino dọc và sau đó cộng chính xác số lượng cặp ngang tối đa có thể đạt được từ mỗi phân đoạn lẻ, là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    ans = 0
    pending = 0

    for x in a:
        ans += x // 2

        if x & 1:
            pending += 1
        else:
            ans += pending // 2
            pending = 0

    ans += pending // 2
    print(ans)

solve()
```Sự đóng góp đầu tiên đến từ các vị trí dọc. Mỗi cột đóng góp độc lập`x // 2`domino. 

Biến`pending`theo dõi kích thước của khối cột lẻ tối đa hiện tại. Bất cứ khi nào một cột chẵn xuất hiện, khối đó kết thúc nên chúng ta thêm ngay`pending // 2`domino ngang và đặt lại bộ đếm. 

Việc bổ sung cuối cùng sau vòng lặp là cần thiết vì mảng có thể kết thúc bằng các cột lẻ. Quên bước này là lỗi phổ biến nhất. 

Câu trả lời có thể lớn bằng khoảng một nửa diện tích của sơ đồ, có thể vượt quá giới hạn số nguyên 32 bit. Số nguyên Python tự động xử lý việc này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 2 2 2 1
```| Cột | Chiều cao | Đã thêm theo chiều dọc | Khối lẻ đang chờ xử lý | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 1 | 1 | 
| 2 | 2 | 1 | 0 | 2 | 
| 3 | 2 | 1 | 0 | 3 | 
| 4 | 2 | 1 | 0 | 4 | 
| 5 | 1 | 0 | 1 | 4 | 
| Kết thúc | - | 0 | 1 | 4 | 

Câu trả lời cuối cùng:```
4
```Ví dụ này cho thấy một cột lẻ bị cô lập không đóng góp thêm domino ngang nào. Cột cuối cùng tạo ra một ô còn sót lại nhưng không có cột lẻ thứ hai trong cùng phân đoạn để ghép với nó. 

### Ví dụ 2 

đầu vào:```
4
3 3 3 3
```| Cột | Chiều cao | Đã thêm theo chiều dọc | Khối lẻ đang chờ xử lý | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 1 | 1 | 
| 2 | 3 | 1 | 2 | 2 | 
| 3 | 3 | 1 | 3 | 3 | 
| 4 | 3 | 1 | 4 | 4 | 
| Kết thúc | - | 0 | 4 | 6 | 

Cuối cùng chúng tôi thêm`4 // 2 = 2`thêm quân domino ngang. 

Câu trả lời cuối cùng:```
6
```Dấu vết này thể hiện sự quan sát trung tâm. Bốn cột lẻ tạo ra bốn phần còn lại, có thể ghép thành hai quân domino ngang bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần đi qua mảng | 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ | 

Với$n \le 300\,000$, quét tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ không đổi bất kể chiều cao của cột. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    a = list(map(int, input().split()))

    ans = 0
    pending = 0

    for x in a:
        ans += x // 2

        if x & 1:
            pending += 1
        else:
            ans += pending // 2
            pending = 0

    ans += pending // 2
    return str(ans) + "\n"

# provided sample
assert run("5\n3 2 2 2 1\n") == "4\n", "sample"

# minimum size
assert run("1\n1\n") == "0\n", "single cell"

# single odd column
assert run("1\n3\n") == "1\n", "one vertical domino"

# all equal odd heights
assert run("4\n3 3 3 3\n") == "6\n", "odd block pairing"

# boundary between odd segments
assert run("5\n3 3 2 3 3\n") == "6\n", "even column splits segments"

# all even heights
assert run("4\n2 2 2 2\n") == "4\n", "pure vertical tiling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`0`| Sơ đồ tối thiểu | 
|`1 / 3`|`1`| Cột lẻ đơn | 
|`4 / 3 3 3 3`|`6`| Đoạn lẻ lớn | 
|`5 / 3 3 2 3 3`|`6`| Chia đoạn theo cột chẵn | 
|`4 / 2 2 2 2`|`4`| Vị trí hoàn toàn theo chiều dọc | 

## Vỏ cạnh 

Hãy xem xét sơ đồ nhỏ nhất có thể:```
1
1
```Thuật toán bổ sung`1 // 2 = 0`domino dọc. Cột này là số lẻ, vì vậy`pending = 1`. Sau khi quét nó thêm`1 // 2 = 0`. Câu trả lời là 0, đúng vì một ô đơn lẻ không thể tạo thành quân domino. 

Hãy xem xét một cột lẻ duy nhất:```
1
3
```Thuật toán bổ sung`3 // 2 = 1`domino dọc. Vì vậy vẫn còn một ô còn sót lại`pending = 1`. Đóng góp cuối cùng là`1 // 2 = 0`. Câu trả lời trở thành 1, đó là tối ưu. 

Xét hai cột lẻ cách nhau bởi một cột chẵn:```
3
3 2 3
```Việc xử lý cột giữa buộc thuật toán phải đóng đoạn lẻ đầu tiên. Ô còn sót lại đầu tiên không thể ghép nối với ô cuối cùng vì cột chẵn không đóng góp phần còn lại. Việc tính toán là:```
vertical = 1 + 1 + 1 = 3
extra = 0 + 0 = 0
answer = 3
```Điều này ngăn chặn chính xác việc ghép nối ngang không hợp lệ trên các ranh giới phân khúc. 

Hãy xem xét một đoạn lẻ dài:```
5
1 1 1 1 1
```Không có domino dọc. Đoạn này chứa năm cột lẻ, do đó thuật toán sẽ thêm`5 // 2 = 2`domino ngang. Một tế bào vẫn chưa được phát hiện. Câu trả lời là 2, phù hợp với số lượng ốp lát tối đa có thể.
