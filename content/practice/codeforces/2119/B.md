---
title: "CF 2119B - Đoạn đường"
description: "Chúng ta có hai điểm cố định trên một mặt phẳng: vị trí xuất phát và điểm đến. Chúng tôi cũng nhận được một chuỗi độ dài chuyển động. Ở bước i, chúng ta phải di chuyển từ vị trí hiện tại đến bất kỳ điểm nào có khoảng cách Euclide đến vị trí hiện tại chính xác là ai."
date: "2026-06-08T03:56:23+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2119
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1035 (Div. 2)"
rating: 1200
weight: 2119
solve_time_s: 71
verified: true
draft: false
---

[CF 2119B - Đoạn đường](https://codeforces.com/problemset/problem/2119/B) 

**Đánh giá:** 1200 
**Tags:** hình học, tham lam, toán học 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai điểm cố định trên một mặt phẳng: vị trí xuất phát và điểm đến. Chúng tôi cũng nhận được một chuỗi độ dài chuyển động. Ở bước`i`, chúng ta phải di chuyển từ vị trí hiện tại đến bất kỳ điểm nào có khoảng cách Euclide từ vị trí hiện tại chính xác bằng`a_i`. Hướng đi hoàn toàn tự do, nhưng khoảng cách là cố định. 

Sau khi thực hiện tất cả các bước di chuyển, chúng tôi muốn biết liệu có thể hạ cánh chính xác vào điểm mục tiêu hay không. 

Khó khăn chính là mỗi bước di chuyển đưa ra sự tự do liên tục về hướng, do đó tập hợp các điểm có thể tiếp cận sau mỗi bước là một ranh giới đĩa chứ không phải một điểm duy nhất. Sau nhiều bước, vùng có thể truy cập sẽ trở thành một hình dạng hình học đang phát triển được xác định bởi tất cả các tổng vectơ có thể có của các đoạn có độ dài cố định. 

Các ràng buộc quan trọng theo một cách rất cụ thể. Tổng số thao tác trên tất cả các trường hợp thử nghiệm lên tới 2 · 10^5, do đó, bất kỳ phương pháp mô phỏng hình học rõ ràng hoặc lưu trữ tập hợp điểm nào đều không thể thực hiện được. Ngay cả việc duy trì đa giác hoặc hướng lấy mẫu cũng sẽ quá chậm. 

Kiểu thất bại tự nhiên xuất phát từ việc suy nghĩ tham lam về những lựa chọn hướng đi. Ví dụ: luôn cố gắng nhắm về phía mục tiêu có thể thất bại vì những lựa chọn sớm có thể buộc các bước di chuyển sau đó vượt quá hoặc thiếu hụt khoảng cách cuối cùng. Một lỗi khác là chỉ giả định tổng số tiền là quan trọng mà bỏ qua hiệu ứng hủy bỏ. 

Ví dụ: nếu chúng ta chỉ so sánh khoảng cách giữa điểm bắt đầu và điểm kết thúc với tổng của tất cả`a_i`, chúng ta có thể nói: 

- nếu khoảng cách ≤ tổng thì có 

Điều này không đúng. Coi như:```
start = (0,0), end = (1,0)
a = [2, 2]
```Tổng tổng là 4, do đó logic ngây thơ cho rằng có thể truy cập được, nhưng trên thực tế, chúng ta không thể kết thúc ở khoảng cách 1 vì hai đoạn có độ dài 2 chỉ có thể đạt khoảng cách trong [0,4] nhưng có các ràng buộc giống như chẵn lẻ về thành phần vectơ. 

Giải pháp đúng không chỉ phụ thuộc vào tổng chiều dài mà còn phụ thuộc vào mức độ hủy bỏ giữa các phân đoạn. 

## Phương pháp tiếp cận 

Giải thích bạo lực là theo dõi tập hợp đầy đủ tất cả các vị trí có thể tiếp cận sau mỗi lần di chuyển. Sau bước`i`, mọi điểm có thể tiếp cận sẽ mở rộng thành một vòng tròn có tâm tại mỗi điểm trước đó có bán kính`a_i`. Đây nhanh chóng trở thành một khu vực liên tục có ranh giới ngày càng phức tạp. Trên thực tế, vùng có thể tiếp cận sau`i`bước là một đĩa có bán kính chỉ phụ thuộc vào cấu trúc tổng thể của độ dài đoạn, nhưng việc suy ra điều này về mặt hình học vẫn đòi hỏi phải hiểu tổng Minkowski. Mô phỏng rõ ràng là không thể. 

Quan sát quan trọng là bất biến duy nhất quan trọng là phạm vi có thể có của khoảng cách cuối cùng tính từ điểm bắt đầu. Sau mỗi đoạn, chúng ta sẽ thêm một vectơ có độ dài cố định nhưng có hướng tùy ý. Điều này làm giảm vấn đề trong việc theo dõi khoảng cách tối thiểu và tối đa có thể từ điểm gốc sau tất cả các lần di chuyển. 

Nếu chúng ta nghĩ ngược lại, sau tất cả các phân đoạn, điểm cuối so với điểm bắt đầu là tổng các vectơ có độ lớn cố định. Tập hợp tất cả các tổng như vậy tạo thành một hình khuyên: tất cả các khoảng cách giữa giá trị tối thiểu và giá trị tối đa. 

Khoảng cách tối đa là không đáng kể: chúng tôi căn chỉnh tất cả các đoạn theo cùng một hướng, đưa ra:```
max_dist = sum(a_i)
```Khoảng cách tối thiểu phụ thuộc vào việc hủy bỏ. Nếu một đoạn quá lớn so với phần còn lại thì không thể hủy hoàn toàn. Nếu chúng ta không sắp xếp gì và suy luận trực tiếp thì khoảng cách tối thiểu có thể đạt được là:```
min_dist = max(0, 2 * max(a_i) - sum(a_i))
```Điều này xuất phát từ ý tưởng rằng tất cả các phân khúc ngoại trừ phân khúc lớn nhất đều có thể cố gắng chống lại nó. Nếu phân khúc lớn nhất lớn hơn tổng của tất cả những phân khúc khác thì phần dư vẫn còn. 

Khi chúng tôi biết khoảng thời gian có thể truy cập`[min_dist, max_dist]`, vấn đề giảm xuống còn việc kiểm tra xem khoảng cách thực tế giữa điểm bắt đầu và điểm kết thúc có nằm bên trong nó hay không. 

Cho phép:```
D = distance between start and end
```Chúng tôi trả lời "Có" nếu:```
min_dist ≤ D ≤ max_dist
```| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hình học vũ phu | Hàm mũ / Liên tục | Lớn | Quá chậm | 
| Khoảng cách | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta chuyển lý luận thành một quy trình cụ thể. 

1. Tính bình phương khoảng cách Euclide giữa điểm đầu và điểm cuối. Chúng ta tránh căn bậc hai bằng cách so sánh các giá trị bình phương sau này. 
2. Tính tổng độ dài của tất cả các đoạn. Điều này thể hiện khoảng cách xa nhất có thể đạt được nếu mọi chuyển động đều được căn chỉnh theo cùng một hướng. 
3. Xác định độ dài đoạn lớn nhất. Điều này xác định xem các phân đoạn khác có thể hủy hoàn toàn nó hay không. 
4. Tính khoảng cách cuối cùng tối thiểu có thể bằng cách sử dụng:```
min_dist = max(0, 2 * max_a - sum_a)
```Điều này ghi lại độ dài “không cân bằng” còn sót lại sau khi hủy tối ưu. 
5. So sánh khoảng cách mục tiêu với quãng đường`[min_dist, sum_a]`. Nếu nó nằm bên trong thì xuất ra “Có”, nếu không thì xuất ra “Không”. 

Lý do điều này có tác dụng là vì các điểm cuối có thể tiếp cận tạo thành một phạm vi khoảng cách liên tục. Bất kỳ giá trị nào giữa mức tối thiểu và tối đa đều có thể đạt được vì chúng tôi có thể liên tục xoay hướng phân đoạn để nội suy giữa các cấu hình cực đoan. Hình dạng của tổng vectơ đảm bảo không có khoảng trống trong khoảng này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        px, py, qx, qy = map(int, input().split())
        a = list(map(int, input().split()))
        
        total = sum(a)
        mx = max(a)
        
        dx = px - qx
        dy = py - qy
        dist_sq = dx * dx + dy * dy
        
        min_dist = max(0, 2 * mx - total)
        
        # compare squared distances to avoid sqrt
        if min_dist * min_dist <= dist_sq <= total * total:
            print("Yes")
        else:
            print("No")

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc kiểm tra khoảng thời gian xuất phát. Sự tinh tế duy nhất là sử dụng khoảng cách bình phương để tránh lỗi dấu phẩy động. Khoảng cách tối thiểu và tối đa được tính theo thời gian tuyến tính. 

biểu hiện`2 * mx - total`được giới hạn ở mức 0 vì việc hủy bỏ có thể loại bỏ hoàn toàn sự dịch chuyển khi không có phân đoạn nào chiếm ưu thế. Lần kiểm tra cuối cùng hoàn toàn là bài kiểm tra tư cách thành viên theo khoảng thời gian. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2
start = (1,1), end = (5,1)
a = [3,3]
```| Bước | tổng cộng | max_a | phút_dist | D^2 | 
| --- | --- | --- | --- | --- | 
| ban đầu | 6 | 3 | 0 | 16 | 

Chúng tôi tính toán: 

- tổng cộng = 6 
- max_a = 3 
- min_dist = max(0, 2*3 - 6) = 0 
- Đ = 4 

Kiểm tra:```
0 ≤ 4 ≤ 6 → Yes
```Điều này cho thấy về nguyên tắc có thể hủy bỏ hoàn toàn, do đó, mọi khoảng cách trung gian lên tới 6 đều có thể đạt được. 

### Ví dụ 2 

đầu vào:```
n = 2
start = (100,100), end = (100,100)
a = [4,5]
```| Bước | tổng cộng | max_a | phút_dist | D^2 | 
| --- | --- | --- | --- | --- | 
| ban đầu | 9 | 5 | 1 | 0 | 

Chúng tôi tính toán: 

- tổng cộng = 9 
- max_a = 5 
- min_dist = max(0, 10 - 9) = 1 
-D = 0 

Kiểm tra:```
1 ≤ 0 is false → No
```Mặc dù chúng ta quay trở lại cùng một điểm về mặt hình học, sự mất cân bằng tạo ra ít nhất khoảng cách bằng 1 độ dịch chuyển, khiến cho việc quay trở lại chính xác là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | một lần để tính tổng và tối đa | 
| Không gian | O(1) | chỉ tổng hợp được lưu trữ | 

Tổng độ phức tạp trong tất cả các trường hợp thử nghiệm là tuyến tính ở kích thước đầu vào, vừa vặn thoải mái trong giới hạn lên tới 2 · 10^5 thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    out = []
    def fake_print(*args):
        out.append(" ".join(map(str, args)))
    builtins.print = fake_print

    solve()
    return "\n".join(out)

# provided samples
assert run("""5
2
1 1 5 1
3 3
3
1 1 3 3
2 3 4
2
100 100 100 100
4 5
1
5 1 1 4
5
2
10000000 10000000 10000000 10000000
10000 10000
""") == """Yes
Yes
No
Yes
Yes"""

# all equal, return to origin possible
assert run("""1
2
0 0 0 0
3 3
""") == "Yes"

# dominant segment prevents cancellation
assert run("""1
1
0 0 10 0
1
""") == "No"

# exact boundary case
assert run("""1
1
0 0 1 0
1
""") == "Yes"

# large symmetric case
assert run("""1
3
0 0 0 0
5 5 5
""") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả khoảng cách bằng 0 | Có | có thể hủy bỏ hoàn toàn | 
| phân khúc nhỏ duy nhất so với mục tiêu xa | Không | không thể dưới ràng buộc tối thiểu | 
| khả năng tiếp cận chính xác | Có | bao gồm ranh giới | 
| đoạn lớn đối xứng | Có | khoảng đúng | 

## Vỏ cạnh 

Khi tất cả các độ dài đoạn giống hệt nhau và mục tiêu là cùng một điểm, việc hủy bỏ cho phép quay trở lại điểm gốc hoàn toàn. Thuật toán tính toán`max_a = total / n`, cho`min_dist = 0`, vì vậy mọi mục tiêu có khoảng cách bằng 0 đều được chấp nhận. 

Khi chỉ có một đoạn, tập hợp có thể truy cập chính xác là một vòng tròn bán kính`a1`. Công thức mang lại`min_dist = max_dist = a1`, do đó chỉ điểm ở khoảng cách chính xác`a1`được chấp nhận, khớp hình học một cách chính xác. 

Khi một phân khúc thống trị tất cả những phân khúc khác, chẳng hạn như`[10,1,1]`, khoảng cách tối thiểu trở thành dương. Thuật toán ghi lại điều này như`2 * 10 - 12 = 8`, nghĩa là vị trí cuối cùng không thể đến gần hơn khoảng cách 8 tính từ điểm bắt đầu, phản ánh mức vượt quá không thể giảm được của nước đi dài nhất.
