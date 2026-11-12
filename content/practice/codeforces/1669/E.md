---
title: "CF 1669E - Dây 2 Chữ"
description: "Chúng ta được cung cấp một tập hợp các chuỗi ngắn, mỗi chuỗi bao gồm chính xác hai chữ cái viết thường trong một bảng chữ cái nhỏ. Nhiệm vụ là đếm xem có bao nhiêu cặp chỉ số không có thứ tự tương ứng với các chuỗi “gần như giống hệt nhau” theo nghĩa là chúng khác nhau ở đúng một vị trí."
date: "2026-06-10T01:56:34+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "math", "strings"]
categories: ["algorithms"]
codeforces_contest: 1669
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 784 (Div. 4)"
rating: 1200
weight: 1669
solve_time_s: 93
verified: true
draft: false
---

[CF 1669E - Chuỗi 2 chữ cái](https://codeforces.com/problemset/problem/1669/E) 

**Đánh giá:** 1200 
**Tags:** cấu trúc dữ liệu, toán học, chuỗi 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các chuỗi ngắn, mỗi chuỗi bao gồm chính xác hai chữ cái viết thường trong một bảng chữ cái nhỏ. Nhiệm vụ là đếm xem có bao nhiêu cặp chỉ số không có thứ tự tương ứng với các chuỗi “gần như giống hệt nhau” theo nghĩa là chúng khác nhau ở đúng một vị trí. 

Hai chuỗi góp phần đưa ra câu trả lời nếu một trong hai ký tự của chúng khớp nhau trong khi vị trí còn lại khác nhau. Ví dụ,`"ab"`Và`"ac"`chỉ khác nhau ở vị trí thứ hai nên chúng hợp lệ. Nhưng`"ab"`Và`"cd"`khác nhau ở cả hai vị trí, vì vậy chúng không phải như vậy. Tương tự như vậy, các chuỗi giống hệt nhau như`"aa"`Và`"aa"`cũng không hợp lệ vì chúng khác nhau ở vị trí 0. 

Kích thước đầu vào có thể lớn, tổng cộng lên tới 100000 chuỗi trong các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ phương pháp so sánh cặp bậc hai nào, vì việc so sánh mỗi cặp sẽ yêu cầu thứ tự các phép toán 10¹⁰ trong trường hợp xấu nhất. Thay vào đó, giải pháp phải dựa vào sự tổng hợp theo cấu trúc, sử dụng tính năng đếm tần số hoặc hàm băm để giảm vấn đề về thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Một trường hợp thất bại tinh tế đối với cách suy luận ngây thơ là tính hai lần hoặc thiếu tính đối xứng. Ví dụ: nếu chúng ta cố gắng so khớp các chuỗi bằng cách thay đổi một ký tự và đếm các kết quả trùng khớp một cách độc lập cho mỗi vị trí, chúng ta có thể vô tình đếm các cặp hai lần, một lần từ mỗi điểm cuối. Một vấn đề khác nảy sinh nếu chúng ta cố gắng “cố định vị trí này và nhóm vị trí kia” mà không tách biệt cẩn thận, dẫn đến các trường hợp trộn lẫn trong đó cả hai vị trí đều khác nhau. 

## Phương pháp tiếp cận 

Giải pháp brute-force kiểm tra từng cặp chuỗi và so sánh chúng theo từng ký tự. Vì mỗi chuỗi có độ dài bằng 2 nên mỗi lần so sánh đều có thời gian không đổi và chúng tôi trực tiếp kiểm tra xem có chính xác một vị trí khác nhau hay không. Điều này đúng vì nó thực thi điều kiện một cách rõ ràng. Tuy nhiên, với n lên tới 100000 cho mỗi trường hợp thử nghiệm, điều này dẫn đến so sánh khoảng n²/2, vượt xa giới hạn khả thi. 

Quan sát quan trọng là để hai chuỗi khác nhau ở đúng một vị trí, chúng phải giống nhau ở một vị trí và khác nhau ở vị trí kia. Điều này gợi ý việc chia điều kiện thành hai mẫu độc lập. Ký tự đầu tiên khớp và ký tự thứ hai khác nhau, hoặc ký tự thứ hai khớp và ký tự đầu tiên khác nhau. 

Cấu trúc này cho phép chúng ta đếm các cặp hợp lệ bằng cách sử dụng bản đồ tần số. Nếu sửa ký tự đầu tiên thì trong số tất cả các chuỗi có chung ký tự đầu tiên đó, chúng ta chỉ cần đếm xem có bao nhiêu chuỗi có ký tự thứ hai khác nhau. Tương tự để sửa ký tự thứ hai. Khó khăn là tránh tính hai lần: mỗi cặp hợp lệ được tính chính xác một lần ở một trong hai chế độ xem nếu chúng ta cẩn thận trừ đi các cặp giống hệt nhau. 

Chúng tôi xử lý vấn đề này một cách rõ ràng bằng cách nhóm các chuỗi và sử dụng số đếm tần số trên các nhận dạng chuỗi đầy đủ và các phép chiếu một phần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Nhóm tần số | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi chuỗi là một cặp ký tự`(a, b)`và xây dựng bảng tần số trên các cặp này. 

1. Đối với mỗi trường hợp kiểm thử, hãy đọc tất cả các chuỗi và đếm xem mỗi chuỗi xuất hiện chính xác bao nhiêu lần. Điều này cho phép chúng tôi truy cập trực tiếp vào số lượng cặp giống hệt nhau tồn tại. 
2. Xây dựng bảng tần số chỉ được khóa bằng ký tự đầu tiên, đếm số chuỗi bắt đầu bằng mỗi chữ cái. Điều này cho phép suy luận về các cặp có chung vị trí đầu tiên. 
3. Tương tự, xây dựng bảng tần số được khóa bằng ký tự thứ hai, đếm xem mỗi chữ cái kết thúc bằng bao nhiêu chuỗi. 
4. Đối với mỗi nhóm chuỗi có chung ký tự đầu tiên, hãy tính xem có bao nhiêu cặp chéo tồn tại với các ký tự thứ hai khác nhau một cách gián tiếp thông qua sự kết hợp tần số của ký tự thứ hai. 
5. Thực hiện tính toán đối xứng cho các nhóm có cùng ký tự thứ hai. 
6. Trừ các đóng góp tương ứng với các chuỗi giống hệt nhau để đảm bảo loại trừ các cặp khác nhau ở vị trí 0 và các cặp khác nhau ở cả hai vị trí không được đưa vào không chính xác. 
7. Tích lũy kết quả của tất cả các nhóm. 

Câu trả lời cuối cùng đến từ việc kết hợp các đóng góp “cùng ký tự thứ nhất, khác nhau thứ hai” và các đóng góp “cùng ký tự thứ hai, khác nhau thứ nhất” theo cách tránh trùng lặp với các chuỗi giống hệt nhau. 

### Tại sao nó hoạt động 

Mỗi cặp chuỗi hợp lệ phải khớp chính xác ở một tọa độ. Điều đó có nghĩa là mỗi cặp hợp lệ được phân loại duy nhất thành một trong hai loại riêng biệt: các cặp có chung ký tự đầu tiên hoặc các cặp có chung ký tự thứ hai, nhưng không phải cả hai. Việc đếm dựa trên tần số liệt kê tất cả các cặp trong mỗi danh mục bằng cách sử dụng tổ hợp trên số lượng được nhóm và loại trừ các chuỗi giống hệt nhau đảm bảo không có cặp không hợp lệ nào được đưa ra. Bởi vì mỗi cặp hợp lệ có chính xác một tọa độ trùng khớp nên nó được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        freq = {}
        f1 = {}
        f2 = {}

        arr = []
        for _ in range(n):
            s = input().strip()
            arr.append(s)
            freq[s] = freq.get(s, 0) + 1
            f1[s[0]] = f1.get(s[0], 0) + 1
            f2[s[1]] = f2.get(s[1], 0) + 1

        ans = 0

        for s, c in freq.items():
            a, b = s
            # pairs with same first char but different second
            ans += c * (f1[a] - c)
            # pairs with same second char but different first
            ans += c * (f2[b] - c)

        print(ans // 2)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì ba bản đồ tần số. Một theo dõi số lượng chuỗi đầy đủ, trong khi hai theo dõi các hình chiếu còn lại lên từng vị trí ký tự. Đối với mỗi chuỗi riêng biệt, chúng tôi tính toán xem nó tạo thành bao nhiêu cặp với các chuỗi có chung ký tự đầu tiên nhưng khác nhau ở ký tự thứ hai và tương tự đối với ký tự thứ hai. Mỗi cặp được tính hai lần, một lần từ mỗi điểm cuối, đó là lý do tại sao kết quả cuối cùng được chia cho hai. 

Chi tiết triển khai chính là sử dụng phép trừ`f1[a] - c`Và`f2[b] - c`, loại bỏ các chuỗi giống hệt nhau khỏi việc xem xét để chỉ các chuỗi đối tác khác nhau mới đóng góp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
ab
ac
db
```| chuỗi | tần số | cùng đóng góp đầu tiên | cùng đóng góp thứ hai | 
| --- | --- | --- | --- | 
| ab | 1 | 1*(2-1)=1 | 1*(1-1)=0 | 
| ac | 1 | 1*(2-1)=1 | 1*(1-1)=0 | 
| db | 1 | 1*(1-1)=0 | 1*(1-1)=0 | 

Tổng thô = 2, đáp án cuối cùng = 1 sau khi chia. 

Điều này cho thấy rằng`(ab, ac)`được tính hai lần, một lần từ mỗi điểm cuối, xác nhận lý do tại sao cần chia cho hai. 

### Ví dụ 2 

đầu vào:```
aa
ab
ba
bb
```| chuỗi | tương tự đầu tiên | cùng giây | 
| --- | --- | --- | 
| aa | 1*(2-1)=1 | 1*(2-1)=1 | 
| ab | 1*(2-1)=1 | 1*(2-1)=1 | 
| ba | 1*(2-1)=1 | 1*(2-1)=1 | 
| bb | 1*(2-1)=1 | 1*(2-1)=1 | 

Tổng thô = 8, câu trả lời cuối cùng = 4. 

Chúng tương ứng với bốn cặp hợp lệ trong đó các chuỗi khác nhau ở chính xác một vị trí: thay đổi hàng hoặc cột trong cách diễn giải lưới 2x2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chuỗi được xử lý một số lần không đổi | 
| Không gian | O(1) | Bảng chữ cái bị giới hạn, bản đồ tần số vẫn nhỏ | 

Các ràng buộc cho phép tổng cộng lên tới 100000 chuỗi và thuật toán chỉ thực hiện tổng hợp tuyến tính cho mỗi trường hợp thử nghiệm, do đó nó dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import comb

    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        freq = {}
        f1 = {}
        f2 = {}
        arr = []
        for _ in range(n):
            s = input().strip()
            arr.append(s)
            freq[s] = freq.get(s, 0) + 1
            f1[s[0]] = f1.get(s[0], 0) + 1
            f2[s[1]] = f2.get(s[1], 0) + 1

        ans = 0
        for s, c in freq.items():
            a, b = s
            ans += c * (f1[a] - c)
            ans += c * (f2[b] - c)

        out.append(str(ans // 2))
    return "\n".join(out)

# provided samples
assert run("""4
6
ab
cb
db
aa
cc
ef
7
aa
bb
cc
ac
ca
bb
aa
4
kk
kk
ab
ab
5
jf
jf
jk
jk
jk
""") == """5
6
0
6"""

# all identical
assert run("""1
4
aa
aa
aa
aa
""") == "0"

# alternating grid
assert run("""1
4
ab
ac
db
dc
""") == "4"

# minimal
assert run("""1
1
aa
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các chuỗi giống hệt nhau | 0 | không có cặp khác nhau hợp lệ | 
| mẫu lưới 2x2 | 4 | ghép nối có cấu trúc đầy đủ | 
| chuỗi đơn | 0 | trường hợp ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các chuỗi giống hệt nhau. Trong tình huống đó, cả hai lần đếm tần số vị trí đều khớp với tần số đầy đủ, do đó, mọi phép trừ sẽ hủy bỏ mọi thứ và kết quả bằng 0, điều này tránh được việc đếm các cặp giống hệt nhau. 

Một trường hợp quan trọng khác là khi các chuỗi được cân bằng hoàn hảo trên tất cả các tổ hợp của hai chữ cái, chẳng hạn như`"ab", "ac", "db", "dc"`. Ở đây, mỗi cặp khác nhau ở chính xác một vị trí và công thức sẽ tính tất cả các kết hợp chéo một cách tự nhiên một lần cho mỗi điểm cuối trước khi giảm một nửa. 

Cuối cùng, khi n bằng 1, tất cả các chênh lệch tần số sẽ giảm xuống 0 ngay lập tức và không có truy cập không hợp lệ hoặc đóng góp tiêu cực ngẫu nhiên nào xảy ra vì tất cả các biểu thức đều có dạng`c * (f - c)`trở thành số không.
