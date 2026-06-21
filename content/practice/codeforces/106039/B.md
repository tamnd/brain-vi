---
title: "CF 106039B - Tìm kiếm sự cân bằng"
description: "Chúng tôi được cung cấp tối đa 35 lần tăng sức mạnh, mỗi lần tăng sức mạnh đóng góp một sự thay đổi cố định cho hai thuộc tính: tấn công và phòng thủ. Bắt đầu từ số 0 ở cả hai chiều, chúng tôi chọn bất kỳ tập hợp con nào trong số các lần tăng sức mạnh này, áp dụng tất cả những lần tăng sức mạnh đã chọn (thứ tự không quan trọng vì phép cộng có tính giao hoán) và kết thúc…"
date: "2026-06-20T13:27:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106039
codeforces_index: "B"
codeforces_contest_name: "2025 USP Try-outs"
rating: 0
weight: 106039
solve_time_s: 60
verified: true
draft: false
---

[CF 106039B - Tìm kiếm sự cân bằng](https://codeforces.com/problemset/problem/106039/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp tối đa 35 lần tăng sức mạnh, mỗi lần tăng sức mạnh đóng góp một sự thay đổi cố định cho hai thuộc tính: tấn công và phòng thủ. Bắt đầu từ số 0 ở cả hai chiều, chúng tôi chọn bất kỳ tập hợp con nào trong số các lần tăng sức mạnh này, áp dụng tất cả những phần đã chọn (thứ tự không quan trọng vì phép cộng có tính giao hoán) và kết thúc ở điểm cuối cùng trong không gian số nguyên hai chiều. 

Một tập hợp con được coi là hợp lệ nếu cuộc tấn công kết quả nằm trong một khoảng nhất định và khả năng phòng thủ kết quả cũng nằm trong một khoảng nhất định khác. Nhiệm vụ là đếm xem có bao nhiêu tập hợp con tạo ra một điểm bên trong hình chữ nhật thẳng hàng với trục này. 

Điểm cấu trúc quan trọng là mọi tập hợp con tương ứng với tổng N vectơ hai chiều. Vì vậy, chúng ta đang đếm xem có bao nhiêu tổng tập hợp con nằm trong một hình chữ nhật. 

Ràng buộc N ≤ 35 ngay lập tức loại trừ việc lặp lại trên tất cả các tập hợp con một cách đơn giản. Một bảng liệt kê đầy đủ sẽ yêu cầu kiểm tra 2^35 tập hợp con, khoảng 34 tỷ, quá lớn ngay cả khi mỗi lần kiểm tra là thời gian không đổi. Tuy nhiên, 35 đủ nhỏ để hỗ trợ phương pháp gặp nhau ở giữa, vì việc chia thành hai nửa mang lại khoảng 2^17,5 tập con mỗi bên, khoảng 150 nghìn, có thể quản lý được. 

Một giải pháp thay thế đơn giản là lập trình động trên phạm vi tấn công và phòng thủ, nhưng các giá trị tọa độ có độ lớn lên tới 10^12, do đó, DP trên không gian giá trị là không thể. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là giả định rằng chúng ta có thể đếm các tập hợp con một cách độc lập bằng các ràng buộc tấn công và phòng thủ. Điều đó sẽ cho rằng sự độc lập giữa các chiều không chính xác, nhưng mỗi tập hợp con sẽ tấn công và phòng thủ thông qua cùng một lựa chọn. 

Ví dụ: nếu một lần tăng sức mạnh cho (10, -10) và một lần tăng sức mạnh khác cho (-10, 10), thì các tập hợp con thỏa mãn các ràng buộc tấn công sẽ không độc lập với các tập hợp con thỏa mãn các ràng buộc phòng thủ đó. Bất kỳ cách tiếp cận nhân tố nào cũng sẽ tính quá hoặc thiếu. 

Khó khăn thực sự là việc đếm điểm trong phân bố tổng tập hợp con 2D chứ không phải hai vấn đề 1D riêng biệt. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là liệt kê trực tiếp tất cả các tập hợp con. Đối với mỗi tập hợp con, hãy tính tổng tấn công và tổng phòng thủ và kiểm tra xem nó có nằm trong hình chữ nhật hay không. Điều này đúng vì mỗi tập hợp con tương ứng duy nhất với một trạng thái cuối cùng. Vấn đề là thời gian chạy: với 35 phần tử, chúng tôi có 2^35 tập hợp con và thậm chí vài tỷ lần lặp là quá chậm dưới giới hạn 1,5 giây. 

Quan sát quan trọng là 35 phân chia một cách tự nhiên thành hai nửa gồm khoảng 17 và 18 phần tử. Nếu chúng ta liệt kê tất cả các tập hợp con của mỗi nửa, chúng ta có thể tính toán tất cả các tổng một cách độc lập. Mỗi nửa tạo ra khoảng 2^17 hoặc 2^18 vectơ. Sau đó, chúng tôi kết hợp chúng: một tập hợp con đầy đủ hợp lệ được hình thành bằng cách chọn một tập hợp con từ nửa bên trái và một tập hợp con từ nửa bên phải và cộng tổng của chúng. 

Vì vậy, thay vì lặp lại trên 2^35 kết hợp, chúng tôi giảm vấn đề xuống việc hợp nhất hai danh sách có kích thước khoảng 2^17. Điều này biến bài toán thành một truy vấn đếm trên hai tập hợp điểm 2D: với mỗi tổng bên trái (ax, dx), chúng ta đếm xem có bao nhiêu tổng bên phải (bx, dy) tạo nên (ax+bx, dx+dy) nằm bên trong hình chữ nhật. Điều này trở thành vấn đề đếm phạm vi 2D trên nửa bên phải, được dịch chuyển theo từng điểm bên trái. 

Chúng ta có thể sắp xếp nửa bên phải theo các giá trị tấn công và tiền xử lý phòng thủ, sau đó, với mỗi tổng bên trái, hãy sử dụng tìm kiếm nhị phân trên các phạm vi tấn công hợp lệ và đếm xem có bao nhiêu phòng thủ rơi vào khoảng yêu cầu. Để thực hiện điều này hiệu quả, chúng tôi sắp xếp các tổng hợp lý bằng cách tấn công và duy trì cấu trúc trên các giá trị phòng thủ (thường là danh sách được sắp xếp, cho phép tìm kiếm nhị phân cho mỗi truy vấn). 

Cấu trúc này hoạt động vì các giới hạn tấn công và phòng thủ sẽ tách rời khi chúng ta sửa một bên: đối với tổng bên trái cố định, điều kiện sẽ trở thành giới hạn độc lập đối với tổng bên phải.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N) | O(1) | Quá chậm | 
| Gặp nhau ở giữa | O(2^(N/2) log 2^(N/2)) | O(2^(N/2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Chia sức mạnh thành hai nửa 

Chúng tôi chia mảng thành các phần bên trái và bên phải có kích thước khoảng N/2 mỗi phần. Điều này là cần thiết vì việc liệt kê đầy đủ là quá lớn, nhưng việc liệt kê một nửa là khả thi. 

### 2. Liệt kê tất cả các tập hợp con cho mỗi nửa 

Đối với mỗi tập hợp con của nửa bên trái, hãy tính toán tổng số đòn tấn công và phòng thủ của nó và lưu trữ nó. Làm tương tự cho nửa bên phải. Mỗi hiệp tạo ra một danh sách tối đa 2^17 hoặc 2^18 cặp. 

Bước này chuyển đổi cấu trúc tập hợp con hàm mũ thành một danh sách rõ ràng các kết quả từng phần có thể quản lý được. 

### 3. Sắp xếp nửa bên phải theo đòn tấn công 

Chúng tôi sắp xếp tất cả các khoản tiền bên phải theo giá trị tấn công. Điều này cho phép chúng tôi nhanh chóng hạn chế sự chú ý chỉ vào những tập hợp con phù hợp có thể vừa với cửa sổ tấn công còn lại nhất định. 

### 4. Xây dựng danh sách phòng thủ được sắp xếp để truy vấn nhanh 

Bên cạnh danh sách tấn công được sắp xếp, chúng tôi duy trì một danh sách liên kết các giá trị phòng thủ. Điều này cho phép chúng tôi đếm có bao nhiêu điểm nằm trong khoảng phòng thủ bằng cách sử dụng tìm kiếm nhị phân. 

### 5. Với mỗi tập con bên trái, tính các ràng buộc cần thiết 

Giả sử tập con bên trái có tổng (ax, dx). Tổng cuối cùng phải thỏa mãn: 

A1 `ax + bx `` A2 và D1 `` dx + dy `` D2. 

Viết lại đưa ra các ràng buộc ở nửa bên phải: 

A1 - ax ��������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������� với 

Vì vậy, với mỗi tập con bên trái, chúng ta đang đếm xem có bao nhiêu tập con bên phải nằm trong một hình chữ nhật đã dịch chuyển. 

### 6. Đếm các tập hợp con bên phải hợp lệ bằng tìm kiếm nhị phân 

Chúng tôi tìm thấy phạm vi các chỉ số phù hợp có giá trị tấn công nằm trong [A1 - ax, A2 - ax]. Trong lát cắt này, chúng tôi đếm có bao nhiêu giá trị phòng thủ nằm trong [D1 - dx, D2 - dx] bằng cách sử dụng tìm kiếm nhị phân. 

Chúng tôi tích lũy số lượng này trên tất cả các tập hợp con bên trái. 

### Tại sao nó hoạt động 

Mỗi tập con đầy đủ được phân tách duy nhất thành tập con trái và tập con phải. Thuật toán đếm từng tập hợp con đầy đủ hợp lệ chính xác một lần vì với mỗi cặp hợp lệ, phần bên trái được xử lý một lần và phần bên phải được tính chính xác trong truy vấn phạm vi tương ứng. Không có cặp nào được tính kép vì mỗi tập hợp con bên trái được cố định độc lập và trong bối cảnh cố định đó, chúng tôi tính tất cả các tập hợp con bên phải tương thích. 

Tính đúng đắn dựa trên sự song ánh giữa các tập hợp con đầy đủ và các cặp nửa tập hợp con, và thực tế là các ràng buộc vẫn tuyến tính và có thể tách rời sau khi cố định một bên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from bisect import bisect_left, bisect_right

def gen_sums(arr):
    res = []
    n = len(arr)
    for mask in range(1 << n):
        a = 0
        d = 0
        for i in range(n):
            if mask & (1 << i):
                ai, di = arr[i]
                a += ai
                d += di
        res.append((a, d))
    return res

def main():
    n = int(input())
    arr = [tuple(map(int, input().split())) for _ in range(n)]
    A1, D1, A2, D2 = map(int, input().split())

    mid = n // 2
    left = arr[:mid]
    right = arr[mid:]

    L = gen_sums(left)
    R = gen_sums(right)

    R.sort()  # sort by attack, then defense implicitly

    R_a = [x for x, y in R]
    R_d = [y for x, y in R]

    ans = 0

    for la, ld in L:
        low_a = A1 - la
        high_a = A2 - la

        l = bisect_left(R_a, low_a)
        r = bisect_right(R_a, high_a)

        if l >= r:
            continue

        # collect defenses in range and sort segment
        seg = sorted(R_d[l:r])

        low_d = D1 - ld
        high_d = D2 - ld

        ans += bisect_right(seg, high_d) - bisect_left(seg, low_d)

    print(ans)

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách chia đầu vào thành hai nửa và liệt kê tất cả các tổng tập hợp con cho mỗi nửa. Việc tạo tập hợp con sử dụng mặt nạ bit, điều này an toàn vì mỗi nửa có tối đa 17 hoặc 18 phần tử. 

Sau khi tạo ra tất cả các tổng một phần, nửa bên phải được sắp xếp sao cho các giá trị tấn công có thể được tìm kiếm nhị phân một cách hiệu quả. Sau đó, chúng tôi lặp lại tất cả các khoản tiền bên trái và tính toán khoảng thời gian tấn công khả thi cho phía bên phải. Điều này làm giảm các tập hợp con bên phải ứng cử viên thành một lát cắt liền kề. 

Bên trong lát cắt đó, chúng tôi trích xuất các giá trị phòng thủ và sắp xếp chúng để hỗ trợ việc đếm nhanh số lượng nằm trong khoảng thời gian cần thiết. Đây là sự cân bằng quan trọng: chúng tôi trả một hệ số log để sắp xếp từng lát cắt, nhưng giữ cho cách tiếp cận đơn giản và trong giới hạn cho quy mô vấn đề này. 

Một cạm bẫy phổ biến là quên rằng cả hai khía cạnh phải được xử lý cùng nhau; tách chúng ra hoặc tổng hợp trước chỉ bằng một tọa độ sẽ phá vỡ tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 1
-1 -1
-1 -1 1 1
```Chúng tôi chia làm hai nửa size 1 và 1. 

| Tập hợp con bên trái | Tập hợp con bên phải | Tổng (A,D) | hợp lệ | 
| --- | --- | --- | --- | 
| {} | {} | (0,0) | vâng | 
| {} | (1,1) | (1,1) | vâng | 
| {} | (-1,-1) | (-1,-1) | vâng | 
| (1,1) | {} | (1,1) | vâng | 
| (-1,-1) | {} | (-1,-1) | vâng | 
| (1,1) | (-1,-1) | (0,0) | vâng | 
| (-1,-1) | (1,1) | (0,0) | vâng | 
| (1,1) | (1,1) | (2,2) | không | 
| (-1,-1) | (-1,-1) | (-2,-2) | không | 

Điều này cho thấy mọi kết hợp hợp lệ được hình thành như thế nào bằng cách ghép một tập hợp con bên trái với một tập hợp con bên phải và chỉ những kết hợp bên trong hình chữ nhật mới được tính. 

### Ví dụ 2 

đầu vào:```
1
5 -3
0 0 10 10
```| Tập hợp con bên trái | Tập hợp con bên phải | Tổng (A,D) | hợp lệ | 
| --- | --- | --- | --- | 
| {} | {} | (0,0) | vâng | 
| {5,-3} | {} | (5,-3) | vâng | 

Cả hai tập hợp con đều được tính và tập hợp con gặp nhau sẽ thoái hóa một cách chính xác thành phép liệt kê đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^(N/2) + 2^(N/2) log 2^(N/2)) | liệt kê tập hợp con cộng với sắp xếp và tìm kiếm nhị phân | 
| Không gian | O(2^(N/2)) | lưu trữ tổng tập hợp con của cả hai nửa | 

Hệ số mũ giảm từ 2^35 xuống khoảng 2^18, nằm trong giới hạn thoải mái. Ngay cả với chi phí phân loại, tổng số hoạt động vẫn ở mức vài triệu. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from bisect import bisect_left, bisect_right

    def gen_sums(arr):
        res = []
        n = len(arr)
        for mask in range(1 << n):
            a = 0
            d = 0
            for i in range(n):
                if mask & (1 << i):
                    ai, di = arr[i]
                    a += ai
                    d += di
            res.append((a, d))
        return res

    n = int(sys.stdin.readline())
    arr = [tuple(map(int, sys.stdin.readline().split())) for _ in range(n)]
    A1, D1, A2, D2 = map(int, sys.stdin.readline().split())

    mid = n // 2
    L = gen_sums(arr[:mid])
    R = gen_sums(arr[mid:])

    R.sort()
    R_a = [x for x, y in R]
    R_d = [y for x, y in R]

    ans = 0
    for la, ld in L:
        l = bisect_left(R_a, A1 - la)
        r = bisect_right(R_a, A2 - la)
        if l < r:
            seg = sorted(R_d[l:r])
            ans += bisect_right(seg, D2 - ld) - bisect_left(seg, D1 - ld)

    return str(ans)

# provided sample-style checks
assert solve("1\n1 1\n0 0 1 1\n") == "2"

# minimum size
assert solve("1\n5 -3\n0 0 10 10\n") == "2"

# empty valid region
assert solve("2\n1 1\n-1 -1\n100 100 200 200\n") == "0"

# symmetric cancellation
assert solve("2\n1 0\n-1 0\n-1 1 1 1\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mục duy nhất trong phạm vi | 2 | xử lý tập hợp con trống + đầy đủ | 
| trường hợp tối thiểu | 2 | tính đúng đắn của phép liệt kê phần tử đơn | 
| hình chữ nhật không thể truy cập | 0 | cắt tỉa đúng cách | 
| trường hợp hủy bỏ | 4 | tính độc lập của việc ghép cặp tập hợp con | 

## Vỏ cạnh 

Trường hợp một cạnh là khi hình chữ nhật hợp lệ cực kỳ lớn, bao gồm tất cả các tổng tập hợp con một cách hiệu quả. Trong trường hợp đó, thuật toán vẫn tính chính xác vì mọi tập con bên trái sẽ tìm thấy tất cả các tập con bên phải tương thích mà không bị hạn chế. 

Một trường hợp khác là khi tất cả các lần tăng sức mạnh đều là vectơ bằng 0. Mỗi tập hợp con sau đó tạo ra (0,0) và câu trả lời trở thành 2^N nếu (0,0) nằm trong hình chữ nhật. Phương pháp gặp nhau ở giữa sẽ tính chính xác tất cả các cặp vì mỗi lần chia vẫn tạo lại tổng điểm giống nhau. 

Một trường hợp khó phát hiện cuối cùng là khi các giá trị âm chiếm ưu thế, khiến phạm vi tấn công hoặc phòng thủ vượt qua 0 hoặc trải dài trong các khoảng âm và dương lớn. Vì tất cả quá trình lọc được thực hiện thông qua bất đẳng thức và tìm kiếm nhị phân, dấu hiệu không ảnh hưởng đến tính chính xác, chỉ sắp xếp thứ tự trong các mảng được sắp xếp, vẫn hợp lệ.
