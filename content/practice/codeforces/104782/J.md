---
title: "CF 104782J - Hình bình hành"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm có một tập hợp các độ dài thanh. Từ bộ sưu tập này, chúng ta muốn biết liệu chúng ta có thể chọn ra bốn que riêng biệt sao cho sau khi xoay tự do và sắp xếp lại chúng trong mặt phẳng, chúng có thể tạo thành một hình bình hành bằng cách sử dụng tất cả…"
date: "2026-06-28T15:04:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104782
codeforces_index: "J"
codeforces_contest_name: "2023 Romanian Collegiate Programming Contest (RCPC)"
rating: 0
weight: 104782
solve_time_s: 56
verified: true
draft: false
---

[CF 104782J - Hình bình hành](https://codeforces.com/problemset/problem/104782/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm có một tập hợp các độ dài thanh. Từ bộ sưu tập này, chúng ta muốn biết liệu chúng ta có thể chọn ra bốn cây gậy riêng biệt sao cho sau khi xoay tự do và sắp xếp lại chúng trong mặt phẳng, chúng có thể tạo thành một hình bình hành sử dụng cả bốn cây gậy làm các cạnh của nó. 

Hình bình hành có hai cặp cạnh đối diện bằng nhau. Vì vậy, trong số bốn thanh dài đã chọn, chúng ta phải ghép chúng thành hai cặp bằng nhau. Nói cách khác, nếu chúng ta sắp xếp bốn giá trị đã chọn, chúng phải trông giống như$x, x, y, y$đối với một số độ dài dương$x$Và$y$. Thứ tự lựa chọn trong đầu vào không quan trọng ngoài các chỉ số riêng biệt. 

Kích thước đầu vào lớn: tổng số que trên tất cả các trường hợp thử nghiệm có thể đạt tới$2 \cdot 10^5$, và có tới$10^4$trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng kiểm tra tất cả các bộ bốn một cách rõ ràng, vì ngay cả một trường hợp thử nghiệm duy nhất với$n = 2 \cdot 10^5$sẽ làm$O(n^4)$hoặc$O(n^3)$không thể nào. 

Hạn chế chính là độ dài thanh được giới hạn bởi$n$, điều này cho thấy khả năng suy luận dựa trên tần số có thể thực hiện được trong thời gian tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi có nhiều giá trị giống hệt nhau. Ví dụ: nếu tất cả các que đều bằng nhau$[1, 1, 1, 1]$, câu trả lời rõ ràng là CÓ vì chúng ta có thể tạo thành một hình bình hành có tất cả các cạnh bằng nhau, đây là trường hợp đặc biệt. Một trường hợp khác là khi có chính xác hai giá trị khác nhau nhưng không đủ số lần lặp lại, như$[1, 1, 2, 3]$, trong đó chúng ta có thể có một cặp nhưng không thể tạo thành hai cặp đầy đủ. 

Một sai lầm ngây thơ là nghĩ rằng “hai cặp bất kỳ ở bất kỳ đâu trong mảng là đủ” mà không đảm bảo các chỉ số riêng biệt. Ví dụ: nếu các giá trị là$[1, 1, 2]$, chúng ta có thể lầm tưởng rằng mình có thể tạo thành hai cặp vì có một cặp số 1, nhưng chúng ta vẫn cần một cặp khác, cặp này không tồn tại. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ thử tất cả bốn lần$i < j < k < p$, kiểm tra bốn giá trị và kiểm tra xem chúng có thể được phân chia thành hai cặp bằng nhau hay không. Điều này đúng, bởi vì nó cố gắng hết sức để lựa chọn bốn cây gậy có thể. Tuy nhiên, số lượng bốn lần là$\binom{n}{4}$, tăng trưởng như$O(n^4)$. Ngay cả đối với$n = 2000$, tốc độ này đã quá chậm rồi, và đây$n$tùy thuộc vào$2 \cdot 10^5$, làm cho nó hoàn toàn không thể thực hiện được. 

Cấu trúc của điều kiện là thứ đơn giản hóa vấn đề. Chúng tôi không quan tâm đến chỉ số nào tạo thành các cặp ngoài việc đảm bảo tính khác biệt. Điều duy nhất quan trọng là liệu có tồn tại ít nhất hai giá trị riêng biệt, mỗi giá trị xuất hiện ít nhất hai lần hay không. Nếu chúng ta có một giá trị xuất hiện bốn lần thì điều đó cũng có tác dụng vì nó có thể tạo thành hai cặp có độ dài bằng nhau. Nếu chúng ta có hai giá trị khác nhau, mỗi giá trị xuất hiện ít nhất hai lần, chúng ta có thể gán một cặp cho mỗi giá trị. 

Vì vậy, vấn đề trở thành câu hỏi về tần số: chúng ta cần kiểm tra xem có tồn tại ít nhất hai cặp giá trị bằng nhau hay không, tính bội số một cách cẩn thận. Một giá trị có tần số ít nhất là bốn đã đóng góp hai cặp và hai giá trị có tần số ít nhất hai mỗi giá trị cũng đủ. 

Điều này làm giảm vấn đề quét tần số và đếm số lượng cặp rời rạc mà chúng ta có thể trích xuất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^4)$|$O(1)$| Quá chậm | 
| Đếm tần số |$O(n)$mỗi bài kiểm tra |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề sang việc đếm xem có bao nhiêu cặp giá trị bằng nhau tồn tại trong nhiều tập hợp độ dài thanh. 

1. Đếm tần số của mỗi chiều dài thanh bằng cách sử dụng bản đồ hoặc mảng băm. 

Bước này là cần thiết vì điều kiện ghép nối chỉ phụ thuộc vào số lượng bản sao của mỗi giá trị tồn tại chứ không phụ thuộc vào vị trí của chúng. 
2. Với mỗi giá trị riêng biệt có tần số$f$, hãy tính xem nó đóng góp bao nhiêu cặp rời nhau$f // 2$. 

Điều này nắm bắt số lượng cặp tối đa mà chúng ta có thể tạo từ giá trị đó mà không cần sử dụng lại các phần tử. 
3. Tính tổng các cặp này trên tất cả các giá trị. 

Tổng số đại diện cho tổng thể có bao nhiêu cặp có độ dài bằng nhau mà chúng ta có thể tạo thành. 
4. Nếu tổng số cặp ít nhất là 2, hãy viết chữ in CÓ. Ngược lại in ra NO. 

Chúng ta cần ít nhất hai cặp vì hình bình hành cần có hai cạnh đối diện có độ dài này và hai cạnh đối diện có độ dài khác. 

### Tại sao nó hoạt động 

Bất kỳ lựa chọn hợp lệ nào gồm bốn que tạo thành hình bình hành đều phải chia thành hai cặp bằng nhau. Mỗi cặp phải đến từ các giá trị giống hệt nhau, vì vậy mọi giải pháp hợp lệ đều tương ứng với việc chọn hai cặp rời nhau từ nhiều tập hợp tần số. Ngược lại, nếu chúng ta có thể trích xuất hai cặp rời nhau từ nhiều tập hợp, chúng ta có thể gán chúng là các cạnh đối diện của hình bình hành. Không có ràng buộc hình học nào ngoài sự bằng nhau của các cạnh đối diện ảnh hưởng đến tính khả thi vì được phép xoay và sắp xếp lại. 

Vì vậy, bài toán hoàn toàn tương đương với việc kiểm tra xem tổng số cặp bằng nhau có sẵn có ít nhất là hai hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1

        pairs = 0
        for v in freq.values():
            pairs += v // 2

        if pairs >= 2:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp được xây dựng xung quanh việc tổng hợp tần số trực tiếp. Từ điển tích lũy số lượng theo thời gian tuyến tính. Bước thứ hai chuyển đổi mỗi tần số thành số cặp có thể sử dụng được bằng phép chia số nguyên. Điều này tránh mọi nhu cầu theo dõi các chỉ số hoặc sự kết hợp thực tế một cách rõ ràng. 

Một cạm bẫy triển khai phổ biến là quên rằng một giá trị có tần số 4 đóng góp hai cặp, đó là lý do tại sao`v // 2`là điều cần thiết hơn là chỉ kiểm tra xem`v >= 2`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
4
1 2 2 3
```| Bước | Bản đồ tần số | Đóng góp theo cặp | Tổng số cặp | 
| --- | --- | --- | --- | 
| Bắt đầu | {} | 0 | 0 | 
| Sau khi quét | {1:1, 2:2, 3:1} | 1 (từ 2) | 1 | 

Chúng ta kết thúc chỉ với một cặp có thể sử dụng được (hai số 2). Vì hình bình hành cần có hai cặp nên câu trả lời là KHÔNG. 

### Ví dụ 2 

đầu vào:```
1
5
1 1 2 2 3
```| Bước | Bản đồ tần số | Đóng góp theo cặp | Tổng số cặp | 
| --- | --- | --- | --- | 
| Bắt đầu | {} | 0 | 0 | 
| Sau khi quét | {1:2, 2:2, 3:1} | 2 | 2 | 

Chúng ta có thể tạo thành một cặp từ 1 và một cặp từ 2, tạo ra hai cặp rời nhau. Điều này thỏa mãn điều kiện nên câu trả lời là CÓ. 

Những ví dụ này cho thấy điều kiện chỉ phụ thuộc vào bội số tổng hợp chứ không phụ thuộc vào cách các giá trị được xen kẽ trong mảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Một lượt đếm tần số và một lượt đếm các giá trị riêng biệt | 
| Không gian |$O(n)$| Bản đồ tần số lưu trữ số lượng chiều dài thanh khác nhau | 

Tổng cộng$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó nghiệm tuyến tính này phù hợp thoải mái trong giới hạn điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1
        pairs = sum(v // 2 for v in freq.values())
        out.append("YES" if pairs >= 2 else "NO")
    return "\n".join(out)

# provided samples
assert run("1\n4\n1 2 3 3\n") == "NO"
assert run("1\n4\n1 1 1 1\n") == "YES"

# custom cases
assert run("1\n3\n1 1 2\n") == "NO", "only one pair exists"
assert run("1\n4\n1 2 3 4\n") == "NO", "no pairs at all"
assert run("1\n6\n1 1 1 1 2 2\n") == "YES", "one value gives two pairs"
assert run("1\n6\n1 1 2 2 3 3\n") == "YES", "three pairs available"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 2 | KHÔNG | không đủ cặp | 
| 1 1 1 1 2 2 | CÓ | giá trị duy nhất có thể cung cấp hai cặp | 
| 1 1 2 2 3 3 | CÓ | nhiều nguồn cặp | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một giá trị duy nhất chiếm ưu thế trong mảng. Đối với đầu vào như`1 1 1 1`, tần số là 4, tạo ra hai cặp có cùng giá trị. Thuật toán tính toán`4 // 2 = 2`, trả về chính xác CÓ. 

Một trường hợp khác là khi các cặp tồn tại nhưng không đủ số lượng, chẳng hạn như`1 1 2 3`. Ở đây tần số tạo ra một cặp từ`1`và số không từ những người khác, tổng cộng là một. Thuật toán từ chối nó một cách chính xác. 

Một trường hợp lừa đảo hơn là`1 1 1 2 2`. Tần số là`1:3`Và`2:2`. Điều này mang lại`1 + 1 = 2`theo cặp, vì vậy câu trả lời là CÓ mặc dù có thể không nhìn thấy rõ ràng rằng bốn que có thể được sắp xếp một cách thích hợp. Sự trừu tượng ghép nối đảm bảo tính chính xác mà không cần phải suy luận về mặt hình học.
