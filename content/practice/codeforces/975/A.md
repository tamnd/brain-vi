---
title: "CF 975A - Chữ Ả Rập"
description: "Mỗi từ trong đầu vào nhằm mô tả một đối tượng, nhưng ngôn ngữ có quy tắc chuẩn hóa: hai từ biểu thị cùng một đối tượng nếu chúng chứa chính xác cùng một bộ chữ cái riêng biệt, bỏ qua số lần mỗi chữ cái xuất hiện và bỏ qua thứ tự."
date: "2026-06-17T01:33:04+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 975
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 478 (Div. 2)"
rating: 900
weight: 975
solve_time_s: 71
verified: true
draft: false
---

[CF 975A - Chữ viết Ả Rập](https://codeforces.com/problemset/problem/975/A) 

**Xếp hạng:** 900 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi từ trong đầu vào nhằm mô tả một đối tượng, nhưng ngôn ngữ có quy tắc chuẩn hóa: hai từ biểu thị cùng một đối tượng nếu chúng chứa chính xác cùng một bộ chữ cái riêng biệt, bỏ qua số lần mỗi chữ cái xuất hiện và bỏ qua thứ tự. 

Vì vậy, thay vì coi các từ là chuỗi, chúng tôi thực sự nhóm chúng theo bộ ký tự duy nhất. Ví dụ: “a”, “aa” và “aaaa” đều có cùng cách biểu diễn vì chúng chỉ chứa chữ cái “a”. Tương tự, “ab”, “baba” và “aabb” đều quy về cùng một tân ngữ vì các chữ cái riêng biệt của chúng là`{a, b}`. 

Nhiệm vụ là đếm xem có bao nhiêu bộ chữ cái như vậy xuất hiện trong số tất cả các từ đã cho. 

Các ràng buộc đủ nhỏ để chúng ta có thể đủ khả năng xử lý từng từ một cách độc lập và xây dựng cách biểu diễn trực tiếp tập hợp chữ cái của nó. Có tối đa 1000 từ và mỗi từ có độ dài tối đa 1000, do đó, việc quét đơn giản trên tất cả các ký tự sẽ dẫn đến tối đa 10^6 thao tác ký tự, tốc độ này rất nhanh trong Python. 

Một vấn đề tế nhị sẽ nảy sinh nếu chúng ta cố gắng sử dụng từ thô làm chìa khóa. Hai từ như “ab” và “baab” là các chuỗi khác nhau nhưng đại diện cho cùng một đối tượng, do đó việc xử lý trực tiếp các chuỗi sẽ bị tính quá mức. Một sai lầm tiềm ẩn khác là tính các hoán vị là khác biệt; ví dụ: “abc”, “cba” và “bca” đều là các đối tượng giống hệt nhau theo quy tắc. 

Trường hợp cạnh chính là sự lặp lại nhiều chữ cái. Một từ như “aaaaaaa” phải thu gọn thành một đại diện duy nhất và nếu bỏ qua bước chuẩn hóa này, câu trả lời sẽ bị thổi phồng. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ so sánh mọi từ với mọi từ khác, kiểm tra xem chúng có định nghĩa cùng một đối tượng hay không. Để thực hiện việc so sánh này, chúng tôi sẽ tính toán bộ ký tự cho mỗi cặp và so sánh chúng. Nếu có n từ, mỗi so sánh có giá lên tới O(26) hoặc O(độ dài) và việc thực hiện điều này cho tất cả các cặp sẽ dẫn đến O(n² · k), điều này là không cần thiết nhưng vẫn nằm ở ranh giới có thể chấp nhận được ở giới hạn nhỏ. 

Quan sát chính là sự bình đẳng của các đối tượng là sự tương đương trong “bộ ký tự”. Điều này gợi ý việc xây dựng một dạng chuẩn cho mỗi từ và chèn nó vào bộ băm. Dạng chuẩn có thể được xây dựng bằng cách thu thập tất cả các chữ cái riêng biệt và mã hóa chúng thành một chuỗi được sắp xếp hoặc một mặt nạ bit. Vì các chữ cái tiếng Anh viết thường có kích thước cố định (26), nên dễ dàng xây dựng một biểu diễn nhỏ gọn và không xung đột. 

Điều này làm giảm vấn đề phải tính toán một chữ ký cho mỗi từ và đếm xem có bao nhiêu chữ ký duy nhất tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force So sánh theo cặp | O(n² · k) | O(n · k) | Quá chậm ở dạng chung | 
| Bộ Canonical + Băm | O(n · k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các từ từ đầu vào. Mỗi từ sẽ được xử lý độc lập vì nhận dạng đối tượng chỉ phụ thuộc vào các ký tự của chính nó. 
2. Đối với mỗi từ, hãy xây dựng một biểu diễn tập hợp các ký tự mà nó chứa. Một phương pháp đơn giản và đáng tin cậy là đánh dấu chữ nào trong số 26 chữ cái viết thường xuất hiện trong từ đó. Bước này nén từ thành một cấu trúc bỏ qua tần suất và thứ tự. 
3. Chuyển đổi cấu trúc này thành dạng có thể băm. Một cách tiếp cận thực tế là xây dựng một chuỗi bằng cách nối tất cả các chữ cái xuất hiện theo thứ tự bảng chữ cái. Điều này đảm bảo rằng bất kỳ hai từ nào có cùng bộ sẽ tạo ra chính xác cùng một chuỗi. 
4. Chèn biểu diễn chuẩn này vào một tập hợp. Bộ này tự động loại bỏ các bản sao, do đó, các lần xuất hiện lặp lại của cùng một đối tượng sẽ được hợp nhất một cách tự nhiên. 
5. Sau khi xử lý tất cả các từ, xuất ra kích thước của tập hợp tương ứng với số lượng đối tượng riêng biệt. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là mỗi từ được ánh xạ tới một dạng chuẩn duy nhất được xác định chỉ bởi tập hợp các ký tự của nó. Hai từ ánh xạ tới cùng một dạng chuẩn khi và chỉ khi chúng chứa các chữ cái giống hệt nhau. Vì bộ này chỉ lưu trữ các dạng chính tắc nên nó nhóm tất cả các từ tương đương lại với nhau và đếm từng lớp tương đương chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    words = input().split()

    seen = set()

    for w in words:
        mask = [0] * 26
        for ch in w:
            mask[ord(ch) - 97] = 1

        key = []
        for i in range(26):
            if mask[i]:
                key.append(chr(i + 97))

        seen.add("".join(key))

    print(len(seen))

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các từ trong một lượt, sau đó chuyển đổi từng từ thành một mảng hiện diện có độ dài 26. Điều này tránh mọi sự phụ thuộc vào độ dài từ hoặc tần số ký tự. Bước xây dựng lại bảng chữ cái đảm bảo rằng các hoán vị như “ab” và “ba” thu gọn về cùng một khóa. 

Một cạm bẫy triển khai phổ biến là quên chuẩn hóa từ và chèn chuỗi thô vào tập hợp. Điều đó sẽ coi đảo chữ là khác biệt một cách không chính xác. Một lỗi khác là sử dụng danh sách trực tiếp trong một tập hợp, điều này không hợp lệ trong Python vì danh sách không thể băm được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
a aa aaa ab abb
```Chúng tôi theo dõi dạng chuẩn của từng từ. 

| Lời | Bộ ký tự | Khóa Canonical | Đã Xem Bộ | 
| --- | --- | --- | --- | 
| một | {a} | một | {a} | 
| aa | {a} | một | {a} | 
| aaa | {a} | một | {a} | 
| ab | {a,b} | ab | {a, ab} | 
| abb | {a,b} | ab | {a, ab} | 

Câu trả lời cuối cùng là 2. 

Điều này xác nhận rằng các chữ cái và hoán vị lặp lại không ảnh hưởng đến việc nhóm. 

### Ví dụ 2 

đầu vào:```
4
abc bca aabb cc
```| Lời | Bộ ký tự | Khóa Canonical | Đã Xem Bộ | 
| --- | --- | --- | --- | 
| abc | {a,b,c} | abc | {abc} | 
| bca | {a,b,c} | abc | {abc} | 
| aabb | {a,b} | ab | {abc, ab} | 
| cc | {c} | c | {abc, ab, c} | 

Câu trả lời cuối cùng là 3. 

Điều này cho thấy các tập hợp con khác nhau của các chữ cái tạo thành các đối tượng riêng biệt như thế nào ngay cả khi các từ là hoán vị hoặc có sự lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · k) | Mỗi từ được quét một lần và mỗi ký tự được xử lý theo thời gian không đổi | 
| Không gian | O(n) | Mỗi khóa chính tắc riêng biệt được lưu trữ trong một bộ | 

Với n ≤ 1000 và tổng số ký tự ≤ 10^6, giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. Bảng chữ cái 26 chữ cái còn đảm bảo chi phí hệ số không đổi cho quá trình chuẩn hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    words = input().split()

    seen = set()

    for w in words:
        mask = [0] * 26
        for ch in w:
            mask[ord(ch) - 97] = 1

        key = []
        for i in range(26):
            if mask[i]:
                key.append(chr(i + 97))

        seen.add("".join(key))

    return str(len(seen))

# provided sample
assert run("5\na aa aaa ab abb") == "2"

# all identical
assert run("3\na aa aaa") == "1"

# all distinct single letters
assert run("3\na b c") == "3"

# permutations
assert run("3\nab ba ab") == "1"

# mixed case
assert run("4\nabc bca aabb cc") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| à aaa | 1 | sự lặp lại sụp đổ | 
| a b c | 3 | đối tượng một chữ cái độc lập | 
| ab ba ab | 1 | tương đương hoán vị | 
| abc bca aabb cc | 3 | tập hợp con hỗn hợp và nhóm | 

## Vỏ cạnh 

Một trường hợp quan trọng là sự lặp lại cực độ bên trong một từ. Đối với đầu vào như “aaaaa”, thuật toán chỉ đánh dấu một chữ cái trong mặt nạ, do đó khóa chuẩn sẽ trở thành “a”. Điều này đảm bảo các ký tự lặp lại không làm tăng sự biểu diễn. 

Một trường hợp khác là nhiều đảo chữ cái. Ví dụ: “abc”, “bca” và “cab” đều tạo ra cùng một mặt nạ và do đó có cùng một khóa. Tập hợp này sẽ thu gọn chúng thành một mục nhập một cách chính xác vì mọi hoán vị đều ánh xạ tới biểu diễn chữ cái được sắp xếp giống hệt nhau. 

Trường hợp cạnh cuối cùng là khi tất cả các từ giống nhau về cấu trúc nhưng khác nhau về độ dài, chẳng hạn như “a”, “aa”, “aaa”, “aaaa”. Mỗi cái tạo ra cùng một khóa chính tắc “a”, vì vậy chỉ còn một mục nhập trong tập hợp, tạo ra số đếm chính xác là 1.
