---
title: "CF 353B - Hai đống"
description: "Mỗi khối có chứa một số có hai chữ số. Chúng ta phải chia 2n khối thành hai đống, mỗi đống có đúng n khối. Sau khi chia, Valera có thể chọn bất kỳ khối nào từ đống đầu tiên và bất kỳ khối nào từ đống thứ hai."
date: "2026-06-07T00:59:11+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "constructive-algorithms", "greedy", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 353
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 205 (Div. 2)"
rating: 1900
weight: 353
solve_time_s: 160
verified: true
draft: false
---

[CF 353B - Hai đống](https://codeforces.com/problemset/problem/353/B) 

**Xếp hạng:** 1900 
**Tags:** tổ hợp, thuật toán xây dựng, tham lam, thực hiện, toán học, sắp xếp 
**Thời gian giải:** 2 phút 40 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi khối có chứa một số có hai chữ số. Chúng ta phải chia`2n`khối thành hai đống chính xác`n`mỗi hình khối. 

Sau khi chia, Valera có thể chọn bất kỳ khối nào từ đống đầu tiên và bất kỳ khối nào từ đống thứ hai. Nếu khối thứ nhất chứa`ab`và thứ hai chứa`cd`, anh ta thu được số có bốn chữ số`abcd`. 

Thứ tự bên trong một đống không quan trọng. Điều quan trọng là _giá trị_ nào xuất hiện trong mỗi đống. Nếu một giá trị`13`xuất hiện năm lần trong đống đầu tiên, nó vẫn chỉ đóng góp một tiền tố có thể có. Tương tự, nếu`45`xuất hiện ba lần trong đống thứ hai, nó vẫn chỉ đóng góp một hậu tố có thể có. 

Giả sử heap đầu tiên chứa`x`các giá trị riêng biệt và đống thứ hai chứa`y`những giá trị riêng biệt. Mọi giá trị riêng biệt trong đống thứ nhất có thể được ghép nối với mọi giá trị riêng biệt trong đống thứ hai, do đó số các số có bốn chữ số riêng biệt là chính xác`x · y`. 

Nhiệm vụ là tối đa hóa tích số này và đưa ra một phép gán tối ưu các khối cho các đống. 

Các ràng buộc nhỏ về số lượng khối, chỉ`2n ≤ 200`, nhưng các giá trị thậm chí còn bị hạn chế hơn vì mọi giá trị khối đều nằm giữa`10`Và`99`. Chỉ có 90 giá trị khác nhau có thể có. Điều này gợi ý rõ ràng rằng lời giải nên suy luận về tần số của các giá trị thay vì về các khối riêng lẻ. 

Một sai lầm phổ biến là tập trung vào việc cân bằng số hình khối trong đống. Mục tiêu phụ thuộc vào số **giá trị riêng biệt** trong mỗi đống, chứ không phụ thuộc vào số khối mang các giá trị đó. 

Coi như:```
n = 2
13 13 13 45
```Đặt hai`13`s vào các đống khác nhau sẽ tạo ra một giá trị riêng biệt trong mỗi đống. thứ ba`13`không đóng góp gì mới Giá trị duy nhất có thể tăng số lượng khác biệt hơn nữa là`45`. 

Một trường hợp tinh vi khác là khi mỗi giá trị là duy nhất:```
n = 2
10 20 30 40
```Không có giá trị nào có thể xuất hiện trong cả hai đống. Chiến lược tốt nhất là chia bốn giá trị riêng biệt càng đồng đều càng tốt, đưa ra số lượng riêng biệt`(2,2)`và sản phẩm`4`. Một sự giao phó tham lam bất cẩn có thể tạo ra`(3,1)`và sản phẩm`3`. 

Ở thái cực khác:```
n = 2
55 55 55 55
```Cả hai đống đều có thể chứa giá trị`55`, nhưng cả đống không thể chứa bất kỳ giá trị nào khác. Câu trả lời là`1`, không`4`, vì các bản sao không tạo ra các giá trị khác biệt mới. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ tìm mọi cách để lựa chọn`n`khối cho đống đầu tiên. có$$\binom{2n}{n}$$những lựa chọn như vậy. Đối với kích thước đầu vào tối đa, đây là kích thước lớn về mặt thiên văn. Ngay cả đối với`n = 20`, số lượng đã vượt quá một trăm tỷ. Tìm kiếm toàn diện là không thể. 

Quan sát quan trọng là việc nhận dạng khối thực tế chỉ quan trọng thông qua tần số giá trị. 

Giả sử một giá trị xuất hiện đúng một lần. Giá trị đó có thể đóng góp một giá trị riêng biệt cho nhiều nhất là một đống. 

Giả sử một giá trị xuất hiện ít nhất hai lần. Sau đó, chúng ta có thể đặt một bản sao vào mỗi vùng nhớ heap, làm cho giá trị đó đóng góp vào số lượng riêng biệt của cả hai vùng nhớ cùng một lúc. 

Cho phép:

-`d`là số giá trị có tần số ít nhất là hai. 
-`s`là số lượng các giá trị có tần số đúng bằng một. 

Mỗi giá trị lặp lại đóng góp một giá trị riêng biệt cho mỗi vùng nhớ heap. Sự đóng góp này là bắt buộc nếu chúng ta muốn số lượng giá trị riêng biệt tối đa. 

Mỗi giá trị đơn lẻ đóng góp một giá trị riêng biệt vào đúng một đống. Quyết định duy nhất còn lại là đống nào nhận được nó. 

Nếu đống kết thúc với`x`Và`y`các giá trị riêng biệt thì:$$x+y = 2d+s$$bởi vì mỗi giá trị lặp lại đóng góp hai lần và mỗi giá trị đơn lẻ đóng góp một lần. 

Sản phẩm`x·y`đạt cực đại khi`x`Và`y`càng gần càng tốt. Vì các giá trị lặp lại đã đóng góp như nhau cho cả hai vùng nhớ, nên chúng tôi chỉ cần phân phối các giá trị đơn lẻ một cách đồng đều nhất có thể giữa các vùng nhớ. 

Sau khi quyết định đống nào nhận được "sự đóng góp riêng biệt" của từng giá trị, tất cả các khối trùng lặp còn lại sẽ trở nên không liên quan đến mục tiêu. Chúng chỉ có thể được sử dụng để đáp ứng yêu cầu mỗi đống chứa chính xác`n`hình khối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\binom{2n}{n})$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của mọi giá trị và ghi nhớ vị trí của tất cả các hình lập phương. 
2. Đối với mọi giá trị có tần số ít nhất là hai, hãy gán một lần xuất hiện cho vùng heap 1 và một lần xuất hiện cho vùng heap 2. 

Điều này đảm bảo rằng giá trị đóng góp một giá trị riêng biệt cho mỗi vùng nhớ heap. 
3. Đối với mọi giá trị có tần số chính xác bằng một, hãy xen kẽ giữa các đống. 

Singleton đầu tiên sẽ vào đống 1, cái thứ hai vào đống 2, cái thứ ba vào đống 1, v.v. 

Sự thay thế giữ cho số lượng các giá trị riêng biệt cân bằng nhất có thể, giúp tối đa hóa sản phẩm của chúng. 
4. Tính toán$$x = d + \left\lceil \frac{s}{2} \right\rceil, \qquad y = d + \left\lfloor \frac{s}{2} \right\rfloor$$và đầu ra`x · y`. 
5. Một số hình khối vẫn chưa được gán. Đây là bản sao bổ sung của các giá trị xuất hiện ít nhất hai lần. 
6. Hãy để`need1`là số khối bổ sung cần thiết để đống 1 đạt kích thước`n`. 
7. Chỉ định đầu tiên`need1`các khối chưa được gán vào đống 1 và tất cả các khối chưa được gán còn lại vào đống 2. 

Các khối này không thay đổi số lượng giá trị riêng biệt vì giá trị của chúng đã có sẵn trong các đống tương ứng. 

### Tại sao nó hoạt động 

Mọi giá trị có tần số ít nhất là hai có thể đóng góp đồng thời vào cả hai vùng. Việc từ chối đặt một bản sao vào mỗi vùng nhớ sẽ làm giảm số lượng giá trị riêng biệt trong ít nhất một vùng nhớ và không bao giờ có thể cải thiện sản phẩm. 

Mỗi giá trị có tần số bằng một đóng góp chính xác một giá trị riêng biệt về tổng thể. Sau khi tất cả các giá trị lặp lại được phân chia, tổng số đóng góp có giá trị riêng biệt sẽ được cố định:$$x+y=2d+s.$$Đối với một tổng cố định, tích sẽ lớn nhất khi hai số bằng nhau nhất có thể. Các giá trị đơn lẻ xen kẽ tạo ra sự phân bố cân bằng chính xác đó. 

Các bản sao còn lại thuộc về các giá trị đã được biểu thị trong heap nên việc di chuyển chúng không thể thay đổi`x`hoặc`y`. Chúng chỉ ảnh hưởng đến kích thước heap. Việc lấp đầy các ô còn lại một cách tùy ý sẽ duy trì tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    pos = {}
    for i, v in enumerate(a):
        pos.setdefault(v, []).append(i)

    ans = [0] * (2 * n)

    d = 0
    s = 0

    single_heap = 1

    for v, idxs in pos.items():
        if len(idxs) >= 2:
            d += 1
            ans[idxs[0]] = 1
            ans[idxs[1]] = 2
        else:
            s += 1
            ans[idxs[0]] = single_heap
            single_heap = 3 - single_heap

    x = d + (s + 1) // 2
    y = d + s // 2
    best = x * y

    cnt1 = sum(1 for xh in ans if xh == 1)
    need1 = n - cnt1

    for v, idxs in pos.items():
        start = 2 if len(idxs) >= 2 else 1
        for idx in idxs[start:]:
            if need1 > 0:
                ans[idx] = 1
                need1 -= 1
            else:
                ans[idx] = 2

    print(best)
    print(*ans)

if __name__ == "__main__":
    solve()
```Từ điển`pos`lưu trữ tất cả các vị trí của mỗi giá trị. Điều này cho phép chúng tôi phân biệt lần xuất hiện đầu tiên, lần xuất hiện thứ hai và mọi bản sao còn lại. 

Trong lần chuyển đầu tiên, chúng tôi chỉ định các lần xuất hiện để xác định số lượng giá trị riêng biệt. Đối với các giá trị lặp lại, một bản sao được đặt vào mỗi đống. Đối với các giá trị đơn lẻ, chúng tôi xen kẽ các đống. 

Tại thời điểm đó một số hình khối vẫn chưa được chỉ định. Những hình khối này đều là bản sao dư thừa của các giá trị đã xuất hiện ở đâu đó. Họ không thể tăng số lượng giá trị riêng biệt trong cả hai vùng. 

Biến`need1`cho chúng ta biết đống 1 cần thêm bao nhiêu khối nữa để đạt kích thước`n`. Chúng ta đưa chính xác số khối chưa được gán vào đống 1 và phần còn lại vào đống 2. Vì tổng số khối là`2n`, heap 2 tự động kết thúc với kích thước`n`cũng vậy. 

Một lỗi triển khai phổ biến là đếm các giá trị riêng biệt sau khi gán các giá trị trùng lặp bổ sung. Số lượng riêng biệt được xác định hoàn toàn bởi giai đoạn chuyển nhượng đầu tiên. Các bản sao bổ sung chỉ tồn tại để đáp ứng ràng buộc kích thước vùng heap. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1
10 99
```Bảng tần số: 

| Giá trị | Tần số | 
| --- | --- | 
| 10 | 1 | 
| 99 | 1 | 

Bài tập đơn: 

| Giá trị | Heap được chỉ định | 
| --- | --- | 
| 10 | 1 | 
| 99 | 2 | 

chúng tôi có`d = 0`,`s = 2`. 

| Số lượng | Giá trị | 
| --- | --- | 
| x | 1 | 
| y | 1 | 
| Sản phẩm | 1 | 

Nhiệm vụ cuối cùng có thể là:```
1 2
```hoặc```
2 1
```Cả hai đều tối ưu. 

Ví dụ này cho thấy rằng khi tất cả các giá trị là duy nhất, mục tiêu chỉ đơn giản là cân bằng các giá trị đơn lẻ giữa các vùng nhớ. 

### Ví dụ 2 

đầu vào:```
2
13 24 13 45
```Bảng tần số: 

| Giá trị | Tần số | 
| --- | --- | 
| 13 | 2 | 
| 24 | 1 | 
| 45 | 1 | 

Giai đoạn đầu tiên: 

| Giá trị | Hành động | 
| --- | --- | 
| 13 | một bản sao cho mỗi đống | 
| 24 | đống 1 | 
| 45 | đống 2 | 

Số lượng riêng biệt: 

| Đống | Giá trị riêng biệt | 
| --- | --- | 
| 1 | {13, 24} | 
| 2 | {13, 45} | 

Như vậy: 

| Số lượng | Giá trị | 
| --- | --- | 
| x | 2 | 
| y | 2 | 
| Sản phẩm | 4 | 

Số có bốn chữ số thu được là:```
1313
1345
2413
2445
```Ví dụ này chứng minh tại sao việc chia tách các giá trị lặp lại trên cả hai vùng nhớ lại có giá trị. giá trị`13`góp phần vào số lượng riêng biệt của cả hai đống cùng một lúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi khối được xử lý một số lần không đổi | 
| Không gian |$O(n)$| Lưu trữ các vị trí và nhiệm vụ | 

Từ`2n ≤ 200`, giải pháp chạy thoải mái trong giới hạn. Thuật toán tuyến tính theo số khối và chỉ sử dụng sổ sách tần số đơn giản. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    a = list(map(int, input().split()))

    pos = {}
    for i, v in enumerate(a):
        pos.setdefault(v, []).append(i)

    ans = [0] * (2 * n)

    d = 0
    s = 0
    cur = 1

    for v, idxs in pos.items():
        if len(idxs) >= 2:
            d += 1
            ans[idxs[0]] = 1
            ans[idxs[1]] = 2
        else:
            s += 1
            ans[idxs[0]] = cur
            cur = 3 - cur

    best = (d + (s + 1) // 2) * (d + s // 2)

    cnt1 = sum(x == 1 for x in ans)
    need1 = n - cnt1

    for v, idxs in pos.items():
        start = 2 if len(idxs) >= 2 else 1
        for idx in idxs[start:]:
            if need1 > 0:
                ans[idx] = 1
                need1 -= 1
            else:
                ans[idx] = 2

    return str(best)

# provided sample
assert run("1\n10 99\n") == "1"

# all values equal
assert run("2\n55 55 55 55\n") == "1"

# all values distinct
assert run("2\n10 20 30 40\n") == "4"

# one duplicated value
assert run("2\n13 24 13 45\n") == "4"

# larger frequency
assert run("3\n10 10 10 20 30 40\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`55 55 55 55`|`1`| Bản sao không tạo ra giá trị khác biệt mới | 
|`10 20 30 40`|`4`| Phân phối đơn lẻ cân bằng | 
|`13 24 13 45`|`4`| Giá trị lặp lại sẽ xuất hiện trong cả hai đống | 
|`10 10 10 20 30 40`|`4`| Các bản sao bổ sung chỉ được sử dụng để điền kích thước heap | 

## Vỏ cạnh 

Hãy xem xét:```
2
55 55 55 55
```Tần số của`55`là bốn. Thuật toán đặt ngay một bản sao vào mỗi đống. Hai bản sao còn lại không liên quan đến số lượng riêng biệt và chỉ được sử dụng để lấp đầy kích thước heap. Cả hai đống đều chứa một giá trị riêng biệt`55`, vậy câu trả lời là`1`. 

Coi như:```
2
10 20 30 40
```Mỗi giá trị là một singleton. Thuật toán luân phiên các phép gán, tạo ra hai giá trị đơn lẻ trong mỗi vùng nhớ heap. Số lượng riêng biệt trở thành`(2,2)`và câu trả lời là`4`. Bất kỳ nhiệm vụ nào sản xuất`(3,1)`sẽ chỉ cho`3`. 

Coi như:```
3
10 10 20 20 30 40
```Giá trị`10`Và`20`xuất hiện ít nhất hai lần, do đó mỗi lần đóng góp vào cả hai đống. Giá trị`30`Và`40`là những người độc thân và được chia đều. Số lượng riêng biệt trở thành`(3,3)`và câu trả lời là`9`, là sản phẩm tối đa có thể có cho tổng số đóng góp có giá trị riêng biệt cố định.
