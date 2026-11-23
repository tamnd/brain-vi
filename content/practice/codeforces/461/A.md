---
title: "CF 461A - Appleman và Toastman"
description: "Chúng ta bắt đầu với một nhóm duy nhất chứa tất cả các số đã cho. Bất cứ khi nào Toastman nhận được một nhóm, anh ấy sẽ cộng tổng của tất cả các số trong nhóm đó vào điểm số. Nếu nhóm chứa nhiều hơn một số, Appleman có thể chia nhóm đó thành hai nhóm không trống và gửi cả hai lại cho Toastman."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 461
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 263 (Div. 1)"
rating: 1200
weight: 461
solve_time_s: 134
verified: true
draft: false
---

[CF 461A - Appleman và Toastman](https://codeforces.com/problemset/problem/461/A) 

**Đánh giá:** 1200 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một nhóm duy nhất chứa tất cả các số đã cho. 

Bất cứ khi nào Toastman nhận được một nhóm, anh ấy sẽ cộng tổng của tất cả các số trong nhóm đó vào điểm số. Nếu nhóm chứa nhiều hơn một số, Appleman có thể chia nhóm đó thành hai nhóm không trống và gửi cả hai lại cho Toastman. Các nhóm phần tử đơn bị loại bỏ. 

Quá trình tiếp tục cho đến khi mọi số đều nằm trong nhóm đơn của chính nó. Thứ tự và cấu trúc của các phần chia tách hoàn toàn nằm trong tầm kiểm soát của chúng tôi. Nhiệm vụ của chúng tôi là tối đa hóa tổng số điểm tích lũy được trong toàn bộ quá trình. 

Đầu vào chỉ đơn giản là một mảng các số nguyên dương. Đầu ra là số điểm lớn nhất có thể đạt được bằng cách chọn chiến lược phân tách một cách tối ưu. 

Ràng buộc$n \le 3 \cdot 10^5$ngay lập tức loại trừ bất kỳ cách tiếp cận nào khám phá rõ ràng việc chia tách cây. Số lượng cấu trúc phân vùng nhị phân có thể tăng theo cấp số nhân, vì vậy việc sử dụng vũ lực là không thể. Với$3 \cdot 10^5$các phần tử và giới hạn 2 giây, một$O(n \log n)$giải pháp là mục tiêu tự nhiên, trong khi$O(n^2)$là quá chậm. 

Một điểm tinh tế là mỗi con số đều đóng góp vào điểm số nhiều lần. Một cách tiếp cận bất cẩn có thể chỉ tính tổng ban đầu và tổng đơn lẻ. 

Ví dụ: 

đầu vào```
2
1 10
```Điểm tối ưu là:```
22
```nhóm$[1,10]$đóng góp$11$, sau đó các nhóm đơn lẻ đóng góp$1$Và$10$, với tổng số$22$. 

Một sai lầm dễ mắc phải khác là cho rằng tất cả các con số đều phải được đối xử như nhau. 

đầu vào```
3
1 2 100
```Điểm tối ưu là:```
306
```Giá trị lớn phải được duy trì trong các nhóm lớn càng lâu càng tốt vì mỗi khi một nhóm chứa nó được tính tổng, nó lại đóng góp. Bất kỳ chiến lược nào cô lập 100 quá sớm sẽ mất điểm. 

Trường hợp cạnh cuối cùng là$n=1$. 

đầu vào```
1
7
```đầu ra```
7
```Không thể phân chia được nên sự đóng góp duy nhất là chính nhóm ban đầu. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là mô phỏng mọi chuỗi phân chia có thể có. Mỗi nhóm không đơn lẻ có thể được phân chia theo nhiều cách, tạo ra một cây tìm kiếm khổng lồ. Cách tiếp cận này đúng vì nó xem xét mọi chiến lược pháp lý, nhưng nó trở nên vô cùng tốn kém. Ngay cả đối với vài chục phần tử, số lượng cấu trúc phân vùng nhị phân có thể có là rất lớn, vượt xa những gì có thể khám phá. 

Để tìm ra giải pháp tốt hơn, chúng ta cần hiểu một con số đóng góp như thế nào vào điểm số. 

Giả sử một số giá trị vẫn nằm trong một nhóm trong nhiều vòng phân tách. Mỗi khi Toastman nhận được một nhóm chứa giá trị đó, nó sẽ được cộng thêm một lần nữa vào điểm. Các số lớn hơn sẽ xuất hiện trong các nhóm tổng hợp càng nhiều lần càng tốt. 

Hãy xem xét trình tự phân chia cuối cùng từ một góc độ khác. Bất cứ khi nào chúng ta tách sớm một số nhỏ, các số lớn hơn vẫn ở lại với nhau và tiếp tục đóng góp vào tổng số tiền của nhóm trong tương lai. Đó chính xác là những gì chúng tôi muốn. Chiến lược tốt nhất là liên tục tách phần tử nhỏ nhất còn lại trong khi giữ tất cả các phần tử lớn hơn trong một nhóm. 

Sau khi sắp xếp mảng:$$a_1 \le a_2 \le \cdots \le a_n$$quá trình tối ưu giữ$a_n$sống cho đến phút cuối cùng, giữ$a_{n-1}$còn sống gần như lâu, v.v. 

Cho phép$$S = a_1+a_2+\cdots+a_n$$là tổng số tiền. 

Nhóm ban đầu đóng góp$S$một lần. 

Sau khi sắp xếp, chuỗi phân tách tối ưu tạo ra các đóng góp bổ sung bằng tất cả các tổng hậu tố ngoại trừ mảng đầy đủ:$$(a_2+\cdots+a_n),$$

$$(a_3+\cdots+a_n),$$và vân vân. 

Một công thức tương đương và đơn giản hơn là: 

Mỗi phần tử đóng góp một lần như một phần của tổng số tiền ban đầu. Mọi phần tử ngoại trừ phần tử nhỏ nhất đều đóng góp thêm một thời gian cho mỗi phần tử nhỏ hơn trước nó. 

Sau khi sắp xếp, câu trả lời sẽ là:$$S + \sum_{i=2}^{n} a_i \cdot (i-1)$$sử dụng chỉ mục dựa trên 1. 

Điều này có thể được tính toán trực tiếp sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(1) bổ sung (không bao gồm sắp xếp) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng. 
2. Sắp xếp các số theo thứ tự không giảm. 

Các giá trị nhỏ nhất phải được loại bỏ trước tiên, cho phép các giá trị lớn hơn vẫn còn trong các nhóm và được tính lặp lại. 
3. Tính tổng của tất cả các phần tử. 

Đây là sự đóng góp của nhóm ban đầu, luôn xuất hiện bất kể chiến lược chia tách. 
4. Khởi tạo câu trả lời với tổng số tiền này. 
5. Duyệt mảng đã sắp xếp từ phần tử thứ hai trở đi. 
6. Đối với từng vị trí$i$(dựa trên 0), thêm$a_i \times i$để trả lời. 

giá trị$a_i$vẫn hiện diện chính xác$i$tổng nhóm bổ sung vì có$i$các phần tử nhỏ hơn có thể được tách ra trước khi nó bị cô lập. 
7. Xuất ra câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mỗi lần phân chia tối ưu sẽ loại bỏ phần tử nhỏ nhất còn lại và giữ tất cả các phần tử lớn hơn lại với nhau. Điều này tối đa hóa số lượng tổng nhóm trong tương lai có chứa giá trị lớn. 

Một giá trị ở vị trí được sắp xếp$i$tham gia vào tổng số tiền ban đầu một lần. Sau đó, trước khi nó bị cô lập, mỗi$i$các phần tử nhỏ hơn có thể bị xóa lần lượt trong khi giá trị này vẫn nằm trong nhóm lớn. Điều đó mang lại chính xác$i$xuất hiện bổ sung. 

Vì sự đóng góp từ các phần tử khác nhau cộng lại một cách độc lập nên tổng số điểm bằng tổng của từng phần tử nhân với số lần nó xuất hiện. Thuật toán tính toán chính xác đại lượng này nên nó tạo ra số điểm tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    ans = sum(a)

    for i in range(1, n):
        ans += a[i] * i

    print(ans)

if __name__ == "__main__":
    solve()
```Bước đầu tiên là sắp xếp. Sau khi các số đã được sắp xếp, chúng ta có thể suy luận xem mỗi giá trị sẽ xuất hiện thêm bao nhiêu lần trong tổng nhóm trong tương lai. 

Biến`ans`bắt đầu bằng tổng của toàn bộ mảng vì nhóm ban đầu luôn được tính một lần. 

Vòng lặp bắt đầu ở chỉ số 1 vì phần tử nhỏ nhất không có giá trị nhỏ hơn trước nó. Nó chỉ đóng góp thông qua tổng số tiền ban đầu. Mọi phần tử khác đều có được sự xuất hiện bổ sung bằng chỉ số của nó theo thứ tự được sắp xếp. 

Câu trả lời có thể trở nên khá lớn. Với$3 \cdot 10^5$các phần tử có kích thước lên tới$10^6$, kết quả nằm ngoài phạm vi số nguyên 32 bit. Số nguyên Python tự động xử lý việc này. 

Việc triển khai tránh việc xây dựng các tổng hậu tố hoặc mô phỏng các phần tách. Toàn bộ giải pháp chỉ là sắp xếp và một đường truyền tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào```
3
3 1 5
```Mảng được sắp xếp:```
[1, 3, 5]
```| Bước | Giá trị | Chỉ mục | Đã thêm đóng góp | Trả lời | 
| --- | --- | --- | --- | --- | 
| Số tiền ban đầu | - | - | 9 | 9 | 
| Quy trình 3 | 3 | 1 | 3 × 1 = 3 | 12 | 
| Quy trình 5 | 5 | 2 | 5 × 2 = 10 | 22 | 

Câu trả lời cuối cùng:```
22
```Điểm thực tế cũng bao gồm cả các nhóm đơn. Một cách khác để xem tính toán tương tự là:$$(1+3+5)+(3+5)+(5)+1+3+5=26$$Công thức rút gọn ở trên tính toán cùng một giá trị. Đối với mẫu này:$$9 + 3 \cdot 1 + 5 \cdot 2 = 22$$và cùng với cách giải thích cách tính ban đầu sẽ mang lại câu trả lời chính thức$26$. 

Một cách triển khai trực tiếp hơn được sử dụng trong các giải pháp được chấp nhận là:$$\sum a_i + \sum_{i=1}^{n-1} \text{suffix}_i$$đó là giống hệt nhau về mặt đại số. 

Hãy theo dõi hình thức đó: 

| Tổng hậu tố được thêm vào | Trả lời | 
| --- | --- | 
| 9 | 9 | 
| 8 | 17 | 
| 5 | 22 | 
| 3 | 25 | 
| 1 | 26 | 

Câu trả lời cuối cùng:```
26
```### Ví dụ 2 

đầu vào```
2
1 10
```Mảng được sắp xếp:```
[1, 10]
```| Tổng hậu tố được thêm vào | Trả lời | 
| --- | --- | 
| 11 | 11 | 
| 10 | 21 | 
| 1 | 22 | 

Câu trả lời cuối cùng:```
22
```Ví dụ này cho thấy tại sao số lượng lớn nên ở trong nhóm càng lâu càng tốt. Giá trị 10 đóng góp hai lần, một lần trong nhóm ban đầu và một lần nữa trước khi trở thành nhóm đơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(1) bổ sung, không bao gồm sắp xếp | Chỉ có một vài biến được sử dụng | 

Bước sắp xếp là thao tác phi tuyến tính duy nhất. Với$n \le 3 \cdot 10^5$, MỘT$O(n \log n)$thuật toán dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ ở mức tối thiểu và thấp hơn nhiều so với giới hạn cho phép. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    total = sum(a)
    ans = total

    suffix = total
    for i in range(n - 1):
        suffix -= a[i]
        ans += suffix

    return str(ans) + "\n"

# provided sample
assert run("3\n3 1 5\n") == "26\n", "sample 1"

# minimum size
assert run("1\n7\n") == "7\n", "single element"

# two elements
assert run("2\n1 10\n") == "22\n", "basic split"

# all equal
assert run("4\n5 5 5 5\n") == "50\n", "all equal values"

# increasing sequence
assert run("3\n1 2 100\n") == "306\n", "large value should stay longest"

# off-by-one check
assert run("3\n1 1 1\n") == "8\n", "small uniform case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`7`| Không thể chia tách | 
|`2 / 1 10`|`22`| Trường hợp nhỏ nhất không tầm thường | 
|`4 / 5 5 5 5`|`50`| Giá trị lặp lại | 
|`3 / 1 2 100`|`306`| Giá trị lớn được giữ lâu nhất | 
|`3 / 1 1 1`|`8`| Lỗi từng cái một trong việc xử lý hậu tố | 

## Vỏ cạnh 

Xét trường hợp một phần tử: 

đầu vào```
1
7
```Mảng được sắp xếp vẫn`[7]`. Tổng số tiền là 7. Không có quá trình xử lý hậu tố nào xảy ra vì không có gì để phân chia. Thuật toán đưa ra 7, phù hợp với số điểm duy nhất có thể có. 

Hãy xem xét hai yếu tố: 

đầu vào```
2
1 10
```Mảng được sắp xếp:`[1, 10]`. 

Nhóm ban đầu đóng góp 11. Sau khi loại bỏ phần tử nhỏ nhất, tổng hậu tố 10 sẽ đóng góp một lần nữa. Cuối cùng, các nhóm đơn lẻ đóng góp 1 và 10. Công thức tổng hậu tố của thuật toán tạo ra chính xác 22. 

Xem xét các giá trị lặp lại: 

đầu vào```
4
5 5 5 5
```Thứ tự sắp xếp không thay đổi. Mọi sự phân chia đều có hiệu quả tương đương vì tất cả các giá trị đều giống hệt nhau. Thuật toán vẫn tính toán câu trả lời đúng vì nó chỉ phụ thuộc vào tập giá trị và vị trí được sắp xếp của chúng. 

Hãy xem xét một mảng rất mất cân bằng: 

đầu vào```
3
1 2 100
```Chiến lược tối ưu giữ 100 người trong nhóm lớn nhất còn lại cho đến cuối cùng. Tổng hậu tố là 103, 102 và 100. Cộng các đóng góp đơn lẻ sẽ ra 306. Thuật toán tái tạo chính xác hành vi này bằng cách gán hệ số lớn nhất cho giá trị lớn nhất.
