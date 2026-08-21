---
title: "CF 104101C - Thêm 9 số 0"
description: "Chúng ta được giao một tập hợp các bài toán, mỗi bài được đặc trưng bởi một giá trị số nguyên duy nhất biểu thị số 0 ở cuối thang độ khó của nó theo lũy thừa mười."
date: "2026-07-02T02:07:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104101
codeforces_index: "C"
codeforces_contest_name: "The 2022 Zhejiang University City College Freshman Programming Contest"
rating: 0
weight: 104101
solve_time_s: 46
verified: true
draft: false
---

[CF 104101C - Thêm 9 số 0](https://codeforces.com/problemset/problem/104101/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một tập hợp các bài toán, mỗi bài được đặc trưng bởi một giá trị số nguyên duy nhất biểu thị số 0 ở cuối thang độ khó của nó theo lũy thừa mười. Cụ thể, bài toán thứ i gặp khó khăn$10^{a_i}$, vì vậy chúng ta có thể coi mỗi vấn đề như được gắn nhãn bởi số mũ của nó$a_i$. Tất cả$a_i$các giá trị khác nhau nên mỗi mức độ khó là duy nhất. 

Từ tập hợp này, chúng ta được phép chọn một số tập con của bài toán. Với mỗi bài toán đã chọn có số mũ$a$, chúng ta tạo ra một bài toán mới có số mũ trở thành$a + 9$, nhân độ khó của nó một cách hiệu quả với$10^9$. Những bài toán biến đổi này tạo thành một tập B mới. Tuy nhiên, chúng ta không được phép tạo một bài toán biến đổi đã tồn tại trong tập A ban đầu. Nói cách khác, nếu$a + 9$đã có mặt trong số các số mũ ban đầu, chúng ta không thể bao gồm$a$trong tập hợp con đã chọn của chúng tôi. 

Nhiệm vụ là tối đa hóa số lượng bài toán chúng ta có thể chọn theo hạn chế này. 

Kích thước đầu vào có thể lớn như$5 \times 10^5$, điều này ngay lập tức loại trừ mọi phương trình bậc hai hoặc thậm chí$O(n \log n)$cách tiếp cận liên tục tìm kiếm hoặc mô phỏng xung đột giữa các cặp. Cấu trúc gợi ý rằng mỗi giá trị chỉ tương tác với một giá trị khác, cụ thể là$a + 9$, gợi ý về một giải pháp tra cứu trực tiếp hoặc dựa trên hàm băm. 

Một trường hợp lỗi tinh vi xuất hiện khi nhiều chuỗi giá trị gián tiếp thông qua các phép cộng lặp đi lặp lại, ví dụ: nếu$a$,$a+9$, Và$a+18$tất cả đều tồn tại. Một kẻ tham lam ngây thơ xử lý theo thứ tự tùy ý có thể dễ dàng đưa ra những lựa chọn không nhất quán nếu nó không thực thi rõ ràng quy tắc nhất quán toàn cầu. Một cạm bẫy khác là coi đây như một vấn đề sắp xếp mà không nhận ra rằng chỉ có va chạm chính xác +9 mới quan trọng chứ không phải thứ tự tương đối. 

Ví dụ: nếu đầu vào là:```
a = [1, 10, 19]
```thì chọn 1 cấm 10, chọn 10 cấm 19, nhưng chọn 1 và 19 cùng nhau là hợp lệ vì 1+9=10 tồn tại nhưng 19+9=28 không tồn tại. Câu trả lời đúng là 2, nhưng một kẻ tham lam bất cẩn luôn chặn hàng xóm theo thứ tự được sắp xếp có thể đếm thiếu một cách không chính xác. 

## Phương pháp tiếp cận 

Chế độ xem brute-force rất đơn giản: đối với mỗi tập hợp con của chỉ mục, hãy kiểm tra xem việc chọn nó có hợp lệ hay không. Đối với tập S đã chọn, chúng ta xác minh rằng với mọi$a \in S$, giá trị$a+9$không có trong mảng ban đầu. Điều này đòi hỏi phải kiểm tra tư cách thành viên của từng phần tử và việc thử tất cả các tập hợp con sẽ dẫn đến$O(2^n)$, điều này hoàn toàn không khả thi ngay cả đối với những ràng buộc nhỏ. 

Ngay cả khi chúng tôi cố gắng cải thiện điều này bằng cách kiểm tra từng ứng viên một cách độc lập, chúng tôi vẫn gặp phải cấu trúc phụ thuộc: việc chọn một giá trị sẽ ảnh hưởng đến nhiều nhất một giá trị khác. Quan sát quan trọng là mỗi số chỉ xung đột với chính xác một đối tác có thể có, số chín lớn hơn nó. Không còn sự phụ thuộc trong phạm vi nữa. Điều này có nghĩa là vấn đề giảm xuống còn việc ghép nối các phần tử và tránh chọn cả hai đầu của các cạnh có hướng nhất định$a \to a+9$. 

Khi được nhìn theo cách này, cấu trúc sẽ trở thành một biểu đồ có hướng trong đó mỗi nút có nhiều nhất một cạnh hướng ra ngoài. Chiến lược tối ưu chỉ đơn giản là tránh chọn các nút bị “chặn” bởi sự hiện diện của đối tác đến của chúng. Nếu như$a$tồn tại và$a-9$tồn tại thì chọn$a-9$cấm$a$. Như vậy mỗi cặp$(a, a+9)$đóng góp tối đa một phần tử có thể sử dụng được. Mọi phần tử không liên quan đến một cặp như vậy luôn có thể được chọn. 

Do đó, chúng ta có thể tính toán câu trả lời bằng cách lặp qua tất cả các giá trị và đếm xem có bao nhiêu giá trị không có dạng$x+9$đối với một số hiện có$x$hoặc tương đương là đếm tất cả các phần tử và trừ đi những phần tử "bắt đầu không hợp lệ" trong một cặp xung đột. Một cái nhìn rõ ràng hơn là: cho mọi$a$, nếu như$a-9$tồn tại thì$a$không thể được chọn nếu chúng ta muốn tối đa hóa các lựa chọn mà không có xung đột tính hai lần, vì vậy mỗi mối quan hệ như vậy sẽ giảm các lựa chọn tiềm năng bằng cách thực thi ràng buộc theo cặp. 

Một phương pháp trực tiếp và tối ưu là lưu trữ tất cả các giá trị trong một tập hợp băm và đếm xem có bao nhiêu giá trị “không được ghép nối theo nghĩa loại trừ bắt buộc”. Vì mỗi số chỉ có một xung đột có thể xảy ra nên chúng tôi giải quyết hiệu quả tất cả các cặp một cách độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force |$O(2^n)$|$O(n)$| Quá chậm | 
| Kiểm tra cặp bộ băm |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi danh sách số mũ thành một tập hợp cho các truy vấn thành viên liên tục. Mục tiêu là xác định có bao nhiêu phần tử có thể được chọn sao cho chúng ta không bao giờ chọn cả hai$a$Và$a+9$. 

1. Lưu trữ tất cả các giá trị trong bộ băm. Điều này cho phép chúng ta kiểm tra sự tồn tại của$x+9$trong thời gian không đổi. 
2. Khởi tạo bộ đếm cho câu trả lời bằng 0. 
3. Lặp lại mọi giá trị$a$trong mảng. 
4. Kiểm tra xem$a-9$tồn tại trong tập hợp. 
5. Nếu$a-9$không tồn tại, hãy tăng câu trả lời lên một, bởi vì$a$không bị buộc phải loại trừ bởi một phần tử nhỏ hơn. 
6. Nếu không, hãy bỏ qua nó, vì nó là một phần của cặp xung đột trong đó phần tử nhỏ hơn đã đại diện cho đại diện tối ưu. 

Lý do đằng sau quy tắc này là trong mọi cặp xung đột$(a, a+9)$, chỉ giá trị nhỏ hơn mới được phép tính. Bằng cách luôn ưu tiên điểm cuối nhỏ hơn, chúng tôi đảm bảo rằng không có cặp nào đóng góp nhiều hơn một phần tử đã chọn và mọi phần tử độc lập vẫn được tính. 

### Tại sao nó hoạt động 

Mỗi xung đột đều mang tính cục bộ: nếu$a$Và$a+9$cả hai đều tồn tại, chúng tạo thành một sự phụ thuộc duy nhất trong đó việc chọn cả hai đều bị cấm. Thuật toán chỉ định chính xác một đại diện cho mỗi chuỗi như vậy bằng cách luôn ưu tiên phần tử nhỏ nhất trong bất kỳ chuỗi liên tiếp nào được liên kết bởi +9 điểm khác biệt. Vì không có nút nào có thể thuộc về nhiều hơn một cấu trúc phân nhánh chuỗi độc lập (mỗi nút có nhiều nhất một nút tiền nhiệm và một nút kế tiếp trong mối quan hệ này), việc chỉ đếm các phần tử không có tiền thân đảm bảo chính xác một phần tử cho mỗi thành phần hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    s = set(arr)
    
    ans = 0
    for a in arr:
        if (a - 9) not in s:
            ans += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai hoàn toàn phụ thuộc vào tư cách thành viên đã thiết lập. Lựa chọn thiết kế chính là lặp lại các phần tử và chỉ đếm những phần tử không được "bao phủ" bởi phần tử tiền nhiệm khác 9. Điều này tránh việc tính hai lần trong các chuỗi như$1 \to 10 \to 19$, trong đó chỉ phần tử đầu tiên trong chuỗi được tính. 

Một lỗi phổ biến là kiểm tra$a+9$thay vì$a-9$, dẫn đến việc đếm hai lần hoặc xử lý hướng không nhất quán. Việc sử dụng kiểm tra trước đó sẽ thực thi một đại diện duy nhất cho mỗi chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [1, 10, 19]
```| một | a-9 trong bộ? | tính | 
| --- | --- | --- | 
| 1 | không | vâng | 
| 10 | vâng | không | 
| 19 | vâng | không | 

Đầu ra là 1. 

Điều này chứng tỏ rằng cả ba giá trị tạo thành một chuỗi duy nhất và chỉ phần tử nhỏ nhất được tính. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [2, 11, 5, 20]
```| một | a-9 trong bộ? | tính | 
| --- | --- | --- | 
| 2 | không | vâng | 
| 11 | vâng | không | 
| 5 | không | vâng | 
| 20 | vâng | không | 

Đầu ra là 2. 

Ở đây chúng tôi có hai chuỗi độc lập:$2 \to 11$Và$5 \to 14$(mặc dù vắng mặt 14) nên 2 và 5 là đại diện độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử được xử lý một lần với tra cứu băm theo thời gian liên tục | 
| Không gian |$O(n)$| Đặt lưu trữ tất cả các giá trị đầu vào | 

Các ràng buộc cho phép lên đến$5 \times 10^5$các phần tử và đường truyền tuyến tính có hàm băm phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal
assert run("1\n5\n") == "1"

# no conflicts
assert run("3\n1 2 3\n") == "3"

# single chain
assert run("3\n1 10 19\n") == "1"

# mixed chains
assert run("4\n2 11 5 20\n") == "2"

# larger mixed
assert run("5\n1 10 2 11 100\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở | 
| không xung đột | n | trường hợp độc lập | 
| 1 10 19 | 1 | phụ thuộc xích | 
| 2 11 5 20 | 2 | nhiều cặp độc lập | 
| 1 10 2 11 100 | 3 | chuỗi chồng chéo | 

## Vỏ cạnh 

Trường hợp cạnh chính là một chuỗi dài gồm +9 điểm khác biệt, chẳng hạn như:```
1 10 19 28 37
```Thuật toán xử lý từng phần tử và chỉ tính những phần tử không có phần tử trước: 

- 1 không có 1-9, tính 
- 10 có 1 trong bộ, bỏ qua 
- 19 có 10 trong bộ, bỏ qua 
- 28 có 19 trong bộ, bỏ qua 
- 37 có 28 trong bộ, bỏ qua 

Đầu ra là 1, phù hợp với quy tắc dự kiến là chỉ chọn một đại diện cho mỗi chuỗi. 

Một trường hợp cạnh khác là các cặp rời rạc:```
1 10 3 12
```Cả hai chuỗi hoạt động độc lập và thuật toán đếm chính xác 2 phần tử, chọn phần tử tối thiểu của mỗi cặp.
