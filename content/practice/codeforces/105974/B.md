---
title: "CF 105974B - Truy vấn chuỗi Xor riêng biệt"
description: "Nhiệm vụ là duy trì một chuỗi trong khi các giá trị được thêm vào nó. Sau mỗi thao tác nối thêm, một số truy vấn yêu cầu giá trị riêng biệt nhỏ thứ k có thể thu được bằng cách lấy bất kỳ chuỗi con nào của chuỗi hiện tại và XOR các phần tử đã chọn."
date: "2026-06-25T13:33:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105974
codeforces_index: "B"
codeforces_contest_name: "Introductory Problems: XOR Basis"
rating: 0
weight: 105974
solve_time_s: 44
verified: true
draft: false
---

[CF 105974B - Truy vấn chuỗi con Xor riêng biệt](https://codeforces.com/problemset/problem/105974/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là duy trì một chuỗi trong khi các giá trị được thêm vào nó. Sau mỗi thao tác nối thêm, một số truy vấn yêu cầu giá trị riêng biệt nhỏ thứ k có thể thu được bằng cách lấy bất kỳ chuỗi con nào của chuỗi hiện tại và XOR các phần tử đã chọn. Cho phép dãy con trống nên số 0 luôn là một trong những đáp án khả thi. Nếu tồn tại ít hơn k kết quả XOR khác nhau, truy vấn trả về -1. Đây là phiên bản truy vấn để duy trì khoảng XOR của một chuỗi. 

Độ dài chuỗi có thể tăng lên 200000 và mọi giá trị đều phù hợp với 60 bit. Một giải pháp cố gắng tạo ra các chuỗi con ngay lập tức là không thể vì số lượng các chuỗi con tăng theo cấp số nhân. Ngay cả một giải pháp lưu trữ mọi giá trị XOR có thể truy cập cuối cùng cũng sẽ cần xử lý tối đa 2^60 giá trị có thể, những giá trị này không thể vừa với bộ nhớ. Quan sát hữu ích là XOR hoạt động giống như đại số tuyến tính trên bit. Số lượng bit được cố định ở mức 60, do đó có thể thực hiện các phép toán tỷ lệ thuận với số lượng bit. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Hãy xem xét một chuỗi chỉ chứa các giá trị lặp lại.```
Input
3
1 5
1 5
2 3
```Sau hai lần chèn, các giá trị XOR có thể chỉ là 0 và 5. Câu trả lời là -1, không phải giá trị được tạo khác. Giải pháp đếm các phần tử được chèn thay vì các hướng XOR độc lập sẽ thất bại ở đây. 

Một trường hợp khác là khi một giá trị có thể được hình thành từ các giá trị trước đó.```
Input
4
1 1
1 2
1 3
2 4
```Các giá trị có thể truy cập là 0, 1, 2, 3. Câu trả lời là -1 vì chỉ có bốn giá trị riêng biệt. Việc triển khai bất cẩn có thể cho rằng ba số tạo ra tám dãy con và trả lời như thể có tám kết quả XOR khác nhau. 

Trường hợp ranh giới là truy vấn đầu tiên.```
Input
2
2 1
2 2
```Trước bất kỳ thao tác chèn nào, XOR duy nhất có thể là 0 từ chuỗi con trống. Truy vấn đầu tiên trả về 0 và truy vấn thứ hai trả về -1. Cơ sở phải hỗ trợ giá trị XOR trống mặc dù không có số nào được chèn vào. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là giữ một tập hợp tất cả các giá trị XOR có thể được tạo. Khi một số mới xuất hiện, mọi giá trị hiện có có thể không thay đổi hoặc được XOR với số mới. Điều này có tác dụng vì mọi dãy con cũ đều bỏ qua phần tử mới hoặc lấy nó. Tuy nhiên, sau nhiều thao tác, bộ có thể đạt đến kích thước không gian XOR đầy đủ, lên tới 2^60. Cập nhật và lưu trữ nhiều giá trị đó là không thể. 

Lý do khiến lực lượng vũ phu phát triển nhanh chóng như vậy là vì nhiều chuỗi tiếp theo khác nhau thực sự là dư thừa. Đối với XOR, mỗi số là một vectơ bit và việc lấy XOR tương đương với việc cộng các vectơ trong đó phép cộng được thực hiện theo modulo 2. Một tập hợp các vectơ độc lập là đủ để mô tả mọi kết quả XOR có thể có. Cấu trúc này được gọi là cơ sở tuyến tính. 

Chúng tôi duy trì một vectơ cơ sở cho mỗi bit được đặt cao nhất có thể. Khi chèn một số, chúng tôi cố gắng loại bỏ bit cao nhất của nó bằng cách sử dụng vectơ cơ sở hiện có. Nếu số trở thành số 0 thì nó đã có thể biểu diễn được. Nếu nó vẫn có bit cao nhất mà không có vectơ cơ sở nào sở hữu, chúng tôi sẽ lưu trữ nó ở đó. Vì chỉ có 60 bit nên mỗi lần chèn đều nhanh. 

Để trả lời truy vấn nhỏ thứ k, cơ sở biểu thị một khoảng trắng có chính xác 2^giá trị xếp hạng khác nhau. Các vectơ cơ sở có thể được coi là các quyết định nhị phân. Bắt đầu từ bit cao nhất, chúng tôi quyết định xem câu trả lời có nên đặt bit đó hay không bằng cách kiểm tra xem có bao nhiêu giá trị có thể có với bit bằng 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) mỗi lần cập nhật | O(2^60) | Quá chậm | 
| Tối ưu | O(60) mỗi lần phẫu thuật | O(60) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì một mảng`basis`có kích thước 60.`basis[i]`lưu trữ vectơ có bit được đặt cao nhất là`i`hoặc bằng 0 nếu không tồn tại vectơ như vậy. 

Khi một số được chèn vào, chúng ta giảm nó từ bit cao xuống bit thấp. Bit cao nhất là phần quan trọng nhất vì hai vectơ cơ sở không thể chia sẻ cùng một bit cao nhất. 

1. Để chèn, hãy quét các bit từ 59 xuống 0. Nếu bit hiện tại của số bằng 0, điều đó không ảnh hưởng đến việc giảm. Nếu đã có vectơ cơ sở ở bit này, hãy XOR nó đi. Ngược lại, lưu trữ số hiện tại làm vectơ cơ sở mới. 

Điều này bảo toàn cùng một tập hợp các giá trị XOR có thể truy cập được vì việc thay thế một số bằng XOR của nó bằng một số có thể truy cập khác không làm thay đổi những gì có thể được hình thành. 

1. Giữ thứ hạng hiện tại, là số vectơ cơ sở khác 0. Số lượng giá trị XOR riêng biệt là`2^rank`. 

Nếu một truy vấn yêu cầu k lớn hơn số tiền này thì sẽ không có câu trả lời. 

1. Để tìm giá trị nhỏ thứ k, xử lý cơ sở từ bit cao nhất trở xuống. Các vectơ có bit cao nhất nhỏ hơn không thể thay đổi các quyết định đã được thực hiện trên các bit lớn hơn. 

Đối với một vectơ cơ sở tại bit`i`, chính xác một nửa số kết hợp còn lại có bit`i`bằng 0 và một nửa có nó bằng một. Nếu như`k`nằm trong nhóm 0, bỏ qua vectơ. Ngược lại, lấy vectơ và trừ kích thước của nhóm 0 khỏi`k`. 

1. Trả về giá trị đã xây dựng. 

Tại sao nó hoạt động: các vectơ cơ sở là độc lập, do đó, mọi giá trị XOR có thể tiếp cận đều có chính xác một biểu diễn dưới dạng tập hợp con của các vectơ cơ sở. Bit khác biệt cao nhất giữa hai biểu diễn quyết định giá trị nào lớn hơn, có nghĩa là việc tìm kiếm từ bit cao đến bit thấp khớp với thứ tự sắp xếp của tất cả các giá trị có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    basis = [0] * 60
    rank = 0
    ans = []

    def insert(x):
        nonlocal rank
        for b in range(59, -1, -1):
            if ((x >> b) & 1) == 0:
                continue
            if basis[b]:
                x ^= basis[b]
            else:
                basis[b] = x
                rank += 1
                return

    def kth_smallest(k):
        if k > (1 << rank):
            return -1

        res = 0
        remaining = rank

        for b in range(59, -1, -1):
            if basis[b]:
                remaining -= 1
                cnt_zero = 1 << remaining
                if k > cnt_zero:
                    k -= cnt_zero
                    res ^= basis[b]

        return res

    for _ in range(q):
        t, x = map(int, input().split())
        if t == 1:
            insert(x)
        else:
            ans.append(str(kth_smallest(x)))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`insert`hàm là bước loại bỏ Gaussian trên các vectơ nhị phân. Vòng lặp bắt đầu từ bit 59 vì giá trị đầu vào nhỏ hơn 2^60, do đó vị trí bit trên 59 không bao giờ xuất hiện. 

Chức năng truy vấn đầu tiên kiểm tra xem vị trí được yêu cầu có tồn tại hay không. biểu hiện`1 << rank`hoạt động vì mọi vectơ cơ sở độc lập đều nhân đôi số giá trị XOR có thể có. 

Biến`remaining`đếm xem có bao nhiêu lựa chọn cơ bản vẫn chưa được quyết định sau bit hiện tại. Điều này cho biết kích thước của khối trong đó bit hiện tại bằng 0. Phép trừ từ`k`chỉ xảy ra khi chúng ta bỏ qua toàn bộ khối đó và chọn bit hiện tại làm một. 

Không có mảng giá trị XOR lớn nào được tạo nên việc triển khai vẫn nằm trong giới hạn bộ nhớ. 

## Ví dụ đã hoạt động 

Mẫu 1:```
7
1 5
1 6
2 1
2 2
2 3
2 4
2 5
```| Hoạt động | Trạng thái cơ bản | Xếp hạng | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| Chèn 5 | 101 | 1 | | 
| Chèn 6 | 101, 011 | 2 | | 
| Truy vấn 1 | giá trị: 0,3,5,6 | 2 | 0 | 
| Truy vấn 2 | giá trị: 0,3,5,6 | 2 | 3 | 
| Truy vấn 3 | giá trị: 0,3,5,6 | 2 | 5 | 
| Truy vấn 4 | giá trị: 0,3,5,6 | 2 | 6 | 
| Truy vấn 5 | giá trị: 0,3,5,6 | 2 | -1 | 

Điều này cho thấy thứ hạng cơ sở chứ không phải số phần tử được chèn sẽ kiểm soát số lượng câu trả lời khác nhau tồn tại. 

Mẫu 2:```
3
1 1000
1 1000
2 3
```| Hoạt động | Trạng thái cơ bản | Xếp hạng | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| Chèn 1000 | 1111101000 | 1 | | 
| Chèn 1000 | không thay đổi | 1 | | 
| Truy vấn 3 | giá trị: 0,1000 | 1 | -1 | 

Lần chèn thứ hai không thêm thứ nguyên mới vì giá trị đã được biểu thị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(60q) | Mỗi lần chèn và truy vấn quét tối đa 60 bit | 
| Không gian | O(60) | Chỉ có một vectơ cơ sở trên mỗi bit được lưu trữ | 

Số lượng thao tác tối đa là 200000, do đó cần khoảng 12 triệu thao tác bit. Điều này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    
    data = sys.stdin.readline
    q = int(data())
    basis = [0] * 60
    rank = 0
    out = []

    def insert(x):
        nonlocal rank
        for b in range(59, -1, -1):
            if (x >> b) & 1:
                if basis[b]:
                    x ^= basis[b]
                else:
                    basis[b] = x
                    rank += 1
                    return

    def kth(k):
        if k > (1 << rank):
            return -1
        res = 0
        rem = rank
        for b in range(59, -1, -1):
            if basis[b]:
                rem -= 1
                if k > (1 << rem):
                    k -= 1 << rem
                    res ^= basis[b]
        return res

    for _ in range(q):
        t, x = map(int, data().split())
        if t == 1:
            insert(x)
        else:
            out.append(str(kth(x)))

    sys.stdin = old
    return "\n".join(out)

assert run("""7
1 5
1 6
2 1
2 2
2 3
2 4
2 5
""") == "0\n3\n5\n6\n-1"

assert run("""3
1 1000
1 1000
2 3
""") == "-1"

assert run("""2
2 1
2 2
""") == "0\n-1"

assert run("""4
1 1
1 2
1 3
2 4
""") == "-1"

assert run("""5
1 7
1 2
1 5
2 1
2 8
""") == "0\n-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Truy vấn trình tự trống | 0, -1 | Xử lý chuỗi tiếp theo trống | 
| Giá trị lặp lại | -1 | Phần chèn phụ thuộc | 
| Ba giá trị độc lập | -1 cho ngoài phạm vi | Đếm thứ hạng đúng | 
| Giá trị cơ bản hỗn hợp | 0, -1 | Ranh giới đặt hàng và truy vấn | 

## Vỏ cạnh 

Đối với các giá trị lặp lại, chẳng hạn như:```
3
1 5
1 5
2 3
```Lần chèn đầu tiên tạo ra một vectơ cơ sở. Lần chèn thứ hai giảm về 0 vì nó giống với hướng hiện có. Thứ hạng vẫn là một nên chỉ tồn tại hai giá trị: 0 và 5. Giá trị nhỏ thứ ba không tồn tại. 

Đối với các giá trị kết hợp thành một giá trị khác:```
4
1 1
1 2
1 3
2 4
```Lần chèn thứ ba không tăng thứ hạng vì`1 XOR 2 = 3`. Cơ sở vẫn chỉ có hai vectơ độc lập, cho ra bốn kết quả XOR. Thuật toán trả về đúng -1. 

Đối với chuỗi trống:```
2
2 1
2 2
```Thứ hạng bằng 0, vì vậy số lượng giá trị có thể truy cập là`2^0 = 1`. Giá trị duy nhất là 0, xuất phát từ việc không chọn gì. Truy vấn đầu tiên trả về 0 và truy vấn thứ hai không thể được trả lời.
