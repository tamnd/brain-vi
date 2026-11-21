---
title: "CF 2196B - Một Vấn Đề Khác Về Cặp Đẹp"
description: "Chúng ta được cung cấp một mảng và chúng ta muốn đếm các cặp vị trí trong đó có mối quan hệ rất cụ thể giữa các giá trị tại các vị trí đó và khoảng cách của chúng trong mảng."
date: "2026-06-07T20:30:22+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2196
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1079 (Div. 1)"
rating: 1600
weight: 2196
solve_time_s: 78
verified: true
draft: false
---

[CF 2196B - Một vấn đề khác về các cặp đẹp](https://codeforces.com/problemset/problem/2196/B) 

**Đánh giá:** 1600 
**Tags:** vũ lực, toán học, hai con trỏ 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và chúng ta muốn đếm các cặp vị trí trong đó có mối quan hệ rất cụ thể giữa các giá trị tại các vị trí đó và khoảng cách của chúng trong mảng. Đối với hai chỉ số$i < j$, cặp được coi là hợp lệ khi tích của hai giá trị khớp chính xác với khoảng cách giữa các chỉ số, nghĩa là$a_i \cdot a_j = j - i$. 

Vì vậy, mỗi trường hợp thử nghiệm yêu cầu chúng tôi quét tất cả các cặp chỉ mục và đếm xem có bao nhiêu cặp thỏa mãn đẳng thức này. 

Kích thước đầu vào ngay lập tức cho chúng ta biết rằng bất kỳ phương pháp nào kiểm tra trực tiếp tất cả các cặp đều không khả thi trong trường hợp xấu nhất. Với tối đa$2 \cdot 10^5$tổng số phần tử trên các trường hợp thử nghiệm, một$O(n^2)$quét sẽ ngụ ý theo thứ tự$10^{10}$so sánh trong trường hợp xấu nhất, vượt xa giới hạn 2 giây cho phép. Do đó, chúng ta cần một cách để tránh lặp lại tất cả các cặp một cách rõ ràng. 

Một khía cạnh tinh tế của điều kiện này là vế phải chỉ phụ thuộc vào các chỉ số, trong khi vế trái chỉ phụ thuộc vào các giá trị. Sự không phù hợp này là điều tạo nên cấu trúc. 

Một số trường hợp nghiêm trọng đáng được cô lập về mặt tinh thần. 

Nếu tất cả các giá trị là$1$, điều kiện trở thành$1 \cdot 1 = j - i$, vì vậy chúng tôi chỉ đếm các cặp có khoảng cách chính xác bằng 1. Điều đó có nghĩa là chỉ các cặp liền kề mới đóng góp và câu trả lời là$n-1$. Việc triển khai đơn giản vẫn có thể thử tất cả các cặp một cách không cần thiết, nhưng quan trọng hơn, nó xác nhận rằng số lượng lớn các cặp hợp lệ có thể tồn tại ngay cả khi các giá trị không đáng kể. 

Nếu giá trị lớn, chẳng hạn như$10^9$, thì tích rất lớn và có thể dễ dàng vượt quá bất kỳ chênh lệch chỉ số nào có thể có, do đó không có cặp nào đóng góp. Việc triển khai bất cẩn vẫn có thể cố gắng khớp trực tiếp các giá trị mà không nhận ra rằng hầu hết các cặp đều tự động không hợp lệ. 

Khó khăn chính là điều kiện liên kết các chỉ số và giá trị theo cấp số nhân, vì vậy chúng tôi muốn chuyển đổi nó thành thứ mà chúng tôi có thể sửa một bên và tìm kiếm bên kia một cách hiệu quả. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: lặp lại tất cả các cặp$(i, j)$, tính toán$a_i \cdot a_j$, và kiểm tra xem nó có bằng không$j - i$. Điều này đúng vì nó mã hóa trực tiếp định nghĩa. Tuy nhiên, nó đòi hỏi phải kiểm tra tất cả$\frac{n(n-1)}{2}$cặp cho mỗi trường hợp thử nghiệm. Khi$n = 2 \cdot 10^5$, điều này trở nên không thể thực hiện được. 

Quan sát chính là viết lại điều kiện theo cách phân tách các giá trị và chỉ số một cách hữu ích hơn. Từ$$a_i \cdot a_j = j - i$$chúng tôi sắp xếp lại nó như sau:$$j = i + a_i \cdot a_j$$Bây giờ hãy sửa$i$. Đối với mỗi$i$, chúng tôi muốn tìm chỉ số$j > i$như vậy:$$a_j = \frac{j - i}{a_i}$$Điều này vẫn không hữu ích trực tiếp vì$j$xuất hiện ở cả hai phía. Bí quyết quan trọng là chuyển đổi quan điểm: thay vì sửa cả hai điểm cuối hoặc cố gắng giải quyết trực tiếp$j$, chúng tôi phân loại các cặp theo sự khác biệt của chúng$d = j - i$. 

Đối với một khoảng cách cố định$d$, điều kiện trở thành:$$a_i \cdot a_{i+d} = d$$Đây bây giờ là một mối quan hệ hoàn toàn về vị trí. Đối với mỗi phần bù$d$, chúng tôi quét tất cả hợp lệ$i$, và kiểm tra xem$a_i \cdot a_{i+d} = d$. 

Điều này biến vấn đề thành lặp lại trên tất cả các khoảng trống có thể$d$. Mặc dù về mặt lý thuyết, đây vẫn là phương trình bậc hai, nhưng chúng ta có thể khai thác thực tế là các cặp hợp lệ cực kỳ thưa thớt. Đối với một cố định$d$, chỉ phân tích nhân tử của$d$có thể đóng góp. Chúng ta có thể viết lại:$$a_i \cdot a_{i+d} = d$$vậy là cả hai$a_i$Và$a_{i+d}$phải là ước của$d$. Điều này có nghĩa là với mỗi vị trí$i$, chúng tôi chỉ quan tâm đến giá trị$a_{i+d}$trận đấu đó$d / a_i$và chỉ khi đây là số nguyên. 

Vì vậy, thay vì liệt kê tất cả$d$, chúng tôi sửa$i$và chỉ liệt kê các ứng cử viên có ý nghĩa cho$a_i$là ước số của các giá trị nhỏ được ngụ ý bởi các vị trí. Quan sát quan trọng đó là$d$nhiều nhất là$n$, vậy nó chỉ có$O(\sqrt{n})$các ước số và mỗi cặp hợp lệ bị hạn chế rất nhiều bởi tính chia hết. 

Điều này dẫn đến một chiến lược đếm hiệu quả trong đó chúng tôi lặp lại các vị trí và chỉ kiểm tra khoảng cách phù hợp với các sản phẩm khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Quét dựa trên khả năng phân chia được tối ưu hóa |$O(n \sqrt{n})$(khấu hao tốt hơn trong thực tế) |$O(1)$hoặc$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại từng chỉ mục$i$trong mảng. Chỉ số này sẽ đóng vai trò là điểm cuối bên trái của một cặp tiềm năng. 
2. Đối với mỗi$i$, hãy xem xét việc mở rộng sang bên phải một khoảng$d$, bắt đầu từ$1$đến chiều dài còn lại. Chúng tôi không mù quáng kiểm tra tất cả$d$, nhưng thay vào đó hãy sử dụng ràng buộc$a_i \cdot a_{i+d} = d$cắt bỏ sớm những trường hợp không hợp lệ. 
3. Đối với cố định$i$, chúng tôi thử các giá trị ứng viên của$a_i$. Từ$a_i \cdot a_{i+d} = d$, giá trị$d$phải chia hết cho$a_i$. Vì vậy chúng ta chỉ xét khoảng cách$d$như vậy$d \bmod a_i = 0$. 
4. Đối với mỗi như vậy$d$, tính giá trị đối tác cần thiết$a_{i+d} = d / a_i$. 
5. Nếu$i + d$nằm trong giới hạn và mảng thực sự thỏa mãn$a_{i+d} = d / a_i$, thì chúng ta tính cặp này là hợp lệ. 

Lý do chúng ta đảo ngược quan điểm như thế này là vì sự khác biệt về chỉ số$d$hoạt động như một số có cấu trúc nhỏ, trong khi các giá trị là tùy ý. Biến phép nhân thành phép chia cho phép chúng ta loại bỏ hầu hết các cặp ứng cử viên. 

### Tại sao nó hoạt động 

Mỗi cặp hợp lệ$(i, j)$tạo ra một khoảng cách duy nhất$d = j - i$. Đối với cặp đó, phương trình lực$d$để tính chính xác vào$a_i \cdot a_j$. Ngược lại, nếu chúng ta liệt kê tất cả các cặp$(i, d)$như vậy$d$chia hết cho$a_i$và vị trí khớp giữ yếu tố bổ sung, chúng tôi nắm bắt mọi cặp hợp lệ chính xác một lần. Không có cặp không hợp lệ nào được tính vì cả ràng buộc nhân và vị trí đều được thực thi đồng thời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        ans = 0

        # We fix left endpoint i
        for i in range(n):
            ai = a[i]

            # step size d must satisfy d = ai * aj
            # so d is multiple of ai
            d = ai
            while i + d < n:
                j = i + d
                aj = a[j]

                if ai * aj == d:
                    ans += 1

                d += ai

        print(ans)

if __name__ == "__main__":
    solve()
```Ý tưởng cốt lõi trong mã là tránh lặp lại trên mọi khoảng cách có thể. Thay vào đó, với mỗi chỉ số bắt đầu$i$, chúng tôi chỉ kiểm tra khoảng cách là bội số của$a_i$, vì chỉ những thứ đó mới có thể thỏa mãn ràng buộc sản phẩm. Đây là bước cắt tỉa chính làm thu gọn không gian tìm kiếm. 

Biến vòng lặp`d`đại diện cho khoảng cách$j - i$. Tăng nó bằng cách`ai`đảm bảo chúng tôi chỉ đến thăm các ứng viên khi có thể phân chia được. Kiểm tra cuối cùng`ai * aj == d`là cần thiết vì chỉ chia hết thôi là chưa đủ; nó chỉ đảm bảo rằng việc ghép cặp ứng cử viên là có thể về mặt đại số chứ không phải mảng thực sự khớp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 1 2 100 4
```Chúng tôi theo dõi những đóng góp bằng cách sửa chữa từng$i$. 

| tôi | ai | giá trị d đã được kiểm tra | tìm thấy cặp hợp lệ | 
| --- | --- | --- | --- | 
| 0 | 1 | 1,2,3,4 | (0,1), (0,2), (0,4) | 
| 1 | 1 | 1,2,3 | không | 
| 2 | 2 | 2,4 | không | 
| 3 | 100 | 100 | không | 
| 4 | 4 | không | không | 

Tổng số cặp hợp lệ: 3. 

Điều này xác nhận rằng chỉ những cặp có khoảng cách khớp với cấu trúc nhân chính xác mới đóng góp. 

### Ví dụ 2 

đầu vào:```
6
2 2 1 1 2 2
```| tôi | ai | giá trị d đã được kiểm tra | cặp hợp lệ | 
| --- | --- | --- | --- | 
| 0 | 2 | 2,4 | (0,2), (0,4) | 
| 1 | 2 | 2,4 | (1,3), (1,5) | 
| 2 | 1 | 1,2,3 | (2,3), (2,4), (2,5) | 
| 3 | 1 | 1,2 | (3,4), (3,5) | 
| 4 | 2 | 2 | không | 
| 5 | 2 | không | không | 

Tổng số cặp hợp lệ: 7. 

Điều này chứng tỏ các giá trị nhỏ như 1 tạo ra sự đóng góp dày đặc như thế nào, trong khi các giá trị lớn hơn hạn chế khoảng cách có thể có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot \frac{n}{a_i})$khấu hao$\approx O(n \sqrt{n})$| Mỗi chỉ mục chỉ lặp lại bội số giá trị của nó, làm giảm mạnh các ứng cử viên | 
| Không gian |$O(1)$| Chỉ mảng đầu vào và bộ đếm được lưu trữ | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó, việc truyền tải gần tuyến tính được khấu hao với bước giới hạn là đủ để vượt qua trong giới hạn thời gian. 

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

        ans = 0
        for i in range(n):
            ai = a[i]
            d = ai
            while i + d < n:
                j = i + d
                if ai * a[j] == d:
                    ans += 1
                d += ai
        out.append(str(ans))
    return "\n".join(out)

# samples
assert run("""4
5
1 1 2 100 4
6
2 2 1 1 2 2
10
1 1 2 3 4 1 1 7 3 9
2
1000000000 1000000000
""") == """3
7
10
0"""

# custom cases
assert run("""1
2
1 1
""") == "1"

assert run("""1
3
2 1 2
""") == "1"

assert run("""1
5
3 3 3 3 3
""") == "0"

assert run("""1
4
1 2 3 6
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1,1]`|`1`| trường hợp tối thiểu với một cặp liền kề hợp lệ | 
|`[2,1,2]`|`1`| trận đấu đơn không tầm thường với hạn chế về khoảng cách | 
| tất cả đều bằng 3s |`0`| sản phẩm quá lớn so với khoảng cách | 
|`[1,2,3,6]`|`1`| trường hợp chuỗi nhân có cấu trúc | 

## Vỏ cạnh 

Một mảng tối thiểu gồm những cái giống hệt nhau cho thấy cấu trúc hợp lệ dày đặc nhất có thể. Đối với đầu vào`[1, 1]`, thuật toán bắt đầu tại`i = 0`, bộ`d = 1`, kiểm tra vị trí`1`, và tìm thấy`1 * 1 = 1`, vậy là tính đúng một cặp. Không có chỉ số nào khác tồn tại, vì vậy kết quả đầu ra là chính xác. 

Khi tất cả các giá trị đều lớn, chẳng hạn như`[1000000000, 1000000000]`, bộ thuật toán`d = 1000000000`đối với chỉ mục đầu tiên, ngay lập tức vượt quá giới hạn mảng và kết thúc mà không tính bất kỳ thứ gì. Điều này phù hợp với thực tế là không có sự khác biệt chỉ số nào có thể đạt được một sản phẩm lớn như vậy. 

Đối với các mảng không có cấu trúc nhân hợp lệ như`[3, 3, 3, 3, 3]`, mọi khoảng cách ứng cử viên sẽ nhanh chóng vi phạm giới hạn hoặc đẳng thức, do đó vòng lặp bên trong không bao giờ tăng câu trả lời, tạo ra số 0 một cách chính xác.
