---
title: "CF 2125B - Trái và Xuống"
description: "Chúng tôi bắt đầu với một robot được đặt tại tọa độ $(a,b)$ trên một lưới vô hạn và chúng tôi muốn di chuyển nó trở lại điểm gốc $(0,0)$."
date: "2026-06-08T03:26:57+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2125
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 181 (Rated for Div. 2)"
rating: 900
weight: 2125
solve_time_s: 80
verified: true
draft: false
---

[CF 2125B - Trái và Dưới](https://codeforces.com/problemset/problem/2125/B) 

**Xếp hạng:** 900 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một robot được đặt ở tọa độ$(a,b)$trên một lưới vô hạn và chúng tôi muốn di chuyển nó trở lại điểm gốc$(0,0)$. Mỗi nước đi được xác định bằng cách chọn một cặp$(dx,dy)$nơi cả hai thành phần nằm giữa$0$Và$k$, rồi trừ vectơ đó khỏi vị trí hiện tại. Vì vậy, mỗi bước di chuyển của robot sẽ dịch chuyển sang trái$dx$và xuống bởi$dy$. Cùng một cặp$(dx,dy)$có thể được tái sử dụng, nhưng chỉ lần đầu tiên nó có giá 1, và sau đó nó miễn phí. 

Quyết định thực sự không phải là giảm thiểu số lượng chuyển động mà là giảm thiểu số lượng vectơ chuyển động riêng biệt mà chúng ta từng sử dụng. Khi một vectơ được đưa vào, nó có thể được sử dụng lại tùy ý nhiều lần mà không mất thêm chi phí. 

Các ràng buộc đi lên đến$10^{18}$cho cả tọa độ và$k$, do đó, bất kỳ giải pháp nào phụ thuộc vào việc lặp lại các vectơ có thể hoặc xây dựng đường dẫn từng bước là không thể. Toàn bộ lời giải phải đơn giản hóa vấn đề thành lý luận về tính chia hết hoặc tính chất cấu trúc của vectơ. 

Một điểm tinh tế quan trọng là robot có thể kết hợp các vectơ khác nhau. Một trực giác ngây thơ có thể gợi ý rằng chúng ta cần một vectơ duy nhất đạt tới$(a,b)$chính xác, nhưng điều đó là không bắt buộc. Chúng ta chỉ cần một tập hợp các vectơ được phép có tổng bằng$(a,b)$. 

Một trường hợp thất bại phổ biến phát sinh khi một cách tiếp cận tham lam cố gắng trừ đi nhiều lần những thứ như$(k,k)$hoặc$(k,0)$. Ví dụ, nếu$(a,b)=(3,5)$Và$k=2$, việc sử dụng các bước di chuyển tối đa một cách tham lam sẽ không thành công vì không một bước di chuyển nào có thể tiếp cận trực tiếp mục tiêu và việc trộn các hướng không chính xác có thể dẫn đến các vectơ khác biệt không cần thiết. 

Một trường hợp phức tạp khác là khi$a$Và$b$đủ độc lập để chúng ta cần nhiều "hướng" chuyển động, nhưng vẫn đủ nhỏ để hai vectơ được chọn cẩn thận có thể bao trùm cả hai tọa độ một cách chính xác. 

## Phương pháp tiếp cận 

Cấu trúc chi phí biến vấn đề thành việc giảm thiểu số lượng vectơ riêng biệt mà việc sử dụng lặp đi lặp lại có thể biểu thị vectơ$(a,b)$dưới dạng tổng, với mỗi vectơ bị ràng buộc bởi$0 \le dx,dy \le k$. 

Một cách nhìn mạnh mẽ sẽ là xem xét tất cả các tập hợp con có thể có của các vectơ được phép và với mỗi tập hợp con hãy thử xem liệu$(a,b)$có thể được biểu diễn dưới dạng tổ hợp số nguyên không âm của các vectơ đó. Điều này ngay lập tức thất bại vì có$(k+1)^2$các vectơ có thể, lên đến$10^{36}$, làm cho việc liệt kê thậm chí không thể thực hiện được. 

Thông tin chi tiết quan trọng là chúng ta không thực sự quan tâm đến việc chúng ta sử dụng vectơ bao nhiêu lần mà chỉ quan tâm liệu nó có xuất hiện hay không. Vì vậy, vấn đề trở thành: số lượng vectơ tối thiểu bên trong$k \times k$lưới có sự kết hợp tuyến tính không âm có thể tạo ra$(a,b)$? 

Vì tất cả các vectơ đều nằm trong góc phần tư thứ nhất, nên mọi sự kết hợp đều duy trì tính đơn điệu và chúng ta đang bao phủ một điểm trong hình nón 2D một cách hiệu quả bằng cách sử dụng một bộ tạo nhỏ. Trong hai chiều, bất kỳ mục tiêu nào luôn có thể được biểu thị bằng cách sử dụng tối đa hai vectơ nếu chúng được chọn đúng, trừ khi một trong các tọa độ buộc cấu trúc suy biến. 

Vì vậy, câu trả lời quy về việc kiểm tra xem một vectơ có đủ hay không, mặt khác xác định xem hai vectơ có đủ hay không và nếu không thì cần ba vectơ. 

Một vectơ duy nhất chỉ đủ nếu cả hai tọa độ đều chia hết cho cùng một bước nằm trong giới hạn, nghĩa là$(a,b)$bản thân nó phải nằm trên một tia được tạo ra bởi một số$(dx,dy)\le k$, điều này có thể xảy ra chính xác khi$\max(a,b)\le k$hoặc khi cặp có thể được thu nhỏ lại thành một vectơ số nguyên giới hạn hợp lệ. 

Nếu một vectơ không đủ, chúng ta hỏi liệu chúng ta có thể tìm được hai vectơ không$(x_1,y_1)$Và$(x_2,y_2)$như vậy:$$a = \alpha x_1 + \beta x_2,\quad b = \alpha y_1 + \beta y_2$$đối với một số số nguyên không âm$\alpha,\beta$. Trong 2D với tọa độ giới hạn, điều này luôn có thể thực hiện được trừ khi cả hai tọa độ yêu cầu nhiều hơn một "hướng đơn vị", điều này chỉ xảy ra khi cả hai tọa độ$a > k$Và$b > k$và chúng không thể được căn chỉnh thông qua một độ dốc chung. 

Việc đơn giản hóa cấu trúc dẫn đến sự phân loại rõ ràng: 

nếu$\gcd(a,b)$cho phép hướng trong giới hạn, câu trả lời là 1; nếu không thì câu trả lời là 2 trừ khi cả hai tọa độ đều vượt quá$k$theo cách buộc ba hướng độc lập, điều này không bao giờ xảy ra trong cách xây dựng này, vì vậy câu trả lời luôn có nhiều nhất là 2. 

Do đó logic cuối cùng rút gọn thành việc kiểm tra xem$(a,b)$bản thân nó có thể thực hiện được chỉ bằng một nước đi, nếu không thì hai nước đi luôn là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên vectơ |$O(k^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành việc quyết định cần bao nhiêu vectơ bước riêng biệt. 

1. Kiểm tra xem chúng ta có thể làm được điều đó với một vectơ không. Điều này đòi hỏi phải tồn tại$(dx,dy)$sao cho việc áp dụng nhiều lần nó đạt được chính xác$(a,b)$. Điều đó có nghĩa$(dx,dy)$phải chia cả hai tọa độ:$a = t \cdot dx$,$b = t \cdot dy$, và cả hai$dx,dy \le k$. Ứng cử viên tốt nhất là$(a,b)$chính nó nếu cả hai đều nằm trong giới hạn. 
2. Nếu$\max(a,b) \le k$, thì chúng ta có thể chọn thao tác đơn lẻ$(a,b)$và kết thúc trong một động tác. 
3. Mặt khác, chúng tôi kiểm tra xem một vectơ có còn hoạt động hay không bằng cách giảm tỷ lệ. Nếu như$\gcd(a,b) = g$, thì hướng nguyên thủy là$(a/g, b/g)$. Nếu cả hai thành phần của vectơ nguyên thủy này là$\le k$, chúng ta có thể sử dụng nó nhiều lần. 
4. Nếu hướng nguyên thủy vượt quá$k$, thì không một vectơ đơn nào có thể tạo ra cả hai tọa độ một cách chính xác, bởi vì bất kỳ trình tạo hợp lệ nào cũng phải nằm trên cùng một tia nhưng không lớn hơn$k$. 
5. Trong trường hợp đó, chúng ta sử dụng hai vectơ. Người ta có thể xử lý độc lập tiến trình theo chiều ngang và tiến trình khác theo chiều dọc, và bởi vì cả hai trục đều đơn điệu và được giới hạn độc lập nên hai hướng luôn là đủ. 

### Tại sao nó hoạt động 

Tất cả các bước di chuyển đều có tính cộng và bị giới hạn trong góc phần tư thứ nhất, vì vậy mọi giải pháp đều tương ứng với việc phân tách$(a,b)$thành tổng các vectơ trong$[0,k]^2$. Bất kỳ sự phân tách hợp lệ nào đều thu gọn thành tập hợp tối thiểu gồm nhiều nhất hai hướng độc lập trong 2D. Nếu một hướng duy nhất vừa với giới hạn, nó sẽ tạo ra một hình nón cấp 1 bao phủ chính xác mục tiêu. Nếu không, mục tiêu nhất thiết phải yêu cầu ít nhất hai hướng độc lập và ở dạng 2D, hai hướng là đủ để mở rộng bất kỳ điểm nào trong mạng có thể tiếp cận. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        a, b, k = map(int, input().split())
        
        # If we can directly jump to (0,0) in one move
        if max(a, b) <= k:
            out.append("1")
            continue
        
        # Check if a single direction can generate both
        g = math.gcd(a, b)
        x = a // g
        y = b // g
        
        if x <= k and y <= k:
            out.append("1")
        else:
            out.append("2")
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Trước tiên, đoạn mã xử lý trường hợp tầm thường trong đó bản thân điểm đích là một nước đi hợp lệ, vì điều đó ngay lập tức mang lại chi phí là 1. Sau đó, nó làm giảm vectơ$(a,b)$theo hướng nguyên thủy của nó bằng cách sử dụng gcd. Nếu hướng đó vừa với hộp được phép, việc sử dụng lặp lại hướng đó sẽ tạo ra mục tiêu mà không cần bất kỳ vectơ nào khác. Ngược lại, chúng ta kết luận rằng cần có ít nhất hai vectơ phân biệt, vì vậy câu trả lời là 2. 

Một điểm tinh tế là chỉ cần kiểm tra hướng nguyên thủy là đủ. Nếu bất kỳ phiên bản tỷ lệ nào của nó phù hợp với giới hạn thì phiên bản gốc cũng phải phù hợp, vì vậy điều kiện vừa cần vừa đủ. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp để xem cách phân loại hoạt động. 

### Ví dụ 1:$(a,b,k) = (3,5,15)$| Bước | một | b | k | gcd | nguyên thủy (x, y) | quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 3 | 5 | 15 | - | - | bắt đầu | 
| kiểm tra trực tiếp | 3 | 5 | 15 | - | - | max(a,b) ≤ k | 
| kết quả | - | - | - | - | - | 1 | 

Điều này xác nhận trường hợp một nước đi duy nhất có thể đạt được mục tiêu. 

Thuật toán thoát sớm vì bản thân đích đến là một vectơ hợp lệ, nghĩa là không cần phân tách. 

### Ví dụ 2:$(a,b,k) = (2,3,1)$| Bước | một | b | k | gcd | nguyên thủy (x, y) | quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 2 | 3 | 1 | - | - | bắt đầu | 
| kiểm tra trực tiếp | 2 | 3 | 1 | - | - | thất bại | 
| gcd | 2 | 3 | 1 | 1 | (2,3) | kiểm tra nguyên thủy | 
| kiểm tra ràng buộc | 2 | 3 | 1 | 1 | (2,3) | vượt quá k | 
| kết quả | - | - | - | - | - | 2 | 

Ở đây hướng nguyên thủy đã quá lớn, có nghĩa là không một bước di chuyển nào có thể tái sử dụng có thể biểu thị toàn bộ sự dịch chuyển. Do đó chúng ta cần ít nhất hai vectơ phân biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm chỉ sử dụng tính toán gcd và kiểm tra liên tục | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu bổ sung ngoài các biến | 

Giải pháp dễ dàng phù hợp trong giới hạn vì thậm chí$10^4$trường hợp thử nghiệm chỉ yêu cầu các phép tính số học đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io, math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        a, b, k = map(int, input().split())
        if max(a, b) <= k:
            res.append("1")
            continue
        g = math.gcd(a, b)
        if a // g <= k and b // g <= k:
            res.append("1")
        else:
            res.append("2")
    return "\n".join(res)

# provided samples
assert run("""4
3 5 15
2 3 1
12 18 8
9 7 5
""") == """1
2
1
2"""

# custom: already reachable in one move
assert run("""1
10 10 10
""") == "1"

# custom: needs two directions
assert run("""1
10 1 2
""") == "2"

# custom: large gcd-based reducible case
assert run("""1
100 150 50
""") == "1"

# custom: tight boundary
assert run("""1
1 2 1
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (3,5,15) | 1 | trường hợp di chuyển trực tiếp | 
| (10,1,2) | 2 | vector đơn không thể | 
| (100,150,50) | 1 | hướng hợp lệ theo tỷ lệ gcd | 
| (1,2,1) | 2 | trường hợp sai lệch ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi một tọa độ lớn hơn nhiều so với$k$, nhưng cái kia thì nhỏ. Ví dụ,$(a,b,k) = (100,1,50)$. Việc kiểm tra trực tiếp không thành công vì$100 > 50$. gcd là$1$, vậy hướng nguyên thủy là$(100,1)$, cũng vượt quá$k$. Thuật toán đưa ra chính xác 2, vì không có hướng giới hạn duy nhất nào có thể tái tạo độ dốc không cân bằng như vậy. 

Một trường hợp khác là khi cả hai tọa độ đều bằng nhau và đủ nhỏ, chẳng hạn như$(5,5,10)$. Ở đây, điều kiện trực tiếp thành công ngay lập tức và câu trả lời là 1. Đường dẫn gcd cũng xác nhận điều đó vì hướng nguyên thủy là$(1,1)$, nằm trong giới hạn, củng cố rằng các chuyển động chéo lặp đi lặp lại là đủ. 

Một trường hợp tế nhị cuối cùng là khi$(a,b)$chia sẻ một gcd lớn nhưng hướng giảm vẫn vi phạm giới hạn, chẳng hạn như$(30,45,10)$. Nguyên thuỷ là$(2,3)$, nằm trong giới hạn nên chỉ cần một vectơ. Điều này cho thấy rằng việc thu nhỏ quy mô là cách chính xác để phát hiện tính khả thi chứ không phải tọa độ thô.
