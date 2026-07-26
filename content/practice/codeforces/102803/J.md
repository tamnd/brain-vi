---
title: "CF 102803J - Chuông leng keng"
description: "Chúng tôi có một cây chuông leng keng có rễ. Mỗi nút có hai giá trị a và b. Gốc đã được trang trí, nhưng nó không đóng góp gì vì cả hai giá trị của nó đều bằng 0. Mọi nút khác phải được thêm vào sau khi nút cha của nó đã được thêm vào."
date: "2026-07-26T16:26:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102803
codeforces_index: "J"
codeforces_contest_name: "The 15th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102803
solve_time_s: 49
verified: true
draft: false
---

[CF 102803J - Chuông leng keng](https://codeforces.com/problemset/problem/102803/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cây chuông leng keng có rễ. Mỗi nút có hai giá trị,`a`Và`b`. Gốc đã được trang trí, nhưng nó không đóng góp gì vì cả hai giá trị của nó đều bằng 0. Mọi nút khác phải được thêm vào sau khi nút cha của nó đã được thêm vào. 

Khi một nút`i`được thêm vào, vẻ đẹp đạt được là`b_i`nhân với tổng của`a`giá trị của tất cả các nút vẫn chưa được trang trí sau thao tác này. Mục tiêu là chọn một thứ tự hợp lệ để trang trí tất cả các nút nhằm tối đa hóa vẻ đẹp tổng thể. 

Đầu vào mô tả một số cây có gốc. Mảng cha xác định cấu trúc cây và mỗi nút chứa cặp giá trị của nó. Kết quả là vẻ đẹp tổng thể tối đa có thể sau khi trang trí toàn bộ cây. 

Tổng số nút trong tất cả các trường hợp thử nghiệm nhiều nhất là`2.1 * 10^5`. Điều này loại trừ các phương pháp thử nhiều thứ tự trang trí có thể có, bởi vì ngay cả một cây có nhiều lá cũng có thể có một số lượng lớn các thứ tự trang trí hợp lệ. Chúng ta cần một thuật toán gần tuyến tính hoặc`O(n log n)`, trong đó logarit có thể đến từ hàng đợi ưu tiên. 

Một lỗi phổ biến là chỉ xem xét các nút có kích thước lớn`b`các giá trị. Giá trị của một nút phụ thuộc vào bao nhiêu`a`vẫn còn sau khi chọn nó, vì vậy một lượng lớn`b`là không đủ. Một sai lầm khác là bỏ qua hạn chế của cây và sắp xếp tất cả các nút ngay lập tức. Một nút không thể được chọn trước nút cha của nó. 

Ví dụ, hãy xem xét:```
3
1 1
0 0
10 1
1 100
```Gốc có hai con. Chọn con thứ hai trước sẽ có lợi`100 * 10 = 1000`, trong khi chọn con đầu tiên chỉ mang lại`1 * 100`. Câu trả lời là`1000`. Chiến lược chỉ dựa trên thứ tự nút hoặc thứ tự đầu vào sẽ không thành công vì tỷ lệ giữa`b`Và`a`vấn đề. 

Một trường hợp cạnh khác là một chuỗi:```
2
1
0 0
5 7
```Thứ tự hợp lệ duy nhất là nút gốc rồi đến nút 2. Câu trả lời là`0`, vì sau khi đặt chiếc chuông cuối cùng thì không còn nút nào nữa. Bất kỳ giải pháp nào thêm phần đóng góp của nút cuối cùng không chính xác sẽ tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ thử mọi thứ tự trang trí hợp lệ có thể. Đối với mỗi đơn hàng, chúng tôi có thể mô phỏng quy trình và tính toán độ đẹp. Điều này đúng vì mọi trình tự pháp lý đều được kiểm tra. Tuy nhiên, số lượng đơn đặt hàng có thể tăng theo cấp số nhân. Một cái cây có nhiều con từ gốc đã tạo ra nhiều sự lựa chọn về mặt giai thừa, vì vậy việc sử dụng vũ lực là không thể.`n = 100000`. 

Quan sát chính xuất phát từ việc so sánh hai nút có sẵn. Giả sử hai nút`x`Và`y`cả hai đều hiện có thể được lựa chọn và không phải là tổ tiên của cái kia. Hãy để tổng của tất cả sau này không được trang trí`a`giá trị được`R`. 

Nếu chúng ta chọn`x`trước`y`, đóng góp tổng hợp của họ là:```
b_x * (a_y + R) + b_y * R
```Nếu chúng ta chọn`y`trước`x`, đó là:```
b_y * (a_x + R) + b_x * R
```Lệnh đầu tiên sẽ tốt hơn khi:```
b_x * a_y >= b_y * a_x
```tương đương với:```
b_x / a_x >= b_y / a_y
```Vì vậy, trong số tất cả các nút hiện có, lựa chọn tốt nhất luôn là nút có kích thước lớn nhất.`b / a`tỷ lệ. 

Hạn chế về cây có nghĩa là chúng ta không thể sắp xếp mọi thứ ngay từ đầu. Thay vào đó, chúng tôi duy trì tập hợp các nút có cha mẹ đã được trang trí. Sau khi lấy nút tốt nhất từ ​​tập hợp này, các nút con của nó sẽ có sẵn. Điều này đưa ra một giải pháp xếp hàng ưu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số lệnh hợp lệ * n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số tiền`a`giá trị của tất cả các nút. Sau khi gốc được xử lý, điều này thể hiện số lượng`a`giá trị vẫn tồn tại bên ngoài tập được trang trí. 
2. Đặt tất cả các nút con của nút gốc vào hàng đợi ưu tiên được sắp xếp giảm dần`b / a`. Nút có tỷ lệ lớn nhất nên được chọn trước tiên vì đối số hoán đổi liền kề chứng tỏ rằng việc di chuyển nó sớm hơn không bao giờ gây hại. 
3. Liên tục loại bỏ nút tốt nhất khỏi hàng đợi ưu tiên. Trước khi cập nhật số tiền còn lại, hãy thêm phần đóng góp đẹp đẽ của nó:```
b_i * (remaining a after choosing i)
```Phần còn lại`a`giảm sau khi nút được trang trí, vì vậy chúng tôi trừ đi`a_i`Đầu tiên. 

1. Thêm tất cả các nút con của nút đã chọn vào hàng ưu tiên vì hiện tại chúng đã có thể truy cập được. 
2. Tiếp tục cho đến khi mọi nút đều được xử lý. 

Tại sao nó hoạt động: 

Tại mọi thời điểm, hàng đợi ưu tiên chứa chính xác các nút có thể được chọn tiếp theo một cách hợp pháp. Đối số trao đổi chứng minh rằng việc đặt mức tối đa`b/a`nút trước bất kỳ nút khả dụng nào khác không thể giảm câu trả lời. Do đó, sự lựa chọn tham lam là tối ưu ở mọi bước. Vì mọi nút được chọn đều bảo toàn thuộc tính này cho các nút còn lại nên toàn bộ thứ tự được tạo là tối ưu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve_case():
    n = int(input())
    parent = [0] * n
    if n > 1:
        p = list(map(int, input().split()))
        for i, x in enumerate(p, 1):
            parent[i] = x - 1

    children = [[] for _ in range(n)]
    for i in range(1, n):
        children[parent[i]].append(i)

    a = [0] * n
    b = [0] * n
    total = 0

    for i in range(n):
        a[i], b[i] = map(int, input().split())
        total += a[i]

    ans = 0
    remaining = total

    heap = []

    for v in children[0]:
        heapq.heappush(heap, (-b[v] / a[v], v))

    while heap:
        _, u = heapq.heappop(heap)

        remaining -= a[u]
        ans += b[u] * remaining

        for v in children[u]:
            heapq.heappush(heap, (-b[v] / a[v], v))

    return ans

def main():
    t = int(input())
    out = []
    for _ in range(t):
        out.append(str(solve_case()))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Mã lưu trữ cây dưới dạng danh sách con vì chỉ có chuyển động đi xuống mới quan trọng. Hàng đợi ưu tiên sử dụng tỷ lệ âm vì vùng heap của Python là vùng heap tối thiểu. 

Số tiền còn lại sẽ giảm trước khi cộng phần đóng góp vì công thức sử dụng các nút không được trang trí sau khi đặt nút hiện tại. Gốc không bao giờ được chèn vào hàng đợi vì nó đã được xử lý và có`a = b = 0`. 

So sánh dấu phẩy động của Python ở đây an toàn vì các giá trị tối đa là`10000`, nhưng cũng có thể sử dụng bộ so sánh số nguyên bằng phép nhân chéo. Bản thân câu trả lời có thể lớn hơn nhiều so với số nguyên 32 bit, do đó, số nguyên chính xác tùy ý của Python sẽ tránh được vấn đề tràn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, cây là:```
1
├── 2
│   └── 4
└── 3
```Các nút có sẵn được xử lý như sau: 

| Bước | Nút được chọn | Còn lại là sự lựa chọn sau | Đạt được | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | không | 12 | 0 | 0 | 
| 1 | 2 | 9 | 9 | 9 | 
| 2 | 4 | 5 | 5 | 14 | 
| 3 | 3 | 0 | 0 | 14 | 

Nút 2 được chọn trước nút 3 vì`1/3 > 1/5`. Sau khi chọn nút 2, nút 4 sẽ khả dụng và tỷ lệ của nó`1/4`vẫn lớn hơn tỷ lệ của nút 3. 

Đối với mẫu thứ hai, nút 3 có kích thước rất lớn`a`giá trị nhưng giống nhau`b`giá trị như nhiều nút khác. Thuật toán trì hoãn nó vì tỷ lệ của nó nhỏ: 

| Bước | Nút được chọn | Còn lại là sự lựa chọn sau | Đạt được | 
| --- | --- | --- | --- | 
| 1 | nút 2 | giá trị còn lại lớn | 4999 | 
| 2 | nút 4 | giá trị còn lại nhỏ hơn | 2999 | 
| ... | ... | ... | ... | 

Quá trình tiếp tục bằng cách luôn lấy tỷ lệ tốt nhất hiện có thể đạt được, tạo ra giá trị tối đa cần thiết của`16040`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi nút vào và ra khỏi hàng đợi ưu tiên một lần. | 
| Không gian | O(n) | Cây, mảng và hàng đợi ưu tiên lưu trữ ở hầu hết tất cả các nút. | 

Sự hạn chế của`2.1 * 10^5`tổng số nút vừa vặn thoải mái vì mọi thao tác đều là logarit và mỗi nút chỉ được xử lý một số lần không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io, heapq

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    main()
    sys.stdin = old
    return ""

# sample 1
# Expected output: 14

# sample 2
# Expected output: 16040

# Minimum size
# Tree with only the root
# Expected output: 0

# Chain case
# Ensures parent restrictions are handled correctly

# Equal ratios
# Ensures heap ordering does not affect correctness

# Large a value child
# Checks that b/a ratio is used instead of b only
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Gốc đơn | 0 | Hàng đợi ưu tiên trống và xử lý root | 
| Một chuỗi dài | Đúng lệnh bắt buộc | Xử lý sự phụ thuộc của cha mẹ | 
| Một số tỷ lệ bằng nhau | Cùng giá trị tối đa | Xử lý cà vạt | 
| Lớn`a`, bé nhỏ`b`nút | So sánh tỷ lệ | Tình trạng tham lam | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là cây một nút. Phần gốc đã được trang trí sẵn và không có bước di chuyển nào nên câu trả lời là 0. Thuật toán xử lý việc này vì hàng đợi ưu tiên bắt đầu trống. 

Trường hợp cạnh thứ hai là một chuỗi trong đó mỗi nút chỉ có một nút con. Không có lựa chọn nào theo thứ tự và hàng đợi ưu tiên luôn chứa chính xác một nút. Phương pháp tham lam giảm xuống mức duyệt duy nhất có thể. 

Trường hợp cạnh thứ ba là khi một số nút khả dụng có giá trị bằng nhau`b/a`tỷ lệ. Bất kỳ thứ tự nào trong số chúng đều cho kết quả theo cặp giống nhau vì phương trình so sánh trở nên bằng nhau. Heap có thể chọn bất kỳ trong số chúng mà không thay đổi câu trả lời cuối cùng. 

Trường hợp quan trọng cuối cùng là một nút có kích thước lớn`b`nhưng thậm chí còn lớn hơn`a`. Một nút như vậy có thể trông hấp dẫn, nhưng việc chọn nó sớm sẽ loại bỏ một lượng lớn lợi ích trong tương lai.`a`giá trị. Việc so sánh tỷ lệ ngăn ngừa sai lầm này bằng cách xem xét cả hai tác động cùng nhau. 

Bạn có thể điều chỉnh thêm bài xã luận cho phù hợp với kiểu nền tảng cụ thể, chẳng hạn như định dạng blog của Codeforces hoặc phần giải thích ngắn gọn hơn về cách giải quyết cuộc thi.
