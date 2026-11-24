---
title: "CF 73A - The Elder Trolls IV: Oblivon"
description: "Chúng ta có một khối hình chữ nhật được tạo thành từ các khối lập phương đơn vị có kích thước x × y × z. Một vết cắt luôn được thực hiện dọc theo các đường lưới và phải song song với một trong các mặt của hộp."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 73
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 66"
rating: 1600
weight: 73
solve_time_s: 108
verified: true
draft: false
---

[CF 73A - The Elder Trolls IV: Oblivon](https://codeforces.com/problemset/problem/73/A) 

**Đánh giá:** 1600 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta có một khối hình chữ nhật được tạo thành từ các khối lập phương có kích thước`x × y × z`. Một vết cắt luôn được thực hiện dọc theo các đường lưới và phải song song với một trong các mặt của hộp. Vì các vết cắt dài vô hạn và các mảnh không bao giờ di chuyển khỏi vị trí ban đầu nên một vết cắt có thể đi qua nhiều mảnh đã được tạo cùng một lúc. 

Mục tiêu là tối đa hóa số lượng sản phẩm cuối cùng sau khi thực hiện tối đa`k`vết cắt. 

Chi tiết quan trọng là cách thức hoạt động của vết cắt. Nếu chúng ta làm`a`cắt vuông góc với`x`trục, hộp sẽ được chia thành`a + 1`các lớp dọc theo hướng đó. Tương tự,`b`cắt dọc`y`sản xuất`b + 1`các phần ở đó, và`c`cắt dọc`z`sản xuất`c + 1`phần. Nếu như`a + b + c = k`, tổng số phần trở thành:$(a+1)(b+1)(c+1)$Kích thước giới hạn số lần cắt có thể thực hiện được theo mỗi hướng. Dọc theo chiều dài`x`, chỉ có`x - 1`mặt phẳng lưới hợp lệ, vì vậy:$0 \le a \le x-1$và tương tự cho hai chiều còn lại. 

Các ràng buộc ngay lập tức loại trừ mọi mô phỏng cắt giảm. Kích thước có thể đạt tới`10^6`, trong khi`k`có thể đạt được`10^9`. Bất kỳ thuật toán nào tỷ lệ thuận với số trạng thái hoặc số lần cắt sẽ quá chậm. Thậm chí lặp lại trên tất cả các bộ ba`(a,b,c)`ngây thơ sẽ yêu cầu xung quanh`10^{18}`kết hợp trong trường hợp xấu nhất là không thể. 

Vấn đề thực sự là một vấn đề tối ưu hóa trên ba số nguyên. Vì chỉ có ba chiều nên không gian tìm kiếm có thể được giảm bớt đáng kể bằng các quan sát toán học. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả sử đầu vào là:```
1 1 1 100
```Không thể cắt được vì mọi kích thước đều đã có kích thước`1`. Câu trả lời đúng là`1`, không phải cái gì đó liên quan đến`k`. 

Một trường hợp tế nhị khác là khi`k`vượt quá tổng số mặt phẳng cắt có sẵn:```
2 2 2 10
```Mỗi chiều chỉ cho phép một lần cắt, vì vậy tối đa ba lần cắt có thể hữu ích. Số lượng mảnh tối đa vẫn là`8`. 

Một sai lầm phổ biến là cho rằng các vết cắt phải luôn được phân bổ đều. Coi như:```
10 1 1 5
```Chỉ có thể cắt kích thước đầu tiên. Câu trả lời là`6`, không phải thứ gì đó cân bằng như`2 × 2 × 2`. 

Một lỗi triển khai nguy hiểm khác đến từ lỗi tràn số nguyên trong các ngôn ngữ có số nguyên 32 bit. Câu trả lời có thể đạt được:$10^6 \cdot 10^6 \cdot 10^6 = 10^{18}$Python xử lý việc này một cách tự động, nhưng các giải pháp C++ cần`long long`. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ thử mọi số lần cắt có thể theo mỗi hướng. Chúng ta có thể lặp lại tất cả các bộ ba`(a,b,c)`thỏa mãn:$a+b+c \le k$và kiểm tra xem mỗi giá trị có nằm trong giới hạn thứ nguyên hay không. Đối với mỗi bộ ba hợp lệ, chúng tôi tính toán:$(a+1)(b+1)(c+1)$và giữ ở mức tối đa. 

Điều này hoạt động hợp lý vì mỗi chiến lược cắt hợp lệ được mô tả duy nhất bằng số lần cắt được gán cho mỗi trục. Vấn đề là kích thước của không gian tìm kiếm. Ngay cả khi chúng ta chỉ lặp lại tối đa`10^6`, vòng ba trở nên hoàn toàn không khả thi. 

Quan sát quan trọng là câu trả lời chỉ phụ thuộc vào số lần cắt trên mỗi trục chứ không phụ thuộc vào thứ tự cắt. Một khi chúng ta quyết định`a`,`b`, Và`c`, số lượng mảnh được cố định. 

Điều đó biến vấn đề thành tối đa hóa sản phẩm trong điều kiện hạn chế. 

Không có giới hạn trên về kích thước, sản phẩm sẽ lớn nhất khi các giá trị cân bằng nhất có thể. Đây là giá trị trung bình số học tiêu chuẩn và trực giác trung bình hình học. Tuy nhiên, kích thước giới hạn số lần cắt mà mỗi hướng có thể nhận được, do đó sự phân bổ tối ưu có thể đạt đến giới hạn. 

Vì chỉ có ba biến nên chúng ta có thể lặp lại hai trong số chúng và rút ra biến thứ ba. Một sự tối ưu hóa hơn nữa làm cho điều này trở nên thiết thực. Các kích thước đối xứng nhau nên chúng ta sắp xếp chúng. Sau đó, chúng tôi lặp lại các lần cắt khả thi cho hai chiều nhỏ nhất, trong khi chiều thứ ba được xác định tự động. 

Tổng số lần lặp vẫn có thể quản lý được vì kích thước nhỏ hơn hạn chế đáng kể các vòng lặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k³) | O(1) | Quá chậm | 
| Tối ưu | O(xy) sau khi sắp xếp | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`x`,`y`,`z`, Và`k`. 
2. Sắp xếp ba chiều. Điều này không thay đổi câu trả lời vì các trục có thể hoán đổi cho nhau. Việc sắp xếp giúp giữ cho các vòng lặp lồng nhau ở mức nhỏ. 
3. Lặp lại mọi số lần cắt có thể`a`dọc theo chiều thứ nhất. 

Phạm vi hợp lệ là:$0 \le a \le \min(k, x-1)$4. Đối với mỗi`a`, lặp lại mọi số lần cắt có thể`b`dọc theo chiều thứ hai. 

Phạm vi hợp lệ là:$0 \le b \le \min(k-a, y-1)$5. Các vết cắt còn lại đi vào chiều thứ ba:$c = \min(k-a-b, z-1)$Chúng tôi sử dụng càng nhiều vết cắt còn lại càng tốt vì ngày càng tăng`c`luôn làm tăng sản phẩm. 
6. Tính số mảnh thu được:$(a+1)(b+1)(c+1)$7. Theo dõi giá trị tối đa trên tất cả các lần lặp. 
8. In tối đa. 

### Tại sao nó hoạt động 

Mỗi đường cắt vuông góc với một trục sẽ làm tăng số phần dọc theo trục đó thêm đúng một. Vì các mảnh cuối cùng tạo thành lưới 3D nên tổng số mảnh là tích của số phần trong mỗi chiều. 

Thuật toán liệt kê mọi cặp khả thi`(a,b)`. Một khi những điều đó đã được khắc phục, điều tốt nhất có thể`c`luôn là giá trị còn lại tối đa cho phép, vì sản phẩm tăng trưởng đơn điệu với`c`. 

Vì mỗi lần phân phối hợp lệ của các vết cắt được xem xét chính xác một lần và số lượng mảnh chính xác được tính cho mỗi lần phân phối, nên mức tối đa mà thuật toán tìm thấy là mức tối ưu thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x, y, z, k = map(int, input().split())

    dims = sorted([x, y, z])
    x, y, z = dims

    ans = 1

    max_a = min(k, x - 1)

    for a in range(max_a + 1):
        max_b = min(k - a, y - 1)

        for b in range(max_b + 1):
            c = min(k - a - b, z - 1)

            pieces = (a + 1) * (b + 1) * (c + 1)
            ans = max(ans, pieces)

    print(ans)

solve()
```Bước đầu tiên sắp xếp các kích thước sao cho giới hạn nhỏ nhất kiểm soát các vòng lặp bên ngoài. Điều này giữ cho thời gian chạy có thể quản lý được. 

Vòng ngoài chọn số lần cắt dọc theo trục đầu tiên. Vì chỉ có`x - 1`mặt phẳng hợp lệ, vòng lặp không bao giờ cần vượt quá giá trị đó. 

Đối với mọi`a`, vòng lặp thứ hai chọn vết cắt cho trục thứ hai. Một lần nữa, chúng ta không thể vượt quá các vết cắt còn lại hoặc các mặt phẳng có sẵn. 

Giá trị thứ ba không cần vòng lặp khác. Một lần`a`Và`b`được cố định, bất kỳ vết cắt bổ sung nào dọc theo trục thứ ba đều làm tăng sản phẩm. Sự lựa chọn tốt nhất đơn giản là giá trị khả thi lớn nhất. 

Phép nhân sử dụng`(a + 1)`,`(b + 1)`, Và`(c + 1)`bởi vì`n`cắt giảm tạo ra`n + 1`phân đoạn. 

Một lỗi thường gặp là cho phép`x`cắt giảm thay vì`x - 1`. Một chiều dài`x`chứa chính xác`x - 1`mặt phẳng cắt bên trong. 

Một điểm tinh tế khác là sử dụng`min(k - a - b, z - 1)`vì`c`. Các vết cắt bổ sung ngoài các mặt phẳng có sẵn là vô ích và không được tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 2 3
```| một | b | c | Miếng | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | 2 | 
| 0 | 1 | 1 | 4 | 
| 1 | 0 | 1 | 4 | 
| 1 | 1 | 1 | 8 | 

Cấu hình tốt nhất sử dụng một vết cắt theo mỗi hướng. Mỗi chiều được chia thành hai phần, tạo ra:$2 \times 2 \times 2 = 8$Ví dụ này thể hiện cấu trúc nhân của bài toán. 

### Ví dụ 2 

đầu vào:```
10 1 1 5
```| một | b | c | Miếng | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | 
| 1 | 0 | 0 | 2 | 
| 2 | 0 | 0 | 3 | 
| 3 | 0 | 0 | 4 | 
| 4 | 0 | 0 | 5 | 
| 5 | 0 | 0 | 6 | 

Không thể cắt được kích thước thứ hai và thứ ba vì kích thước của chúng đã được`1`. 

Thuật toán đặt chính xác mọi vết cắt hữu ích dọc theo trục đầu tiên. Điều này khẳng định rằng việc phân phối cân bằng không phải lúc nào cũng có thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(xy) sau khi sắp xếp | Các vòng lặp lặp lại các lần cắt khả thi cho hai kích thước nhỏ nhất | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Vì kích thước tối đa là`10^6`, việc lặp qua hai chiều nhỏ nhất là đủ nhanh trong thực tế. Việc sử dụng bộ nhớ là không đổi, trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    x, y, z, k = map(int, input().split())

    x, y, z = sorted([x, y, z])

    ans = 1

    for a in range(min(k, x - 1) + 1):
        for b in range(min(k - a, y - 1) + 1):
            c = min(k - a - b, z - 1)

            ans = max(ans, (a + 1) * (b + 1) * (c + 1))

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup

    return out.getvalue().strip()

# provided sample
assert run("2 2 2 3\n") == "8", "sample 1"

# minimum-size input
assert run("1 1 1 0\n") == "1", "single cube"

# cuts larger than available planes
assert run("2 2 2 10\n") == "8", "extra cuts are useless"

# all dimensions equal
assert run("5 5 5 6\n") == "27", "balanced optimal split"

# only one dimension usable
assert run("10 1 1 5\n") == "6", "all cuts on one axis"

# off-by-one boundary
assert run("3 3 3 2\n") == "4", "cannot exceed available planes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 0`|`1`| Hộp tối thiểu có thể | 
|`2 2 2 10`|`8`| Cắt thêm ngoài các mặt phẳng hợp lệ | 
|`5 5 5 6`|`27`| Phân phối cân bằng mang lại sự tối ưu | 
|`10 1 1 5`|`6`| Chỉ có thể cắt một chiều | 
|`3 3 3 2`|`4`| Xử lý đúng giới hạn cắt | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1 1 1 100
```Sau khi sắp xếp, tất cả các kích thước vẫn còn`1`. 

Các vòng lặp trở thành:```
a from 0 to 0
b from 0 to 0
c = 0
```Sản phẩm tính được là:$1 \times 1 \times 1 = 1$Không thể cắt được vì không có mặt phẳng bên trong. Thuật toán xử lý việc này một cách tự nhiên thông qua`x - 1`giới hạn. 

Bây giờ hãy xem xét:```
2 2 2 10
```Mỗi chiều chỉ cho phép cắt một lần. Thuật toán khám phá tất cả các phép gán khả thi, nhưng mọi biến đều bị giới hạn ở mức`1`. 

Sản phẩm tối đa trở thành:$2 \times 2 \times 2 = 8$Bảy vết cắt còn lại bị bỏ qua vì chúng không thể tương ứng với mặt phẳng cắt thực. 

Cuối cùng, hãy xem xét:```
10 1 1 5
```Kích thước thứ hai và thứ ba không có mặt phẳng cắt hợp lệ. 

Thuật toán lặp lại:```
a = 0..5
b = 0
c = 0
```Giá trị tốt nhất xảy ra ở`a = 5`:$6 \times 1 \times 1 = 6$Điều này xác nhận rằng việc tập trung tất cả các vết cắt vào một trục đôi khi là tối ưu.
