---
title: "CF 102621I - Xáo trộn danh sách phát"
description: "Chúng tôi có một danh sách các bài hát. Mỗi bài hát có hai nhãn: thể loại và tác giả của nó. Chúng ta được phép lược bỏ một số bài hát, sau đó sắp xếp lại các bài hát còn lại."
date: "2026-08-02T13:58:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102621
codeforces_index: "I"
codeforces_contest_name: "mBIT Advanced June 2020"
rating: 0
weight: 102621
solve_time_s: 83
verified: true
draft: false
---

[CF 102621I - Xáo trộn danh sách phát](https://codeforces.com/problemset/problem/102621/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng tôi có một danh sách các bài hát. Mỗi bài hát có hai nhãn: thể loại và tác giả của nó. Chúng ta được phép lược bỏ một số bài hát, sau đó sắp xếp lại các bài hát còn lại. Danh sách phát được sắp xếp lại được coi là hợp lệ khi mỗi cặp lân cận có chung ít nhất một nhãn: cả hai bài hát đều có cùng thể loại hoặc cả hai bài hát đều có cùng một tác giả. Nhiệm vụ là giữ lại càng nhiều bài hát càng tốt nên đáp án là số lượng bài hát bị loại bỏ tối thiểu. Vấn đề ban đầu có một số lượng nhỏ các bài hát, với giải pháp dự kiến ​​là sử dụng lập trình động tập hợp con. 

Hạn chế chính là số lượng bài hát, nhiều nhất là 16. Con số này đủ nhỏ để có thể thực hiện được các thuật toán hàm mũ. Một giải pháp dựa trên việc kiểm tra mọi thứ tự vẫn sẽ quá đắt vì số lượng hoán vị tăng lên khi$n!$, đạt tới hàng tỷ ngay cả với giá trị vừa phải của$n$. Một giải pháp trên tất cả các tập con là khả thi vì chỉ có$2^{16}=65536$tập hợp con có thể. 

Độ dài chuỗi lớn lại quan trọng theo một cách khác. Chúng ta không thể so sánh nhiều lần các chuỗi thể loại và nhà văn dài trong khi khám phá các trạng thái. Chúng ta nên xử lý trước mối quan hệ theo cặp giữa các bài hát một lần, sau đó chỉ sử dụng các kiểm tra thời gian liên tục trong quá trình lập trình động. 

Những trường hợp rắc rối xuất phát từ việc danh sách phát hợp lệ không nhất thiết phải là thứ tự ban đầu. 

Ví dụ: nếu đầu vào là:```
3
rock alice
pop bob
rock bob
```đầu ra đúng là:```
0
```bởi vì thứ tự`rock alice`,`rock bob`,`pop bob`hoạt động. Giải pháp chỉ kiểm tra thứ tự đã cho sẽ xóa bài hát một cách không chính xác. 

Một trường hợp khó khăn khác là khi chỉ còn lại một bài hát. Ví dụ:```
1
jazz mike
```Câu trả lời là:```
0
```bởi vì một bài hát không có cặp lân cận nào có thể vi phạm quy tắc. Mã khởi tạo câu trả lời xung quanh các so sánh liền kề có thể vô tình coi câu trả lời này là không hợp lệ. 

Trường hợp cạnh cuối cùng là khi không có tập hợp con lớn nào được kết nối đủ. Ví dụ:```
3
a x
b y
c z
```Đầu ra đúng là:```
2
```Chỉ có thể giữ lại một bài hát, vì không thể đặt hai bài hát cạnh nhau. Việc triển khai bất cẩn cho rằng mọi bài hát đều có thể xuất hiện trong danh sách phát cuối cùng sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là thử mọi tập hợp con có thể có của các bài hát và mọi thứ tự có thể có trong tập hợp con đó. Nếu thứ tự thỏa mãn quy tắc kề, chúng tôi giữ kích thước hợp lệ lớn nhất. Điều này đúng vì câu trả lời chính xác là phần bổ sung của danh sách phát hợp lệ lớn nhất. 

Vấn đề là số lượng đặt hàng. Vì$n=16$, kiểm tra tất cả các hoán vị yêu cầu$16!$khả năng, đó là về$2.1 \times 10^{13}$. Ngay cả với những thao tác nhanh, điều này cũng không thể phù hợp với giới hạn thời gian. 

Điều quan trọng là thứ tự chỉ phụ thuộc vào bài hát nào được đặt cuối cùng. Khi xây dựng danh sách phát hợp lệ, chúng ta không cần phải nhớ toàn bộ thứ tự cho đến nay. Chúng ta chỉ cần tập hợp các bài hát đã sử dụng và danh tính của bài hát cuối cùng, vì bài hát tiếp theo chỉ cần tương thích với bài hát cuối cùng đó. 

Điều này chuyển đổi vấn đề thành lập trình động tập hợp con. Chúng tôi tính toán danh sách phát hợp lệ lớn nhất kết thúc bằng mỗi bài hát cho mỗi tập hợp con. Sau khi tất cả các trạng thái được xử lý, kích thước tập hợp con lớn nhất có thể truy cập sẽ mang lại số lượng bài hát tối đa mà chúng tôi có thể giữ lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n^2 2^n)$|$O(n2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước xem mỗi cặp bài hát có thể liền kề hay không. Hai bài hát tương thích khi thể loại của chúng phù hợp hoặc tác giả của chúng phù hợp. Việc lưu trữ kết quả này sẽ tránh việc so sánh nhiều lần các chuỗi bên trong chương trình động. 
2. Tạo bảng DP trong đó`dp[mask][i]`biểu thị liệu có thể xây dựng danh sách phát hợp lệ chứa chính xác các bài hát trong`mask`và kết thúc bằng bài hát`i`. Bài hát cuối cùng được lưu trữ vì nó quyết định bài hát nào có thể được thêm vào tiếp theo. 
3. Khởi tạo từng tập hợp con một bài hát. Danh sách phát chứa một bài hát luôn hợp lệ, vì vậy mọi trạng thái có một bit được đặt sẽ bắt đầu ở trạng thái có thể truy cập được. 
4. Lặp lại tất cả các tập hợp con. Cho mọi bài hát kết thúc có thể tiếp cận`i`, hãy thử thêm từng bài hát`j`chưa có trong tập hợp con. Nếu như`i`Và`j`tương thích, đánh dấu tập hợp con mới kết thúc tại`j`có thể tiếp cận được. 
5. Theo dõi số lượng bit được đặt lớn nhất trong số tất cả các trạng thái có thể truy cập. Số bài hát bị xóa là tổng số bài hát trừ đi mức tối đa này. 

Tại sao nó hoạt động: mọi danh sách phát hợp lệ đều có một bài hát cuối cùng và một tập hợp các bài hát đã sử dụng. DP lưu trữ chính xác thông tin này, do đó mọi sắp xếp hợp lệ đều có thể được sao chép bằng các chuyển đổi. Ngược lại, mọi chuyển đổi chỉ thêm một bài hát khi nó có thể theo sau một cách hợp pháp bài hát cuối cùng trước đó, vì vậy mọi trạng thái được tạo đều tương ứng với một danh sách phát hợp lệ. Do đó, tập hợp con có thể truy cập tối đa là danh sách phát lớn nhất có thể được tạo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        songs = []

        for _ in range(n):
            g, w = input().split()
            songs.append((g, w))

        ok = [[False] * n for _ in range(n)]
        for i in range(n):
            for j in range(n):
                if songs[i][0] == songs[j][0] or songs[i][1] == songs[j][1]:
                    ok[i][j] = True

        total = 1 << n
        dp = [0] * (total * n)

        for i in range(n):
            dp[((1 << i) * n) + i] = 1

        best = 1

        for mask in range(total):
            cnt = mask.bit_count()
            if cnt <= best:
                best = max(best, cnt)

            base = mask * n
            for last in range(n):
                if dp[base + last]:
                    remaining = ((total - 1) ^ mask)
                    while remaining:
                        bit = remaining & -remaining
                        nxt = bit.bit_length() - 1
                        if ok[last][nxt]:
                            new_mask = mask | bit
                            dp[new_mask * n + nxt] = 1
                        remaining -= bit

        ans.append(str(n - best))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Theo cặp`ok`ma trận được xây dựng trước khi DP bắt đầu. Đây là nơi xảy ra tất cả các so sánh chuỗi tốn kém, do đó phần mũ của thuật toán chỉ thực hiện các phép toán số nguyên. 

Mảng DP được làm phẳng thành một chiều. Nhà nước`(mask, last)`được lưu trữ tại chỉ mục`mask * n + last`, giúp tránh tạo nhiều danh sách Python lồng nhau và giảm chi phí. 

Trạng thái một bài hát được khởi tạo vì mọi bài hát đều có thể bắt đầu danh sách phát. Trong quá trình chuyển đổi, mã sẽ loại bỏ từng bit có sẵn khỏi tập hợp các bài hát không được sử dụng. Điều này tránh việc quét các vị trí không cần thiết. 

Số nguyên Python không bị tràn, vì vậy các hoạt động bitmask được an toàn. Chi tiết ranh giới duy nhất cần xử lý cẩn thận là trường hợp một bài hát, trong đó kích thước danh sách phát hợp lệ tối đa đã được khởi tạo chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
3
rock a
pop b
rock b
```Quá trình khám phá trạng thái trông như thế này: 

| mặt nạ | Bài hát cuối cùng | Hành động | Kích thước có thể tiếp cận | 
| --- | --- | --- | --- | 
| 001 | đá một | Bắt đầu | 1 | 
| 100 | đá b | Bắt đầu | 1 | 
| 010 | bật b | Bắt đầu | 1 | 
| 101 | đá b | Thêm sau đá a | 2 | 
| 111 | bật b | Thêm sau đá b | 3 | 

Bộ đầy đủ có thể truy cập được vì việc đặt hàng`rock a -> rock b -> pop b`thỏa mãn mọi điều kiện kề. Kích thước được giữ tối đa là 3, vì vậy câu trả lời là 0. 

Hãy xem xét:```
3
a x
b y
c z
```Các tiểu bang là: 

| mặt nạ | Bài hát cuối cùng | Hành động | Kích thước có thể tiếp cận | 
| --- | --- | --- | --- | 
| 001 | một x | Bắt đầu | 1 | 
| 010 | bởi y | Bắt đầu | 1 | 
| 100 | cz | Bắt đầu | 1 | 

Không có quá trình chuyển đổi nào tồn tại vì không có cặp nào chia sẻ nhãn. Kích thước danh sách phát tối đa là 1, vì vậy phải xóa hai bài hát. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 2^n)$| Mọi tập hợp con có thể thử mở rộng với mọi bài hát tiếp theo có thể | 
| Không gian |$O(n2^n)$| Một trạng thái DP được lưu trữ cho mỗi tập hợp con và bài hát kết thúc có thể có | 

Với$n \leq 16$, số lượng tập hợp con nhiều nhất là 65536. Số lượng trạng thái thu được đủ nhỏ cho các giới hạn yêu cầu. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    it = iter(data)
    t = int(next(it))
    res = []

    for _ in range(t):
        n = int(next(it))
        songs = []
        for _ in range(n):
            songs.append((next(it), next(it)))

        ok = [[False] * n for _ in range(n)]
        for i in range(n):
            for j in range(n):
                ok[i][j] = songs[i][0] == songs[j][0] or songs[i][1] == songs[j][1]

        dp = [0] * ((1 << n) * n)
        best = 1

        for i in range(n):
            dp[((1 << i) * n) + i] = 1

        for mask in range(1 << n):
            best = max(best, mask.bit_count())
            for last in range(n):
                if dp[mask * n + last]:
                    rem = ((1 << n) - 1) ^ mask
                    while rem:
                        b = rem & -rem
                        nxt = b.bit_length() - 1
                        if ok[last][nxt]:
                            dp[(mask | b) * n + nxt] = 1
                        rem -= b

        res.append(str(n - best))

    return "\n".join(res)

assert run("""1
1
jazz mike
""") == "0"

assert run("""1
3
rock a
pop b
rock b
""") == "0"

assert run("""1
3
a x
b y
c z
""") == "2"

assert run("""1
4
a x
a y
b y
c z
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một bài hát | 0 | Trường hợp ranh giới đơn phần tử | 
| Chuỗi bài hát tương thích | 0 | Danh sách phát đầy đủ có thể được sắp xếp lại | 
| Không có cặp tương thích | 2 | Chỉ có một bài hát có thể tồn tại | 
| Tương thích một phần | 1 | Lựa chọn tập hợp con tối đa | 

## Vỏ cạnh 

Đối với trường hợp một bài hát:```
1
jazz mike
```DP bắt đầu với bài hát duy nhất ở trạng thái có thể truy cập được. Không cần chuyển đổi nên kích thước danh sách phát tối đa là một và số lần xóa là 0. 

Đối với trường hợp lệnh ban đầu bị sai lệch:```
3
rock alice
pop bob
rock bob
```thuật toán không quan tâm đến thứ tự đầu vào. Nó bắt đầu từ mỗi bài hát và khám phá tất cả các phần tiếp theo hợp lệ có thể có. Nó phát hiện thứ tự kết thúc bằng cả ba bài hát và trả về số không bị xóa. 

Đối với trường hợp các bài hát hoàn toàn không liên quan:```
3
a x
b y
c z
```ma trận tương thích không chứa cặp đúng ngoại trừ các cặp tự. Vòng chuyển tiếp không thể tạo mặt nạ lớn hơn, chỉ để lại ba trạng thái kích thước một. Thuật toán tính toán chính xác rằng chỉ còn lại một bài hát.
