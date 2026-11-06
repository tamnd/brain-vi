---
problem: 1263D
contest_id: 1263
problem_index: D
name: "Secret Passwords"
contest_name: "Codeforces Round 603 (Div. 2)"
rating: 1500
tags: ["dfs and similar", "dsu", "graphs"]
answer: passed_samples
verified: true
solve_time_s: 117
date: 2026-06-13
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2d8071-ffa0-83ec-857d-abcaf5fc2df4
---

#CF 1263D - Mật khẩu bí mật 

**Đánh giá:** 1500 
**Tags:** dfs và tương tự, dsu, đồ thị 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2d8071-ffa0-83ec-857d-abcaf5fc2df4 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các chuỗi, trong đó mỗi chuỗi đại diện cho một mật khẩu. Hệ thống không coi những mật khẩu này là hoàn toàn độc lập. Thay vào đó, nó định nghĩa khái niệm “tương đương” giữa các mật khẩu: hai mật khẩu tương đương nhau nếu chúng có chung ít nhất một ký tự hoặc nếu chúng có thể được kết nối thông qua một chuỗi các mật khẩu khác trong đó mỗi cặp liền kề có chung một ký tự. 

Điều này tự nhiên biến vấn đề thành lý luận về khả năng kết nối. Mỗi mật khẩu có thể được coi là một nút trong biểu đồ. Một cạnh tồn tại giữa hai nút nếu các chuỗi tương ứng có chung ít nhất một chữ cái. Sự tương đương khi đó chính xác là phát biểu rằng hai nút nằm trong cùng một thành phần được kết nối của biểu đồ này. 

Chỉ có một mật khẩu thực sự là mật khẩu quản trị viên chính xác, nhưng không biết mật khẩu nào. Nếu chúng tôi chọn một bộ mật khẩu để thử thì việc thử bất kỳ mật khẩu nào sẽ tự động bao gồm tất cả các mật khẩu trong lớp tương đương của nó, vì tất cả chúng đều dẫn đến cùng một kết quả truy cập. Nhiệm vụ là chọn số lượng mật khẩu nhỏ nhất sao cho bất kể thành phần được kết nối nào có chứa mật khẩu quản trị viên thì ít nhất một trong các mật khẩu đã chọn nằm trong thành phần đó. 

Nói lại cụ thể hơn, chúng ta cần chọn ít nhất một đại diện từ mọi thành phần được kết nối được hình thành bằng cách liên kết các mật khẩu có chung các chữ cái. 

Các ràng buộc rất lớn: lên tới 200.000 chuỗi và tổng chiều dài lên tới 1.000.000. Điều này ngay lập tức loại trừ mọi phương pháp so sánh theo cặp. Việc xây dựng biểu đồ đơn giản kiểm tra từng cặp chuỗi sẽ là bậc hai về số lượng mật khẩu, tốc độ này quá chậm. Ngay cả việc so sánh tất cả các chuỗi với nhau cũng sẽ vượt quá giới hạn thời gian. 

Bảng chữ cái chỉ có 26 chữ cái viết thường, đây là hạn chế về cấu trúc chính. Mỗi mật khẩu chỉ có thể kết nối thông qua 26 “cổng” có thể có này, điều này gợi ý rõ ràng về cấu trúc kết hợp trên các chữ cái hoặc trên các thành phần được kết nối do các chữ cái tạo ra. 

Một trường hợp khó phát hiện khi có sự trùng lặp. Các chuỗi giống hệt nhau tương đương nhau một cách tầm thường, nhưng chúng cũng không nên được coi là các thành phần riêng biệt. Một trường hợp đặc biệt khác là khi mật khẩu chứa nhiều chữ cái riêng biệt, vì nó có thể kết nối nhiều nhóm chữ cái cùng một lúc, hợp nhất các thành phần riêng biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xây dựng một biểu đồ rõ ràng giữa các mật khẩu. Chúng tôi so sánh từng cặp chuỗi và kiểm tra xem chúng có chung một ký tự hay không. Nếu có, chúng tôi hợp nhất hoặc kết nối chúng. Việc kiểm tra một cặp cần tới 50 lần so sánh ký tự và có tới$n^2$cặp, dẫn đến khoảng$10^{10}$hoạt động trong trường hợp xấu nhất. Điều này vượt xa mọi giới hạn khả thi. 

Quan sát chính là việc so sánh trực tiếp mật khẩu với mật khẩu là không cần thiết. Lý do duy nhất khiến hai mật khẩu được kết nối với nhau là sự tồn tại của một lá thư chung. Điều này có nghĩa là các chữ cái đóng vai trò là biểu tượng trung gian thể hiện đầy đủ khả năng kết nối. 

Thay vì nghĩ đến các cạnh giữa các chuỗi, chúng ta lật cấu trúc đồ thị. Chúng tôi coi mỗi chữ cái là một nút và kết nối mật khẩu với tất cả các chữ cái chứa trong đó. Khi đó, hai mật khẩu sẽ tương đương nhau một cách chính xác khi bộ ký tự của chúng trùng nhau qua các nút ký tự này. Điều này cho phép chúng ta sử dụng cấu trúc tập hợp rời rạc (DSU) trên 26 chữ cái. 

Mỗi mật khẩu “nối” tất cả các ký tự của nó thành một cấu trúc được kết nối một cách hiệu quả. Sau khi xử lý tất cả mật khẩu, các chữ cái sẽ tạo thành một số thành phần được kết nối. Bất kỳ mật khẩu nào chứa ít nhất một chữ cái của một thành phần đều thuộc về thành phần đó. Do đó, số lượng thành phần chữ cái riêng biệt được mật khẩu chạm vào tương ứng với số lượng nhóm mật khẩu được kết nối. 

Cuối cùng, vì mỗi nhóm cần ít nhất một mật khẩu đại diện nên câu trả lời là số lượng các thành phần kết nối riêng biệt được tạo ra bởi các chữ cái xuất hiện trong ít nhất một mật khẩu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết nối theo cặp Brute Force | O(n² · 26) | O(n²) | Quá chậm | 
| DSU qua các chữ cái | O(tổng chiều dài α(26)) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một DSU trên 26 nút, một nút cho mỗi chữ cái viết thường. 

1. Khởi tạo một DSU trong đó mỗi chữ cái là chữ cái chính của nó. Điều này thể hiện rằng ban đầu không có chữ cái nào được kết nối. 
2. Đối với mỗi mật khẩu, hãy trích xuất tập hợp các ký tự riêng biệt xuất hiện trong đó. Chúng ta chỉ quan tâm đến tính duy nhất bên trong một chuỗi vì các chữ cái lặp lại không làm thay đổi khả năng kết nối. 
3. Đối với mỗi mật khẩu, hãy lấy ký tự đầu tiên của nó làm tham chiếu và kết hợp nó với mọi ký tự khác trong cùng một mật khẩu. Bước này hợp nhất tất cả các chữ cái trong mật khẩu thành một thành phần được kết nối duy nhất, phản ánh rằng mật khẩu này liên kết chúng. 
4. Lặp lại điều này cho tất cả các mật khẩu, dần dần hợp nhất các nhóm chữ cái khi có nhiều kết nối hơn. 
5. Sau khi xử lý tất cả mật khẩu, chúng tôi quét tất cả các chữ cái xuất hiện trong ít nhất một mật khẩu và thu thập chữ cái đại diện cho mỗi chữ cái. 
6. Số lượng đại diện riêng biệt giữa các chữ cái này là câu trả lời, vì mỗi chữ cái đại diện cho một thành phần tương đương được kết nối giữa các mật khẩu. 

### Tại sao nó hoạt động 

Mỗi mật khẩu buộc tất cả các ký tự của nó thuộc về một lớp tương đương duy nhất. DSU qua các chữ cái duy trì việc đóng chuyển tiếp các ràng buộc này. Nếu hai mật khẩu có chung một ký tự, cả hai sẽ kết hợp ký tự đó thành các bộ tương ứng, điều này đảm bảo các bộ chữ cái của chúng giao nhau trong cấu trúc DSU. Giao điểm này đảm bảo rằng tất cả mật khẩu được kết nối thông qua các chữ cái dùng chung sẽ được gộp vào cùng một thành phần DSU. Do đó, mỗi thành phần DSU tương ứng chính xác với một loại mật khẩu tương đương và việc chọn một đại diện cho mỗi thành phần là đủ và tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.r = [0] * n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return
        if self.r[ra] < self.r[rb]:
            ra, rb = rb, ra
        self.p[rb] = ra
        if self.r[ra] == self.r[rb]:
            self.r[ra] += 1

def solve():
    n = int(input())
    dsu = DSU(26)

    used = set()

    for _ in range(n):
        s = input().strip()
        letters = list(set(s))

        for ch in letters:
            used.add(ord(ch) - 97)

        base = ord(letters[0]) - 97
        for ch in letters[1:]:
            dsu.union(base, ord(ch) - 97)

    comps = set()
    for c in used:
        comps.add(dsu.find(c))

    print(len(comps))

if __name__ == "__main__":
    solve()
```DSU được xây dựng trên kích thước bảng chữ cái cố định là 26. Mỗi chuỗi góp phần kết hợp giữa các chữ cái của nó, đảm bảo tất cả các chữ cái cùng xuất hiện sẽ được kết nối với nhau. các`used`đặt dấu vết những chữ cái nào thực sự xuất hiện trong đầu vào, vì những chữ cái không được sử dụng sẽ không đóng góp vào câu trả lời. 

Một điểm tinh tế là loại bỏ các ký tự trùng lặp bên trong mỗi chuỗi trước các thao tác kết hợp. Nếu không có điều này, các chữ cái lặp đi lặp lại có thể gây ra sự kết hợp dư thừa, nhưng không gây ra vấn đề về tính chính xác; Tuy nhiên, tính năng loại bỏ trùng lặp giúp đơn giản hóa việc lập luận và giữ cho các hoạt động ở mức tối thiểu. 

Cuối cùng, việc chỉ tính các gốc DSU riêng biệt trên các chữ cái được sử dụng sẽ đảm bảo chúng tôi chỉ tính các thành phần có liên quan đến mật khẩu thực tế. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
a
b
ab
d
```Chúng tôi theo dõi việc hợp nhất DSU: 

| Mật khẩu | Thư | Hành động của Liên minh | Thành phần DSU (khái niệm) | 
| --- | --- | --- | --- | 
| một | {a} | không | {a}, {b}, {c}, ... | 
| b | {b} | không | {a}, {b}, ... | 
| ab | {a,b} | a-b | {a,b}, ... | 
| d | {d} | không | {a,b}, {d}, ... | 

Sau khi xử lý, chúng ta có hai thành phần được kết nối: một thành phần chứa {a,b} và một thành phần chứa {d}. Câu trả lời là 2, nghĩa là chúng ta cần ít nhất một lần đoán từ mỗi thành phần. 

### Ví dụ 2 

đầu vào:```
3
abc
cde
f
```| Mật khẩu | Thư | Hành động của Liên minh | Thành phần DSU (khái niệm) | 
| --- | --- | --- | --- | 
| abc | {a,b,c} | a-b, a-c | {a,b,c} | 
| cd | {c,d,e} | c-d, c-e | {a,b,c,d,e} | 
| f | {f} | không | riêng biệt | 

Sau khi xử lý, {a,b,c,d,e} là một thành phần và {f} là một thành phần khác nên đáp án là 2. Thư chung`c`là thứ hợp nhất hai mật khẩu đầu tiên thành một lớp tương đương duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng_chiều dài · α(26)) | Mỗi nhân vật tham gia vào một số ít liên minh DSU trên một bảng chữ cái không đổi | 
| Không gian | O(26) | DSU và mảng phụ trợ trên bảng chữ cái cố định | 

Tổng chiều dài giới hạn đảm bảo tối đa một triệu ký tự, nhưng mỗi thao tác thực sự có thời gian không đổi do kích thước bảng chữ cái cố định, giúp giải pháp đủ nhanh một cách dễ dàng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    class DSU:
        def __init__(self, n):
            self.p = list(range(n))
            self.r = [0] * n
        def find(self, x):
            while self.p[x] != x:
                self.p[x] = self.p[self.p[x]]
                x = self.p[x]
            return x
        def union(self, a, b):
            ra, rb = self.find(a), self.find(b)
            if ra == rb:
                return
            if self.r[ra] < self.r[rb]:
                ra, rb = rb, ra
            self.p[rb] = ra
            if self.r[ra] == self.r[rb]:
                self.r[ra] += 1

    n = int(sys.stdin.readline())
    dsu = DSU(26)
    used = set()

    for _ in range(n):
        s = sys.stdin.readline().strip()
        letters = list(set(s))
        for ch in letters:
            used.add(ord(ch) - 97)
        base = ord(letters[0]) - 97
        for ch in letters[1:]:
            dsu.union(base, ord(ch) - 97)

    comps = set(dsu.find(c) for c in used)
    return str(len(comps))

# provided sample
assert run("""4
a
b
ab
d
""") == "2"

# single string
assert run("""1
abc
""") == "1"

# all disjoint letters
assert run("""3
a
b
c
""") == "3"

# full chain connection
assert run("""3
ab
bc
cd
""") == "1"

# duplicates
assert run("""4
a
a
a
a
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi đơn | 1 | trường hợp tối thiểu | 
| a b c | 3 | không có kết nối | 
| ab bc cd | 1 | sáp nhập bắc cầu | 
| tất cả một | 1 | xử lý trùng lặp | 

## Vỏ cạnh 

Khi tất cả mật khẩu đều là các chuỗi ký tự đơn giống hệt nhau, chẳng hạn như`"a", "a", "a"`, thuật toán xử lý chính xác chúng như một thành phần một chữ cái. DSU không bao giờ giới thiệu cấu trúc mới ngoài nút đơn, do đó kết quả vẫn là 1. 

Khi mật khẩu tạo thành một chuỗi như`"ab", "bc", "cd"`, sự hợp nhất đầu tiên`a-b`, sự hợp nhất thứ hai`b-c`, và lần hợp nhất thứ ba`c-d`. DSU đảm bảo tất cả các chữ cái được gộp lại thành một đại diện duy nhất, tạo ra một lớp tương đương. 

Khi không có mật khẩu chia sẻ bất kỳ chữ cái nào, chẳng hạn như`"a", "b", "c"`, không có sự kết hợp nào được thực hiện nên mỗi chữ cái vẫn là thành phần riêng của nó. Việc đếm các gốc riêng biệt sẽ mang lại ba, phù hợp với thực tế là không có sự tương đương nào tồn tại giữa bất kỳ mật khẩu nào.
