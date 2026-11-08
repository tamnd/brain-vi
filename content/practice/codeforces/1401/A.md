---
title: "CF 1401A - Khoảng cách và trục"
description: "Chúng ta có một điểm $A$ đặt trên trục số nguyên tại tọa độ $n$. Chúng ta được phép điều chỉnh điểm này một đơn vị sang trái hoặc phải cho mỗi lần di chuyển."
date: "2026-06-11T08:40:59+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1401
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 665 (Div. 2)"
rating: 900
weight: 1401
solve_time_s: 95
verified: true
draft: false
---

[CF 1401A - Khoảng cách và Trục](https://codeforces.com/problemset/problem/1401/A) 

**Xếp hạng:** 900 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một điểm$A$đặt trên trục số nguyên tại tọa độ$n$. Chúng ta được phép điều chỉnh điểm này một đơn vị sang trái hoặc phải cho mỗi lần di chuyển. Sau khi điều chỉnh nó đến một số vị trí cuối cùng$n'$, chúng tôi muốn chọn một điểm nguyên khác$B$trên cùng một đường sao cho hiệu tuyệt đối giữa khoảng cách từ điểm gốc đến$B$, và khoảng cách từ$A$ĐẾN$B$, bằng một giá trị cố định$k$. 

Theo thuật ngữ hình học đơn giản hơn, khi chúng ta chọn$n'$, chúng ta đang hỏi liệu có tồn tại một số nguyên không$b$như vậy:$$\big||b| - |b - n'|\big| = k$$Nếu như vậy$b$tồn tại, cấu hình hợp lệ. Nếu không chúng ta phải chuyển$A$cho đến khi điều đó trở nên khả thi và chúng tôi muốn số lần di chuyển tối thiểu. 

Khó khăn chính là giá trị chỉ phụ thuộc vào mối quan hệ giữa$n'$Và$k$, không bật$b$trực tiếp. Vì vậy, nhiệm vụ thực sự là phải hiểu giá trị nào của$n'$một giải pháp tồn tại. 

Các ràng buộc rất lớn, có thể lên tới$6000$trường hợp thử nghiệm và phối hợp lên đến$10^6$. Điều này ngay lập tức loại trừ mọi nỗ lực nhằm mô phỏng các vị trí có thể có của$B$hoặc thử tất cả các cấu hình. Ngay cả việc quét tuyến tính cho mỗi trường hợp kiểm thử cũng sẽ quá chậm nếu nó phụ thuộc vào$n$hoặc$k$. 

Một trường hợp thất bại thường gặp là do hiểu sai tính đối xứng của biểu thức. Ví dụ, giả sử chỉ$b = 0$hoặc$b = n'$vấn đề dẫn đến kết luận không chính xác, bởi vì tối ưu$b$phụ thuộc vào thứ tự tương đối của$0$,$b$, Và$n'$. 

Một vấn đề tế nhị khác là coi biểu thức là tuyến tính. Sự hiện diện của các giá trị tuyệt đối tạo ra hành vi từng phần, do đó điều kiện sẽ chia thành nhiều vùng hình học. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta cố gắng hiểu ý tưởng về vũ lực. Cố định vị trí ứng viên$n'$. Sau đó chúng ta thử tất cả các giá trị nguyên có thể có của$b$và kiểm tra xem:$$\big||b| - |b - n'|\big| = k$$Đối với mỗi$b$, tính toán đây là thời gian không đổi, vì vậy đối với một$n'$đây là$O(M)$, Ở đâu$M$là phạm vi tọa độ có thể mà chúng tôi xem xét. Từ$n'$bản thân nó có thể thay đổi đến$10^6$, một sức mạnh tàn bạo đối với cả hai$n'$Và$b$trở thành theo thứ tự của$10^{12}$, điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không thực sự cần phải tìm kiếm$b$. Biểu thức thể hiện sự mất cân bằng khoảng cách so với điểm gốc và điểm$n'$và cấu trúc này có một sự đơn giản hóa đã biết: tùy thuộc vào vị trí tương đối của$b$, biểu thức thu gọn thành dạng tuyến tính. Điều này dẫn đến một đặc điểm khi các giải pháp tồn tại thuần túy về mặt$n'$Và$k$. 

Nếu chúng ta phân tích hình học, chúng ta thấy rằng các cấu hình hợp lệ phụ thuộc vào việc chúng ta có thể đặt$n'$để nó không quá “nhỏ” so với$k$. Cụ thể, điều kiện giảm xuống:$$n' \ge k \quad \text{or} \quad n' \equiv k \pmod{2}$$Chính xác hơn, việc đơn giản hóa tiêu chuẩn mang lại kết quả là việc điều chỉnh tối thiểu chỉ phụ thuộc vào mức độ$n$là từ giá trị hợp lệ gần nhất trong tập hợp:$$\{x \mid x \ge k \text{ and } x \equiv k \ (\mathrm{mod}\ 2)\}$$Điều này làm giảm vấn đề thành một phép điều chỉnh điểm gần nhất đơn giản trên dòng số nguyên. 

Vì vậy thay vì khám phá$b$, chúng ta rút gọn vấn đề thành việc kiểm tra xem$n$đã thỏa mãn ràng buộc cấu trúc, nếu không thì di chuyển nó tối thiểu đến số nguyên hợp lệ gần nhất. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$b$và điều chỉnh |$O(n^2)$|$O(1)$| Quá chậm | 
| Lý luận chẵn lẻ + ngưỡng |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Lời giải đúng có thể được rút ra từ hai ràng buộc: điều kiện giới hạn dưới và điều kiện chẵn lẻ. 

1. Tính xem vị trí hiện tại$n$đã thỏa mãn rồi$n \ge k$. 

Nếu có, chúng ta chỉ cần kiểm tra khả năng tương thích chẵn lẻ. Nếu không, chúng ta đã biết mình phải tiến lên ít nhất$k$, bởi vì mọi cấu hình hợp lệ đều yêu cầu đủ “quỹ khoảng cách” để nhận ra sự khác biệt tuyệt đối. 
2. Nếu$n < k$, ứng cử viên hợp lệ gần nhất là$k$chính nó, vì vậy câu trả lời chỉ đơn giản là$k - n$. 

This is because increasing$n$duy trì sự thay đổi chẵn lẻ từng cái một, vì vậy con đường ngắn nhất là chuyển động trực tiếp đến ngưỡng. 
3. Nếu$n \ge k$, tính tính chẵn lẻ của$n$và so sánh nó với$k \mod 2$. 

Nếu chúng khớp nhau thì chúng ta có thể chọn một cái phù hợp$b$không cần điều chỉnh thêm, vì vậy câu trả lời là$0$. 
4. Nếu số chẵn lẻ không khớp, chúng ta cần thêm một bước nữa để lật số chẵn lẻ trong khi vẫn ở trên hoặc bằng$k$. Giá trị gần nhất như vậy là$n-1$hoặc$n+1$và cả hai đều giữ chúng ta trong vùng hợp lệ. Do đó câu trả lời là$1$. 

### Tại sao nó hoạt động 

biểu hiện$\big||b| - |b - n|\big|$tạo ra một cấu trúc tuyến tính từng phần tùy thuộc vào vị trí$b$nằm tương đối với$0$Và$n$. Khi$n$đủ lớn so với$k$, hệ thống luôn thừa nhận một giải pháp, nhưng chỉ khi việc căn chỉnh chẵn lẻ cho phép đối xứng giá trị tuyệt đối khớp chính xác$k$. Ngược lại thì chuyển$n$bởi một người chuyển đổi tính chẵn lẻ mà không vi phạm tính khả thi và không thể thực hiện điều chỉnh nhỏ hơn vì việc lật tính chẵn lẻ chỉ xảy ra theo các bước đơn vị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())

    if n < k:
        print(k - n)
    else:
        if (n - k) % 2 == 0:
            print(0)
        else:
            print(1)
```Việc thực hiện trực tiếp tuân theo các điều kiện xuất phát. Nhánh đầu tiên xử lý trường hợp$n$nằm dưới ngưỡng khả thi, buộc phải tăng tuyến tính. Nhánh thứ hai dựa vào căn chỉnh chẵn lẻ, xác định liệu một giá trị có hợp lệ hay không$b$đã có thể được xây dựng. 

Một điểm tinh tế là chúng tôi kiểm tra$(n - k) \% 2$thay vì kiểm tra riêng$n \% 2$Và$k \% 2$, bởi vì chỉ có tính chẵn lẻ tương đối của chúng mới quan trọng. Điều này tránh logic dư thừa và giữ điều kiện đối xứng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 5, k = 8$| Bước | n | k | Tình trạng | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 8 | n < k | Di chuyển lên | 3 | 

Ở đây, vì điểm bắt đầu nằm dưới ngưỡng bắt buộc nên chúng ta phải chuyển từ 5 lên 8. Quá trình đó cần 3 bước và khi chúng ta đạt đến 8, một giá trị hợp lệ$b$tồn tại ngay lập tức. 

### Ví dụ 2:$n = 4, k = 0$| Bước | n | k | Tình trạng | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 0 | n ≥ k | Kiểm tra tính chẵn lẻ | 0 | 

Từ$k = 0$, mọi cấu hình hợp lệ chỉ yêu cầu tính đối xứng. Vị trí hiện tại đã cho phép một sự lựa chọn phù hợp$b$, nên không cần chuyển động. 

Ví dụ này xác nhận rằng một lần$n \ge k$, tính khả thi chỉ phụ thuộc vào khả năng tương thích về cấu trúc chứ không phải độ lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi trường hợp thử nghiệm | Mỗi trường hợp chỉ sử dụng một vài phép kiểm tra số học | 
| Không gian |$O(1)$| Không sử dụng cấu trúc phụ trợ | 

Với tối đa 6000 trường hợp thử nghiệm, phương pháp tiếp cận thời gian không đổi này dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = []
    t = int(sys.stdin.readline())
    for _ in range(t):
        n, k = map(int, sys.stdin.readline().split())
        if n < k:
            out.append(str(k - n))
        else:
            out.append(str(0 if (n - k) % 2 == 0 else 1))
    return "\n".join(out)

# provided samples
assert run("""6
4 0
5 8
0 1000000
0 0
1 0
1000000 1000000
""") == """0
3
1000000
0
1
0"""

# custom cases
assert run("""1
0 5
""") == "5"

assert run("""1
10 10
""") == "0"

assert run("""1
7 2
""") == "1"

assert run("""1
8 2
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (0,5) | 5 | trường hợp chuyển động hướng lên thuần túy | 
| (10,10) | 0 | căn chỉnh ngưỡng chính xác | 
| (7,2) | 1 | chẵn lẻ không khớp trên ngưỡng | 
| (8,2) | 0 | khớp chẵn lẻ trên ngưỡng | 

## Vỏ cạnh 

Một ranh giới quan trọng là khi$k = 0$. Trong trường hợp đó, điều kiện rút gọn thành việc tìm điểm$b$như vậy$|b| = |b - n|$, luôn đúng với một số nguyên$b$khi$n$được cân bằng ở khoảng cách chẵn. Theo thuật toán, nếu$n \ge 0$, chúng tôi trực tiếp kiểm tra tính chẵn lẻ$k = 0$. Nếu như$n$là số chẵn, câu trả lời là số không; nếu không thì một nước đi là đủ để lật ngang. 

Một trường hợp cạnh khác là khi$n = 0$. Nếu như$k > 0$, chúng ta rơi vào$n < k$nhánh và di chuyển trực tiếp đến$k$. Ví dụ,$n = 0, k = 1000000$mang lại câu trả lời$1000000$, phù hợp với nhu cầu đạt được vùng khả thi tối thiểu. 

Trường hợp cạnh cuối cùng là khi$n = k$. Thuật toán coi đây là chi phí bằng 0 vì điều kiện chẵn lẻ tự động được thỏa mãn và cấu hình đã nằm trên ranh giới nơi hợp lệ.$b$có thể được chọn.
