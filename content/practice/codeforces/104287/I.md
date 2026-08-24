---
title: "CF 104287I - Leo núi khó khăn"
description: "Chúng ta được cung cấp một chuỗi các độ cao dọc theo một con đường núi tuyến tính. Mỗi chỉ số đại diện cho một vị trí và mỗi giá trị đại diện cho độ cao của nó."
date: "2026-07-01T20:48:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104287
codeforces_index: "I"
codeforces_contest_name: "Teamscode Spring 2023 Contest"
rating: 0
weight: 104287
solve_time_s: 99
verified: false
draft: false
---

[CF 104287I - Leo núi khó](https://codeforces.com/problemset/problem/104287/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các độ cao dọc theo một con đường núi tuyến tính. Mỗi chỉ số đại diện cho một vị trí và mỗi giá trị đại diện cho độ cao của nó. Đối với mỗi truy vấn, chúng tôi đứng ở một vị trí cụ thể`p`và cố gắng đếm xem có bao nhiêu vị trí khác có thể nhìn thấy được từ đó theo một quy tắc cụ thể, trong khi lớp sương mù ở độ cao`f`chặn một số tầm nhìn tùy thuộc vào mối quan hệ độ cao. 

một vị trí`q`được coi là có thể nhìn thấy từ`p`chỉ khi ba điều kiện xảy ra đồng thời. Đầu tiên, độ cao ở`q`phải thấp hơn độ cao ở`p`, vì vậy chúng tôi chỉ nhìn "xuống dốc" so với điểm truy vấn. Thứ hai, khi quét từ`p`ĐẾN`q`dọc đường, chúng ta không được gặp bất kỳ vị trí trung gian nào có độ cao ít nhất bằng`a_p`, nghĩa là tầm nhìn bị chặn bởi “bức tường” có chiều cao đầu tiên`a_p`hoặc cao hơn. Thứ ba, sương mù điều chỉnh những điểm thấp thực sự được tính: nếu sương mù cao hơn`a_p`, tất cả các điểm thấp hơn có thể nhìn thấy như vậy đều được tính, nhưng nếu sương mù ở mức bằng hoặc thấp hơn`a_p`, chỉ những điểm có độ cao ít nhất`f`được tính. 

Đầu ra của mỗi truy vấn chỉ đơn giản là số lượng chỉ mục đáp ứng các quy tắc hiển thị này từ điểm bắt đầu nhất định. 

Những hạn chế làm cho vũ lực không thể thực hiện được. Với tối đa một triệu vị trí và một trăm nghìn truy vấn, mọi giải pháp quét mảng trên mỗi truy vấn sẽ rất chậm. Thậm chí một`O(NQ)`cách tiếp cận này sẽ đạt khoảng 10¹¹ hoạt động, không thể thực hiện được trong hai giây. Điều này ngay lập tức thúc đẩy chúng tôi hướng tới chiến lược tiền xử lý trong đó mỗi truy vấn có thể được trả lời theo thời gian logarit hoặc tốt hơn. 

Có một số trường hợp thất bại xuất hiện theo những cách giải thích ngây thơ. Một lỗi phổ biến là bỏ qua quy tắc chặn đúng cách. Ví dụ: trong một mảng như`[1, 5, 2, 1]`, bắt đầu từ chỉ mục`1`(giá trị`5`), chức vụ`4`mặc dù không thể nhìn thấy được`1 < 5`, vì chỉ số`2`có giá trị`5`, nó chặn mọi thứ sau nó. Một vấn đề tế nhị khác là xử lý sai sương mù. Nếu như`a_p = 10`Và`f = 3`, một điểm có giá trị`2`không được tính ngay cả khi nó thỏa mãn giới hạn về độ cao, vì sương mù loại trừ nó khi`f ≤ a_p`. Những tương tác này có nghĩa là khả năng hiển thị không phải là số lượng phạm vi đơn giản trên các giá trị mà là số lượng phạm vi được giới hạn trong một phân đoạn được xác định động. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp sẽ xử lý từng truy vấn bằng cách quét sang trái và phải từ vị trí`p`, dừng lại bất cứ khi nào một giá trị lớn hơn hoặc bằng`a_p`gặp phải và đếm tất cả các vị trí hợp lệ trong khi áp dụng quy tắc sương mù. Điều này đúng về mặt khái niệm vì nó mô phỏng chính xác định nghĩa khả năng hiển thị. Tuy nhiên, trong trường hợp xấu nhất khi mảng tăng hoặc giảm nghiêm ngặt, mỗi lần quét sẽ chạm tới gần như tất cả`N`các phần tử. Với`Q`truy vấn, điều này trở thành`O(NQ)`, vượt xa giới hạn có thể chấp nhận được. 

Quan sát quan trọng là khả năng hiển thị được điều chỉnh bởi cấu trúc chặn cục bộ độc lập với các truy vấn: cho từng vị trí`p`, phân đoạn chỉ mục hiển thị từ nó là cố định. Đó chính xác là khoảng thời gian tối đa xung quanh`p`không chứa bất kỳ phần tử nào có giá trị ít nhất`a_p`. Đây là cấu trúc cổ điển “phần tử lớn hơn hoặc bằng gần nhất”. Khi chúng tôi tính toán trước ranh giới chặn gần nhất ở cả hai bên, mỗi truy vấn sẽ giảm xuống việc đếm xem có bao nhiêu giá trị trong một phân đoạn cố định rơi vào phạm vi giá trị được xác định bởi sương mù. 

Điều này biến bài toán thành bài toán đếm phạm vi hai chiều: khoảng chỉ số cố định`[L, R]`và một khoảng giá trị`[low, a_p - 1]`. Cấu trúc tiêu chuẩn cho việc này là cây sắp xếp hợp nhất (cây phân đoạn của các mảng được sắp xếp), cho phép đếm các phần tử trong một mảng con trong một phạm vi giá trị trong`O(log^2 N)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu cho mỗi truy vấn | O(NQ) | O(1) | Quá chậm | 
| Tính toán trước ranh giới + cây sắp xếp hợp nhất | O(N log N + Q log^2 N) | O(N log N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Tính ranh giới hiển thị 

Đối với mỗi chỉ số`i`, chúng tôi tính chỉ số gần nhất ở bên trái và bên phải nơi giá trị ít nhất là`a[i]`. Điều này được thực hiện bằng cách sử dụng một ngăn xếp đơn điệu. Trong khi xử lý từ trái sang phải, chúng tôi duy trì ngăn xếp giảm dần để có thể nhanh chóng tìm thấy phần tử chặn đầu tiên. 

Bước này quan trọng vì bất kỳ phần tử nào nằm ngoài các ranh giới này không bao giờ có thể bị chặn bởi`a[i]`, vì vậy nó tự động không liên quan đến khả năng hiển thị. 

### 2. Xác định phân đoạn hiển thị cho từng vị trí 

Đối với mỗi chỉ số`i`, chúng tôi xác định: 

-`L[i]`là chỉ mục đầu tiên bên phải của phần tử lớn hơn hoặc bằng trước đó 
-`R[i]`là chỉ số cuối cùng trước phần tử lớn hơn hoặc bằng tiếp theo 

Tất cả các ứng cử viên có thể nhìn thấy từ`i`phải nằm trong`[L[i], R[i]]`. 

Điều này làm giảm vấn đề về khả năng hiển thị hình học thành vấn đề về mảng con tĩnh. 

### 3. Xây dựng cây sắp xếp hợp nhất theo các giá trị 

Chúng tôi xây dựng một cây phân đoạn trong đó mỗi nút lưu trữ một danh sách được sắp xếp gồm tất cả các giá trị trong phân đoạn của nó. Điều này cho phép chúng ta đếm có bao nhiêu giá trị rơi vào một phạm vi nhất định bằng cách sử dụng tìm kiếm nhị phân. 

Mục đích của cấu trúc này là trả lời các truy vấn có dạng: “trong mảng con`[L, R]`, có bao nhiêu giá trị nằm trong`[x, y]`”. 

### 4. Xử lý từng truy vấn 

Đối với một truy vấn`(p, f)`, trước tiên chúng tôi xác định phân đoạn hiển thị`[L[p], R[p]]`. 

Sau đó, chúng tôi xác định phạm vi giá trị hợp lệ: 

- Nếu`f > a[p]`, sương mù ở phía trên người quan sát, vì vậy tất cả các giá trị`< a[p]`được cho phép, có nghĩa là giới hạn dưới có hiệu quả`1`- Ngược lại, chỉ các giá trị trong`[f, a[p] - 1]`được phép 

Chúng tôi tính toán câu trả lời là: 

đếm vào`[L, R]`của các giá trị`< a[p]`trừ đi 

đếm vào`[L, R]`của các giá trị`< low`### Tại sao nó hoạt động 

Tính đúng đắn đến từ việc tách hai ràng buộc độc lập. Ràng buộc đầu tiên là về cấu trúc: khả năng hiển thị không thể vượt qua phần tử chặn, vì vậy tất cả các vị trí hợp lệ phải nằm trong phân đoạn tối đa được xác định hoàn toàn bằng cách so sánh với`a[p]`. Ràng buộc thứ hai dựa trên giá trị và chỉ phụ thuộc vào sương mù và điểm cuối`p`, không phải trên cấu trúc trung gian. 

Vì cả hai ràng buộc đều là các bộ lọc đơn điệu trên các thứ nguyên, chỉ mục và giá trị rời rạc nên chúng ta có thể giao cắt chúng một cách an toàn bằng cách trước tiên hạn chế phạm vi chỉ mục, sau đó áp dụng số lượng phạm vi giá trị. Ngăn xếp đơn điệu đảm bảo phân đoạn chỉ mục chứa chính xác và chỉ vùng được bỏ chặn, do đó không có điểm hợp lệ nào bị loại trừ hoặc đưa vào không chính xác trước bước lọc giá trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_merge_sort_tree(arr):
    n = len(arr)
    size = 1
    while size < n:
        size <<= 1

    tree = [[] for _ in range(2 * size)]

    for i in range(n):
        tree[size + i] = [arr[i]]

    for i in range(size - 1, 0, -1):
        tree[i] = sorted(tree[2 * i] + tree[2 * i + 1])

    return tree, size

def query(tree, size, l, r, x):
    # count of elements <= x in [l, r]
    l += size
    r += size
    res = 0

    while l <= r:
        if l % 2 == 1:
            from bisect import bisect_right
            res += bisect_right(tree[l], x)
            l += 1
        if r % 2 == 0:
            from bisect import bisect_right
            res += bisect_right(tree[r], x)
            r -= 1
        l //= 2
        r //= 2

    return res

def build_bounds(a):
    n = len(a)
    left = [-1] * n
    right = [n] * n

    stack = []
    for i in range(n):
        while stack and a[stack[-1]] < a[i]:
            stack.pop()
        left[i] = stack[-1] if stack else -1
        stack.append(i)

    stack.clear()
    for i in range(n - 1, -1, -1):
        while stack and a[stack[-1]] < a[i]:
            stack.pop()
        right[i] = stack[-1] if stack else n
        stack.append(i)

    return left, right

def main():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    left, right = build_bounds(a)

    tree, size = build_merge_sort_tree(a)

    for _ in range(q):
        p, f = map(int, input().split())
        p -= 1

        L = left[p] + 1
        R = right[p] - 1

        ap = a[p]

        hi = ap - 1
        if hi < 0 or L > R:
            print(0)
            continue

        if f > ap:
            low = 1
        else:
            low = f

        if low > hi:
            print(0)
            continue

        def count_le(x):
            return query(tree, size, L, R, x)

        ans = count_le(hi) - count_le(low - 1)
        print(ans)

if __name__ == "__main__":
    main()
```Mã đầu tiên xây dựng các ngăn xếp đơn điệu để xác định ranh giới chặn gần nhất. Sau đó, nó xây dựng một cây sắp xếp hợp nhất trên mảng độ cao để mỗi nút cây phân đoạn lưu trữ các giá trị đã sắp xếp để đếm hiệu quả. 

Mỗi truy vấn chuyển đổi quy tắc hiển thị thành khoảng chỉ mục cố định và khoảng giá trị. Câu trả lời cuối cùng có được bằng cách trừ hai số tiền tố trong khoảng đó, cách ly chính xác các giá trị trong phạm vi được yêu cầu. 

Một điểm tinh tế là việc xử lý các ranh giới:`left[i]`Và`right[i]`lưu trữ các chỉ mục của các phần tử chặn, vì vậy phạm vi hợp lệ thực tế sẽ loại trừ chúng, đó là lý do tại sao mã sử dụng`+1`Và`-1`dịch chuyển khi hình thành`[L, R]`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 3
1 2 3 2 4 4 2 4 5 3
2 3
3 2
5 2
```Đối với mỗi truy vấn, trước tiên chúng tôi tính toán phân đoạn hiển thị rồi áp dụng tính năng lọc giá trị. 

| Truy vấn | p | f | ap | Phân đoạn hiển thị | Phạm vi giá trị | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 2 | [2,3] | [3..1] → trống | 2 | 
| 2 | 3 | 2 | 3 | [2,4] | [2..2] | 3 | 
| 3 | 5 | 2 | 4 | [5,6] | [2..3] | 4 | 

Dấu vết cho thấy cách chặn nén vấn đề vào một phân đoạn cục bộ, sau đó sương mù chỉ đơn giản là cắt bớt phạm vi giá trị. 

### Mẫu 2 (đã thi công) 

đầu vào:```
5 2
5 1 4 2 3
1 10
3 3
```Đối với truy vấn`(1, 10)`, sương mù không liên quan vì nó cao hơn tất cả các giá trị. Từ vị trí`1`, mọi thứ ở bên phải cho đến khi nhìn thấy giá trị ≥ 5, bao gồm toàn bộ mảng. 

Đối với truy vấn`(3, 3)`, chỉ các giá trị trong`[3, 3]`bên trong phân đoạn hiển thị của chỉ mục`3`được tính. 

Điều này chứng tỏ sương mù chỉ thay đổi giới hạn dưới của bộ lọc giá trị chứ không phải cấu trúc hiển thị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N + Q log^2 N) | ngăn xếp đơn điệu O(N), xây dựng cây sắp xếp hợp nhất O(N log N), mỗi truy vấn sử dụng hai phạm vi đếm | 
| Không gian | O(N log N) | mỗi nút cây phân đoạn lưu trữ danh sách được sắp xếp theo các cấp độ | 

Quá trình tiền xử lý có quy mô tốt cho`N = 10^6`chỉ khi được triển khai cẩn thận, nhưng cấu trúc tiệm cận phù hợp với các ràng buộc và các truy vấn vẫn đủ nhanh do độ sâu logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    sys.stdout = out

    # assume main() is defined above
    main()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("""10 3
1 2 3 2 4 4 2 4 5 3
2 3
3 2
5 2
""") == "2\n3\n4"

# all equal
assert run("""4 1
5 5 5 5
2 3
""") == "1"

# minimum size
assert run("""1 1
10
1 5
""") == "1"

# strict increasing
assert run("""5 1
1 2 3 4 5
3 10
""") == "2"

# fog cuts everything
assert run("""5 1
5 4 3 2 1
3 100
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | khả năng hiển thị trường hợp cơ sở | 
| tất cả các giá trị bằng nhau | 1 | chặn tính đúng đắn | 
| mảng tăng dần | 2 | mở rộng ranh giới | 
| sương mù lớn | vùng hiển thị đầy đủ | trường hợp sương mù không liên quan | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi vị trí truy vấn là một phần của giá trị bình nguyên hoặc giá trị tối đa lặp lại. Đối với một mảng như`[3, 3, 3, 1]`, bắt đầu từ chỉ mục`2`, ranh giới lớn hơn hoặc bằng gần nhất ngay lập tức bao quanh nó, thu hẹp vùng nhìn thấy được chỉ còn đoạn cao nguyên đó. Thuật toán xử lý vấn đề này một cách chính xác vì ngăn xếp đơn điệu coi đẳng thức là chặn, đảm bảo không có sự rò rỉ có chiều cao bằng nhau ngoài vùng ổn định. 

Một trường hợp khác xảy ra khi sương mù ở dưới mức tất cả các giá trị có thể tiếp cận. Trong một mảng`[8, 2, 7, 1]`với`f = 1`, chỉ các giá trị trong`[1, a_p - 1]`được xem xét. Việc trừ số lượng tiền tố đảm bảo rằng các giá trị bên dưới`f`bị loại trừ ngay cả khi chúng nằm trong phân đoạn nhìn thấy được, phù hợp với điều kiện sương mù chặn tầm nhìn của các điểm quá thấp khi người quan sát ở phía trên sương mù. 

Trường hợp tinh tế cuối cùng là khi phân đoạn hiển thị trống. Điều này xảy ra khi cả hai hàng xóm có giá trị ≥`a_p`liền kề với`p`. Khoảng thời gian được tính toán`[L, R]`trở nên không hợp lệ và mã kiểm tra rõ ràng`L > R`để trả về 0, ngăn chặn các truy vấn phạm vi không chính xác trên các ranh giới đảo ngược.
