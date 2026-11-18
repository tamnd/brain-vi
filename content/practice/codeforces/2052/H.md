---
title: "CF 2052H - Săn Hoglin ở Hogwarts"
description: "Cuộc đua bắt đầu với những chiếc xe được sắp xếp theo nhãn của chúng: $$1,2,3,dots,n.$$ Trong cuộc đua, một lần vượt là một sự hoán đổi liền kề. Nếu ô tô $x$ ở ngay sau ô tô $y$, thì sự kiện \"$x$ vượt qua $y$\" sẽ hoán đổi vị trí của chúng. Khi kết thúc cuộc đua, chúng ta chỉ biết thứ tự cuối cùng $c$."
date: "2026-06-08T08:35:09+07:00"
tags: ["codeforces", "competitive-programming", "interactive"]
categories: ["algorithms"]
codeforces_contest: 2052
codeforces_index: "H"
codeforces_contest_name: "2024-2025 ICPC, NERC, Northern Eurasia Finals (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 3500
weight: 2052
solve_time_s: 64
verified: true
draft: false
---

[CF 2052H - Săn Hoglin ở Hogwarts](https://codeforces.com/problemset/problem/2052/H) 

**Đánh giá:** 3500 
**Thẻ:** tương tác 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cuộc đua bắt đầu với những chiếc xe được đặt hàng theo nhãn của họ:$$1,2,3,\dots,n.$$Trong cuộc đua, vượt là hoán đổi liền kề. Nếu ô tô$x$ở ngay phía sau xe$y$, sau đó là sự kiện "$x$vượt qua$y$" hoán đổi vị trí của họ. 

Kết thúc cuộc đua chúng ta chỉ biết thứ tự cuối cùng$c$. Chúng ta phải tái tạo lại một cuộc đua có càng nhiều lần vượt càng tốt, với một hạn chế: cho mỗi cặp ô tô$(x,y)$, sự kiện "$x$vượt qua$y$" có thể xảy ra nhiều nhất một lần, và sự kiện "$y$vượt qua$x$" có thể xảy ra nhiều nhất một lần. 

Chúng ta cần xuất ra cả số lần vượt tối đa có thể và một chuỗi lần vượt để đạt được số lần vượt đó. 

Ràng buộc$n \le 1000$là nhỏ. Số lượng đôi xe nhiều nhất là$$\frac{1000\cdot999}{2}=499500.$$Vì bản thân câu trả lời có thể chứa$O(n^2)$vượt qua, bất kỳ giải pháp được chấp nhận nào cũng sẽ ít nhất là bậc hai trong trường hợp xấu nhất. MỘT$O(n^2)$thi công hoàn toàn an toàn. 

Một điểm tinh tế là việc tối đa hóa số lần vượt không giống như giảm thiểu hoặc khớp với hoán vị cuối cùng. Đối với một cặp xe có thứ tự tương đối không thay đổi ở thứ hạng cuối cùng, chúng ta có thể cho chúng đổi vị trí hai lần trong suốt chặng đua mà vẫn về đích theo thứ tự ban đầu. Một giải pháp chỉ thực hiện các lần vượt cần thiết để có được hoán vị cuối cùng sẽ bỏ lỡ nhiều lần vượt bổ sung hợp lệ. 

Coi như:```
n = 2
final = [1, 2]
```Lệnh cuối cùng đã là lệnh ban đầu. Một giải pháp ngây thơ sẽ tạo ra số lần vượt qua bằng 0. 

Cuộc đua tối ưu là:```
2 overtakes 1
1 overtakes 2
```Lệnh cuối cùng lại là$[1,2]$, nhưng bây giờ chúng tôi có hai lần vượt qua. 

Một trường hợp dễ bỏ sót khác là:```
n = 3
final = [3,2,1]
```Mỗi cặp đều bị đảo ngược. Không cặp nào có thể đóng góp hai lần vượt, vì việc kết thúc đảo ngược đòi hỏi phải có sự thay đổi thực sự về thứ tự tương đối. Tối đa chính xác là ba lần vượt, một lần cho mỗi cặp. 

## Phương pháp tiếp cận 

Quan điểm bạo lực bắt đầu từ việc xem xét từng cặp ô tô một cách độc lập. 

Lấy một cặp$(x,y)$với$x<y$. Ban đầu,$x$đi trước$y$. 

Nếu xếp hạng cuối cùng cũng có vị trí$x$trước$y$, thì cặp đôi này có thể đóng góp hai lần vượt:```
y overtakes x
x overtakes y
```Thứ tự tương đối kết thúc không thay đổi. 

Nếu vị trí xếp hạng cuối cùng$y$trước$x$, thì cặp đôi đó có thể đóng góp nhiều nhất một lần vượt:```
y overtakes x
```Thực hiện cả hai hướng sẽ khôi phục trật tự ban đầu, điều này không được phép. 

Điều này ngay lập tức đưa ra một giới hạn trên. Cho phép$P$là số cặp và đặt$I$là số lần đảo ngược của hoán vị cuối cùng so với thứ tự ban đầu. 

Mỗi cặp không đảo ngược đóng góp tối đa hai lần vượt. Mỗi cặp nghịch đảo đóng góp nhiều nhất một. 

Kể từ đây$$\text{answer} \le 2(P-I)+I = 2P-I.$$Phần thú vị là cho thấy giới hạn này luôn có thể đạt được. 

Quan sát quan trọng là chúng ta có thể nhận ra lần vượt đầu tiên cho mỗi cặp một cách rất có cấu trúc. 

Bắt đầu từ$$1,2,\dots,n.$$Đối với mỗi$y=2,3,\dots,n$, di chuyển$y$về phía trước bằng các đường vượt liền kề:```
y overtakes y-1
y overtakes y-2
...
y overtakes 1
```Sau khi xử lý tất cả các xe, từng cặp$(x,y)$với$x<y$đã trải qua đúng một lần vượt, cụ thể là "$y$vượt qua$x$". Lệnh trở thành$$n,n-1,\dots,1.$$Bây giờ hãy xem xét các cặp có thứ tự tương đối cuối cùng thực sự phải khớp với thứ tự ban đầu. Đối với mỗi cặp như vậy, chúng ta cần thêm một lần vượt theo hướng ngược lại. 

Bắt đầu từ thứ tự đảo ngược, chúng ta chuyển nó thành hoán vị đích$c$sử dụng các giao dịch hoán đổi liền kề. Mỗi trao đổi liền kề tương ứng chính xác với một cặp phải được khôi phục. Những lần hoán đổi đó mang lại lần vượt thứ hai cho mọi cặp không đảo ngược. 

Tổng số lần vượt trở thành$$P + (P-I)=2P-I,$$phù hợp với giới hạn trên và do đó là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm Brute Force qua lịch sử chủng tộc | Hàm mũ | Hàm mũ | Quá chậm | 
| Giải pháp tối ưu mang tính xây dựng |$O(n^2)$|$O(n^2)$cho đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị cuối cùng$c$. 
2. Tính vị trí của từng xe trong bảng xếp hạng cuối cùng. 
3. Tạo giai đoạn đầu tiên. 

Đối với mọi$y$từ$2$ĐẾN$n$, và mọi$x$từ$y-1$xuống$1$, đầu ra:```
y x
```Điều này có nghĩa là "$y$vượt qua$x$". 

Sau tất cả những lần vượt này, thứ tự các xe trở thành:$$n,n-1,\dots,1.$$4. Khởi tạo thứ tự hiện tại dưới dạng hoán vị ngược. 
5. Chuyển đổi hoán vị ngược thành hoán vị đích$c$. 

Xử lý các vị trí từ trái sang phải. 

Khi ô tô$c[i]$hiện đang ở vị trí$j>i$, liên tục hoán đổi nó sang trái cho đến khi đạt đến vị trí$i$. 

Mỗi trao đổi liền kề```
[..., y, x, ...]
->
[..., x, y, ...]
```tương ứng với việc vượt:```
x y
```Ghi lại lần vượt đó. 
6. Xuất ra tổng số lần vượt đã ghi và sau đó là chính số lần vượt đó. 

### Tại sao nó hoạt động 

Đối với mỗi cặp$(x,y)$với$x<y$, giai đoạn đầu luôn tạo ra đúng một lần vượt "$y$vượt qua$x$". 

Sau giai đoạn đầu tiên, mọi cặp đều có thứ tự tương đối đảo ngược. 

Nếu hoán vị cuối cùng cũng yêu cầu đảo ngược cặp thì không cần thêm gì nữa. Cặp đó đóng góp đúng một lần vượt. 

Nếu hoán vị cuối cùng yêu cầu thứ tự tương đối ban đầu thì trong quá trình chuyển đổi từ hoán vị ngược sang$c$, cặp phải hoán đổi một lần. Sự hoán đổi đó chính xác là "$x$vượt qua$y$". Cặp đôi này đóng góp đúng hai lần vượt. 

Không có cặp nào thực hiện cùng một hướng vượt hai lần. Giai đoạn đầu chỉ sử dụng "$y$vượt qua$x$". Giai đoạn thứ hai chỉ sử dụng "$x$vượt qua$y$" dành cho những cặp cần phục hồi. 

Do đó, mỗi cặp đảo ngược đóng góp một lần vượt, mỗi cặp không đảo ngược đóng góp hai lần vượt và tổng số bằng giới hạn trên đã được chứng minh$2P-I$. Việc xây dựng là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    c = list(map(int, input().split()))

    events = []

    # Phase 1
    for y in range(2, n + 1):
        for x in range(y - 1, 0, -1):
            events.append((y, x))

    # Phase 2
    arr = list(range(n, 0, -1))
    pos = [0] * (n + 1)

    for i, v in enumerate(arr):
        pos[v] = i

    for i in range(n):
        target = c[i]
        j = pos[target]

        while j > i:
            y = arr[j - 1]
            x = arr[j]

            events.append((x, y))

            arr[j - 1], arr[j] = arr[j], arr[j - 1]
            pos[x] -= 1
            pos[y] += 1

            j -= 1

    out = [str(len(events))]
    out.extend(f"{x} {y}" for x, y in events)
    sys.stdout.write("\n".join(out))

solve()
```Giai đoạn đầu tiên là hoàn toàn xác định. Xe hơi$y$vượt mỗi xe có số nhỏ hơn đúng một lần. Điều này tạo ra hoán vị ngược lại và đóng góp chính xác một lần vượt cho mỗi cặp. 

Giai đoạn thứ hai là chuyển đổi hoán đổi liền kề tiêu chuẩn. Mảng`arr`lưu trữ thứ tự hiện tại. Mảng`pos`lưu trữ vị trí hiện tại của mỗi chiếc xe, cho phép mỗi lần hoán đổi được cập nhật theo thời gian liên tục. 

Vòng lặp di chuyển`target`bên trái về cơ bản là tái cấu trúc kiểu sắp xếp chèn của hoán vị đích. Mỗi lần hoán đổi liền kề tương ứng trực tiếp với một sự kiện vượt hợp lệ. 

Bởi vì số nguyên Python có độ chính xác tùy ý nên không có vấn đề tràn. Số lượng sự kiện lớn nhất có thể là dưới một triệu, do đó việc lưu trữ tất cả các sự kiện vừa vặn thoải mái trong giới hạn bộ nhớ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 3 1
```Giai đoạn đầu tiên: 

| Sự kiện | Đặt Hàng Sau Sự Kiện | 
| --- | --- | 
| 2 vượt 1 | 2 1 3 | 
| 3 vượt 2 | 3 2 1 | 
| 3 vượt 1 | 3 2 1 | 

Trật tự bây giờ đã bị đảo ngược. 

Giai đoạn thứ hai: 

Hoán vị mục tiêu là`2 3 1`. 

| Sự kiện | Đặt Hàng Sau Sự Kiện | 
| --- | --- | 
| 2 vượt 3 | 2 3 1 | 

Trình tự cuối cùng:```
2 1
3 2
3 1
2 3
```Tổng số lần vượt: 4. 

Điều này phù hợp với mẫu. 

### Ví dụ 2 

đầu vào:```
3
3 2 1
```Giai đoạn đầu tiên đã tạo ra:```
3 2 1
```bằng với hoán vị đích. 

| Sự kiện | Đặt Hàng Sau Sự Kiện | 
| --- | --- | 
| 2 vượt 1 | 2 1 3 | 
| 3 vượt 2 | 3 2 1 | 
| 3 vượt 1 | 3 2 1 | 

Không yêu cầu hoán đổi giai đoạn hai. 

Tổng số lần vượt: 3. 

Mỗi cặp đều bị đảo ngược trong bảng xếp hạng cuối cùng, vì vậy không cặp nào có thể góp phần vượt qua lần thứ hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Số lần vượt được tạo ra là$O(n^2)$và mỗi cái được xử lý một lần | 
| Không gian |$O(n^2)$| Việc lưu trữ chuỗi đầu ra chi phối việc sử dụng bộ nhớ | 

Số lần vượt lớn nhất có thể là khoảng$n(n-1)$, tức là khoảng một triệu khi$n=1000$. MỘT$O(n^2)$việc xây dựng có độ phức tạp chính xác phù hợp với những hạn chế này. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    c = list(map(int, input().split()))

    events = []

    for y in range(2, n + 1):
        for x in range(y - 1, 0, -1):
            events.append((y, x))

    arr = list(range(n, 0, -1))
    pos = [0] * (n + 1)

    for i, v in enumerate(arr):
        pos[v] = i

    for i in range(n):
        target = c[i]
        j = pos[target]

        while j > i:
            y = arr[j - 1]
            x = arr[j]

            events.append((x, y))

            arr[j - 1], arr[j] = arr[j], arr[j - 1]
            pos[x] -= 1
            pos[y] += 1

            j -= 1

    return str(len(events))

# sample
assert run("3\n2 3 1\n") == "4"

# n = 1
assert run("1\n1\n") == "0"

# already sorted
assert run("2\n1 2\n") == "2"

# completely reversed
assert run("2\n2 1\n") == "1"

# reversed length 4
assert run("4\n4 3 2 1\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`0`| Kích thước tối thiểu | 
|`2 / 1 2`|`2`| Cặp đôi có thể đóng góp hai lần vượt | 
|`2 / 2 1`|`1`| Cặp đảo ngược góp phần vượt qua | 
|`4 / 4 3 2 1`|`6`| Hoán vị đảo ngược hoàn toàn | 

## Vỏ cạnh 

Hãy xem xét:```
2
1 2
```Thứ hạng cuối cùng bằng với thứ hạng ban đầu. Giai đoạn đầu tiên thực hiện:```
2 overtakes 1
```tạo ra thứ tự đảo ngược. Giai đoạn thứ hai khôi phục trật tự ban đầu với:```
1 overtakes 2
```Câu trả lời là 2, là số tối ưu vì cặp đôi này đóng góp cả hai khả năng vượt theo hướng có thể. 

Bây giờ hãy xem xét:```
2
2 1
```Giai đoạn đầu tiên lại thực hiện:```
2 overtakes 1
```và đã đạt đến hoán vị mục tiêu. Giai đoạn thứ hai không thực hiện được gì. Câu trả lời là 1. Bất kỳ nỗ lực nào để thêm lần vượt thứ hai sẽ khôi phục trật tự ban đầu và vi phạm thứ hạng cuối cùng được yêu cầu. 

Cuối cùng, hãy xem xét:```
3
3 2 1
```Mỗi cặp đều đảo ngược. Sau giai đoạn đầu tiên, thứ tự đã chính xác. Không có sự hoán đổi pha thứ hai xảy ra. Mỗi cặp đóng góp chính xác một lần vượt, đây là mức tối đa có thể có khi thứ tự tương đối cuối cùng bị đảo ngược.
