---
title: "CF 1349B - Orac và trung vị"
description: "Chúng tôi được đưa ra một số truy vấn độc lập. Mỗi truy vấn cung cấp một chuỗi các số nguyên và giá trị đích $k$. Chúng ta được phép chọn nhiều lần bất kỳ đoạn liền kề nào của chuỗi và thay thế mọi phần tử trong đoạn đó bằng trung vị của đoạn đó."
date: "2026-06-11T14:35:16+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1349
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 641 (Div. 1)"
rating: 2000
weight: 1349
solve_time_s: 159
verified: true
draft: false
---

[CF 1349B - Orac và Medians](https://codeforces.com/problemset/problem/1349/B) 

**Xếp hạng:** 2000 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số truy vấn độc lập. Mỗi truy vấn cung cấp một chuỗi các số nguyên và một giá trị đích$k$. Chúng ta được phép chọn nhiều lần bất kỳ đoạn liền kề nào của chuỗi và thay thế mọi phần tử trong đoạn đó bằng trung vị của đoạn đó. Trung vị được xác định là phần tử ở giữa sau khi sắp xếp đoạn, với phần giữa phía dưới được chọn khi độ dài là chẵn. 

Câu hỏi đặt ra là liệu sau khi áp dụng bất kỳ số phép toán phân đoạn nào như vậy, liệu có thể làm cho toàn bộ mảng bằng$k$. 

Khó khăn chính là mỗi thao tác có tính phi cục bộ cao: một thao tác phân đoạn đơn sẽ ghi đè nhiều vị trí cùng một lúc và giá trị được ghi phụ thuộc vào thống kê thứ tự của phân đoạn đó thay vì tổng hoặc cực trị. Điều này làm cho vấn đề có vẻ như có thể yêu cầu theo dõi nhiều trạng thái có thể xảy ra, nhưng các ràng buộc sẽ loại trừ điều đó. Với tổng số$n \le 10^5$, mọi giải pháp mô phỏng hoạt động hoặc khám phá các phân đoạn một cách rõ ràng sẽ thất bại, vì ngay cả việc kiểm tra tất cả các phân đoạn cũng đã là bậc hai. 

Một trường hợp thất bại khó phát hiện phổ biến xuất phát từ việc giả định rằng phép toán trung vị có thể “tạo” các giá trị mới một cách tự do. Ví dụ, nếu$k$không xuất hiện ban đầu, người ta vẫn có thể hy vọng xây dựng nó bằng cách trộn các giá trị nhỏ hơn và lớn hơn. Điều này không chính xác vì mọi giá trị được tạo ra bởi một thao tác phải là một trong các phần tử trong phân đoạn đã chọn. Trung vị luôn là một phần tử hiện có của phân khúc đó. 

Một tình huống gây nhầm lẫn khác là khi$k$tồn tại trong mảng nhưng có vẻ như bị “mắc kẹt”. Ví dụ, ngay cả khi$k$hiện diện, không phải lúc nào cũng có thể trải rộng nó ra toàn bộ mảng tùy thuộc vào cách các giá trị được phân phối xung quanh nó. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng mô hình hóa tất cả các mảng có thể truy cập được. Từ bất kỳ mảng hiện tại nào, chúng ta có thể thử mọi phân đoạn, tính trung vị của nó, áp dụng thao tác và tiếp tục. Điều này tạo ra một quá trình phân nhánh rất lớn. Mỗi bước đều xem xét$O(n^2)$các phân đoạn và mỗi phép chuyển đổi sẽ thay đổi không gian trạng thái theo cách nhanh chóng vượt quá khả năng điều khiển. Ngay cả đối với nhỏ$n$, cách tiếp cận này trở nên không khả thi vì số lượng trạng thái có thể tiếp cận tăng theo cấp số nhân. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải mô phỏng quá trình này. Thay vào đó, chúng tôi hỏi những điều kiện cấu trúc nào phải đáp ứng để có thể đạt được trạng thái cuối cùng. Vì mọi thao tác đều thay thế một phân đoạn bằng một trong các giá trị hiện có của nó (trung vị), nên không thao tác nào có thể đưa ra một giá trị chưa có. Điều này ngay lập tức ngụ ý rằng nếu mảng cuối cùng là tất cả$k$, sau đó$k$phải tồn tại trong mảng ban đầu. 

Quan sát thứ hai là về khi$k$có thể “lây lan”. Để mở rộng một vùng$k$bằng cách sử dụng các phép toán trung vị, chúng ta cần tìm các đoạn có trung vị trở thành$k$. Điều đó đòi hỏi phân khúc phải được cân bằng xung quanh$k$, nghĩa là nó phải chứa các giá trị ở cả hai phía của$k$. Nếu tất cả các phần tử ngoại trừ$k$nằm hoàn toàn về một phía thì không có đoạn nào có thể tạo ra đoạn mới$k$-các khu vực chiếm ưu thế ngoài những gì đã tồn tại. 

Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem mảng có chứa$k$, và liệu sự phân bố xung quanh$k$không phải là một chiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tiểu bang | Hàm mũ | Hàm mũ | Quá chậm | 
| Kiểm tra tình trạng kết cấu |$O(n)$mỗi bài kiểm tra |$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng truy vấn một cách độc lập. 

1. Quét mảng để xác định xem$k$tồn tại. Nếu nó không tồn tại, chúng ta có thể kết luận ngay rằng không thể có được một mảng bao gồm toàn bộ$k$. This follows from the fact that medians are always drawn from existing elements in the chosen segment.
 2. Compute whether there exists at least one element strictly smaller than$k$. 
3. Tính xem có tồn tại ít nhất một phần tử lớn hơn$k$. 
4. Nếu mọi phần tử đều bằng$k$, chúng tôi trả về “CÓ” vì không cần thao tác nào. 
5. Ngược lại, chúng tôi chỉ trả về “CÓ” nếu cả hai đều nhỏ hơn-$k$và lớn hơn-$k$phần tử tồn tại. Nếu thiếu một bên, chúng tôi trả về “KHÔNG”. 

Trực giác đằng sau bước cuối cùng là việc truyền bá$k$ngoài những lần xuất hiện ban đầu, chúng ta cần những phân đoạn trong đó$k$có thể đóng vai trò là trung vị sau khi tái cân bằng các giá trị. Việc tái cân bằng như vậy đòi hỏi các giá trị ở cả hai phía của$k$. Nếu mảng nằm hoàn toàn về một phía của$k$, thì mọi trung vị vẫn ở cùng phía đó và$k$không thể chiếm ưu thế ở mọi nơi. 

### Tại sao nó hoạt động 

Phép toán trung vị không bao giờ đưa ra các giá trị mới bên ngoài phân đoạn đã chọn, vì vậy$k$phải có mặt ban đầu. Một lần$k$tồn tại, cách duy nhất để mở rộng ảnh hưởng của nó là thông qua các phân khúc có giá trị trung bình ước tính là$k$, đòi hỏi sự hiện diện của các giá trị nhỏ hơn và lớn hơn$k$. Nếu mảng là một phía so với$k$, mọi đoạn trung tuyến cũng vẫn ở một phía, ngăn chặn bất kỳ sự thay đổi cấu trúc nào theo hướng thống nhất$k$mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        has_k = False
        has_less = False
        has_greater = False
        
        for x in a:
            if x == k:
                has_k = True
            elif x < k:
                has_less = True
            else:
                has_greater = True
        
        if not has_k:
            print("NO")
        elif all(x == k for x in a):
            print("YES")
        else:
            if has_less and has_greater:
                print("YES")
            else:
                print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai tách mảng thành ba loại logic liên quan đến$k$: bằng nhau, nhỏ hơn và lớn hơn. Việc kiểm tra sự hiện diện cho$k$là cần thiết vì nếu không có sự xuất hiện ban đầu thì không có chuỗi phép tính trung vị nào có thể tạo ra nó. 

Logic quyết định cuối cùng tuân theo trực tiếp từ đặc tính cấu trúc: mảng đã đồng nhất hoặc nó phải chứa các giá trị ở cả hai phía của$k$để cho phép bất kỳ sự lan truyền nào vượt quá những lần xuất hiện ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
1 5 2 6 1
```Chúng tôi theo dõi các điều kiện chính: 

| Bước | has_k | has_less | has_Greater | Quyết định | 
| --- | --- | --- | --- | --- | 
| Quét mảng | Sai | Đúng | Đúng | KHÔNG | 

Từ$k = 3$không xuất hiện ở đâu cả, không thao tác nào có thể giới thiệu được nó. Mọi trung vị đều được chọn từ các giá trị phân đoạn hiện có, vì vậy giá trị 3 không thể truy cập được. 

### Ví dụ 2 

đầu vào:```
3 2
1 2 3
```| Bước | has_k | has_less | has_Greater | Quyết định | 
| --- | --- | --- | --- | --- | 
| Quét mảng | Đúng | Đúng | Đúng | CÓ | 

Đây$k$tồn tại và có các giá trị ở cả hai vế của nó. Điều này cho phép xây dựng các phân đoạn có trung vị ổn định ở mức 2, cho phép truyền bá cho đến khi toàn bộ mảng trở nên đồng nhất. 

Điều này chứng tỏ rằng có cả hai mặt xung quanh$k$là đủ ngay cả trong mảng nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi truy vấn | Mỗi phần tử được quét một lần để phân loại tương ứng với$k$| 
| Không gian |$O(1)$| Chỉ một số cờ boolean được lưu trữ | 

Tổng cộng$n$trên tất cả các trường hợp thử nghiệm là$10^5$, do đó việc quét tuyến tính cho mỗi truy vấn dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        has_k = False
        has_less = False
        has_greater = False

        for x in a:
            if x == k:
                has_k = True
            elif x < k:
                has_less = True
            else:
                has_greater = True

        if not has_k:
            out.append("NO")
        elif all(x == k for x in a):
            out.append("YES")
        else:
            out.append("YES" if (has_less and has_greater) else "NO")

    return "\n".join(out)

# provided samples
assert run("""5
5 3
1 5 2 6 1
1 6
6
3 2
1 2 3
4 3
3 1 2 3
10 3
1 2 3 4 5 6 7 8 9 10
""") == """NO
YES
YES
NO
YES"""

# all equal
assert run("""1
4 7
7 7 7 7
""") == "YES"

# missing k
assert run("""1
5 10
1 2 3 4 5
""") == "NO"

# one-sided distribution
assert run("""1
5 3
1 2 3 3 3
""") == "NO"

# two-sided around k
assert run("""1
5 3
1 3 4 5 2
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | CÓ | trạng thái đã được giải quyết | 
| thiếu k | KHÔNG | không thể thực hiện được do thiếu k | 
| một chiều | KHÔNG | không thể truyền k nếu không có cả hai bên | 
| hai mặt | CÓ | điều kiện đủ để nhân giống | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$n = 1$. Mảng đã đồng nhất nên câu trả lời chỉ phụ thuộc vào việc giá trị đó có bằng hay không$k$. Nếu có, câu trả lời tầm thường là “CÓ”, nếu không thì “KHÔNG”. 

Một trường hợp khác là khi tất cả các giá trị đều nhỏ hơn$k$hoặc thực sự lớn hơn$k$. Kể cả nếu$k$xuất hiện ở đâu đó trong mảng, việc mất một phía của phổ giá trị sẽ ngăn cản mọi hoạt động trung bình ổn định xung quanh$k$ở các vùng mới, ngăn cản sự lan truyền hoàn toàn. 

Một trường hợp tế nhị cuối cùng là khi$k$tồn tại nhưng không phải là “trung tâm” trong phân phối. Nếu mọi giá trị đều ở một phía của$k$, thì mọi trung vị của đoạn vẫn ở phía đó, do đó không có thao tác nào có thể mở rộng ảnh hưởng của (k` ngoài các vị trí bị cô lập.
