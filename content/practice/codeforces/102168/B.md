---
title: "CF 102168B - \u0423\u0434\u0432\u043e\u0435\u043d\u0438\u044f"
description: "Chúng tôi bắt đầu với một tập hợp các số nguyên, do đó các giá trị đầu vào trùng lặp chỉ được lưu trữ một lần. Quy trình được lặp lại (10^{10}) lần. Ở mỗi lần lặp, chúng tôi lấy giá trị tối thiểu hiện tại (x), xóa nó và thử chèn (2x). Nếu (2x) đã có mặt thì tập hợp sẽ mất một phần tử."
date: "2026-08-19T15:15:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102168
codeforces_index: "B"
codeforces_contest_name: "\u041b\u0438\u0447\u043d\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u043e\u0433\u043e \u0443\u043d\u0438\u0432\u0435\u0440\u0441\u0438\u0442\u0435\u0442\u0430 \u0441\u0440\u0435\u0434\u0438 \u043d\u043e\u0432\u0438\u0447\u043a\u043e\u0432 2018-2019"
rating: 0
weight: 102168
solve_time_s: 178
verified: true
draft: false
---

[CF 102168B - \u0423\u0434\u0432\u043e\u0435\u043d\u0438\u044f](https://codeforces.com/problemset/problem/102168/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một tập hợp các số nguyên, do đó các giá trị đầu vào trùng lặp chỉ được lưu trữ một lần. Quy trình được lặp lại (10^{10}) lần. Ở mỗi lần lặp, chúng tôi lấy giá trị tối thiểu hiện tại (x), xóa nó và thử chèn (2x). Nếu (2x) đã có mặt thì tập hợp sẽ mất một phần tử. Nếu không, kích thước đã đặt sẽ không thay đổi. 

Đầu vào chứa (n) số nguyên ban đầu, với (1 \le n \le 200000) và mọi giá trị tối đa là (10^9). Đầu ra bắt buộc là số số nguyên riêng biệt còn lại sau tất cả (10^{10}) phép toán. 

Số lượng lớn các hoạt động ngay lập tức loại trừ việc mô phỏng trực tiếp. Ngay cả việc triển khai (O(1)) được lý tưởng hóa cũng sẽ cần (10^{10}) lần lặp, trong khi một tập hợp hoặc vùng nhớ heap thông thường sẽ thêm một hệ số logarit khác. Với (n) lên tới (200000) và giới hạn hai giây, chúng ta cần thay thế mô phỏng bằng một quan sát toán học. 

Có ba trường hợp đặc biệt thường gây ra câu trả lời sai. Đầu tiên, các nội dung trùng lặp trong đầu vào phải biến mất ngay lập tức. Ví dụ, đầu vào`3\n5 5 5`đại diện cho bộ`{5}`và sau bất kỳ số thao tác nào, kích thước của nó vẫn giữ nguyên`1`, vậy câu trả lời là`1`, không`3`. Ở đây, một giải pháp bất cẩn đếm các phần tử đầu vào thay vì các giá trị riêng biệt đã thất bại. 

Thứ hai, hai số khác nhau cuối cùng có thể hợp nhất. Vì`2\n3 6`, tập hợp ban đầu là`{3, 6}`. Hoạt động đầu tiên thay đổi`3`vào trong`6`, sản xuất`{6}`, vậy câu trả lời là`1`. Chỉ cần đếm các giá trị ban đầu khác biệt sẽ cho`2`. 

Thứ ba, các số có liên hệ lũy thừa hai đều thuộc cùng một chuỗi. Vì`3\n5 10 20`, các giá trị là (5\cdot2^0), (5\cdot2^1) và (5\cdot2^2). Mức tối thiểu được nhân đôi nhiều lần cho đến khi các giá trị này hợp nhất và kích thước cuối cùng là`1`. Chỉ nhìn vào các giá trị số ban đầu sẽ bỏ sót cấu trúc này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là duy trì tập hợp, liên tục tìm mức tối thiểu của nó, loại bỏ nó và chèn gấp đôi giá trị đó. Nó đúng vì nó tuân theo chính xác thao tác, kể cả trường hợp giá trị nhân đôi đã tồn tại. Vấn đề là số lần lặp lại. Quy trình này yêu cầu chính xác (10^{10}) thao tác, do đó, ngay cả khi việc tìm và cập nhật mức tối thiểu mất thời gian không đổi thì quá trình mô phỏng sẽ thực hiện 10 tỷ lần lặp. Với cây hoặc vùng heap cân bằng, chi phí thậm chí còn lớn hơn, khoảng (O(10^{10}\log n)). 

Điều quan trọng là ngừng suy nghĩ về độ lớn thực tế và thay vào đó hãy xem điều gì xảy ra khi một số được nhân đôi liên tục. Mọi số nguyên dương đều có thể viết duy nhất dưới dạng 

[ 
x = lẻ(x)\cdot2^k, 
] 

trong đó (od(x)) là số lẻ. Chỉ thay đổi nhân đôi (k), còn phần lẻ không đổi. Do đó, các giá trị có phần lẻ khác nhau không bao giờ có thể tương tác với nhau. Các giá trị có phần lẻ giống nhau tạo thành một chuỗi độc lập: 

[ 
c,\ 2c,\ 4c,\ 8c,\ldots 
] 

trong đó (c) là số lẻ. 

Bên trong một chuỗi như vậy, phép toán di chuyển số mũ nhỏ nhất sang phải một bước. Bất cứ khi nào số mũ tiếp theo đó đã tồn tại, hai phần tử tập hợp sẽ trở thành một. Cuối cùng, tất cả các giá trị thuộc cùng một chuỗi sẽ hợp nhất thành một giá trị duy nhất. 

Giới hạn (a_i\le10^9) làm cho điều này trở nên đặc biệt mạnh mẽ. Vì (2^{29<10^9<2^{30}), mọi giá trị ban đầu chứa tối đa 29 thừa số của 2. Do đó, một chuỗi cần tối đa 29 bước nhân đôi hữu ích trước khi tất cả các phần tử ban đầu của nó hợp nhất thành một. Có nhiều nhất (n) chuỗi, do đó cần ít hơn (29n\le5.8\cdot10^6) thao tác hữu ích để giảm mỗi chuỗi thành một phần tử. Con số này rất nhỏ so với (10^{10}). 

Sau thời điểm đó, mỗi chuỗi chứa chính xác một phần tử. Nhân đôi phần tử duy nhất của nó sẽ tạo ra một giá trị khác trong cùng một chuỗi, nhưng không có phần tử thứ hai nào ở đó để hợp nhất với nó. Do đó, kích thước được đặt không đổi cho mọi hoạt động còn lại. 

Vì vậy, câu trả lời cuối cùng chỉ đơn giản là số phần lẻ khác nhau giữa các số ban đầu. Chúng ta có thể đạt được điều đó bằng cách liên tục chia mỗi giá trị đầu vào cho hai khi giá trị đó là số chẵn, sau đó chèn giá trị lẻ thu được vào một tập hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(10^{10}\log n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log a_{\max})) dự kiến ​​| (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả (n) giá trị ban đầu và tạo một tập hợp trống`odd_parts`. Chúng ta chỉ cần biết chuỗi lẻ nào tồn tại, bởi vì các chuỗi có phần lẻ khác nhau không bao giờ tương tác với nhau. 
2. Với mọi giá trị`x`, liên tục chia nó cho hai khi nó là số chẵn. Sau vòng lặp này,`x`là phần lẻ của số ban đầu 
3. Chèn phần lẻ này vào`odd_parts`. Nếu một số giá trị ban đầu thuộc cùng một chuỗi nhân đôi, chúng sẽ tạo ra phần lẻ giống nhau và chỉ được lưu trữ một lần. 
4. Xuất ra kích thước của`odd_parts`. Các hoạt động (10^{10}) được đảm bảo có đủ thời gian để thu gọn mọi chuỗi ban đầu thành một phần tử, sau đó các hoạt động tiếp theo không thay đổi kích thước đã đặt. 

### Tại sao nó hoạt động 

Xét một số lẻ cố định (c). Mọi giá trị có phần lẻ (c) đều có dạng (c2^k), do đó, phép toán trên giá trị đó chỉ có thể thay đổi số mũ (k) thành (k+1). Nó không bao giờ có thể tương tác với một giá trị có phần lẻ khác với (c). 

Trong chuỗi của (c), lấy số mũ nhỏ nhất hiện có. Thao tác loại bỏ số mũ đó và chèn số mũ tiếp theo. Nếu số mũ tiếp theo đã có thì số phần tử trong chuỗi sẽ giảm đi một. Việc lặp lại quá trình này cuối cùng sẽ hợp nhất mọi số mũ hiện tại ban đầu thành số mũ có liên quan lớn nhất, để lại chính xác một phần tử trong chuỗi. 

Mỗi số mũ ban đầu tối đa là 29 vì giá trị ban đầu tối đa là (10^9). Do đó, mỗi chuỗi cần tối đa 29 hoạt động góp phần hợp nhất. Trên hầu hết (200000) chuỗi, ít hơn (5,8\cdot10^6) thao tác là đủ để đạt được một phần tử trên mỗi chuỗi, thấp hơn nhiều (10^{10}). 

Khi chuỗi có một phần tử, giá trị tối thiểu của nó sẽ tăng gấp đôi thành giá trị vắng mặt trước đó, do đó kích thước của nó vẫn là một. Do đó, sau khi tất cả các chuỗi đã sụp đổ, kích thước được đặt chính xác là số phần lẻ riêng biệt hiện diện ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = map(int, input().split())

    odd_parts = set()

    for x in a:
        while x % 2 == 0:
            x //= 2
        odd_parts.add(x)

    print(len(odd_parts))

if __name__ == "__main__":
    solve()
```các`odd_parts`bộ đại diện cho chuỗi nhân đôi độc lập. Đối với mỗi số đầu vào,`while`vòng lặp loại bỏ mọi thừa số của hai, để lại chính xác thành phần lẻ duy nhất của nó. 

Vòng lặp thực hiện tối đa 29 lần cho một giá trị, bởi vì (2^{29<10^9). Số nguyên Python không bị tràn khi thực hiện phép chia và trên thực tế, các giá trị chỉ trở nên nhỏ hơn trong quá trình tiền xử lý này. 

trận chung kết`len(odd_parts)`đếm các chuỗi chứ không phải các phần tử ban đầu. Điều này tự động xử lý các giá trị và giá trị đầu vào trùng lặp, chẳng hạn như`5`,`10`, Và`20`cuối cùng hợp nhất. 

Không cần mô phỏng các hoạt động (10^{10}). 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`2`với các giá trị`3 4`. 

| Giá trị gốc | Phần lẻ |`odd_parts`sau khi chèn | 
| --- | --- | --- | 
| 3 | 3 |`{3}`| 
| 4 | 1 |`{1, 3}`| 

Hai giá trị thuộc về các chuỗi khác nhau. Chuỗi bắt đầu từ 3 chứa (3,6,12,\ldots), trong khi chuỗi bắt đầu từ 1 chứa (1,2,4,\ldots). Cả hai chuỗi đều không thể hợp nhất với chuỗi kia nên câu trả lời cuối cùng là`2`. 

Đối với Mẫu 2, đầu vào là`20`với các giá trị`10 5`. 

| Giá trị gốc | Phần lẻ |`odd_parts`sau khi chèn | 
| --- | --- | --- | 
| 10 | 5 |`{5}`| 
| 5 | 5 |`{5}`| 

Cả hai giá trị đều thuộc cùng một chuỗi: 

[ 
5,\ 10,\ 20,\ 40,\ldots 
] 

Hai yếu tố ban đầu cuối cùng sụp đổ thành một. Sau đó, nhân đôi phần tử còn lại chỉ cần di chuyển nó về phía trước trong cùng một chuỗi, do đó kích thước của nó vẫn là một. Câu trả lời là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log a_{\max})) dự kiến ​​| Mỗi giá trị được chia cho hai tối đa 29 lần và dự kiến ​​sẽ chèn tập hợp (O(1)). | 
| Không gian | (O(n)) | Nhiều nhất một phần lẻ được lưu trữ cho mỗi giá trị đầu vào. | 

Với (n\le200000) và nhiều nhất là 29 phép chia cho mỗi giá trị, quá trình tiền xử lý chỉ thực hiện vài triệu thao tác đơn giản. Tập hợp chứa tối đa (200000) số nguyên, nằm trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    n = int(input())
    a = map(int, input().split())

    odd_parts = set()

    for x in a:
        while x % 2 == 0:
            x //= 2
        odd_parts.add(x)

    print(len(odd_parts))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run("2\n3 4\n") == "2", "sample 1"

# Provided sample 2
assert run("2\n10 5\n") == "1", "sample 2"

# Minimum size
assert run("1\n1\n") == "1", "single element"

# All values are equal
assert run("5\n7 7 7 7 7\n") == "1", "duplicates"

# Several values from the same doubling chain
assert run("5\n5 10 20 40 80\n") == "1", "one doubling chain"

# Boundary value 1e9 and values from different chains
assert run("3\n1000000000 500000000 3\n") == "2", "boundary and shared chain"

# Maximum n, all equal
assert run("200000\n" + "1 " * 199999 + "1\n") == "1", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\n3 4`|`2`| Cung cấp mẫu với hai chuỗi độc lập | 
|`2\n10 5`|`1`| Mẫu được cung cấp trong đó cả hai giá trị đều có phần lẻ | 
|`1\n1`|`1`| Kích thước đầu vào tối thiểu | 
|`5\n7 7 7 7 7`|`1`| Giá trị trùng lặp phải được tính một lần | 
|`5\n5 10 20 40 80`|`1`| Toàn bộ đầu vào thuộc về một chuỗi nhân đôi | 
|`3\n1000000000 500000000 3`|`2`| Giá trị lớn và chuỗi chia sẻ ở ranh giới trên | 
|`200000`bản sao của`1`|`1`| Xử lý tối đa (n) và trùng lặp | 

## Vỏ cạnh 

Trường hợp trùng lặp`3\n5 5 5`được rút gọn ngay thành phần lẻ duy nhất`5`. Tập hợp chỉ chứa một phần tử ngay từ đầu và mỗi lần nhân đôi tiếp theo sẽ di chuyển phần tử đó dọc theo chuỗi mà không thay đổi kích thước tập hợp. Thuật toán chèn`5`ba lần vào một tập hợp Python, nhưng tập hợp đó vẫn chỉ chứa một giá trị, do đó nó xuất ra`1`. 

Đối với trường hợp sáp nhập`2\n3 6`, chia lũy thừa của hai cho phần lẻ`3`Và`3`. Thuật toán chỉ lưu trữ`{3}`và đầu ra`1`. Điều này nắm bắt hành vi trong tương lai mặc dù hai giá trị ban đầu là khác nhau. 

Đối với chuỗi dài hơn`3\n5 10 20`, cả ba giá trị đều giảm về cùng một phần lẻ`5`. Do đó, tập hợp này có một chuỗi và câu trả lời là`1`. Thực tế là chuỗi chứa một số lũy thừa liên tiếp của hai không yêu cầu mô phỏng bất kỳ hoạt động nào. 

Ở ranh giới số trên,`1000000000`chẵn và liên tục chia cho hai cuối cùng tạo ra`1953125`. Số chỉ có chín thừa số hai nên việc tiền xử lý vẫn còn rất nhỏ. Thuật toán không bao giờ cần xây dựng các giá trị lớn hơn nhiều có thể xuất hiện sau khi nhân đôi (10^{10}), điều này cũng tránh làm cho câu trả lời phụ thuộc vào các số nguyên mô phỏng khổng lồ. 

Cuối cùng, khi (n=200000) và tất cả các giá trị đều bằng nhau, tập đầu vào có kích thước bằng một mặc dù có chứa (200000) mục nhập. Thuật toán thực hiện một lượng công việc không đổi trên mỗi mục nhập và chỉ lưu trữ một phần lẻ, do đó cả thời gian chạy và mức sử dụng bộ nhớ vẫn ở mức thoải mái trong giới hạn.
