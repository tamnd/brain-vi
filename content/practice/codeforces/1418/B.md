---
title: "CF 1418B - Tiền tố phủ định"
description: "Chúng ta có một mảng và một mảng nhị phân thứ hai mô tả vị trí nào bị khóa và vị trí nào được mở khóa. Vị thế bị khóa phải giữ nguyên giá trị ban đầu. Vị trí đã mở khóa có thể nhận bất kỳ giá trị nào ban đầu thuộc về vị trí đã mở khóa."
date: "2026-06-11T06:50:21+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1418
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 95 (Rated for Div. 2)"
rating: 1300
weight: 1418
solve_time_s: 120
verified: false
draft: false
---

[CF 1418B - Tiền tố phủ định](https://codeforces.com/problemset/problem/1418/B) 

**Đánh giá:** 1300 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng và một mảng nhị phân thứ hai mô tả vị trí nào bị khóa và vị trí nào được mở khóa. 

Vị thế bị khóa phải giữ nguyên giá trị ban đầu. Vị trí đã mở khóa có thể nhận bất kỳ giá trị nào ban đầu thuộc về vị trí đã mở khóa. Nói cách khác, chúng ta có thể tự do hoán đổi các giá trị nằm trên các chỉ mục đã mở khóa, nhưng các giá trị bị khóa sẽ được cố định vĩnh viễn. 

Sau khi chọn cách sắp xếp lại như vậy, chúng ta xem xét tổng tiền tố của mảng kết quả. Cho phép$k$là vị trí cuối cùng có tổng tiền tố âm. Nếu mọi tổng tiền tố đều không âm thì$k=0$. 

Nhiệm vụ của chúng ta là tạo ra bất kỳ sự sắp xếp lại hợp lệ nào làm cho$k$càng nhỏ càng tốt. 

Quan sát quan trọng từ các ràng buộc là$n\le100$. Mặc dù con số này nhỏ nhưng số lượng vị trí được mở khóa cũng có thể lên tới gần 100 và việc thử tất cả các hoán vị sẽ cần tới$100!$khả năng, điều đó hoàn toàn không thể xảy ra. 

Thách thức ở đây là hiểu cách sắp xếp nào giúp giảm thiểu tiền tố phủ định cuối cùng. 

Một điểm tinh tế là chúng ta không được yêu cầu tối đa hóa số lượng tiền tố không âm. Chúng tôi chỉ quan tâm đến vị trí của tiền tố phủ định cuối cùng. Đôi khi tiền tố phải giữ nguyên số âm vì giá trị âm bị khóa xuất hiện sớm và không thể thay đổi. 

Một sai lầm dễ mắc phải khác là nghĩ rằng giá trị thực tế của tổng tiền tố là quan trọng trên toàn cầu. Sự tự do duy nhất đến từ các vị trí đã được mở khóa, vì vậy chúng ta nên tập trung vào cách đặt các giá trị đó để tạo tiền tố càng lớn càng sớm càng tốt. 

Coi như:```
a = [-8, 4, -2, -6, 4, 7, 1]
l = [ 1, 0,  0,  0, 1, 1, 0]
```Phần tử đầu tiên bị khóa và đã làm cho tiền tố đầu tiên trở thành âm. Chúng tôi không thể khắc phục điều đó. Chiến lược tốt nhất là đặt các giá trị đã mở khóa lớn nhất có sẵn càng sớm càng tốt để phục hồi số dư âm đó càng sớm càng tốt. 

Một trường hợp cạnh khác xảy ra khi mọi vị trí đều bị khóa:```
a = [2, -3, 4, -1]
l = [1,  1, 1,  1]
```Không được phép sắp xếp lại. Câu trả lời phải chính xác là mảng ban đầu. 

Trường hợp cạnh thứ ba là khi tất cả các vị trí được mở khóa:```
a = [0, 1, -4]
l = [0, 0,  0]
```Sự sắp xếp tối ưu là:```
[1, 0, -4]
```Việc đặt các giá trị lớn hơn trước tiên sẽ tối đa hóa mọi tổng tiền tố và giảm thiểu thời gian tồn tại của các tiền tố âm. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ thu thập tất cả các giá trị trên các vị trí đã mở khóa, tạo ra mọi hoán vị có thể có, xây dựng lại mảng, tính tổng tiền tố của nó, xác định giá trị tương ứng của$k$, và giữ sự sắp xếp tốt nhất. 

Điều này đúng vì nó xem xét mọi sự sắp xếp lại pháp lý. Thật không may, nếu có$m$vị trí đã mở khóa, nó yêu cầu$m!$hoán vị. Ngay cả đối với$m=15$, điều này đã vượt xa giới hạn thực tế. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến một tiền tố cụ thể. Chúng tôi quan tâm đến việc làm cho tổng tiền tố trở nên không âm càng sớm càng tốt. 

Giả sử chúng ta chỉ nhìn vào các vị trí đã mở khóa. Nếu hai vị trí được mở khóa$i<j$chứa các giá trị$x<y$, sau đó đặt$y$ở vị trí trước đó sẽ tăng mọi tổng tiền tố từ vị trí$i$trở đi bởi$y-x$. Giá trị lớn hơn sẽ giúp ích cho tất cả các tiền tố trong tương lai khi được đặt sớm hơn. 

Đây chính xác là đối số trao đổi tương tự được sử dụng để chứng minh rằng việc sắp xếp theo thứ tự giảm dần sẽ tối đa hóa tất cả các tổng một phần. 

Thực hiện bất kỳ sự sắp xếp nào của các giá trị đã được mở khóa. Nếu vị trí được mở khóa trước đó chứa giá trị nhỏ hơn vị trí được mở khóa sau này, việc hoán đổi chúng không thể làm giảm bất kỳ tổng tiền tố nào trong tương lai. Việc lặp lại quá trình này cuối cùng sẽ tạo ra các giá trị được mở khóa theo thứ tự giảm dần. 

Sự sắp xếp đó tối đa hóa mọi tổng tiền tố cùng một lúc. Vì mọi tiền tố càng lớn càng tốt, tiền tố phủ định sẽ biến mất càng sớm càng tốt, điều này giúp giảm thiểu vị trí cuối cùng có tiền tố phủ định. 

Vì vậy, giải pháp rất đơn giản: 

1. Trích xuất tất cả giá trị từ các vị trí đã mở khóa. 
2. Sắp xếp chúng theo thứ tự giảm dần. 
3. Duyệt mảng từ trái sang phải. 
4. Bất cứ khi nào gặp một vị trí đã mở khóa, hãy đặt giá trị sẵn có lớn nhất tiếp theo vào đó. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m!\cdot n)$|$O(m)$| Quá chậm | 
| Tối ưu |$O(n\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và mảng trạng thái khóa. 
2. Thu thập tất cả các giá trị có vị trí được mở khóa, nghĩa là giá trị khóa của chúng là 0. 
3. Sắp xếp bộ sưu tập này theo thứ tự giảm dần. 

Các giá trị được mở khóa lớn nhất sẽ xuất hiện ở các vị trí được mở khóa sớm nhất vì các vị trí trước đó đóng góp vào nhiều tiền tố hơn. 
4. Quét mảng từ trái sang phải. 
5. Bất cứ khi nào gặp một vị trí bị khóa, hãy giữ nguyên giá trị của nó. 
6. Bất cứ khi nào gặp một vị trí đã mở khóa, hãy lấy giá trị tiếp theo từ danh sách giảm dần đã sắp xếp và đặt nó vào đó. 
7. Xuất ra mảng kết quả. 

### Tại sao nó hoạt động 

Hãy xem xét hai vị trí đã mở khóa$i<j$. Nếu giá trị tại$i$nhỏ hơn giá trị tại$j$, việc hoán đổi chúng sẽ làm tăng mọi tổng tiền tố bao gồm vị trí$i$nhưng chưa có vị trí$j$, trong khi vẫn giữ nguyên các tiền tố sau này. Không có tổng tiền tố nào giảm. 

Do đó, bất kỳ sự sắp xếp nào không được sắp xếp giảm dần theo các vị trí đã mở khóa đều có thể được cải thiện bằng cách hoán đổi như vậy. Việc áp dụng lập luận này nhiều lần sẽ dẫn đến sự sắp xếp giảm dần. 

Sự sắp xếp giảm dần tối đa hóa mọi tổng tiền tố trong số tất cả các sắp xếp lại hợp pháp. Vì mỗi tổng tiền tố riêng lẻ càng lớn càng tốt, nên điểm mà tổng tiền tố không còn âm xảy ra càng sớm càng tốt. Do đó giá trị kết quả của$k$là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        l = list(map(int, input().split()))

        free = []

        for i in range(n):
            if l[i] == 0:
                free.append(a[i])

        free.sort(reverse=True)

        ptr = 0

        for i in range(n):
            if l[i] == 0:
                a[i] = free[ptr]
                ptr += 1

        print(*a)

solve()
```Phần đầu tiên tập hợp tất cả các giá trị nằm ở các vị trí đã mở khóa. 

Sắp xếp theo thứ tự giảm dần thực hiện lựa chọn tham lam đã được chứng minh ở trên. Giá trị sẵn có lớn nhất luôn được gán cho vị trí mở khóa sẵn có sớm nhất. 

Giai đoạn tái thiết đi qua mảng ban đầu. Các vị trí bị khóa không bị ảnh hưởng. Các vị trí đã mở khóa nhận các giá trị từ danh sách được sắp xếp theo thứ tự. 

Không có tổng tiền tố nào cần được tính toán rõ ràng. Bằng chứng cho thấy rằng việc tối đa hóa mọi tổng tiền tố tương đương với việc tối thiểu hóa tiền tố âm cuối cùng. 

Tất cả các giá trị đều vừa vặn thoải mái bên trong số nguyên Python, vì vậy việc tràn không phải là vấn đề đáng lo ngại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [-8, 4, -2, -6, 4, 7, 1]
l = [ 1, 0,  0,  0, 1, 1, 0]
```Các giá trị được mở khóa là:```
[4, -2, -6, 1]
```Sau khi sắp xếp:```
[4, 1, -2, -6]
```| Vị trí | Bị khóa? | Giá trị đặt | Mảng kết quả | 
| --- | --- | --- | --- | 
| 1 | Có | -8 | [-8] | 
| 2 | Không | 4 | [-8, 4] | 
| 3 | Không | 1 | [-8, 4, 1] | 
| 4 | Không | -2 | [-8, 4, 1, -2] | 
| 5 | Có | 4 | [-8, 4, 1, -2, 4] | 
| 6 | Có | 7 | [-8, 4, 1, -2, 4, 7] | 
| 7 | Không | -6 | [-8, 4, 1, -2, 4, 7, -6] | 

Mảng cuối cùng:```
[-8, 4, 1, -2, 4, 7, -6]
```Ví dụ này cho thấy rằng mặc dù tiền tố đầu tiên bị giá trị khóa buộc phải âm, nhưng việc đặt sớm các giá trị đã mở khóa lớn nhất hiện có sẽ giúp phục hồi nhanh nhất có thể. 

### Ví dụ 2 

đầu vào:```
a = [0, 1, -4, 6, 3]
l = [0, 0, 0, 1, 1]
```Các giá trị đã mở khóa:```
[0, 1, -4]
```Sắp xếp giảm dần:```
[1, 0, -4]
```| Vị trí | Bị khóa? | Giá trị đặt | Mảng kết quả | 
| --- | --- | --- | --- | 
| 1 | Không | 1 | [1] | 
| 2 | Không | 0 | [1, 0] | 
| 3 | Không | -4 | [1, 0, -4] | 
| 4 | Có | 6 | [1, 0, -4, 6] | 
| 5 | Có | 3 | [1, 0, -4, 6, 3] | 

Mảng cuối cùng:```
[1, 0, -4, 6, 3]
```Điều này thể hiện ý tưởng tham lam cốt lõi. Các giá trị được mở khóa lớn nhất sẽ được di chuyển càng xa càng tốt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp các giá trị đã mở khóa chiếm ưu thế | 
| Không gian |$O(n)$| Lưu trữ các giá trị đã mở khóa | 

Từ$n\le100$, giải pháp này cực kỳ nhanh chóng. Ngay cả với 1000 trường hợp thử nghiệm, việc sắp xếp tối đa 100 số cho mỗi trường hợp vẫn dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        l = list(map(int, input().split()))

        free = [a[i] for i in range(n) if l[i] == 0]
        free.sort(reverse=True)

        ptr = 0
        for i in range(n):
            if l[i] == 0:
                a[i] = free[ptr]
                ptr += 1

        out.append(" ".join(map(str, a)))

    return "\n".join(out)

# provided samples
assert run(
"""5
3
1 3 2
0 0 0
4
2 -3 4 -1
1 1 1 1
7
-8 4 -2 -6 4 7 1
1 0 0 0 1 1 0
5
0 1 -4 6 3
0 0 0 1 1
6
-1 7 10 4 -8 -1
1 0 0 0 0 1
"""
) == (
"""3 2 1
2 -3 4 -1
-8 4 1 -2 4 7 -6
1 0 -4 6 3
-1 10 7 4 -8 -1"""
)

# minimum size
assert run(
"""1
1
5
0
"""
) == "5"

# all locked
assert run(
"""1
4
1 2 3 4
1 1 1 1
"""
) == "1 2 3 4"

# all unlocked
assert run(
"""1
4
-1 5 2 0
0 0 0 0
"""
) == "5 2 0 -1"

# duplicate values
assert run(
"""1
5
7 7 7 7 7
0 1 0 1 0
"""
) == "7 7 7 7 7"
```### Tóm tắt trường hợp kiểm thử 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn | Cùng một yếu tố | Kích thước tối thiểu | 
| Tất cả các vị trí bị khóa | Mảng gốc | Không được phép sắp xếp lại | 
| Đã mở khóa tất cả các vị trí | Thứ tự giảm dần | Hành vi tham lam thuần túy | 
| Tất cả các giá trị bằng nhau | Mảng không đổi | Xử lý trùng lặp chính xác | 

## Vỏ cạnh 

Hãy xem xét trường hợp mọi vị trí đều bị khóa:```
n = 4
a = [2, -3, 4, -1]
l = [1, 1, 1, 1]
```Bộ đã mở khóa trống. Việc sắp xếp không làm gì, việc xây dựng lại không làm gì và thuật toán đưa ra mảng ban đầu. Đây là câu trả lời hợp pháp duy nhất. 

Hãy xem xét trường hợp mọi vị trí đều được mở khóa:```
n = 3
a = [0, 1, -4]
l = [0, 0, 0]
```Thuật toán trích xuất tất cả các giá trị, sắp xếp chúng thành:```
[1, 0, -4]
```và đặt chúng trở lại. Bất kỳ sự sắp xếp nào khác đều đưa ra một số tiền tố không lớn hơn tổng tiền tố tương ứng của sự sắp xếp này. 

Hãy xem xét một tiền tố phủ định bắt buộc:```
n = 3
a = [-10, 8, 7]
l = [1, 0, 0]
```Giá trị đầu tiên bị khóa nên tiền tố đầu tiên luôn là$-10$. Thuật toán sắp xếp các giá trị đã được mở khóa thành:```
[8, 7]
```mang lại:```
[-10, 8, 7]
```Tổng tiền tố trở thành:```
-10, -2, 5
```Tiền tố phủ định cuối cùng xảy ra ở vị trí 2. Không có sự sắp xếp lại nào có thể làm tốt hơn vì giá trị sẵn có lớn nhất đã được sử dụng càng sớm càng tốt. 

Cuối cùng, hãy xem xét các giá trị được mở khóa trùng lặp:```
n = 5
a = [3, 3, -1, -1, 2]
l = [0, 0, 0, 0, 1]
```Sắp xếp tạo ra:```
[3, 3, -1, -1]
```Việc xây dựng lại vẫn hợp lệ ngay cả với các giá trị lặp lại. Chứng minh chỉ phụ thuộc vào thứ tự tương đối chứ không phụ thuộc vào tính duy nhất.
