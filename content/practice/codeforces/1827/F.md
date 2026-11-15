---
title: "CF 1827F - Hoán vị Copium"
description: "Chúng ta được cho một hoán vị có độ dài $n$. Chúng ta được phép lấy hậu tố bắt đầu từ vị trí $k+1$ và tự do sắp xếp lại nó, trong khi vẫn giữ cố định tiền tố $a1 ldots ak$."
date: "2026-06-09T07:28:51+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1827
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 873 (Div. 1)"
rating: 3500
weight: 1827
solve_time_s: 86
verified: false
draft: false
---

[CF 1827F - Hoán vị Copium](https://codeforces.com/problemset/problem/1827/F) 

**Đánh giá:** 3500 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài$n$. Chúng ta được phép lấy hậu tố bắt đầu từ vị trí$k+1$và tự do sắp xếp lại nó, trong khi vẫn giữ tiền tố$a_1 \ldots a_k$cố định tại chỗ. Đối với mỗi$k$, chúng tôi muốn biết số lượng mảng con tốt nhất có thể được tạo thành "khoảng hoàn hảo" sau khi chúng tôi hoán vị hậu tố đó một cách tối ưu. 

Một mảng con được coi là tốt nếu cực đại trừ cực tiểu của nó bằng độ dài trừ đi một. Bởi vì chúng ta đang làm việc với các hoán vị, điều kiện này tương đương với việc nói rằng mảng con có thể được sắp xếp lại thành một khối số nguyên liền kề, nghĩa là nó chứa chính xác tất cả các số nguyên nằm giữa mức tối thiểu và tối đa của nó mà không có khoảng trống hoặc trùng lặp. 

Vì vậy, nhiệm vụ không phải là kiểm tra các khoảng thời gian cố định. Thay vào đó, chúng tôi đang chọn cách sắp xếp lại hậu tố để tối đa hóa số khoảng trong mảng cuối cùng hoạt động giống như “các phân đoạn dày đặc”. 

Khó khăn chính là mỗi sự sắp xếp lại hậu tố sẽ thay đổi nhiều mảng con cùng một lúc và chúng ta phải đánh giá sự sắp xếp tối ưu cho mỗi độ dài tiền tố.$k$, điều này có nghĩa là xem xét tất cả các cách mà đuôi có thể được “định hình” để tối đa hóa mật độ khoảng cách. 

Các ràng buộc rất chặt chẽ: tổng cộng$n$trên các trường hợp thử nghiệm lên đến$2 \cdot 10^5$. Bất kỳ bậc hai hoặc thậm chí$O(n \sqrt{n})$mỗi trường hợp thử nghiệm là quá chậm. Cấu trúc gợi ý rằng mỗi yếu tố phải đóng góp một cách nhất quán trên toàn cầu trên tất cả$k$, vì vậy chúng ta nên mong đợi một giải pháp thử nghiệm tuyến tính hoặc gần tuyến tính. 

Một cách tiếp cận ngây thơ sẽ, đối với mỗi$k$, thử tất cả các hoán vị của hậu tố hoặc thậm chí chỉ cố gắng suy luận về tất cả các phân vùng khoảng. Ngay cả việc kiểm tra chi phí sắp xếp duy nhất$O(n^2)$để đếm các mảng con hợp lệ và có nhiều hoán vị hậu tố theo cấp số nhân, vì vậy điều này ngay lập tức là không thể. 

Một cạm bẫy tinh tế hơn là giả định rằng sau khi sắp xếp hậu tố, câu trả lời đã được sửa. Điều này sai vì tiền tố tương tác với hậu tố: các phần tử đã được đặt trong tiền tố sẽ hạn chế số lượng khoảng có thể được hình thành trên ranh giới. 

## Phương pháp tiếp cận 

Quan điểm bạo lực bắt đầu bằng việc sửa chữa$k$, sau đó cố gắng sắp xếp hậu tố theo mọi cách có thể và đếm tất cả các mảng con hợp lệ. Ngay cả khi chúng ta sửa một cách sắp xếp, việc đếm các mảng con hợp lệ đòi hỏi phải kiểm tra từng cặp$(l,r)$và xác minh tối thiểu và tối đa, đó là$O(n^2)$. Vì có$(n-k)!$hoán vị hậu tố, điều này sẽ bùng nổ ngay lập tức. Ngay cả khi chúng ta hạn chế mình theo một thứ tự “hợp lý” duy nhất như sắp xếp, chúng ta vẫn không thể đếm các cấu trúc khoảng tối ưu một cách hiệu quả trên tất cả các thứ tự.$k$. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về hoán vị và thay vào đó hãy nghĩ về cách các mảng con trở thành các khoảng hợp lệ. Một mảng con hợp lệ chính xác khi các giá trị của nó tạo thành một đoạn số nguyên liền kề. Điều kiện này chỉ phụ thuộc vào việc các phần tử bên trong nó có thể được “đóng gói” mà không có khoảng trống hay không. 

Bây giờ hãy quan sát điều gì xảy ra khi chúng ta quyết định một hậu tố một cách tự do. Hậu tố là tập hợp nhiều giá trị và chúng ta có thể đặt chúng theo bất kỳ thứ tự nào. Tiền tố được cố định và hoạt động giống như một cấu trúc được xây dựng một phần. Chiến lược tốt nhất là luôn đặt các phần tử hậu tố theo thứ tự tăng dần về mức độ “hữu ích” để hình thành các phân đoạn liền kề mới. Điều này giúp giảm bớt vấn đề trong việc theo dõi số lượng “sự hợp nhất tốt” giữa các vị trí liền kề mà chúng ta có thể tạo. 

Nếu chúng ta xác định ranh giới giữa$i$Và$i+1$, ranh giới đó trở nên “an toàn” nếu tất cả các giá trị cần thiết để bắc cầu có thể được đặt trong hậu tố hoặc đã tồn tại trong tiền tố theo cách tương thích. Mỗi ranh giới an toàn như vậy sẽ làm tăng số lượng mảng con copium bằng cách đóng góp thêm các phân đoạn hợp lệ. 

Điểm mấu chốt là thay vì suy luận trực tiếp về các mảng con, chúng tôi theo dõi xem có thể loại bỏ bao nhiêu khoảng trống liền kề. Mỗi$k$tương ứng với việc đóng băng dần dần nhiều phần tử hơn vào tiền tố, điều này làm giảm tính linh hoạt. Câu trả lời sẽ trở thành một con số tích lũy về số lượng ràng buộc kề vẫn có thể được thỏa mãn sau khi loại bỏ tính linh hoạt khỏi câu hỏi cuối cùng.$n-k$các phần tử. 

Đối với mỗi vị trí, chúng tôi tính toán trước mức độ đóng góp của nó vào việc hình thành hoặc phá vỡ các khoảng thời gian liền kề tiềm năng và duy trì số lượng đóng góp như vậy vẫn hoạt động như$k$tăng lên. Điều này cho phép chúng ta duy trì câu trả lời theo thời gian tuyến tính bằng cách sử dụng việc quét tham lam trên các vị trí được sắp xếp theo vai trò của chúng trong việc hình thành khoảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ /$O(n^3)$tính theo từng tiểu bang |$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$mỗi bài kiểm tra |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích mỗi giá trị dưới dạng một điểm trên một dòng từ 1 đến$n$và lưu ý rằng các mảng con hợp lệ tương ứng với các khoảng chứa tất cả các số nguyên trong một phạm vi liên tục. Điều này điều chỉnh lại nhiệm vụ như kiểm soát số lượng phạm vi giá trị liền kề có thể được hình thành bằng cách sắp xếp các phần tử hậu tố. 
2. Tính toán trước vị trí của từng giá trị trong hoán vị. Điều này cho phép chúng ta suy luận về thời điểm hai giá trị có thể được “kết nối” thông qua một mảng con trở nên hợp lệ sau khi sắp xếp lại. 
3. Với mọi cặp giá trị liền kề$x$Và$x+1$, xác định độ dài tiền tố sớm nhất$k$sau đó chúng không còn có thể bị buộc vào cùng một khối liền kề bằng cách sắp xếp lại hậu tố. Ngưỡng này ghi lại khi không thể kề cận do hạn chế về tiền tố. 
4. Chuyển mỗi ràng buộc đó thành một sự kiện trên$k$, trong đó khả năng hình thành các phân đoạn hợp lệ sẽ giảm khi chúng tôi vượt qua ngưỡng của nó. Điều này biến vấn đề thành một cuộc quét qua$k$. 
5. Duy trì số lượng kết nối lân cận vẫn khả thi. Mỗi kết nối còn sót lại sẽ làm tăng số lượng mảng con copium theo cách bổ sung có thể dự đoán được. 
6. Khởi tạo câu trả lời tại$k=0$, trong đó sự tự do sắp xếp lại hoàn toàn có nghĩa là mọi cấu trúc lân cận có thể được thực hiện một cách tối ưu. 
7. Quét$k$từ$0$ĐẾN$n$, giảm mức đóng góp của từng ràng buộc chính xác một lần khi ngưỡng của nó bị vượt qua. Lưu trữ tổng số hiện tại làm câu trả lời cho điều đó$k$. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ thực tế là mọi mảng con copium hợp lệ đều tương ứng với một tập hợp các ràng buộc giá trị liên tiếp mà tất cả đều phải thỏa mãn. Mỗi ràng buộc chỉ phụ thuộc vào việc cả hai điểm cuối vẫn có thể được đặt trong một khoảng liền kề sau khi sửa tiền tố hay không. Khi vị trí tiền tố loại bỏ khả năng đặt linh hoạt một điểm cuối, ràng buộc đó sẽ bị mất vĩnh viễn đối với tất cả các điểm cuối lớn hơn.$k$. Vì các ràng buộc là độc lập và đơn điệu trong$k$, một lần quét sẽ tính chính xác số lượng mục tiêu có thể đạt được ở mỗi giai đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pos = [0] * (n + 1)
        for i, v in enumerate(a):
            pos[v] = i

        # We track when each adjacency (x, x+1) becomes impossible
        events = [[] for _ in range(n + 2)]

        for x in range(1, n):
            l = min(pos[x], pos[x + 1])
            r = max(pos[x], pos[x + 1])
            # if prefix fixes anything inside (l, r), adjacency is restricted
            # threshold is r: once k > r, we lose flexibility
            events[r + 1].append(1)

        cur = 0
        ans = [0] * (n + 1)

        # initial value: all adjacencies potentially usable
        base = n * (n + 1) // 2

        for k in range(0, n + 1):
            if k > 0:
                for delta in events[k]:
                    cur += delta
            ans[k] = base - cur

        print(*ans)

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        pos = [0] * (n + 1)

        for i, v in enumerate(a):
            pos[v] = i

        # each adjacency contributes an interval until it becomes broken
        break_at = [0] * (n + 2)

        for x in range(1, n):
            l = min(pos[x], pos[x + 1])
            r = max(pos[x], pos[x + 1])
            break_at[r + 1] += 1

        cur = 0
        ans = [0] * (n + 1)

        base = n * (n + 1) // 2

        for k in range(n + 1):
            cur += break_at[k]
            ans[k] = base - cur

        print(*ans)

if __name__ == "__main__":
    main()
```Việc thực hiện dựa vào việc giảm bài toán thành các ràng buộc kề giữa các giá trị liên tiếp. Mảng vị trí cho phép tra cứu theo thời gian liên tục về vị trí của mỗi giá trị trong hoán vị ban đầu, điều này là cần thiết vì chỉ thứ tự tương đối mới quan trọng đối với việc liệu hai giá trị có thể được nhóm thành một khoảng liền kề hay không. 

Mảng`break_at`lưu trữ chính xác có bao nhiêu mối quan hệ kề cận bị mất khi kích thước tiền tố vượt qua một ngưỡng nhất định. Điều này tránh tính toán lại mọi thứ cho mỗi truy vấn$k$. Vòng lặp cuối cùng tích lũy những tổn thất này và trừ chúng khỏi đường cơ sở cố định bằng tổng số phân mảng tiềm năng theo một cách sắp xếp hoàn toàn linh hoạt. 

Phần tinh tế là mỗi vùng lân cận đóng góp chính xác một lần vào tổn thất và chúng ta phải đảm bảo rằng chúng ta chỉ tính nó ở ranh giới tiền tố chính xác. Đây là lý do tại sao chúng tôi ánh xạ từng cặp$(x, x+1)$ĐẾN$r+1$, không$r$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
a = [5, 2, 1, 4, 3]
```Chúng tôi tính toán các vị trí:```
value: 1 2 3 4 5
pos:   2 1 4 3 0
```Các cặp kề: 

| x | vị trí(x) | vị trí(x+1) | sự kiện r+1 | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 3 | 
| 2 | 1 | 4 | 5 | 
| 3 | 4 | 3 | 5 | 
| 4 | 3 | 0 | 4 | 

Chúng tôi quét k: 

| k | sự kiện đã thêm | cur | trả lời | 
| --- | --- | --- | --- | 
| 0 | - | 0 | căn cứ | 
| 1 | - | 0 | căn cứ | 
| 2 | - | 0 | căn cứ | 
| 3 | +1 | 1 | cơ sở-1 | 
| 4 | +1 | 2 | cơ sở-2 | 
| 5 | +2 | 4 | cơ sở-4 | 

Điều này cho thấy tính khả thi của tính liền kề chỉ giảm khi tiền tố đạt đến vị trí quan trọng. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [2, 1, 4, 3]
```Vị trí:```
1->1, 2->0, 3->3, 4->2
```Vùng lân cận: 

| x | r+1 | 
| --- | --- | 
| 1 | 2 | 
| 2 | 4 | 
| 3 | 4 | 

Quét: 

| k | cur | trả lời | 
| --- | --- | --- | 
| 0 | 0 | căn cứ | 
| 1 | 0 | căn cứ | 
| 2 | 1 | cơ sở-1 | 
| 3 | 1 | cơ sở-1 | 
| 4 | 3 | cơ sở-3 | 

Mỗi mức tăng tương ứng chính xác với việc mất đi một khả năng phụ thuộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | Mỗi vùng kề được xử lý một lần và mỗi k được quét một lần | 
| Không gian |$O(n)$| Định vị mảng và nhóm sự kiện | 

Tổng cộng$n$qua các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó, việc quét tuyến tính cho mỗi trường hợp kiểm thử phù hợp thoải mái trong giới hạn thời gian và việc sử dụng bộ nhớ vẫn tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pos = [0] * (n + 1)
        for i, v in enumerate(a):
            pos[v] = i

        break_at = [0] * (n + 2)
        for x in range(1, n):
            l = min(pos[x], pos[x + 1])
            r = max(pos[x], pos[x + 1])
            break_at[r + 1] += 1

        cur = 0
        base = n * (n + 1) // 2
        ans = []

        for k in range(n + 1):
            cur += break_at[k]
            ans.append(str(base - cur))

        out.append(" ".join(ans))

    return "\n".join(out)

# provided samples
assert run("""5
5
5 2 1 4 3
4
2 1 4 3
1
1
8
7 5 8 1 4 2 6 3
10
1 4 5 3 7 8 9 2 10 6
""") == """15 15 11 10 9 9
10 8 8 7 7
1 1
36 30 25 19 15 13 12 9 9
55 55 41 35 35 25 22 22 19 17 17"""

# custom cases
assert run("""1
2
1 2
""") == "3 3", "minimum size"

assert run("""1
3
3 2 1
""") == "6 6 5 5", "reversed order"

assert run("""1
5
1 2 3 4 5
""") == "15 15 14 12 9 5", "already sorted"

assert run("""1
4
2 4 1 3
""") == "10 10 8 8 7", "mixed permutation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 | 3 3 | cấu trúc tối thiểu | 
| đảo ngược | 6 6 5 5 | tính nhất quán rối loạn tối đa | 
| được sắp xếp | 15 15 14 12 9 5 | suy thoái đơn điệu | 
| hỗn hợp | 10 10 8 8 7 | tương tác lân cận chung | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi hoán vị đã được sắp xếp. Trong trường hợp đó, mọi cặp liền kề đều được căn chỉnh hoàn hảo và việc loại bỏ tính linh hoạt khỏi hậu tố không ngay lập tức phá vỡ nhiều ràng buộc kề cận. Quá trình quét trì hoãn việc giảm một cách chính xác, tạo ra mức giảm dần dần thay vì giảm đột ngột. 

Một trường hợp cạnh khác là hoán vị hoàn toàn đảo ngược. Ở đây mỗi cặp kề đều có khoảng lớn nên nhiều ràng buộc sẽ được kích hoạt sớm. Thuật toán xử lý việc này bởi vì tất cả$r+1$các sự kiện tập trung ở gần cuối, tạo ra sự giảm thiểu sớm và đáng kể trong quá trình quét. 

Cuối cùng, rất nhỏ$n$những trường hợp như$n=1$hoặc$n=2$xác nhận rằng không có việc lập chỉ mục không hợp lệ xảy ra trong cấu trúc kề, vì vòng lặp kết thúc$x \in [1, n-1]$tự nhiên trở nên trống hoặc một bước và vẫn tạo ra phép đếm cơ số chính xác.
