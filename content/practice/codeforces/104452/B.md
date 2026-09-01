---
title: "CF 104452B - Đã đến lúc thu hoạch"
description: "Chúng ta được cung cấp một danh sách chiều cao cây dọc theo một dòng và một tập hợp các truy vấn. Mỗi truy vấn đưa ra chiều cao cắt $L$ và chúng ta phải tính toán lượng vật liệu còn lại trên mức cắt đó. Đối với mỗi bụi cây có chiều cao $ai$, chỉ phần trên $L$ mới đóng góp và chỉ khi nó dương."
date: "2026-06-30T14:42:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104452
codeforces_index: "B"
codeforces_contest_name: "ICPC Central Russia Regional Contest - 2020"
rating: 0
weight: 104452
solve_time_s: 191
verified: true
draft: false
---

[CF 104452B - Đã đến lúc thu hoạch](https://codeforces.com/problemset/problem/104452/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 11s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách chiều cao cây dọc theo một dòng và một tập hợp các truy vấn. Mỗi truy vấn đưa ra chiều cao cắt$L$, và chúng ta phải tính toán xem vật liệu còn lại bao nhiêu trên vết cắt đó. 

Đối với mỗi bụi cây có chiều cao$a_i$, chỉ phần trên$L$đóng góp, và chỉ khi nó là tích cực. Vì vậy mỗi bụi cây góp phần$\max(a_i - L, 0)$. Nhiệm vụ là tính tổng đóng góp này trên tất cả các nhóm cho mỗi truy vấn. 

Vì vậy, mỗi truy vấn là độc lập: chúng ta tưởng tượng việc đặt một đường cắt ngang ở độ cao$L$, loại bỏ mọi thứ bên dưới nó và tính tổng các chiều dài dọc còn sót lại. 

Những hạn chế$N, K \le 10^5$buộc chúng tôi tránh tính toán lại tổng từ đầu cho mỗi truy vấn. Một mô phỏng trực tiếp sẽ tốn kém$O(NK)$, đó là về$10^{10}$hoạt động trong trường hợp xấu nhất là quá chậm. Điều này ngay lập tức gợi ý xử lý trước độ cao để mỗi truy vấn có thể được trả lời theo thời gian logarit hoặc không đổi. 

Một lỗi ngây thơ thường xuất hiện ở đây là chỉ tính toán lại các phần tử$a_i > L$không cần xử lý trước. Ví dụ, nếu chiều cao là$[10^9, 10^9, \dots]$và truy vấn có nhiều giá trị nhỏ, mọi truy vấn vẫn quét toàn bộ mảng, gây ra thời gian chờ mặc dù hầu hết các giá trị đều hoạt động tương tự trên các truy vấn. 

Một cạm bẫy tinh vi khác là quên rằng sự đóng góp phụ thuộc cả vào số lượng phần tử vượt quá$L$và họ vượt quá nó bao nhiêu. Coi nó chỉ là số lượng các phần tử ở trên$L$mất thông tin về độ lớn và tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force đánh giá từng truy vấn một cách độc lập. Đối với một nhất định$L$, nó lặp qua tất cả$a_i$, tích lũy$a_i - L$khi$a_i > L$, và xuất ra tổng. Điều này đúng vì nó trực tiếp tuân theo định nghĩa, nhưng nó lặp lại cùng một lần quét$K$lần, dẫn đến$O(NK)$thời gian. 

Quan sát quan trọng là đối với một cố định$L$, chỉ những phần tử lớn hơn$L$quan trọng, và sự đóng góp của chúng có thể được viết lại theo cách tách biệt việc tính toán khỏi phép tính tổng. Nếu chúng ta sắp xếp mảng và duy trì tổng tiền tố, chúng ta có thể nhanh chóng tính toán cả số lượng phần tử vượt quá$L$và tổng số tiền của chúng. Điều này làm giảm mỗi truy vấn thành tìm kiếm nhị phân cộng với số học, cho phép đánh giá hiệu quả. 

Chúng ta biến đổi biểu thức:$$\sum \max(a_i - L, 0)
= \sum_{a_i > L} a_i - L \cdot (\text{count of } a_i > L)$$Khi mảng được sắp xếp, cả hai thuật ngữ có thể thu được bằng một tìm kiếm nhị phân duy nhất và tra cứu tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NK)$|$O(1)$| Quá chậm | 
| Sắp xếp + Tổng tiền tố |$O(N \log N + K \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng chiều cao theo thứ tự không giảm. Điều này cho phép chúng tôi tách biệt tất cả các phần tử lớn hơn một giá trị truy vấn nhất định bằng cách sử dụng tìm kiếm nhị phân. 
2. Xây dựng một mảng tổng tiền tố trong đó`pref[i]`lưu trữ tổng của số đầu tiên`i`các phần tử trong mảng đã được sắp xếp. Điều này cho phép truy vấn tổng phạm vi thời gian không đổi. 
3. Với mỗi giá trị truy vấn$L$, sử dụng tìm kiếm nhị phân để tìm chỉ mục đầu tiên`pos`như vậy`a[pos] > L`. Mọi thứ từ`pos`ĐẾN$N-1$góp phần trả lời. 
4. Tính tổng các phần tử lớn hơn$L$BẰNG`total = pref[n] - pref[pos]`. 
5. Tính số phần tử vượt quá$L$BẰNG`cnt = n - pos`. 
6. Trừ chiều cao cơ sở bị mất$L \cdot cnt$từ`total`để có được câu trả lời cuối cùng. 
7. Xuất kết quả cho từng truy vấn một cách độc lập. 

Ý tưởng chính là khi ngưỡng phân chia mảng, phần đóng góp sẽ trở thành hàm tuyến tính của số phần tử còn lại, do đó cả hai số liệu thống kê bắt buộc đều đến trực tiếp từ quá trình tiền xử lý. 

### Tại sao nó hoạt động 

Việc sắp xếp đảm bảo rằng tất cả những người đóng góp hợp lệ tạo thành một hậu tố liền kề của mảng. Tổng tiền tố mã hóa tổng tích lũy, do đó, bất kỳ tổng hậu tố nào cũng có thể được tính theo thời gian không đổi. Vì phép biến đổi viết lại biểu thức max phi tuyến ban đầu thành hiệu của hai đại lượng tuyến tính trên hậu tố đó nên mọi truy vấn sẽ giảm xuống còn một phép tính khoảng duy nhất. Không có thông tin nào bị mất vì mỗi thuật ngữ được tính chính xác một lần trong tiền tố hoặc hậu tố bị loại trừ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    k = int(input())
    
    a.sort()
    
    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]
    
    from bisect import bisect_right
    
    for _ in range(k):
        L = int(input())
        pos = bisect_right(a, L)
        cnt = n - pos
        total = pref[n] - pref[pos]
        ans = total - L * cnt
        print(ans)

if __name__ == "__main__":
    solve()
```Sau khi sắp xếp, chúng ta có thể nhanh chóng tách biệt hậu tố của các phần tử vượt quá từng ngưỡng truy vấn. Mảng tổng tiền tố chuyển đổi hậu tố đó thành truy vấn tổng theo thời gian không đổi. Tìm kiếm nhị phân xác định ranh giới giữa các phần tử đóng góp và không đóng góp. Sau đó, mỗi truy vấn được đánh giá bằng cách sử dụng một biểu thức số học cố định được lấy trực tiếp từ định nghĩa ban đầu. 

Một lỗi triển khai phổ biến là sử dụng`bisect_left`thay vì`bisect_right`, bao gồm không chính xác các phần tử bằng$L$. Một vấn đề tế nhị khác là quên rằng phép trừ phải được áp dụng sau khi tính tổng hậu tố đầy đủ, không phải cho mỗi phần tử, nếu không các phép tính số nguyên sẽ biến thành các vòng lặp không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [0, 0, 0, 0]
queries = [0, 1, 2]
```Mảng được sắp xếp không thay đổi, tổng tiền tố đều bằng 0. 

| L | pos (đầu tiên > L) | cnt | tổng hậu tố | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 0 | 0 | 0 | 
| 1 | 4 | 0 | 0 | 0 | 
| 2 | 4 | 0 | 0 | 0 | 

Mọi giá trị đều ≤ L nên không có đóng góp nào tồn tại. Điều này xác nhận tính chính xác trên các vết cắt bão hòa hoàn toàn. 

### Ví dụ 2 

đầu vào:```
a = [4, 0, 2, 1, 2]
queries = [0, 1, 2, 3, 4]
```Đã sắp xếp:`[0, 1, 2, 2, 4]`, tổng tiền tố:`[0,1,3,5,9]`| L | tư thế | cnt | tổng hậu tố | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 4 | 9 | 9 | 
| 1 | 2 | 3 | 8 | 8 - 3 = 5 | 
| 2 | 3 | 2 | 6 | 6 - 4 = 2 | 
| 3 | 4 | 1 | 4 | 4 - 3 = 1 | 
| 4 | 5 | 0 | 0 | 0 | 

Dấu vết này cho thấy câu trả lời giảm tuyến tính như thế nào khi ngưỡng tăng, phản ánh kích thước hậu tố bị thu hẹp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N + K \log N)$| sắp xếp cộng với một tìm kiếm nhị phân cho mỗi truy vấn | 
| Không gian |$O(N)$| tổng tiền tố được lưu trữ cho tổng phạm vi thời gian không đổi | 

Các ràng buộc cho phép lên đến$2 \times 10^5$nên giải pháp này phù hợp một cách thoải mái trong giới hạn vì việc sắp xếp chiếm ưu thế và mỗi truy vấn đều có tính logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    k = int(input())
    a.sort()

    pref = [0]
    for x in a:
        pref.append(pref[-1] + x)

    from bisect import bisect_right

    out = []
    for _ in range(k):
        L = int(input())
        pos = bisect_right(a, L)
        cnt = n - pos
        total = pref[n] - pref[pos]
        out.append(str(total - L * cnt))
    return "\n".join(out)

# provided samples
assert run("4\n0 0 0 0\n3\n0\n1\n2\n") == "0\n0\n0"
assert run("5\n4 0 2 1 2\n5\n0\n1\n2\n3\n4\n") == "9\n5\n2\n1\n0"

# custom cases
assert run("1\n10\n3\n0\n5\n10\n") == "10\n5\n0"
assert run("3\n1 2 3\n2\n2\n1\n") == "1\n3"
assert run("6\n5 5 5 5 5 5\n2\n4\n6\n") == "6\n0"
assert run("4\n0 100 0 100\n3\n50\n100\n0\n") == "100\n0\n200"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | giảm đóng góp | trường hợp cơ sở đúng đắn | 
| hỗn hợp được sắp xếp nhỏ | logic tách ngưỡng | ranh giới tìm kiếm nhị phân | 
| mảng thống nhất | đối xứng và phân rã tuyến tính | xử lý giá trị bằng nhau | 
| xen kẽ các thái cực | tính đúng đắn của việc kết hợp hậu tố | tính chính xác của tiền tố-hậu tố | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các phần tử nhỏ hơn hoặc bằng$L$. Trong tình huống đó, hậu tố trống và câu trả lời phải bằng 0. Thuật toán xử lý việc này vì`pos = n`, do đó cả hậu tố tổng và số đếm đều bằng 0 và công thức tự nhiên trả về 0. 

Một trường hợp khác là khi$L = 0$. Sau đó, mọi phần tử đều đóng góp đầy đủ và câu trả lời sẽ trở thành tổng của mảng. Tìm kiếm nhị phân trả về`pos = 0`, vì vậy hậu tố là toàn bộ mảng và phép trừ không làm gì cả. 

Khi tất cả các phần tử đều bằng nhau, mỗi truy vấn sẽ giữ lại mọi thứ hoặc xóa mọi thứ trong một bước duy nhất. Công thức nắm bắt chính xác sự chuyển đổi đột ngột này vì cả hậu tố tổng và tỷ lệ đếm cùng nhau, duy trì khả năng hủy chính xác khi$L$vượt quá giá trị.
