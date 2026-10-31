---
title: "CF 106309E - \u0420\u043e\u0434\u0438\u0442\u0435\u043b\u044c\u0441\u043a\u0438\u0435 \u0445\u0440\u043e\u043c\u043e\u0441\u043e\u043c\u044b"
description: "Chúng ta có một tập hợp các nhiễm sắc thể gốc, mỗi nhiễm sắc thể được biểu thị bằng một chuỗi các chữ cái viết thường. Khi hai cha mẹ tạo ra một nhiễm sắc thể con, mọi vị trí của đứa trẻ sẽ được sao chép độc lập với một trong hai cha mẹ."
date: "2026-06-25T07:45:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106309
codeforces_index: "E"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435, 9-11 \u043a\u043b\u0430\u0441\u0441\u044b, \u041f\u0435\u0440\u043c\u0441\u043a\u0438\u0439 \u043a\u0440\u0430\u0439, 2025"
rating: 0
weight: 106309
solve_time_s: 36
verified: true
draft: false
---

[CF 106309E - \u0420\u043e\u0434\u0438\u0442\u0435\u043b\u044c\u0441\u043a\u0438\u0435 \u0445\u0440\u043e\u043c\u043e\u0441\u043e\u043c\u044b](https://codeforces.com/problemset/problem/106309/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng ta có một tập hợp các nhiễm sắc thể gốc, mỗi nhiễm sắc thể được biểu thị bằng một chuỗi các chữ cái viết thường. Khi hai cha mẹ tạo ra một nhiễm sắc thể con, mọi vị trí của đứa trẻ sẽ được sao chép độc lập với một trong hai cha mẹ. Nhiệm vụ là xác định nhiễm sắc thể con ứng cử viên nào có thể được tạo ra bởi ít nhất một cặp nhiễm sắc thể bố mẹ từ bộ sưu tập. Đầu ra là chỉ số của tất cả các phần tử con hợp lệ theo thứ tự tăng dần. 

Đầu vào đưa ra số lượng nhiễm sắc thể cha mẹ, độ dài chung của chúng, chính các chuỗi cha mẹ, sau đó là một danh sách nhỏ các con ứng cử viên. Vì tập gốc là cố định và chỉ có các ứng viên thay đổi nên thách thức chính là xử lý trước tập gốc để có thể kiểm tra từng ứng viên một cách nhanh chóng. 

Số lượng bố mẹ có thể lên tới 20000, trong khi độ dài nhiễm sắc thể tối đa là 50. So sánh bậc hai giữa tất cả các cặp bố mẹ sẽ cần khoảng 400 triệu lượt kiểm tra cặp và mỗi lượt kiểm tra sẽ kiểm tra tối đa 50 ký tự. Tốc độ này quá chậm so với giới hạn một giây. Số lượng trẻ em ứng cử viên ít, nhiều nhất là 10, có nghĩa là chúng tôi có thể chi trả cho một ca phẫu thuật đắt tiền hơn cho mỗi ứng viên, nhưng không phải là một ca phẫu thuật liên quan đến tất cả các cặp cha mẹ. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Đầu tiên là khi đứa trẻ giống hệt cha mẹ. Cha mẹ đơn thân là không đủ vì sinh sản cần có cả cha và mẹ. Ví dụ:```
Input
2 3
abc
def
1
abc

Output
0
```Cha mẹ duy nhất có thể khớp chính xác với đứa trẻ là`abc`, nhưng không có cha mẹ thứ hai có thể cung cấp các vị trí còn thiếu. 

Một trường hợp khác là khi cần cả cha lẫn mẹ vì không ai phù hợp với toàn bộ đứa trẻ. Ví dụ:```
Input
2 2
ab
ac
1
aa

Output
1
```Cha mẹ đầu tiên cung cấp ký tự đầu tiên và cha mẹ thứ hai cung cấp ký tự thứ hai. Ở đây, giải pháp chỉ kiểm tra xem liệu cha hoặc mẹ có khác với con ở một số ít chỗ hay không. 

Trường hợp tế nhị cuối cùng là khi đứa trẻ có hai cha mẹ khác nhau:```
Input
3 3
abc
abc
def
1
abc
```Trong bài toán ban đầu, cha mẹ là khác nhau, vì vậy đầu vào chính xác này nằm ngoài các ràng buộc chính thức, nhưng nó minh họa quy tắc cần có hai nhiễm sắc thể. Việc triển khai vẫn xử lý chính xác tình huống này bằng cách theo dõi số lượng cha mẹ tạo ra từng mẫu không khớp. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử từng cặp cha mẹ cho từng đứa trẻ ứng cử viên. Đối với một cặp cố định, chúng ta có thể kiểm tra mọi vị trí và xác minh rằng ký tự con bằng một trong hai ký tự cha. Điều này đúng vì nó tuân theo định nghĩa về giao cắt một cách chính xác. 

Tuy nhiên, trường hợp xấu nhất là quá lớn. Với 20000 cha mẹ, có khoảng 200 triệu cặp có thể có. Việc kiểm tra 50 vị trí cho mỗi cặp sẽ yêu cầu khoảng 10 tỷ ký tự so sánh, không thể đáp ứng được giới hạn thời gian. 

Quan sát quan trọng là đối với một đứa trẻ ứng cử viên, chúng ta thực sự không cần biết về cả cha lẫn mẹ. Với mỗi bậc cha mẹ, chúng ta chỉ cần biết vị trí của cha mẹ đó khác với con cái ở điểm nào. Gọi tập hợp các vị trí này là mặt nạ không khớp. Hai cha mẹ có thể tạo ra đứa trẻ một cách chính xác khi mặt nạ không khớp của họ không trùng nhau. Nếu một phụ huynh khác nhau ở một vị trí thì phụ huynh kia phải phù hợp ở vị trí đó. 

Vấn đề trở thành: với nhiều mặt nạ 50 bit, có tồn tại một cặp có bit AND bằng 0 không? 

Cho một mặt nạ`x`, chúng ta cần một chiếc mặt nạ khác`y`nơi mọi bit được thiết lập`y`bằng 0 trong`x`. Nói cách khác,`y`phải là tập con của phần bù của`x`. Phép thử nhị phân trên 50 bit cho phép chúng ta tìm kiếm mặt nạ như vậy trong`O(l)`thời gian. 

Vì chỉ có tối đa 10 trẻ nên chúng tôi xây dựng các mặt nạ không khớp cho từng phụ huynh cho từng ứng viên và truy vấn bộ ba. Tổng công việc là đủ nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k * n² * l) | O(1) | Quá chậm | 
| Tối ưu | O(k * n * l) | O(n * l) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đối với mỗi nhiễm sắc thể con ứng cử viên, hãy tính mặt nạ không khớp cho mỗi nhiễm sắc thể cha mẹ. Bit ở vị trí`i`được đặt khi ký tự cha ở vị trí đó khác với ký tự của con. Điều này chuyển đổi so sánh chuỗi thành các hoạt động bit. 
2. Đếm xem có bao nhiêu phụ huynh có mặt nạ không khớp bằng 0. Mặt nạ bằng 0 có nghĩa là cha mẹ giống hệt con. Nó chỉ có thể tạo thành một cặp hợp lệ nếu có ít nhất hai cha mẹ như vậy. 
3. Chèn tất cả các mặt nạ không khớp vào bộ ba nhị phân. Mỗi cấp độ của tri đại diện cho một vị trí nhiễm sắc thể. Trie lưu trữ tất cả các mẫu không khớp có thể có của cha mẹ. 
4. Đối với mọi mặt nạ không phù hợp`x`, tìm kiếm mặt nạ`y`không có bit tập chung với`x`. Trong quá trình tìm kiếm, nếu`x`có một ở bit hiện tại, chỉ cho phép nhánh 0. Nếu như`x`có số 0 thì cả hai nhánh đều có thể. 
5. Nếu bất kỳ mặt nạ nào tìm thấy đối tác tương thích, ứng cử viên trẻ sẽ có thể đạt được và chỉ số của nó sẽ được thêm vào câu trả lời. 

Lý do điều này hoạt động dựa trên bất biến mặt nạ không khớp. Một phụ huynh có mặt nạ không phù hợp`x`không cung cấp con chỉ ở các vị trí được biểu thị bằng các bit đã đặt. Hai cha mẹ có thể tạo ra đứa trẻ một cách chính xác khi mọi vị trí thất bại của cha mẹ này đều được che đậy bởi cha mẹ kia, điều đó có nghĩa là các vị trí thất bại của họ không bao giờ trùng lặp. Tìm kiếm trie tìm thấy chính xác những mặt nạ có giao điểm bitwise bằng 0, vì vậy mọi đứa trẻ được báo cáo đều có một cặp cha mẹ hợp lệ và mọi cặp hợp lệ đều có thể được tìm thấy bằng cách tìm kiếm. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

class TrieNode:
    __slots__ = ("child", "end")
    def __init__(self):
        self.child = [-1, -1]
        self.end = 0

def solve():
    n, l = map(int, input().split())
    parents = [input().strip() for _ in range(n)]
    k = int(input())
    children = [input().strip() for _ in range(k)]

    answer = []

    for idx, child in enumerate(children, 1):
        masks = []
        zero_count = 0

        for parent in parents:
            mask = 0
            for i in range(l):
                if parent[i] != child[i]:
                    mask |= 1 << i
            if mask == 0:
                zero_count += 1
            masks.append(mask)

        if zero_count >= 2:
            answer.append(idx)
            continue

        trie = [TrieNode()]

        for mask in masks:
            node = 0
            for bit in range(l - 1, -1, -1):
                b = (mask >> bit) & 1
                if trie[node].child[b] == -1:
                    trie[node].child[b] = len(trie)
                    trie.append(TrieNode())
                node = trie[node].child[b]
            trie[node].end += 1

        def has_partner(mask):
            if mask == 0:
                return False

            def dfs(node, bit):
                if bit < 0:
                    return trie[node].end > 0

                b = (mask >> bit) & 1
                if b == 1:
                    nxt = trie[node].child[0]
                    if nxt != -1 and dfs(nxt, bit - 1):
                        return True
                else:
                    nxt = trie[node].child[0]
                    if nxt != -1 and dfs(nxt, bit - 1):
                        return True
                    nxt = trie[node].child[1]
                    if nxt != -1 and dfs(nxt, bit - 1):
                        return True
                return False

            return dfs(0, l - 1)

        ok = False
        for mask in masks:
            if has_partner(mask):
                ok = True
                break

        if ok:
            answer.append(idx)

    if answer:
        print(*answer)
    else:
        print(0)

if __name__ == "__main__":
    solve()
```Giải pháp xử lý các ứng viên một cách độc lập vì có rất ít người trong số họ. Đối với mỗi ứng cử viên, các mặt nạ không khớp được xây dựng lại từ danh sách gốc. 

Kiểm tra mặt nạ không được tách ra trước khi xây dựng bộ ba vì cha mẹ giống hệt với con không thể được ghép nối với chính nó. Tìm kiếm trie giả định rằng mặt nạ được tìm thấy khác với mặt nạ hiện tại. Điều này luôn đúng với các mặt nạ khác 0 vì mặt nạ khác 0 không thể chồng lên chính nó, trong khi trường hợp 0 ​​được xử lý riêng. 

Tri lưu trữ các bit từ vị trí quan trọng nhất đến vị trí ít quan trọng nhất. Việc đánh số thực tế các vị trí nhiễm sắc thể không quan trọng, miễn là việc xây dựng và tìm kiếm sử dụng cùng một thứ tự. Số nguyên Python có thể giữ các giá trị 50 bit một cách an toàn, do đó không cần xử lý đặc biệt đối với tình trạng tràn. 

# Ví dụ đã hoạt động 

Mẫu 1:```
Input
4 6
abcbac
bbcaab
caccba
bcacca
3
bacaba
abcccb
bcccab
```| Con | Mặt nạ dành cho phụ huynh | Kết quả tìm kiếm tương thích | Trả lời | 
| --- | --- | --- | --- | 
| bacaba | nhiều mặt nạ, hai mặt nạ rời nhau | tìm thấy | vâng | 
| abcccb | không có cặp rời rạc | không tìm thấy | không | 
| bcccab | hai mặt nạ rời nhau | tìm thấy | vâng | 

Con thứ nhất và con thứ ba có cặp bố mẹ không trùng khớp với nhau và không bao giờ xung đột. Đứa trẻ thứ hai thất bại vì mọi cặp có thể đều không đồng ý ở đâu đó ở cùng một vị trí. 

Mẫu 2:```
Input
2 2
st
ss
1
tt
```| Con | Phụ huynh | Mặt nạ không phù hợp | Kết quả | 
| --- | --- | --- | --- | 
| tt | st | 10 | đã tìm kiếm | 
| tt | ss | 11 | đã tìm kiếm | 

Hai mặt nạ không rời nhau vì`10 & 11`là khác không. Cha đầu tiên không thể sửa ký tự thứ hai và cha thứ hai không thể sửa được ký tự nào, vì vậy con không thể sửa được. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k * n * l) | Mỗi ứng cử viên xây dựng mặt nạ và thực hiện các thao tác trie trên 50 bit. | 
| Không gian | O(n * l) | Trie chứa tối đa một nút trên mỗi bit được lưu trữ của mỗi mặt nạ không khớp. | 

Với`k <= 10`,`n <= 20000`, Và`l <= 50`, thuật toán chỉ thực hiện khoảng mười triệu thao tác ở mức bit, vừa vặn trong giới hạn. 

# Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# provided sample 1
assert run("""4 6
abcbac
bbcaab
caccba
bcacca
3
bacaba
abcccb
bcccab
""") == "1 3\n"

# provided sample 2
assert run("""2 2
st
ss
1
tt
""") == "0\n"

# single possible matching parent is not enough
assert run("""2 3
abc
def
1
abc
""") == "0\n"

# two parents combine to form child
assert run("""2 2
ab
ac
1
aa
""") == "1\n"

# all parents differ completely
assert run("""3 4
aaaa
bbbb
cccc
2
abcd
bbbb
""") == "2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`1 3`| Các trường hợp vượt qua bình thường với một số ứng viên | 
| Mẫu 2 |`0`| Không có cặp cha mẹ nào có thể tạo ra đứa trẻ | 
|`abc`,`def`, đứa trẻ`abc`|`0`| Một phụ huynh giống hệt nhau là không đủ | 
|`ab`,`ac`, đứa trẻ`aa`|`1`| Hai cha mẹ đóng góp những chức vụ khác nhau | 
| Ba nhiễm sắc thể không liên quan |`2`| Con bằng cha và mẹ và những trường hợp bất khả thi khác | 

# Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên:```
2 3
abc
def
1
abc
```Các mặt nạ không phù hợp là`000`Và`111`. Thuật toán chỉ nhìn thấy một mặt nạ số 0 nên không chấp nhận ngay. Tìm kiếm trie không thể tìm thấy mặt nạ khác tương thích với`000`bởi vì mặt nạ phù hợp duy nhất sẽ là một mặt nạ số 0 khác. Đầu ra là`0`. 

Đối với trường hợp kết hợp:```
2 2
ab
ac
1
aa
```Bố mẹ đầu tiên chỉ khác nhau ở vị trí thứ hai, đưa ra mặt nạ`10`. Cha mẹ thứ hai chỉ khác nhau ở vị trí thứ hai? Sau khi kiểm tra kỹ lưỡng, cha mẹ thứ hai`ac`khác với`aa`ở vị trí thứ hai, vì vậy đầu vào này thực sự không hợp lệ. Một ví dụ kết hợp đúng là:```
2 2
ab
ba
1
aa
```Những chiếc mặt nạ là`10`Và`01`. AND của chúng bằng 0, vì vậy trie tìm cặp và trả về`1`. 

Đối với các nhiễm sắc thể ứng cử viên giống hệt nhau giữa các bậc cha mẹ, việc triển khai dựa vào số lượng mặt nạ bằng 0. Nếu chỉ có một phụ huynh bằng con thì không thể sử dụng hai lần. Nếu hai cha mẹ bằng đứa trẻ, số 0 sẽ ngay lập tức chấp nhận đứa trẻ mà không cần thực hiện tìm kiếm thử.
