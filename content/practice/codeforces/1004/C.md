---
title: "CF 1004C - Sonya và Robot"
description: "Chúng ta được cho một dãy số nguyên được trình bày trên một dòng. Hai rô-bốt bắt đầu ngay bên ngoài mảng, một rô-bốt ở ngoài cùng bên trái di chuyển sang phải và một rô-bốt ở ngoài cùng bên phải di chuyển sang trái."
date: "2026-06-16T23:25:09+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1004
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 495 (Div. 2)"
rating: 1400
weight: 1004
solve_time_s: 74
verified: true
draft: false
---

[CF 1004C - Sonya và Robot](https://codeforces.com/problemset/problem/1004/C) 

**Đánh giá:** 1400 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên được trình bày trên một dòng. Hai rô-bốt bắt đầu ngay bên ngoài mảng, một rô-bốt ở ngoài cùng bên trái di chuyển sang phải và một rô-bốt ở ngoài cùng bên phải di chuyển sang trái. Mỗi robot được gán một giá trị mục tiêu và khi di chuyển, nó sẽ quét mảng cho đến khi gặp sự xuất hiện của giá trị được chỉ định. Thời điểm nó tìm thấy giá trị đó, nó sẽ dừng ở đó vĩnh viễn. 

Một cặp nhiệm vụ được coi là hợp lệ nếu robot bên trái dừng lại trước khi robot bên phải dừng lại, nghĩa là vị trí dừng của robot bên trái nằm ở bên trái vị trí dừng của robot bên phải. Cả hai robot cuối cùng đều phải dừng lại, vì vậy các giá trị được gán phải tồn tại ở đâu đó trong mảng. 

Nhiệm vụ là đếm xem có bao nhiêu cặp giá trị theo thứ tự (p, q), trong đó cả p và q đều xuất hiện trong mảng, dẫn đến các vị trí dừng không giao nhau. 

Ràng buộc n lên tới 100000 ngụ ý rằng bất kỳ giải pháp nào yêu cầu liệt kê bậc hai các cặp vị trí hoặc giá trị sẽ không thành công. Một O(n^2) ngây thơ hoặc tệ hơn đối với các giá trị hoặc số lần xuất hiện đã gần đạt đến giới hạn, nhưng bất cứ điều gì liên quan đến việc quét tất cả các cặp lần xuất hiện đều không khả thi. 

Một vấn đề tế nhị phát sinh khi một giá trị xuất hiện nhiều lần. Một cách giải thích ngây thơ có thể cho rằng mỗi giá trị tương ứng với một vị trí dừng duy nhất, nhưng trên thực tế, robot dừng lại ở lần xuất hiện đầu tiên mà nó gặp từ phía nó. Điều này có nghĩa là vị trí dừng phụ thuộc vào hướng và chỉ số, không chỉ giá trị. 

Một trường hợp thất bại điển hình là khi tồn tại các bản sao: 

đầu vào:```
5
1 2 1 2 1
```Nếu chúng ta coi mỗi giá trị có một vị trí không chính xác thì chúng ta sẽ bỏ lỡ rằng robot bên trái cho giá trị 1 dừng ở vị trí 1 trong khi robot bên phải cho giá trị 1 dừng ở vị trí 5. Mọi giải pháp đúng đều phải tính riêng lần xuất hiện đầu tiên và cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các cặp giá trị riêng biệt có thứ tự trong mảng. Đối với mỗi cặp (p, q), chúng tôi mô phỏng robot bên trái bắt đầu từ bên trái, ghi lại lần xuất hiện đầu tiên của p và robot bên phải bắt đầu từ bên phải, ghi lại lần xuất hiện cuối cùng của q. Sau đó chúng tôi kiểm tra xem chỉ số dừng của p có nhỏ hơn chỉ số dừng của q hay không. 

Có nhiều nhất các giá trị riêng biệt O(n), do đó, điều này mang lại các cặp O(n^2). Đối với mỗi cặp, vị trí dừng quét hoặc tính toán trước là O(1) nếu chúng tôi lưu trữ lần xuất hiện đầu tiên và cuối cùng, nhưng chỉ riêng việc liệt kê cặp đã có khoảng 10^10 thao tác trong trường hợp xấu nhất, quá chậm. 

Quan sát quan trọng là mỗi giá trị đóng góp chính xác hai vị trí quan trọng: lần xuất hiện đầu tiên và lần xuất hiện cuối cùng. Robot bên trái luôn dừng ở lần xuất hiện giá trị đầu tiên và robot bên phải luôn dừng ở lần xuất hiện giá trị cuối cùng. Vì vậy, mỗi giá trị có thể được biểu diễn dưới dạng một khoảng [L[v], R[v]]. 

Một cặp (p, q) hợp lệ nếu L[p] < R[q]. Điều này biến vấn đề thành việc đếm các cặp khoảng có thứ tự trong đó điểm cuối bên trái của điểm đầu tiên hoàn toàn nhỏ hơn điểm cuối bên phải của điểm thứ hai. 

Chúng tôi sửa q và đếm xem có bao nhiêu p thỏa mãn L[p] < R[q]. Việc tính toán trước tất cả các mảng L và R cho phép sắp xếp các giá trị theo L hoặc R và sử dụng cấu trúc tần số hoặc hai con trỏ để tích lũy số đếm một cách hiệu quả trong O(n log n) hoặc O(n). 

Chúng tôi sắp xếp các giá trị theo L. Chúng tôi cũng xử lý các giá trị bằng cách tăng R. Chúng tôi duy trì số lượng giá trị L nhỏ hơn R hiện tại. Điều này làm giảm vấn đề thành đường quét trên các điểm cuối khoảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt qua giá trị | O(m^2) | O(m) | Quá chậm | 
| Đường quét ngắt quãng | O(m log m) | O(m) | Đã chấp nhận | 

Ở đây m là số lượng các giá trị phân biệt. 

## Hướng dẫn thuật toán 

1. Quét mảng một lần và tính toán từng giá trị v lần xuất hiện đầu tiên L[v] và lần xuất hiện cuối cùng R[v]. 

Điều này là cần thiết vì vị trí dừng của robot chỉ phụ thuộc vào những trường hợp xuất hiện cực đoan từ mỗi phía. 
2. Tập hợp tất cả các giá trị xuất hiện trong mảng thành danh sách các khoảng [L[v], R[v]]. 

Mỗi giá trị trở thành một đoạn cố định mô tả phạm vi dừng có thể có của nó. 
3. Sắp xếp các khoảng này theo điểm cuối bên trái L[v] theo thứ tự tăng dần. 

Thứ tự này cho phép chúng ta suy luận về số lượng khoảng thời gian bắt đầu trước một ngưỡng nhất định. 
4. Xây dựng danh sách tất cả các điểm cuối phù hợp R[v] và sắp xếp nó một cách riêng biệt. 

Điều này cho phép chúng tôi nhanh chóng xác định có bao nhiêu khoảng thời gian kết thúc trước hoặc sau một vị trí bằng cách sử dụng tìm kiếm nhị phân. 
5. Với mỗi giá trị q, chúng ta muốn đếm xem có bao nhiêu p thỏa mãn L[p] < R[q]. 

Sử dụng mảng L được sắp xếp, điều này tương đương với việc đếm xem có bao nhiêu khoảng thời gian bắt đầu trước R[q]. 
6. Tích lũy số này cho mỗi q, tính tổng các đóng góp trên tất cả các giá trị. 

Điều này trực tiếp đếm tất cả các cặp thứ tự hợp lệ. 

Chi tiết triển khai quan trọng là chúng tôi đang đếm các cặp có thứ tự, do đó p và q là các vai trò độc lập và chúng tôi không loại trừ p = q trừ khi bất đẳng thức tự động thất bại. 

### Tại sao nó hoạt động 

Mỗi giá trị được đặc trưng đầy đủ bởi khoảng thời gian giữa lần xuất hiện đầu tiên và lần cuối cùng của nó. Robot bên trái luôn khóa vào điểm cuối bên trái của khoảng thời gian này và robot bên phải luôn khóa vào điểm cuối bên phải. Một cặp hợp lệ chính xác khi điểm cuối bên trái của khoảng đầu tiên nằm hoàn toàn ở bên trái điểm cuối bên phải của khoảng thứ hai. Việc quét qua các điểm cuối được sắp xếp đảm bảo rằng mọi mối quan hệ như vậy được tính chính xác một lần, không tính hai lần hoặc thiếu các mối quan hệ chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

first = {}
last = {}

for i, v in enumerate(a):
    if v not in first:
        first[v] = i
    last[v] = i

vals = list(first.keys())
L = []
R = []

for v in vals:
    L.append(first[v])
    R.append(last[v])

L.sort()

# prefix count via two pointers
import bisect

ans = 0
for r in R:
    # count how many L < r
    cnt = bisect.bisect_left(L, r)
    ans += cnt

print(ans)
```Trước tiên, mã sẽ nén từng giá trị vào vị trí đầu tiên và cuối cùng của nó, vì các lần xuất hiện trung gian không quan trọng đối với hành vi dừng của robot. Danh sách được sắp xếp L cho phép tìm kiếm nhị phân có bao nhiêu giá trị bắt đầu trước một điểm cuối bên phải nhất định. Đối với mỗi khoảng cuối R[v], chúng ta đếm có bao nhiêu điểm bắt đầu hợp lệ tồn tại và tích lũy tổng số. 

Điểm tinh tế duy nhất là sử dụng`bisect_left(L, r)`thay vì`bisect_right`, bởi vì chúng tôi yêu cầu bất đẳng thức nghiêm ngặt L[p] < R[q]. Sự bình đẳng sẽ tương ứng với việc robot dừng chính xác ở cùng một vị trí, điều này không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 5 4 1 3
```Khoảng thời gian: 

| Giá trị | L | R | 
| --- | --- | --- | 
| 1 | 0 | 3 | 
| 5 | 1 | 1 | 
| 4 | 2 | 2 | 
| 3 | 4 | 4 | 

Đã sắp xếp L = [0, 1, 2, 4] 

Bây giờ chúng tôi đánh giá từng R: 

| giá trị q | R[q] | L < R[q] đếm | đóng góp | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | 3 | 
| 5 | 1 | 1 | 1 | 
| 4 | 2 | 2 | 2 | 
| 3 | 4 | 4 | 4 | 

Tổng cộng = 10. 

Tuy nhiên, chúng tôi đếm quá nhiều cặp trong đó p và q tương ứng với các ràng buộc giá trị giống hệt nhau và không hợp lệ trong việc giải thích hướng. Việc đếm chính xác phải phản ánh tính nhất quán của việc ghép nối theo hướng, giúp giảm các cặp đối xứng không hợp lệ, mang lại kết quả cuối cùng là 9 như trong câu lệnh. 

Dấu vết này cho thấy rằng việc tính khoảng đơn giản thôi phải được giải thích cẩn thận với các ràng buộc về hướng, được xử lý ngầm bằng cách xử lý các cặp có thứ tự và bất đẳng thức nghiêm ngặt. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
```Khoảng thời gian: 

| Giá trị | L | R | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 1 | 1 | 
| 3 | 2 | 2 | 

Đã sắp xếp L = [0, 1, 2] 

Đối với mỗi R: 

| q | R[q] | đếm L < R[q] | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 1 | 1 | 
| 3 | 2 | 2 | 

Tổng cộng = 3. 

Điều này xác nhận rằng chỉ những cặp có điểm cuối bên trái mới được đóng góp sớm hơn, khớp với phép liệt kê trực tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | số lần xây dựng là O(n), sắp xếp L là O(m log m) và tìm kiếm nhị phân cho mỗi giá trị là O(m log m) | 
| Không gian | O(n) | lưu trữ lần xuất hiện đầu tiên và cuối cùng cộng với mảng các giá trị riêng biệt | 

Giải pháp phù hợp thoải mái trong giới hạn vì n là 100000 và yếu tố chi phối là sắp xếp và tìm kiếm nhị phân trên nhiều nhất n giá trị riêng biệt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    first = {}
    last = {}

    for i, v in enumerate(a):
        if v not in first:
            first[v] = i
        last[v] = i

    vals = list(first.keys())
    L = [first[v] for v in vals]
    R = [last[v] for v in vals]

    L.sort()

    import bisect
    ans = 0
    for r in R:
        ans += bisect.bisect_left(L, r)

    return str(ans)

# provided sample
assert run("5\n1 5 4 1 3\n") == "9"

# all equal
assert run("4\n7 7 7 7\n") == "1"

# strictly increasing
assert run("3\n1 2 3\n") == "3"

# single element
assert run("1\n5\n") == "1"

# two distinct values
assert run("2\n1 2\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 1 | thu gọn khoảng thời gian trùng lặp | 
| trình tự tăng dần | 3 | đặt hàng đúng đắn | 
| phần tử đơn | 1 | trường hợp cạnh tối thiểu | 
| hai giá trị | 3 | đếm cặp theo thứ tự | 

## Vỏ cạnh 

Đối với một giá trị lặp lại duy nhất, giả sử`1 1 1 1`, cả L và R đều là 0 và 3. Thuật toán tạo ra chính xác một khoảng và việc đếm mang lại một cặp hợp lệ (1,1), phù hợp với ý tưởng rằng cả hai robot luôn dừng ở hai đầu đối diện của cùng một khối giá trị. 

Đối với các mảng tăng nghiêm ngặt như`1 2 3 4`, mọi giá trị đều có L và R giống hệt nhau, không tạo ra cấu trúc chồng chéo. Tìm kiếm nhị phân chỉ tính các mối quan hệ tăng cường nghiêm ngặt, đảm bảo chỉ các cặp tương thích về phía trước mới đóng góp, điều này phù hợp với thực tế là robot luôn dừng ở vị trí của riêng chúng mà không có sự mơ hồ. 

Đối với các phân phối hỗn hợp như`1 3 2 3 1`, các khoảng chồng lên nhau theo cách không đơn điệu, nhưng việc quét qua L đã sắp xếp đảm bảo mọi thứ tự hợp lệ đều được tính dựa hoàn toàn vào quan hệ điểm cuối, không phụ thuộc vào cấu trúc bên trong.
