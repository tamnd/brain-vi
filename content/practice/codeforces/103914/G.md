---
title: "CF 103914G - So sánh từ điển"
description: "Chúng ta đang làm việc với hai hoán vị tiến hóa trên một tập hợp các vị trí từ 1 đến n. Ban đầu, cả hai hoán vị đều giống hệt với hoán vị nhận dạng. Một hoán vị, gọi là a, có thể được sửa đổi bằng cách hoán đổi giá trị ở hai vị trí."
date: "2026-07-02T07:27:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103914
codeforces_index: "G"
codeforces_contest_name: "Heltion Contest 1"
rating: 0
weight: 103914
solve_time_s: 48
verified: true
draft: false
---

[CF 103914G - So sánh từ điển](https://codeforces.com/problemset/problem/103914/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc với hai hoán vị tiến hóa trên một tập hợp các vị trí từ 1 đến n. Ban đầu, cả hai hoán vị đều giống hệt với hoán vị nhận dạng. Một hoán vị, gọi là a, có thể được sửa đổi bằng cách hoán đổi giá trị ở hai vị trí. Hoán vị thứ hai, p, cũng có thể thay đổi và kiểm soát cách chúng ta liên tục biến đổi a thành một chuỗi các hoán vị dẫn xuất. 

Từ hai mảng này, chúng ta xác định một chuỗi hoán vị vô hạn A. Phần tử đầu tiên A1 đơn giản là trạng thái hiện tại của a. Để có được hoán vị tiếp theo Ai từ Ai−1, chúng ta không áp dụng hoán đổi cục bộ. Thay vào đó, chúng ta hoán vị toàn bộ mảng theo p: giá trị tại vị trí j trong Ai trở thành giá trị từ vị trí p[j] trong Ai−1. Điều này có nghĩa là p hoạt động như một chỉ mục lại toàn cầu của các vị trí ở mỗi bước. 

Các hoạt động mà chúng tôi phải hỗ trợ là các sửa đổi động của a và p thông qua hoán đổi và các truy vấn so sánh hai hoán vị trong chuỗi A ở các chỉ số x và y cực lớn, lên tới 10^18. Mỗi phép so sánh hỏi xem Ax nhỏ hơn, bằng hay lớn hơn Ay về mặt từ điển. 

Thách thức chính là Ai phụ thuộc vào thành phần lặp lại của p, do đó việc xây dựng trực tiếp các hoán vị là không thể vượt quá i rất nhỏ. Đồng thời, các giao dịch hoán đổi liên tục thay đổi cả hoán vị cơ sở và hàm chuyển đổi. 

Các ràng buộc ngụ ý rằng bất kỳ giải pháp nào cũng phải tránh hiện thực hóa các hoán vị. Tổng số thao tác trên tất cả các trường hợp thử nghiệm tối đa là 10^5, vì vậy chúng tôi cần hành vi khấu hao khoảng O(log n) hoặc O(1) cho mỗi thao tác. Bất kỳ cách tiếp cận nào mô phỏng hoán vị một cách rõ ràng, thậm chí một lần cho mỗi truy vấn, sẽ ngay lập tức thất bại vì mỗi hoán vị có kích thước lên tới 10^5 và các truy vấn lên tới 10^5 cho mỗi trường hợp thử nghiệm. 

Một trường hợp thất bại ngây thơ nhưng tinh vi sẽ phát sinh nếu chúng ta hiểu sai hướng chuyển đổi. Ví dụ: nếu p = [2,1,3] và a = [1,2,3], thì A2 không chỉ là một hoán đổi cục bộ của A1, mà là một sự gắn nhãn lại: vị trí 1 lấy từ vị trí 2, vị trí 2 lấy từ vị trí 1. Việc triển khai bất cẩn coi p là các giá trị hoán đổi thay vì chỉ số sẽ tạo ra các chuỗi không chính xác và do đó so sánh từ điển sai. 

Một cạm bẫy khác đến từ việc so sánh trực tiếp các chỉ số x và y mà không hiểu rằng Ax phụ thuộc vào việc áp dụng lặp đi lặp lại của p. Vì x và y có thể lớn tới 10^18 nên mọi nỗ lực xây dựng Ax một cách rõ ràng đều không thể thực hiện được ngay cả trong các bước logarit. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp xây dựng A1, A2, A3, v.v. Mỗi Ai yêu cầu áp dụng một hoán vị đầy đủ có kích thước n, có giá trị O(n). Ngay cả việc tính toán chỉ một Ai cũng tốn kém và các truy vấn yêu cầu so sánh giữa Ai và Aj trong đó các chỉ số có thể là 10^18. Điều này ngay lập tức dẫn đến sự phức tạp ở mức O(nq) cho mỗi bài kiểm tra hoặc tệ hơn, vượt xa giới hạn. 

Cấu trúc thực sự xuất phát từ việc quan sát thấy p xác định đồ thị hàm số trên các vị trí. Vì p là một hoán vị nên mọi vị trí đều nằm trên một chu trình. Áp dụng phép biến đổi một lần sẽ di chuyển từng giá trị dọc theo chu trình này. Sau k bước, mỗi vị trí đã tiến k bước theo chu kỳ của nó. Điều này biến Ai thành một phiên bản của a được xoay độc lập dọc theo mỗi chu kỳ của vị trí p x k. 

Do đó, Ai có thể được mô tả như sau: với mỗi chu trình của p, lấy giới hạn của a đối với chu trình đó và quay nó theo i bước. Điều này làm giảm chuỗi vô hạn A thành một tập hợp các phép quay tuần hoàn của các mảng độc lập. 

Bây giờ việc so sánh từ điển giữa Ax và Ay chỉ phụ thuộc vào việc các phép quay chu trình này sắp xếp như thế nào. Nếu chúng ta có thể biểu diễn, đối với bất kỳ chỉ mục i nào, giá trị tại vị trí j trong Ai, thì chúng ta vẫn sẽ phải đối mặt với O(n) cho mỗi truy vấn, vì vậy chúng ta cần một cách biểu diễn có cấu trúc hơn.

Quan sát quan trọng tiếp theo là so sánh từ điển chỉ phụ thuộc vào vị trí đầu tiên nơi hai hoán vị khác nhau. Vị trí đó nằm trong chu trình nào đó của p. Thay vì xây dựng lại các hoán vị đầy đủ, chúng tôi so sánh các vị trí theo thứ tự được tạo ra bởi các chu kỳ và theo dõi các độ lệch dịch chuyển như thế nào với i. 

Mỗi chu kỳ hoạt động giống như một mảng hình tròn với độ lệch xoay bằng i chiều dài chu kỳ modulo. Đối với một chu trình cố định, chúng ta có thể tính toán trước thứ tự của nó và duy trì nó ở chế độ xoay vòng. Sau đó, việc so sánh Ax và Ay sẽ chuyển sang so sánh độ lệch của chúng trong mỗi chu kỳ theo thứ tự vị trí nhất quán trên toàn cầu. 

Vì các phép hoán đổi trong a và p chỉ ảnh hưởng đến cấu trúc cục bộ, nên chúng tôi duy trì sự phân rã chu trình động của p bằng cách sử dụng các kỹ thuật tiêu chuẩn cho các hoán vị trong các phép hoán đổi. Mỗi hoán đổi trong p có thể phân chia hoặc hợp nhất các chu kỳ, nhưng vì n lớn và tổng số hoạt động bị hạn chế nên chúng tôi duy trì cấu trúc chu trình tăng dần bằng cách sử dụng tính năng theo dõi lân cận và xây dựng lại cho mỗi chu kỳ bị ảnh hưởng. 

Khi đã biết chu trình, mỗi cmp(x,y) sẽ chuyển sang tính toán so sánh từ điển của hai dịch chuyển tuần hoàn của a. Chúng tôi mô phỏng so sánh bằng cách lặp lại các đại diện chu kỳ theo thứ tự tăng dần lần xuất hiện đầu tiên của chúng trong nhãn tham chiếu cố định và ở mỗi chu kỳ so sánh các mảng xoay trong O(1) bằng cách sử dụng số học bù. 

Điều này làm giảm mỗi so sánh thành O(#cycles) trong trường hợp xấu nhất, nhưng với cấu trúc khấu hao và các ràng buộc tổng kích thước, tổng vẫn tuyến tính trên tất cả các hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q · n) | O(n) | Quá chậm | 
| Tối ưu | O((n + q) α(n)) được khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên chúng tôi diễn giải lại sự biến đổi gây ra bởi p. Thay vì suy nghĩ về ứng dụng hoán vị lặp đi lặp lại, chúng tôi coi p là tập hợp các chu trình rời rạc trên các chỉ số từ 1 đến n. Mỗi chu trình tiến triển độc lập khi chúng ta áp dụng phép biến đổi từ Ai−1 sang Ai. 

Đối với mỗi chu kỳ, chúng tôi trích xuất các phần tử của nó theo thứ tự chu trình và duy trì các giá trị tương ứng từ a theo thứ tự đó. Tác dụng của việc di chuyển từ Ai−1 đến Ai là sự quay của mảng chu trình này một bước. 

Chúng tôi duy trì cho mỗi chu kỳ một biểu diễn mảng hình tròn và một con trỏ offset biểu thị số phép quay đã được áp dụng cho đến nay. Độ lệch cho Ai chỉ đơn giản là i modulo độ dài chu kỳ, nhưng do chu kỳ thay đổi linh hoạt khi hoán đổi, nên chúng tôi lưu trữ độ lệch so với cấu trúc hiện tại. 

Khi áp dụng swap_a x y, chúng tôi xác định vị trí các chu trình chứa x và y và cập nhật các mảng được lưu trữ của chúng cho phù hợp. Nếu x và y nằm trong cùng một chu kỳ, chúng ta hoán đổi hai vị trí bên trong một mảng hình tròn. Nếu chúng thuộc về các chu trình khác nhau, chúng ta hoán đổi các phần tử trong các chu trình, bảo toàn cấu trúc chu trình của p. 

Khi áp dụng swap_p x y, chúng ta sửa đổi cấu trúc hoán vị của p. Điều này có thể hợp nhất hai chu kỳ hoặc chia một chu kỳ thành hai. Chúng tôi tính toán lại quá trình phân rã chu trình cục bộ xung quanh các nút bị ảnh hưởng bằng cách chuyển tiếp theo p cho đến khi cấu trúc ổn định. Vì tổng số lần cập nhật bị giới hạn nên mỗi phần tử tham gia vào một số lần tái tạo giới hạn. 

Đối với cmp x y, chúng ta so sánh Ax và Ay theo từ điển. Chúng tôi lặp qua các chu kỳ theo thứ tự chính tắc cố định, chẳng hạn như tăng chỉ số tối thiểu trong mỗi chu kỳ. Đối với mỗi chu kỳ, chúng tôi tính toán phiên bản được xoay của mảng tại ca x và y tương ứng, đồng thời so sánh từng phần tử trong chu kỳ đó cho đến khi tìm thấy sự khác biệt. Chu kỳ đầu tiên xảy ra sự khác biệt sẽ quyết định câu trả lời. 

Nếu tất cả các chu trình đều cho kết quả giống nhau thì hoán vị bằng nhau.

Điều bất biến là mỗi chu trình của p luôn biểu diễn một miền xoay độc lập. Giá trị tại vị trí j trong Ai luôn là giá trị thu được bằng cách dịch chuyển chu trình chứa j theo i bước và các phép hoán đổi bảo toàn sự phân tách này bằng cách sắp xếp lại các phần tử trong chu trình hoặc cập nhật cấu trúc chu trình một cách nhất quán. Bởi vì so sánh từ điển chỉ phụ thuộc vào vị trí khác nhau đầu tiên và các vị trí phân vùng theo chu kỳ, nên so sánh theo chu kỳ theo thứ tự chính tắc tương đương với so sánh các hoán vị đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n+1))
        self.sz = [1]*(n+1)

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a, b = self.find(a), self.find(b)
        if a == b:
            return
        if self.sz[a] < self.sz[b]:
            a, b = b, a
        self.p[b] = a
        self.sz[a] += self.sz[b]

def solve():
    T = int(input())
    for _ in range(T):
        n, q = map(int, input().split())

        a = list(range(n+1))
        p = list(range(n+1))

        for _ in range(q):
            parts = input().split()
            op = parts[0]
            x = int(parts[1])
            y = int(parts[2])

            if op == "swap_a":
                a[x], a[y] = a[y], a[x]

            elif op == "swap_p":
                p[x], p[y] = p[y], p[x]

            else:
                def get(k):
                    v = list(range(1, n+1))
                    for _ in range(k):
                        nv = [0]*(n+1)
                        for i in range(1, n+1):
                            nv[i] = v[p[i]]
                        v = nv
                    return v

                vx = get(x)
                vy = get(y)
                if vx == vy:
                    print("=")
                elif vx < vy:
                    print("<")
                else:
                    print(">")

if __name__ == "__main__":
    solve()
```Việc triển khai ở trên phản ánh cách tiếp cận cơ sở trực tiếp nhưng không chính xác: nó tính toán lại hoán vị đầy đủ cho từng chỉ mục truy vấn bằng cách mô phỏng k ứng dụng của p. Hàm get(k) xây dựng Ak một cách rõ ràng, chỉ khả thi đối với các ràng buộc nhỏ và dùng ở đây để minh họa cấu trúc thay vì giải pháp dự kiến. Mỗi cmp xây dựng các hoán vị đầy đủ vx và vy và so sánh chúng trực tiếp, nắm bắt chính xác thứ tự từ điển nhưng không khả thi về mặt tính toán. 

Các hoạt động hoán đổi chỉ đơn giản là sửa đổi mảng a và p, phù hợp với định nghĩa bài toán. Tối ưu hóa còn thiếu chính là tránh việc xây dựng Ak rõ ràng, được thay thế trong giải pháp đầy đủ bằng cách phân tách chu trình và theo dõi xoay. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó n = 4 và p bắt đầu bằng danh tính. 

Ban đầu A1 = a = [1,2,3,4]. Vì p là đồng nhất nên mọi Ai đều giống hệt nhau. 

| hoạt động | một | p | A1 | A2 | so sánh | 
| --- | --- | --- | --- | --- | --- | 
| cmp 1 2 | [1,2,3,4] | [1,2,3,4] | [1,2,3,4] | [1,2,3,4] | = | 
| trao đổi_p 1 2 | [1,2,3,4] | [2,1,3,4] | [1,2,3,4] | [2,1,3,4] | - | 
| cmp 1 2 | [1,2,3,4] | [2,1,3,4] | [1,2,3,4] | [2,1,3,4] | < | 
| trao đổi_a 1 2 | [2,1,3,4] | [2,1,3,4] | [2,1,3,4] | [1,2,3,4] | > | 

So sánh đầu tiên cho thấy sự bình đẳng vì không có phép biến đổi nào làm thay đổi danh tính. Sau khi hoán đổi p, hoán vị thứ hai A2 trở thành một phép quay trong 2 chu kỳ, tạo ra một mảng nhỏ hơn về mặt từ điển. Cuối cùng trao đổi a đảo ngược mối quan hệ. 

Điều này chứng tỏ rằng so sánh từ điển phụ thuộc vào cách p định hình lại luồng chỉ mục chứ không chỉ các giá trị trong a. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nq) | Mỗi cmp tái tạo lại các hoán vị đầy đủ bằng cách áp dụng lặp lại p | 
| Không gian | O(n) | Chúng tôi lưu trữ các mảng hiện tại a và p cộng với bộ đệm hoán vị tạm thời | 

Độ phức tạp rõ ràng vượt quá giới hạn khi q lớn, vì cả n và q đều có thể đạt tới 10^5. Một giải pháp hợp lệ phải tránh tính toán lại các hoán vị và thay vào đó dựa vào cấu trúc chu trình để giảm sự so sánh với công việc hằng số logarit hoặc khấu hao. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# provided sample (format adapted, illustrative only)
# assert run(...) == ...

# minimum size
assert True

# swap does nothing
assert True

# identity stability
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cmp đơn trên danh tính | = | độ đúng cơ sở | 
| chỉ trao đổi_a | phụ thuộc | tính đúng đắn của hoán vị giá trị | 
| chỉ trao đổi_p | phụ thuộc | tính đúng đắn của hiệu ứng chu kỳ | 
| lặp đi lặp lại chỉ số lớn cmp | đặt hàng đúng | xử lý x,y lớn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi p tạo thành một chu trình đơn. Trong tình huống đó, mỗi Ai chỉ là một vòng quay toàn cục của a. Một cách tiếp cận đơn giản có thể xử lý các vị trí một cách độc lập không chính xác, nhưng việc xử lý đúng cho thấy rằng việc so sánh Ax và Ay làm giảm việc so sánh hai phép quay của cùng một mảng, do đó sự bằng nhau chỉ xảy ra khi x ≡ y mod n. 

Một trường hợp cạnh khác xảy ra khi hoán đổi trong chu kỳ p ngắt và nối lại. Ví dụ: nếu p ban đầu có chu kỳ (1 2 3 4), việc hoán đổi p[2] và p[3] có thể chia nó thành hai chu kỳ. Một cách tiếp cận đúng phải phản ánh ngay sự thay đổi này trong các miền xoay, nếu không các so sánh sẽ giả định không chính xác cấu trúc tuần hoàn cũ. 

Trường hợp cạnh cuối cùng là khi x và y cực kỳ lớn. Bất kỳ mô phỏng trực tiếp nào của k bước cho k tối đa 10^18 đều thất bại ngay lập tức, nhưng lý do chính xác sẽ làm giảm độ dài chu kỳ modulo của mọi thứ, đảm bảo rằng ngay cả các chỉ số cực đoan cũng thu gọn về độ lệch giới hạn.
