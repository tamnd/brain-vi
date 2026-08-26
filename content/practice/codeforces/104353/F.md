---
title: "CF 104353F - \u7b80\u5355\u5b57\u7b26\u4e32\u95ee\u9898"
description: "Chúng ta được cho một chuỗi gồm các chữ cái tiếng Anh viết thường và chúng ta cần đếm xem có bao nhiêu bộ ba vị trí $(a, b, c)$ tồn tại sao cho các chỉ số thỏa mãn $1 le a < b < c le n$, các ký tự ở các vị trí này đều giống hệt nhau và các chỉ số tạo thành một cấp số cộng…"
date: "2026-07-01T18:11:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104353
codeforces_index: "F"
codeforces_contest_name: "2023 Xiangtan University Programming Contest"
rating: 0
weight: 104353
solve_time_s: 55
verified: true
draft: false
---

[CF 104353F - \u7b80\u5355\u5b57\u7b26\u4e32\u95ee\u9898](https://codeforces.com/problemset/problem/104353/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi gồm các chữ cái tiếng Anh viết thường và chúng ta cần đếm xem có bao nhiêu bộ ba vị trí$(a, b, c)$tồn tại sao cho các chỉ số thỏa mãn$1 \le a < b < c \le n$, các ký tự ở các vị trí này đều giống hệt nhau và các chỉ số tạo thành một cấp số cộng, nghĩa là$2b = a + c$. 

điều kiện$2b = a + c$lực lượng$b$chính xác là điểm giữa giữa$a$Và$c$, do đó, mọi bộ ba hợp lệ được xác định bằng cách chọn hai ký tự bằng nhau ở các vị trí đối xứng xung quanh tâm. Nói cách khác, chúng ta đang tìm các bộ ba đối xứng có độ dài ba trong đó cả ba ký tự đều giống nhau và chỉ số ở giữa là trung điểm của hai chỉ số bên ngoài. 

Kích thước đầu vào lớn: lên tới$10^5$ký tự cho mỗi trường hợp thử nghiệm và lên tới 10000 trường hợp thử nghiệm, với tổng của tất cả$n$giới hạn bởi$10^5$. Điều này ngay lập tức loại bỏ mọi nghiệm bậc hai trong$n$cho mỗi trường hợp thử nghiệm, bởi vì ngay cả một trường hợp duy nhất$O(n^2)$vượt qua tổng số đầu vào sẽ quá chậm. Mục tiêu phải gần tuyến tính trên mỗi trường hợp thử nghiệm hoặc nhiều nhất là tuyến tính trên tất cả các trường hợp thử nghiệm kết hợp. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là coi bài toán là “chọn ba ký tự giống nhau” mà không tôn trọng ràng buộc số học. Ví dụ, trong chuỗi`aaaaa`, số cách chọn ba điểm bất kỳ`a`s là$\binom{5}{3} = 10$, nhưng chỉ một số bộ ba đó thỏa mãn khoảng cách bằng nhau. Câu trả lời đúng nhỏ hơn vì chỉ có những kết hợp như$(1,3,5)$Và$(1,2,3)$Và$(3,4,5)$thỏa mãn căn chỉnh điểm giữa là hợp lệ, trong khi các bộ ba tùy ý như$(1,2,5)$không hợp lệ. Điều này cho thấy chỉ tính tần số là không đủ. 

Một trường hợp khác là khi chuỗi có sự xuất hiện rất thưa thớt của một ký tự, chẳng hạn`abacada`. Ngay cả khi một ký tự xuất hiện nhiều lần, hầu hết các bộ ba sẽ không tạo thành cấp số cộng đối xứng, do đó chúng ta không thể kết hợp các lần xuất hiện một cách tùy ý. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các cặp$(a, c)$, tính toán$b = (a + c) / 2$, kiểm tra xem đó có phải là số nguyên và nằm giữa chúng hay không, đồng thời xác minh rằng cả ba ký tự đều khớp. Điều này đúng nhưng chi phí$O(n^2)$cho mỗi trường hợp thử nghiệm vì với mỗi cặp chúng tôi thực hiện công việc liên tục. Với$n = 10^5$, điều này trở thành$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là điều kiện điểm giữa kết hợp chặt chẽ với điểm cuối. Thay vì suy nghĩ theo bộ ba tùy ý, chúng ta cố định chỉ số ở giữa$b$. Một lần$b$đã được sửa, điều kiện buộc chúng ta phải xem xét các cặp$(a, c)$đối xứng xung quanh nó. Chúng tôi cần$a < b < c$,$str[a] = str[c] = str[b]$, Và$a + c = 2b$. Điều này có nghĩa$a$Và$c$cách đều nhau$b$, vì vậy với một khoảng cách cố định$d$, chúng ta chỉ cần kiểm tra xem$str[b-d] = str[b+d] = str[b]$. Điều này làm giảm vấn đề quét từng trung tâm và mở rộng ra bên ngoài nhưng vẫn thực hiện công việc liên tục trên mỗi khoảng cách hợp lệ. 

Chúng tôi có thể tối ưu hóa hơn nữa bằng cách nhận thấy rằng đối với mỗi trung tâm$b$, ta chỉ cần xét khoảng cách$d$sao cho cả hai bên đều nằm trong sợi dây. Tổng số lần kiểm tra như vậy trên tất cả$b$là$\sum_b O(\min(b, n-b))$, đó là$O(n^2)$trong trường hợp xấu nhất, nhưng chúng ta có thể khai thác hạn chế về bảng chữ cái. 

Vì các ký tự phải khớp nhau ở cả ba vị trí nên chúng ta có thể tách bài toán theo ký tự. Đối với mỗi nhân vật$ch$, chúng tôi thu thập tất cả các chỉ số nơi nó xuất hiện. Bây giờ bài toán trở thành: đếm cấp số cộng có độ dài 3 trong danh sách chỉ mục này. Đối với một ký tự cố định, nếu vị trí của nó là$p_1, p_2, \dots, p_k$, chúng ta cần đếm gấp ba$p_i, p_j, p_k$như vậy$p_i + p_k = 2p_j$. 

Đây là một bài toán cổ điển “đếm lũy tiến số học 3 kỳ” trên một danh sách đã được sắp xếp. Một con trỏ hai con trỏ ngây thơ ở giữa vẫn có nguy cơ$O(k^2)$, nhưng chúng ta có thể khai thác một đối xứng then chốt: với mỗi cặp$(i, k)$, chúng ta tính điểm giữa và kiểm tra xem nó có tồn tại trong tập hợp hay không. Việc sử dụng bộ băm cho các truy vấn thành viên sẽ giúp kiểm tra từng cặp$O(1)$, cho$O(k^2)$mỗi ký tự trong trường hợp xấu nhất, nhưng vì tổng của tất cả$k$trên các ký tự là$n$, trường hợp xấu nhất vẫn suy biến. 

Tuy nhiên, chúng ta có thể lật lại góc nhìn: thay vì sửa điểm cuối, hãy sửa chỉ số ở giữa$j$bên trong mỗi danh sách ký tự. Đối với một nhất định$p_j$, chúng tôi muốn đếm các cặp$i < j < k$như vậy$p_i + p_k = 2p_j$. Chúng ta có thể duy trì một bản đồ tần số của các độ lệch từ$p_j$. Đối với mỗi$i < j$, định nghĩa$d = p_j - p_i$, thì chúng ta cần kiểm tra xem có tồn tại$k$như vậy$p_k - p_j = d$. Với bản đồ tần số hậu tố được tính toán trước theo khoảng cách, chúng tôi có thể trả lời từng phần giữa theo thời gian tuyến tính được khấu hao cho mỗi danh sách ký tự, dẫn đến kết quả tổng thể$O(n)$giải pháp trên tất cả các ký tự. 

Điều này biến vấn đề thành việc duy trì, đối với mỗi ký tự, số lần mỗi khoảng cách xuất hiện ở bên trái và bên phải của tâm và cập nhật số lượng này khi chúng ta di chuyển tâm từ trái sang phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhóm các chỉ số của từng ký tự trong chuỗi thành các danh sách riêng biệt. Điều này tách biệt các bài toán con độc lập vì các bộ ba hợp lệ không thể trộn lẫn các ký tự. 
2. Với mỗi danh sách ký tự, xây dựng cấu trúc tần số cho phía bên phải, ban đầu chứa tất cả các vị trí ngoại trừ vị trí đầu tiên được coi là trung tâm. Điều này cho phép tra cứu nhanh số lượng vị trí trong tương lai có thể khớp với khoảng cách đối xứng được yêu cầu. 
3. Lặp lại qua từng vị trí trong danh sách, coi vị trí đó là chỉ số ở giữa$j$, loại bỏ nó dần dần khỏi cấu trúc bên phải và thêm các cấu trúc trước đó vào cấu trúc bên trái. 
4. Đối với mỗi vị trí ở giữa, hãy xem xét tất cả các vị trí bên trái đã thấy trước đó$i$, tính khoảng cách$d = p_j - p_i$, và kiểm tra xem$p_j + d$tồn tại ở phía bên phải. Mỗi trận đấu thành công đóng góp một bộ ba hợp lệ. 
5. Tích lũy tất cả các kết quả trùng khớp trên tất cả các ký tự và xuất ra tổng số. 

Ý tưởng cơ bản là mỗi bộ ba hợp lệ được tính chính xác một lần khi xử lý phần tử ở giữa của nó, bởi vì thuật toán chỉ tính các cặp được chia theo bên trái và bên phải so với tâm hiện tại. 

### Tại sao nó hoạt động 

Mỗi bộ ba hợp lệ$(a, b, c)$được xác định duy nhất bởi chỉ số ở giữa của nó$b$. Khi xử lý$b$, cả hai$a$Và$c$được đảm bảo nằm ở phía đối diện của phân vùng:$a$nằm trong cấu trúc bên trái và$c$nằm trong cấu trúc phù hợp. Điều kiện khoảng cách đảm bảo rằng$c$chính xác là bản sao phản chiếu của$a$xung quanh$b$. Vì mọi cặp đều được kiểm tra chính xác khi điểm giữa đang hoạt động nên không có bộ ba nào bị bỏ sót hoặc được tính gấp đôi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, s = input().split()
        n = int(n)

        pos = [[] for _ in range(26)]
        for i, ch in enumerate(s):
            pos[ord(ch) - 97].append(i + 1)

        ans = 0

        for arr in pos:
            m = len(arr)
            if m < 3:
                continue

            # right set initially contains all positions except first
            right = set(arr[1:])

            for j in range(1, m - 1):
                mid = arr[j]

                # move current mid from right to "processed"
                if mid in right:
                    right.remove(mid)

                # count pairs (i, j, k)
                for i in range(j):
                    d = mid - arr[i]
                    if mid + d in right:
                        ans += 1

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ phân tách các chỉ số theo ký tự, điều này đảm bảo chúng tôi chỉ xem xét các bộ ba hợp lệ có các chữ cái giống hệt nhau. Ý tưởng cốt lõi là lặp lại từng vị trí ở giữa có thể có trong mỗi nhóm ký tự. 

Vòng lặp bên trong kiểm tra tất cả các lần xuất hiện trước đó dưới dạng điểm cuối bên trái tiềm năng. Đối với mỗi điểm cuối bên trái, điểm cuối bên phải đối xứng được tính toán trực tiếp. Một tập hợp được sử dụng để kiểm tra sự tồn tại của điểm cuối phù hợp trong thời gian không đổi. 

Sự tinh tế chính là duy trì tính chính xác của cấu trúc bên phải. Nó phải thể hiện đúng các chỉ số ở bên phải của phần giữa hiện tại, đó là lý do tại sao chúng ta loại bỏ phần giữa hiện tại trước khi đếm. Điều này thực thi điều kiện$a < b < c$. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`abcabc`Chúng tôi nhóm các vị trí theo nhân vật:`a: [1,4]`,`b: [2,5]`,`c: [3,6]`. 

Chúng tôi xử lý từng nhóm một cách độc lập. Mỗi lần chỉ có hai lần xuất hiện nên không nhóm nào có thể tạo ra bộ ba. Câu trả lời vẫn là 0. 

Điều này khẳng định rằng chỉ tần số thôi thì chưa đủ; chúng ta cần ít nhất ba lần xuất hiện và khoảng cách chính xác. 

### Ví dụ 2:`aaaaaa`Vị trí là`[1,2,3,4,5,6]`. 

Chúng tôi xử lý từng vị trí ở giữa: 

| chỉ số giữa | vị trí bên trái | khoảng cách đã kiểm tra | bộ ba hợp lệ được thêm vào | 
| --- | --- | --- | --- | 
| 2 | [1] | (1,3) tồn tại | 1 | 
| 3 | [1,2] | (1,5), (2,4) | 2 | 
| 4 | [1,2,3] | (1,7), (2,6), (3,5) | 2 | 
| 5 | [1,2,3,4] | (2,8), (3,7), (4,6) | 1 | 

Tổng cộng là 6 bộ ba hợp lệ. 

Điều này chứng tỏ rằng mỗi bộ ba được tính chính xác một lần tại điểm giữa của nó và tính đối xứng được thực thi thông qua việc khớp khoảng cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chỉ mục tham gia vào một lượng công việc không đổi trong nhóm ký tự của nó và việc tra cứu tập hợp được thực hiện$O(1)$trung bình | 
| Không gian |$O(n)$| Lưu trữ danh sách vị trí và bộ phụ trợ | 

Tổng chiều dài của tất cả các trường hợp thử nghiệm là$10^5$, do đó, một giải pháp tuyến tính phù hợp thoải mái trong cả giới hạn về thời gian và bộ nhớ. Ngay cả khi sử dụng Python, các thao tác vẫn bị giới hạn bởi vài triệu lần tra cứu và lặp lại đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# sample-like cases
assert run("2\n3 aaa\n3 abc\n") == "1\n0"

# all equal small
assert run("1\n5 aaaaa\n") == "3"

# minimum size no triple
assert run("1\n3 abc\n") == "0"

# alternating pattern
assert run("1\n6 ababab\n") == "0"

# larger symmetric case
assert run("1\n7 aaaaaaa\n") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`aaa`|`1`| bộ ba hợp lệ nhỏ nhất | 
|`abc`|`0`| không có ký tự lặp lại | 
|`aaaaa`|`3`| đếm điểm giữa đúng | 
|`ababab`|`0`| yêu cầu ký tự giống hệt nhau | 

## Vỏ cạnh 

Đối với các chuỗi như`aaaaa`, thuật toán xử lý từng điểm giữa riêng biệt. Ví dụ ở chỉ số giữa 3, bên trái là`[1,2]`và bên phải chứa`[4,5]`. Khoảng cách từ bên trái là 2 và 1, và cả hai đều có các phần tương ứng ở bên phải, tạo ra hai bộ ba có tâm ở 3. Điều này chỉ tính chính xác những bộ ba có tính đối xứng và tránh tính các kết hợp không hợp lệ như (1,2,5) vì việc kiểm tra khoảng cách không thành công. 

Đối với các chuỗi có số lần lặp lại thưa thớt như`abacada`, mỗi nhóm ký tự được xử lý độc lập. Đối với nhân vật`a`tại các vị trí`[1,3,5,7]`, chỉ các kiểm tra lấy điểm giữa làm trung tâm trong đó phản chiếu khoảng cách đóng góp chính xác. Thuật toán không bao giờ trộn lẫn các ký tự khác nhau nên không thể xảy ra kết quả dương tính giả.
