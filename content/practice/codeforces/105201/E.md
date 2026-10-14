---
title: "CF 105201E - Thuật toán kỳ lạ (Phiên bản dễ dàng)"
description: "Chúng ta có một chuỗi trong đó mọi vị trí được coi là một đỉnh trong biểu đồ. Hai vị trí được kết nối bằng một cạnh khi chuỗi con giữa các vị trí đó, bao gồm cả hai điểm cuối, đọc giống nhau từ trái sang phải và từ phải sang trái."
date: "2026-06-27T02:47:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105201
codeforces_index: "E"
codeforces_contest_name: "IME++ Open Contest 2024"
rating: 0
weight: 105201
solve_time_s: 65
verified: true
draft: false
---

[CF 105201E - Thuật toán kỳ lạ (Phiên bản dễ dàng)](https://codeforces.com/problemset/problem/105201/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi trong đó mọi vị trí được coi là một đỉnh trong biểu đồ. Hai vị trí được kết nối bằng một cạnh khi chuỗi con giữa các vị trí đó, bao gồm cả hai điểm cuối, đọc giống nhau từ trái sang phải và từ phải sang trái. 

Nhiệm vụ không phải là xây dựng toàn bộ biểu đồ một cách rõ ràng. Biểu đồ có thể có nhiều cạnh vì một chuỗi có thể chứa một số chuỗi con palindromic bậc hai. Chúng ta chỉ cần số nhóm vị trí được kết nối sau khi tất cả các cạnh hợp lệ được thêm vào. 

Gọi độ dài của chuỗi là`n`. Phiên bản dễ dàng cho phép`n`lên đến`10000`. Một cách tiếp cận trực tiếp để kiểm tra mọi cặp vị thế có xung quanh`n^2`các cặp và kiểm tra xem mỗi chuỗi con có phải là một palindrome có giá trị khác không`O(n)`nếu thực hiện một cách ngây thơ. Điều đó tạo ra một`O(n^3)`thuật toán, xung quanh`10^12`hoạt động cho đầu vào lớn nhất và vượt xa giới hạn hai giây cho phép. 

Mục tiêu hữu ích là`O(n^2)`giải pháp. Xung quanh`10^8`các thao tác đơn giản gần đạt đến giới hạn trên đối với các ngôn ngữ được tối ưu hóa, vì vậy chúng ta cần tránh những công việc tốn kém bên trong vòng lặp bậc hai. Thách thức chính là cũng có thể có`O(n^2)`chuỗi con palindromic, đặc biệt đối với các chuỗi như`aaaaaaaaaa`, vì vậy việc lưu trữ mọi palindrome là không thực tế. 

Một số trường hợp nguy hiểm có thể phá vỡ việc triển khai bất cẩn. Đối với một chuỗi ký tự như`a`, không có cạnh nào, nên câu trả lời là`1`. Một giải pháp giả sử mọi ký tự được kết nối thông qua chính nó có thể vô tình trả về một giá trị khác. 

Vì`ab`, không vị trí nào có thể được kết nối vì chuỗi con`ab`không phải là một palindrome. Câu trả lời đúng là`2`. Một giải pháp xử lý các chuỗi con có độ dài bằng nhau hoặc khớp các ký tự riêng lẻ làm kết nối sẽ hợp nhất hai vị trí một cách không chính xác. 

Vì`abcacba`, toàn bộ chuỗi là một palindrome, nhưng ký tự ở giữa không được kết nối với các vị trí khác. Câu trả lời đúng là`4`. Một sai lầm phổ biến là cho rằng mọi ký tự bên trong một palindrome đều thuộc về cùng một thành phần, nhưng một cạnh chỉ kết nối hai điểm cuối của một palindrome. 

## Phương pháp tiếp cận 

Phương pháp đơn giản là kiểm tra mọi chuỗi con có thể có, kiểm tra xem đó có phải là một chuỗi palindrome hay không và nếu có thì hợp nhất hai điểm cuối của nó trong một cấu trúc hợp tập hợp rời rạc. Tính đúng đắn là ngay lập tức vì mỗi cạnh của đồ thị chính xác là một phép toán hợp. Vấn đề là chi phí. có`O(n^2)`chuỗi con và kiểm tra từng chuỗi bằng cách so sánh các ký tự`O(n)`, cho`O(n^3)`thời gian. 

Một hướng tốt hơn đến từ việc quan sát cách các palindrome được tạo ra một cách tự nhiên. Mỗi palindrome có thể được tìm thấy bằng cách mở rộng xung quanh trung tâm của nó. Một palindrome có chiều dài lẻ có một ký tự ở giữa, trong khi một palindrome có chiều dài chẵn có một khoảng trống ở giữa. Nếu chúng ta mở rộng từ mọi trung tâm có thể thì mỗi palindrome sẽ xuất hiện đúng một lần. 

Trong quá trình mở rộng, chúng ta chỉ cần thêm cạnh giữa hai điểm cuối hiện tại. Chúng ta không cần phải xây dựng biểu đồ hoặc giữ lại bảng màu. Số lần mở rộng trên tất cả các trung tâm là`O(n^2)`và mỗi lần mở rộng thành công sẽ mang lại chính xác một phép toán hợp. 

Phương pháp Brute Force không thành công vì nó liên tục tìm kiếm các palindromes từ đầu. Việc mở rộng trung tâm sẽ loại bỏ công việc lặp đi lặp lại đó bằng cách sử dụng trực tiếp tính đối xứng của các palindromes. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Mở rộng trung tâm với DSU | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo cấu trúc hợp tập hợp rời rạc với một tập hợp cho mỗi vị trí chuỗi. Mỗi bộ đại diện cho một thành phần có thể được kết nối và việc hợp nhất hai bộ đại diện cho việc thêm một cạnh của biểu đồ. 
2. Coi mọi vị trí là tâm của một bảng màu có độ dài lẻ. Mở rộng sang trái và phải trong khi các ký tự khớp nhau. Đối với mỗi lần mở rộng thành công, hãy hợp nhất các vị trí bên trái và bên phải hiện tại vì chuỗi con giữa chúng là một bảng màu. 
3. Coi mọi khoảng trống giữa các vị trí lân cận là tâm của một bảng màu có độ dài chẵn. Mở rộng theo cách tương tự và hợp nhất hai điểm cuối của mỗi palindrome được tìm thấy. 
4. Đếm xem còn lại bao nhiêu gốc DSU sau khi tất cả các lần mở rộng kết thúc. Mỗi nghiệm còn lại tương ứng với một thành phần liên thông của đồ thị gốc. 

Lý do điều này có hiệu quả là vì mọi palindrome có thể có chính xác một tâm. Quá trình mở rộng phát hiện ra mọi cạnh hợp lệ và DSU ghi lại chính xác kết nối được tạo bởi các cạnh đó. Vì các thành phần được kết nối chỉ phụ thuộc vào việc các đỉnh có thể tiếp cận nhau nên việc thêm các cạnh theo thứ tự bất kỳ sẽ tạo ra cùng một phân vùng cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(s):
    n = len(s)

    parent = list(range(n))
    size = [1] * n

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        ra = find(a)
        rb = find(b)
        if ra == rb:
            return
        if size[ra] < size[rb]:
            ra, rb = rb, ra
        parent[rb] = ra
        size[ra] += size[rb]

    for center in range(n):
        left = center
        right = center
        while left >= 0 and right < n and s[left] == s[right]:
            if left != right:
                union(left, right)
            left -= 1
            right += 1

    for center in range(n - 1):
        left = center
        right = center + 1
        while left >= 0 and right < n and s[left] == s[right]:
            union(left, right)
            left -= 1
            right += 1

    ans = 0
    for i in range(n):
        if find(i) == i:
            ans += 1
    return ans

def main():
    s = input().strip()
    print(solve(s))

if __name__ == "__main__":
    main()
```Mảng DSU lưu trữ đại diện hiện tại của từng thành phần. Nén đường dẫn trong`find`giúp kiểm tra kết nối lặp đi lặp lại nhanh chóng, trong khi liên kết theo kích thước giữ cho cây ở trạng thái nông. 

Vòng lặp trung tâm lẻ bắt đầu bằng cả hai con trỏ ở cùng một ký tự. Việc mở rộng đầu tiên biểu thị độ dài bằng một palindrome, điều này không tạo ra một cạnh hữu ích vì cả hai điểm cuối đều là cùng một đỉnh. Việc mở rộng dài hơn sẽ ngay lập tức đưa ra các cạnh đồ thị cần thiết. 

Vòng lặp trung tâm chẵn bắt đầu giữa hai ký tự liền kề. Mỗi lần mở rộng thành công đều mang lại hai điểm cuối khác nhau, vì vậy nó luôn thực hiện hợp nhất. 

Lần quét cuối cùng đếm các gốc thay vì các giá trị mảng vì cha mẹ DSU không được đảm bảo trỏ trực tiếp đến chính chúng sau khi kết hợp. Đang gọi`find`trước khi đếm sẽ tránh được các lỗi do đường dẫn đã nén nhưng chưa được cập nhật. 

## Ví dụ đã hoạt động 

cho`abcacba`, các bản mở rộng thành công là: 

| Loại trung tâm | Trái | Đúng | Hành động | 
| --- | --- | --- | --- | 
| Trung tâm lẻ ở 3 | 2 | 4 | Hợp nhất 2 và 4 | 
| Trung tâm lẻ ở 3 | 1 | 5 | Hợp nhất 1 và 5 | 
| Trung tâm lẻ ở 3 | 0 | 6 | Hợp nhất 0 và 6 | 

Các thành phần cuối cùng là`{0,6}`,`{1,5}`,`{2,4}`, Và`{3}`, đưa ra câu trả lời`4`. 

Dấu vết này cho thấy một bảng màu lớn không tự động hợp nhất mọi ký tự bên trong nó. Chỉ các điểm cuối của nó được kết nối. 

Vì`navarrolikestacocat`, các kết nối palindrome quan trọng đến từ việc mở rộng xung quanh tâm của các vùng palindrome. 

| Bước | Ví dụ về bảng màu được tìm thấy | Đoàn biểu diễn | 
| --- | --- | --- | 
| 1 |`rr`| Hợp nhất cả hai`r`vị trí | 
| 2 |`ara`| Hợp nhất hai bên ngoài`a`vị trí | 
| 3 |`tacocat`| Hợp nhất hai bên ngoài`t`vị trí | 
| 4 | Các phần mở rộng còn lại | Hợp nhất các điểm cuối palindrom khác | 

Sau tất cả các lần mở rộng, DSU chứa`14`rễ. 

Ví dụ này chứng minh rằng thuật toán không phụ thuộc vào hình dạng palindrome đặc biệt. Mọi palindrome đều được xử lý theo cùng một quy tắc mở rộng tâm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi bản mở rộng trung tâm thực hiện tối đa một số so sánh ký tự tuyến tính và có`O(n)`các trung tâm. | 
| Không gian | O(n) | DSU lưu trữ một giá trị gốc và giá trị kích thước cho mỗi vị trí. | 

Với`n = 10000`, số lượng so sánh ký tự bậc hai có thể quản lý được và mức sử dụng bộ nhớ vẫn ở dưới mức giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(s):
    n = len(s)
    parent = list(range(n))
    size = [1] * n

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        a = find(a)
        b = find(b)
        if a == b:
            return
        if size[a] < size[b]:
            a, b = b, a
        parent[b] = a
        size[a] += size[b]

    for c in range(n):
        l = r = c
        while l >= 0 and r < n and s[l] == s[r]:
            if l != r:
                union(l, r)
            l -= 1
            r += 1

    for c in range(n - 1):
        l, r = c, c + 1
        while l >= 0 and r < n and s[l] == s[r]:
            union(l, r)
            l -= 1
            r += 1

    return str(sum(find(i) == i for i in range(n)))

def run(inp: str) -> str:
    return solve(inp.strip()) + "\n"

assert run("abcacba") == "4\n", "sample 1"
assert run("navarrolikestacocat") == "14\n", "sample 2"

assert run("a") == "1\n", "single character"
assert run("ab") == "2\n", "no palindrome of length two"
assert run("aaaa") == "1\n", "all characters connected"
assert run("abcba") == "3\n", "nested palindrome boundaries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`1`| Kích thước tối thiểu và không có cạnh | 
|`ab`|`2`| Những nhân vật không bao giờ được kết nối | 
|`aaaa`|`1`| Số lượng lớn các palindromes chồng chéo | 
|`abcba`|`3`| Các palindrome lồng nhau chỉ kết nối các điểm cuối | 

## Vỏ cạnh 

cho`a`, thuật toán bắt đầu một khai triển lẻ với`left = right = 0`. Chuỗi con là một chuỗi palindrome, nhưng các điểm cuối có cùng chỉ mục, do đó không có sự kết hợp nào xảy ra. DSU giữ lại một thành phần, tạo ra câu trả lời đúng`1`. 

Vì`ab`, các phần mở rộng lẻ chỉ tìm thấy các ký tự đơn. Việc mở rộng chẵn kiểm tra chuỗi con`ab`, thấy hai ký tự này khác nhau thì dừng lại ngay. Không có sự kết hợp nào được thực hiện, để lại hai thành phần. 

Vì`abcacba`, tâm tại chỉ số`3`mở rộng thông qua`c`,`cac`,`bcacb`, Và`abcacba`. Các hiệp hội thuật toán`(2,4)`,`(1,5)`, Và`(0,6)`trong khi rời khỏi chỉ mục`3`một mình. Điều này cung cấp bốn thành phần, phù hợp với định nghĩa biểu đồ. 

Vì`aaaa`, nhiều palindromes chồng chéo lên nhau rất nhiều. Việc mở rộng tìm các cặp liền kề và các cặp lớn hơn, cuối cùng kết nối mọi chỉ mục thông qua DSU. Số gốc cuối cùng là`1`, cho thấy rằng việc khám phá palindrome lặp đi lặp lại là vô hại vì DSU bỏ qua các cạnh trùng lặp.
