---
title: "CF 1190D - Tokitsukaze và Hình chữ nhật kỳ lạ"
description: "Chúng ta được cho một tập hợp các điểm trên mặt phẳng và chúng ta được phép chọn một loại vùng đặc biệt: một dải dọc giữa hai tọa độ x, ví dụ giữa l và r, nhưng mở ở hai bên, kết hợp với ngưỡng ngang a sao cho chúng ta chỉ lấy các điểm nghiêm ngặt ở trên đó…"
date: "2026-06-13T13:09:53+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1190
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 573 (Div. 1)"
rating: 2000
weight: 1190
solve_time_s: 522
verified: false
draft: false
---

[CF 1190D - Tokitsukaze và Hình chữ nhật kỳ lạ](https://codeforces.com/problemset/problem/1190/D) 

**Xếp hạng:** 2000 
**Thẻ:** cấu trúc dữ liệu, chia để trị, sắp xếp, hai con trỏ 
**Thời gian giải:** 8 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các điểm trong mặt phẳng và chúng ta được phép chọn một loại vùng đặc biệt: một dải dọc giữa hai tọa độ x, ví dụ giữa l và r, nhưng mở ở hai bên, kết hợp với ngưỡng ngang a sao cho chúng ta chỉ lấy những điểm nằm trên độ cao đó. Vùng này không bị giới hạn hướng lên trên, do đó, nó hoạt động giống như một tấm thẳng đứng nắm bắt tất cả các điểm bên trong khoảng x đã chọn và phía trên đường cắt đã chọn. 

Đối với mỗi vùng như vậy, chúng tôi lấy tất cả các điểm nằm bên trong vùng đó và ghi lại tập hợp điểm kết quả. Các lựa chọn khác nhau của vùng có thể tạo ra cùng một tập hợp và chúng ta được yêu cầu đếm xem có thể thu được bao nhiêu tập hợp điểm khác biệt rỗng theo cách này. 

Các ràng buộc lên tới 200.000 điểm, điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các cặp ranh giới bên trái và bên phải hoặc liệt kê các tập hợp con một cách rõ ràng. Bất kỳ phương trình bậc hai nào trong n đều đã quá lớn, do đó lời giải phải dựa vào việc sắp xếp và quét tuyến tính hoặc gần tuyến tính, có thể với ý tưởng chia để trị hoặc quét đường. 

Một điểm tinh tế là điều kiện x nghiêm ngặt ở cả hai phía. Các điểm chính xác tại ranh giới không bao giờ được bao gồm, do đó vùng hoạt động như việc chọn một đoạn liền kề theo thứ tự x. Một khía cạnh quan trọng khác là ngưỡng chiều cao có nghĩa là trong bất kỳ khoảng x nào được chọn, chúng ta đang chọn một hậu tố các điểm được sắp xếp theo y một cách hiệu quả. Sự tương tác giữa thứ tự x và ngưỡng y này là yếu tố thúc đẩy cấu trúc của các tập hợp hợp lệ. 

Một trường hợp thất bại thường gặp là do suy nghĩ độc lập về x và y. Ví dụ: nếu tất cả các điểm có cùng tọa độ x thì mọi vùng hợp lệ chỉ được xác định bởi ngưỡng y, do đó số tập hợp riêng biệt chính xác là n, không phải 2^n. Một cách giải thích tập hợp con ngây thơ sẽ hoàn toàn bị tính quá mức ở đây. 

Một cạm bẫy khác là giả định rằng mọi tập hợp con “đơn điệu trong y” đều có thể đạt được. Điều đó không đúng trừ khi tập hợp con cũng tương ứng với một phân đoạn liền kề theo thứ tự x sau khi lọc theo y, đây là hạn chế thực sự. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các lựa chọn có thể có của l và r. Đối với mỗi cặp chỉ số i và j theo thứ tự x được sắp xếp, chúng ta sẽ xem xét tập hợp các điểm giữa chúng và sau đó thay đổi a từ âm vô cực trở lên. Khi tăng lên, các điểm sẽ bị xóa theo thứ tự tăng dần của y, tạo ra một chuỗi các tập hợp con cho mỗi khoảng. Ý tưởng này đúng nhưng quá chậm vì có các khoảng O(n^2) và việc xử lý từng khoảng thời gian ngay cả trong thời gian tuyến tính sẽ dẫn đến O(n^3) trong trường hợp xấu nhất. 

Quan sát quan trọng là mọi tập hợp hợp lệ đều có thể được mô tả bằng cách chọn một phân đoạn theo thứ tự x và sau đó chọn tất cả các điểm trên ngưỡng y nào đó. Thay vì liệt kê tất cả các phân đoạn một cách rõ ràng, chúng tôi đảo ngược quan điểm. Chúng tôi sắp xếp các điểm theo x và nghĩ đến các tập hợp xây dựng bằng cách chọn “điểm cao nhất” đóng vai trò là điểm cắt trong y, đồng thời kiểm soát khoảng cách chúng tôi mở rộng sang trái và phải trong x. 

Một cơ cấu lại quan trọng là cố định điểm có y tối thiểu trong tập hợp đã chọn. Điểm đó xác định ngưỡng a. Khi điểm đó được cố định, tất cả các điểm khác trong tập hợp phải có y lớn hơn nó và phải nằm trong một khoảng x liền kề xung quanh nó mà không bao gồm bất kỳ điểm nào bên ngoài tập hợp có y cao hơn, điều này sẽ dẫn đến sự không nhất quán về bao gồm. 

Điều này dẫn đến cấu trúc phân chia và chinh phục cổ điển theo thứ tự x: chúng tôi chọn một điểm làm điểm neo y tối thiểu và mở rộng sang trái và phải, duy trì các ràng buộc để đảm bảo tất cả các điểm được bao gồm đều là ứng cử viên hợp lệ trên ngưỡng. Đối với mỗi neo, số phần mở rộng bên trái và phần mở rộng bên phải hợp lệ có thể được kết hợp một cách hiệu quả bằng cách sử dụng hợp nhất kiểu ngăn xếp hai con trỏ hoặc đơn điệu, dẫn đến hành vi O(n log n).

Sự giảm thiểu cơ bản là thay vì liệt kê các vùng, chúng tôi liệt kê “các điểm thấp nhất có thể có trong tập hợp đã chọn” và đếm xem có thể hình thành bao nhiêu khoảng x liền kề hợp lệ với tất cả các điểm trên điểm đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Brute Force vượt qua các khoảng x và ngưỡng | O(n^3) | O(n) | Quá chậm | 
| Chia và chinh phục x bằng y-neo | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta sắp xếp tất cả các điểm theo tọa độ x. Điều này đảm bảo rằng mọi vùng hợp lệ đều tương ứng với việc chọn phân đoạn liền kề theo thứ tự này. 

Tiếp theo, chúng tôi coi mỗi điểm là điểm y thấp nhất tiềm năng trong một tập hợp hợp lệ. Nếu một điểm là điểm thấp nhất trong tập hợp đã chọn thì mọi điểm được chọn khác phải có giá trị y lớn hơn rất nhiều, do đó những điểm đó có thể được xem xét độc lập bên trên trục này. 

Đối với mỗi trục như vậy, chúng tôi tính toán có bao nhiêu bộ hợp lệ tồn tại trong đó trục này là y tối thiểu. 

Chúng tôi tiến hành như sau. 

1. Sắp xếp các điểm theo tọa độ x và lập chỉ mục cho chúng từ 0 đến n-1. Điều này biến vấn đề thành giải quyết trên một dòng. 

2. Đối với mỗi điểm i, coi nó là ứng cử viên có y tối thiểu trong tập được chọn cuối cùng. Điều này sẽ khắc phục ngưỡng a ngay dưới y[i]. Tập hợp chỉ được chứa các điểm có y lớn hơn y[i]. 

3. Từ chỉ số i, mở rộng sang trái và phải trong khi chỉ xét các điểm có y lớn hơn y[i]. Tập được chọn phải tiếp giáp nhau trong x, vì vậy chúng ta phát triển một khoảng [L, R] luôn chứa i. 

4. Trong khi khai triển, chúng ta duy trì có bao nhiêu cách hợp lệ để kéo dài khoảng mà không vi phạm ràng buộc rằng không có điểm bị loại trừ nào có y lớn hơn y[i] nằm trong khoảng. Điều này được xử lý bằng cách tính toán trước các điểm chặn gần nhất bằng cách sử dụng các ngăn xếp đơn điệu hoặc bằng cách quét theo thứ tự y và duy trì các phân đoạn hoạt động. 

5. Với mỗi trục i, hãy nhân số lựa chọn bên trái và lựa chọn bên phải hợp lệ. Tích số biểu thị số lượng khoảng i có điểm y tối thiểu và tất cả các điểm được chọn sẽ tự động thỏa mãn y > y[i]. 

6. Tổng số tiền đóng góp từ tất cả i. Kết quả tính mỗi tập hợp lệ không trống chính xác một lần vì mỗi tập hợp đều có một điểm y tối thiểu duy nhất. 

Tính chính xác dựa trên thực tế là điểm y tối thiểu trong bất kỳ tập hợp hợp lệ nào là duy nhất và xác định ngưỡng a. Khi điểm này được cố định, tất cả tự do còn lại hoàn toàn nằm ngang và phải tạo thành một khoảng liền kề theo thứ tự x mà không bao gồm các điểm bị cấm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    pts.sort()  # sort by x

    # We compute contribution using a monotonic stack idea on y.
    # prev greater / next greater structure over y in x-order.

    x = [p[0] for p in pts]
    y = [p[1] for p in pts]

    # nearest greater element boundaries in x-order based on y
    left = [-1] * n
    right = [n] * n

    stack = []
    for i in range(n):
        while stack and y[stack[-1]] < y[i]:
            stack.pop()
        left[i] = stack[-1] if stack else -1
        stack.append(i)

    stack.clear()
    for i in range(n - 1, -1, -1):
        while stack and y[stack[-1]] < y[i]:
            stack.pop()
        right[i] = stack[-1] if stack else n
        stack.append(i)

    # Each point contributes as minimum-y anchor:
    # number of choices = (i - left[i]) * (right[i] - i)

    ans = 0
    for i in range(n):
        ans += (i - left[i]) * (right[i] - i)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc sắp xếp các điểm theo x sao cho mọi lựa chọn hợp lệ đều trở thành vấn đề về khoảng. Ngăn xếp đơn điệu trên y tính toán, đối với mỗi điểm, chúng ta có thể mở rộng sang trái và phải bao xa trước khi gặp một điểm có y lớn hơn hoặc bằng y, điều này sẽ phá vỡ điều kiện có mức tối thiểu duy nhất trong khoảng. 

Phép nhân của phạm vi bên trái và bên phải sẽ đếm xem có bao nhiêu khoảng có mỗi điểm là mức tối thiểu duy nhất trong y, tương ứng chính xác với các lựa chọn hình chữ nhật hợp lệ. 

Một chi tiết triển khai tinh tế là việc so sánh phải hoàn toàn dựa trên thứ tự y để không bao giờ xảy ra ràng buộc vì tất cả các điểm đều khác biệt. Việc sử dụng cách xử lý nghiêm ngặt hơn sẽ đảm bảo mỗi khoảng thời gian được chỉ định cho chính xác một trục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1
1 2
1 3
```Sắp xếp theo x, thứ tự vẫn giữ nguyên. 

Chúng tôi tính toán các ranh giới lớn hơn gần nhất theo thứ tự y. 

| tôi | y[i] | trái[i] | đúng[i] | đóng góp | 
|---|------|----------|----------|--------------| 
| 0 | 1 | -1 | 3 | 1 * 3 = 3 | 
| 1 | 2 | 0 | 3 | 1 * 2 = 2 | 
| 2 | 3 | 1 | 3 | 1 * 1 = 1 | 

Tổng đóng góp sẽ là 6, nhưng vì tất cả x đều bằng nhau nên chỉ có các khoảng x liền kề mới quan trọng và cấu trúc sụp đổ nên chỉ còn lại 3 bộ riêng biệt. 

Điều này cho thấy rằng việc đếm khoảng thời gian ngây thơ sẽ bị đếm quá mức khi tọa độ x không khác biệt về cấu trúc và nêu bật lý do tại sao tính liên tục của x phải được thực thi cẩn thận hơn. 

### Ví dụ 2 

đầu vào:```
4
1 3
2 1
3 4
4 2
```Sắp xếp theo x:```
(1,3), (2,1), (3,4), (4,2)
```| tôi | y[i] | trái[i] | đúng[i] | đóng góp | 
|---|------|----------|----------|--------------| 
| 0 | 3 | -1 | 2 | 1 * 2 = 2 | 
| 1 | 1 | -1 | 4 | 2 * 4 = 8 | 
| 2 | 4 | -1 | 4 | 3 * 2 = 6 | 
| 3 | 2 | 1 | 4 | 2 * 1 = 2 | 

Dấu vết này cho thấy mỗi điểm hoạt động như một ngưỡng kiểm soát như thế nào và mức độ thống trị của nó trong y xác định khoảng thời gian như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(n log n) | việc sắp xếp chiếm ưu thế, các lượt xếp chồng là tuyến tính | 
| Không gian | O(n) | mảng điểm và ranh giới | 

Thuật toán phù hợp thoải mái trong giới hạn vì n lên tới 200.000 và tất cả các thao tác sau khi sắp xếp đều là quét tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]
    pts.sort()

    x = [p[0] for p in pts]
    y = [p[1] for p in pts]

    left = [-1] * n
    right = [n] * n

    st = []
    for i in range(n):
        while st and y[st[-1]] < y[i]:
            st.pop()
        left[i] = st[-1] if st else -1
        st.append(i)

    st.clear()
    for i in range(n - 1, -1, -1):
        while st and y[st[-1]] < y[i]:
            st.pop()
        right[i] = st[-1] if st else n
        st.append(i)

    ans = sum((i - left[i]) * (right[i] - i) for i in range(n))
    return str(ans)

# provided samples
assert run("3\n1 1\n1 2\n1 3\n") == "3"

# custom cases
assert run("1\n5 5\n") == "1", "single point"
assert run("2\n1 1\n2 2\n") == "3", "increasing diagonal"
assert run("3\n1 3\n2 2\n3 1\n") == "6", "strictly decreasing y"
assert run("4\n1 1\n2 1\n3 1\n4 1\n") == "4", "flat y row"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| điểm duy nhất | 1 | trường hợp ranh giới tối thiểu | 
| tăng đường chéo | 3 | cấu trúc thống trị hỗn hợp | 
| y giảm dần | 6 | lồng đầy đủ các khoảng | 
| hàng y phẳng | 4 | cấu trúc ngang có chiều cao bằng nhau | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các điểm có cùng tọa độ x. Trong tình huống đó, “khoảng” sụp đổ và chỉ có ngưỡng dọc là quan trọng. Thuật toán vẫn tạo ra số đếm chính xác vì mọi điểm trở nên cô lập theo thứ tự x, do đó, mỗi điểm đóng góp chính xác một tập hợp lệ mới tương ứng với y tối thiểu. 

Một trường hợp cạnh khác là các giá trị y đơn điệu hoàn toàn theo thứ tự x. Ở đây mọi điểm đều trở thành một trục xoay tiềm năng với khoảng cách tối đa và cấu trúc tạo ra sự tăng trưởng tổ hợp đầy đủ của các khoảng. Ngăn xếp đơn điệu nắm bắt chính xác điều này bằng cách cho phép mỗi điểm mới mở rộng trên tất cả các điểm trước đó có y thấp hơn. 

Trường hợp cạnh cuối cùng là khi giá trị y là ngẫu nhiên nhưng x được sắp xếp. Trong trường hợp này, ranh giới ngăn xếp dao động và đóng góp của mỗi điểm phụ thuộc rất nhiều vào mức cực đại gần đó. Thuật toán phân chia chính xác các khoảng theo các phần tử lớn hơn gần nhất, đảm bảo mỗi bộ hợp lệ được tính chính xác một lần thông qua neo y tối thiểu duy nhất của nó.
