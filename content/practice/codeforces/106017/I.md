---
title: "CF 106017I - Máy nếm thử Perm\u00e1tomo (Phiên bản cứng)"
description: "Bài toán yêu cầu chúng ta xem xét từng cặp vị trí có thứ tự trong một mảng và tính tổng của hai giá trị đã chọn. Cặp (i, j) khác với (j, i), vì vậy có chính xác N2 tổng có thể có. Chúng ta cần tìm giá trị nhỏ nhất thứ K trong số tất cả các tổng này."
date: "2026-06-25T13:15:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106017
codeforces_index: "I"
codeforces_contest_name: "2025 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 106017
solve_time_s: 40
verified: true
draft: false
---

[CF 106017I - Máy nếm thử Perm\u00e1tomo (Phiên bản cứng)](https://codeforces.com/problemset/problem/106017/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải quyết:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta xem xét từng cặp vị trí có thứ tự trong một mảng và tính tổng của hai giá trị đã chọn. Cặp đôi`(i, j)`khác với`(j, i)`, vậy có chính xác`N²`số tiền có thể. Chúng ta cần tìm`K`-giá trị nhỏ nhất trong số tất cả các số tiền này. 

Đầu vào chứa một số trường hợp thử nghiệm. Với mỗi test, chúng ta nhận được kích thước của mảng, vị trí mong muốn`K`trong danh sách các tổng cặp được sắp xếp và chính mảng đó. Đầu ra là giá trị sẽ xuất hiện ở vị trí`K`nếu chúng ta tạo ra tất cả các tổng cặp một cách rõ ràng và sắp xếp chúng. 

Phiên bản cứng cho phép`N`để đạt được`100000`Và`K`có thể lớn như`N²`. Việc xây dựng trực tiếp sẽ yêu cầu tạo ra số tiền lên tới mười tỷ, vượt xa những gì phù hợp với trí nhớ hoặc thời gian. Ngay cả việc kiểm tra từng cặp cũng mất`O(N²)`hoạt động và với`N = 100000`đó là về`10^10`kiểm tra cặp, vì vậy mọi giải pháp đều phải tránh liệt kê các cặp. 

Các giá trị có thể âm và có thể lớn bằng`10^9`về giá trị tuyệt đối. Điều này có nghĩa là câu trả lời có thể nhỏ như`-2 * 10^9`hoặc lớn như`2 * 10^9`, do đó không gian tìm kiếm đủ nhỏ để tìm kiếm nhị phân. Số lượng ca kiểm thử cũng ít, điều này cho phép`O(N log C)`giải pháp phong cách, ở đâu`C`là phạm vi giá trị. 

Một số trường hợp phức tạp có thể phá vỡ việc triển khai đơn giản hơn. Coi như:```
1
2 4
1 1
```Bốn tổng đó là`2, 2, 2, 2`, vậy câu trả lời là`2`. Một phương pháp chỉ xét các cặp có`i < j`sẽ chỉ nhìn thấy một cặp không chính xác và không thành công vì vấn đề tính các cặp có thứ tự. 

Một trường hợp khác là:```
1
3 2
-5 0 10
```Tổng số tiền là`-10, -5, 5, -5, 0, 10, 5, 10, 20`. Nhỏ thứ hai là`-5`. Việc triển khai giả định mảng đã được sắp xếp và sử dụng tìm kiếm nhị phân trên các vị trí mà không sắp xếp trước có thể đếm không chính xác. 

Một trường hợp khó khăn cuối cùng là:```
1
3 7
1 2 100
```Tổng được sắp xếp là`2, 3, 3, 4, 101, 101, 102, 102, 200`, vậy câu trả lời là`102`. Một lỗi phổ biến là đếm các cặp có tổng nhỏ hơn giá trị ứng viên thay vì nhỏ hơn hoặc bằng giá trị đó, điều này sẽ làm thay đổi kết quả tìm kiếm nhị phân khi tồn tại nhiều tổng bằng nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là tạo ra tất cả các cặp có thể. Đối với mỗi chỉ số`i`, chúng ta có thể ghép đôi`a[i]`với mọi`a[j]`, cửa hàng`a[i] + a[j]`, sắp xếp danh sách kết quả và trả về phần tử tại vị trí`K`. 

Điều này đúng vì nó thực sự tạo ra nhiều tập hợp chính xác được mô tả bởi vấn đề. Tuy nhiên, nó cần`N²`trí nhớ và`N² log(N²)`thời gian để sắp xếp. Với`N = 100000`, số tiền được tạo ra là`10^10`, nên cách tiếp cận này là không thể. 

Quan sát quan trọng là chúng ta không cần danh sách tổng được sắp xếp thực tế. Chúng ta chỉ cần biết nơi`K`-th giá trị nằm. Nếu chúng ta có thể trả lời câu hỏi "có bao nhiêu tổng cặp nhỏ hơn hoặc bằng X?", thì chúng ta có thể tìm kiếm nhị phân nhỏ nhất`X`số lượng của ai đạt đến`K`. 

Sau khi sắp xếp mảng ban đầu, việc đếm trở nên hiệu quả. Đối với mỗi phần tử đầu tiên`a[i]`, chúng ta cần biết có bao nhiêu phần tử thứ hai thỏa mãn:```
a[i] + a[j] <= X
```tương đương với:```
a[j] <= X - a[i]
```Vì mảng đã được sắp xếp nên số lượng hợp lệ`j`các giá trị có thể được tìm thấy bằng tìm kiếm nhị phân giới hạn trên. Tổng hợp các số đếm này trên tất cả`i`đưa ra số lượng tổng cặp không vượt quá`X`. 

Brute Force hoạt động vì nó trực tiếp xây dựng danh sách có thứ tự, nhưng nó không thành công khi`N`là lớn. Quan sát cho thấy câu trả lời có thể được tìm thấy bằng cách tìm kiếm giá trị, kết hợp với việc đếm nhanh bằng cách sử dụng mảng đã sắp xếp, đã giảm vấn đề từ hàng tỷ cặp xuống còn khoảng một triệu phép tính cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2 log N) | O(N2) | Quá chậm | 
| Tìm kiếm nhị phân trên câu trả lời | O(N log N log A) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng. Thứ tự cho phép chúng ta đếm có bao nhiêu phần tử nằm dưới ngưỡng bằng cách sử dụng tìm kiếm nhị phân. 
2. Đặt phạm vi tìm kiếm nhị phân cho câu trả lời. Tổng cặp nhỏ nhất có thể là tổng của phần tử tối thiểu với chính nó và tổng cặp lớn nhất có thể là tổng của phần tử tối đa với chính nó. 
3. Đối với giá trị trung bình`mid`, tính xem có bao nhiêu cặp thứ tự có tổng nhiều nhất`mid`. Đối với mọi`a[i]`, tìm chỉ số lớn nhất`j`Ở đâu`a[j] <= mid - a[i]`. Số lượng vị trí thứ hai hợp lệ là số phần tử cho đến chỉ mục đó. 
4. Nếu số lượng ít nhất là`K`, câu trả lời nhiều nhất là`mid`, do đó hãy di chuyển phạm vi tìm kiếm sang nửa bên trái. Ngược lại, số tiền quá ít sẽ đủ nhỏ nên hãy chuyển sang nửa bên phải. 
5. Khi khoảng tìm kiếm chứa một giá trị, giá trị đó là số nhỏ nhất có số tổng cặp ít nhất là`K`. 

Lý do hàm đếm hoạt động là vì mỗi cặp có thứ tự đều chọn một vị trí cố định đầu tiên. Đối với vị trí đó, mảng được sắp xếp cho chúng ta biết chính xác có bao nhiêu lựa chọn ở vị trí thứ hai giữ cho tổng đủ nhỏ. Việc cộng các số đếm độc lập đó bao gồm tất cả`N²`đặt hàng các cặp đúng một lần. 

## Giải pháp Python```python
import sys
from bisect import bisect_right

input = sys.stdin.readline

def solve_case(n, k, a):
    a.sort()

    def count_leq(x):
        total = 0
        for v in a:
            total += bisect_right(a, x - v)
        return total

    low = a[0] + a[0]
    high = a[-1] + a[-1]

    while low < high:
        mid = (low + high) // 2
        if count_leq(mid) >= k:
            high = mid
        else:
            low = mid + 1

    return low

def main():
    t = int(input())
    ans = []

    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        ans.append(str(solve_case(n, k, a)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    main()
```Giải pháp trước tiên là sắp xếp mảng vì mọi thao tác sau này đều phụ thuộc vào các phần tử được sắp xếp. các`count_leq`chức năng thực hiện quan sát cốt lõi từ hướng dẫn. Đối với mỗi phần tử đầu tiên có thể,`bisect_right`trả về vị trí đầu tiên sau tất cả các phần tử thứ hai hợp lệ, do đó chỉ mục được trả về chính xác là số lượng lựa chọn hợp lệ. 

Tìm kiếm nhị phân sử dụng phạm vi bao gồm các câu trả lời có thể. Tính toán điểm giữa hoạt động chính xác ngay cả đối với các giá trị âm trong Python vì phép chia số nguyên làm tròn xuống, nhưng quy tắc cập nhật vẫn duy trì tính bất biến là câu trả lời vẫn nằm trong khoảng. 

Một lỗi triển khai phổ biến là sử dụng`bisect_left`thay vì`bisect_right`. Việc đếm cần các giá trị bằng giới hạn vì chúng ta đang đếm số tiền nhỏ hơn hoặc bằng ứng cử viên. Sử dụng giới hạn sai sẽ bỏ qua các cặp hợp lệ và có thể di chuyển tìm kiếm nhị phân đi quá xa về bên phải. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2
3 4
1 3 5
2 2
-1 2
```Đối với trường hợp thử nghiệm đầu tiên, quá trình tìm kiếm diễn ra như sau: 

| thấp | cao | giữa | đếm (giữa) | hành động | 
| --- | --- | --- | --- | --- | 
| 2 | 10 | 6 | 6 | giữ bên trái | 
| 2 | 6 | 4 | 2 | di chuyển sang phải | 
| 5 | 6 | 5 | 2 | di chuyển sang phải | 

Câu trả lời cuối cùng là`6`. 

Số lượng đạt ít nhất`K`chỉ tại`6`, nghĩa là có tối đa bốn tổng giá trị này. 

Đối với trường hợp thử nghiệm thứ hai: 

| thấp | cao | giữa | đếm (giữa) | hành động | 
| --- | --- | --- | --- | --- | 
| -2 | 4 | 1 | 3 | giữ bên trái | 
| -2 | 1 | -1 | 1 | di chuyển sang phải | 
| 0 | 1 | 0 | 1 | di chuyển sang phải | 

Câu trả lời trở thành`1`. 

Ví dụ này cho thấy rằng các giá trị âm được xử lý một cách tự nhiên vì tìm kiếm dựa trên các tổng có thể chứ không phải trên các chỉ số mảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N + N log N log A) | Chi phí sắp xếp O(N log N). Mỗi lần lặp tìm kiếm nhị phân được tính bằng O(N log N) và có khoảng 32 lần lặp vì phạm vi câu trả lời được giới hạn bởi các giá trị. | 
| Không gian | O(N) | Chỉ có mảng được sắp xếp và các biến tạm thời được lưu trữ. | 

Tối đa`N`đủ lớn để các thuật toán bậc hai thất bại, nhưng các thừa số logarit giữ cho tổng số phép toán có thể quản lý được. Phạm vi câu trả lời chỉ có vài tỷ giá trị, do đó tìm kiếm nhị phân bên ngoài rất nhỏ. 

## Trường hợp thử nghiệm```python
import sys
import io
from bisect import bisect_right

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve_case(n, k, a):
        a.sort()

        def count_leq(x):
            total = 0
            for v in a:
                total += bisect_right(a, x - v)
            return total

        low = a[0] + a[0]
        high = a[-1] + a[-1]

        while low < high:
            mid = (low + high) // 2
            if count_leq(mid) >= k:
                high = mid
            else:
                low = mid + 1
        return low

    t = int(input())
    res = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        res.append(str(solve_case(n, k, a)))
    return "\n".join(res)

assert solve("""2
3 4
1 3 5
2 2
-1 2
""") == """6
1""", "samples"

assert solve("""1
1 1
7
""") == "7", "single element"

assert solve("""1
3 9
5 5 5
""") == "10", "all equal values"

assert solve("""1
4 1
-1000000000 0 3 9
""") == "-2000000000", "minimum boundary"

assert solve("""1
3 7
1 2 100
""") == "102", "duplicate sums"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 7`|`7`| Mảng nhỏ nhất có thể | 
|`3 9 / 5 5 5`|`10`| Tất cả các cặp tổng giống hệt nhau | 
|`4 1 / -1000000000 0 3 9`|`-2000000000`| Giá trị biên âm | 
|`3 7 / 1 2 100`|`102`| Tổng bằng nhau và ranh giới tìm kiếm nhị phân | 

## Vỏ cạnh 

Đối với trường hợp phần tử đơn:```
1
1 1
7
```Cặp duy nhất có thể là`(1,1)`, tính tổng`7`. Mảng được sắp xếp chứa một giá trị và cả hai giới hạn tìm kiếm nhị phân đều là`7`, do đó thuật toán trả về ngay kết quả đúng. 

Đối với các giá trị lặp lại:```
1
3 9
5 5 5
```Mỗi cặp đặt hàng sẽ mang lại`10`. Trong quá trình đếm, mọi ngưỡng tìm kiếm bên dưới`10`cho 0 cặp, trong khi`10`cho tất cả chín cặp. Tìm kiếm nhị phân tìm thấy ngưỡng đầu tiên đạt đến`K`. 

Đối với các giá trị âm:```
1
4 1
-1000000000 0 3 9
```Tổng nhỏ nhất có thể là phần tử đầu tiên được thêm vào chính nó,`-2000000000`. Việc tìm kiếm câu trả lời bắt đầu chính xác ở đó, do đó không cần bất kỳ xử lý đặc biệt nào đối với số âm. 

Đối với số tiền trùng lặp xung quanh câu trả lời:```
1
3 7
1 2 100
```Tổng nhỏ thứ bảy là`102`. Hàm đếm xử lý các tổng bằng nhau một cách chính xác vì nó đếm các giá trị nhỏ hơn hoặc bằng điểm giữa. Nếu trung điểm là`102`, nó đếm tất cả bảy cặp hợp lệ, do đó tìm kiếm nhị phân giữ giá trị này thay vì di chuyển qua nó.
